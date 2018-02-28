---
layout:     post
title:      PowerShell in Azure Functions
date:       2018-02-27 06:00:00 -0800
summary:    Building your first Azure Function with PS
categories: blog
thumbnail:  bolt
author:     brianbunke
image:      /images/FunctionSplash.png
tags:
 - powershell
 - azure
 - serverless
 - api
 - functions
 - code
---

Prev: [Why?] \| **Serverless REST API series** \| Next: [GitHub integration]

---

Just hopping on the [serverless] train in 2018? Join the club! Lately, I've been a little obsessed with [Azure Functions] and the doors it opens.

However, PowerShell in Azure Functions is still a bit unintuitive when you're getting started. PowerShell is currently available only as an unsupported "experimental" language, and you should be aware of the expectations Azure Functions casts onto your code.

This post will show you:

- How to create your first Azure Function in PowerShell
- What the default code is actually doing
- What to apply from the example to your code
- Finished sample code to return JSON objects

[![splash](/images/FunctionSplash.png)](/images/FunctionSplash.png)

---

## Table of Contents
{: .no_toc}

- ToC
{:toc}

---

## Start with the official docs

The [official walkthrough] is useful. I'll mirror the steps here, because we have to deviate for PowerShell halfway through. (Hopefully the doc doesn't change tomorrow...)

#### 1) Log in to Azure

You just need a free, personal Azure account to get started.

Azure Functions is [mostly free] for personal use...you won't exceed the free tier execution/bandwidth limits without trying, so it's just a question of whether you need to store data for your use case.

The walkthrough in this post isn't storing anything in Azure.

#### 2) Create a function app

[![newafapp](/images/FunctionNew1.png)](/images/FunctionNew1.png)

_Your "app name" becomes a public subdomain: APPNAME.azurewebsites.net_

#### 3) Favorite Functions in the portal

Uh, sure. Ok, on to the good stuff.

#### 4) Create an HTTP triggered function

Follow these screenshots to create a not-yet-supported PowerShell function, instead of the default C# walkthrough.

[![newaf](/images/FunctionNew2.png)](/images/FunctionNew2.png)

[![newaftemplate](/images/FunctionNew3.png)](/images/FunctionNew3.png)

[![newafname](/images/FunctionNew4.png)](/images/FunctionNew4.png)

Spoiler alert on the function name: Yes, this means you will `/ScriptingGuysGet -Method Get`, which is silly. I thought I could mask this at the end of the series, but instead I'll briefly cover why I didn't. Name retained as my mark of shame and your warning.

#### 5) Test the function

You have a working PowerShell Azure Function! (We'll look at the code in a sec.) First, use PowerShell on your local workstation to test the new "HTTP trigger":

[![aftest1](/images/FunctionTest1.png)](/images/FunctionTest1.png)

```powershell
# Replace this with yours ;)
$YourURI = 'https://bbpowershellapi.azurewebsites.net/api/ScriptingGuysGet?code=blahblahKEYblahblah'

# Try a normal GET
Invoke-RestMethod -Method Get -Uri $YourURI
# GET using the "name" query parameter
Invoke-RestMethod -Method Get -Uri "$($YourURI)&name=World!"

# Use the POST method provided
$Body = @{name = 'Max Power'} | ConvertTo-Json
Invoke-RestMethod -Method Post -Body $Body -Uri $YourURI
```

[![aftest2](/images/FunctionTest2.png)](/images/FunctionTest2.png)

## Explaining the default PowerShell function code

Here's the default code when you create a PowerShell function:

```powershell
# POST method: $req
$requestBody = Get-Content $req -Raw | ConvertFrom-Json
$name = $requestBody.name

# GET method: each querystring parameter is its own variable
if ($req_query_name)
{
    $name = $req_query_name
}

Out-File -Encoding Ascii -FilePath $res -inputObject "Hello $name"
```

That code in English:

1. Accepts an optional `$name`
    - via POST
    - or via GET
2. Outputs the string `Hello $name`

If you wanted to always output the same string, your Azure Function could be one line:

```powershell
'Hello World!' | Out-File -Encoding Ascii -FilePath $res
```

### POST and GET

As you saw in our test section, POST and GET are two standard [REST API verbs]. This default script supports providing a name via both operations. (Which is weird. But that's ok; it's still a good, concise example.)

### Where are these variables coming from?

`$req` and `$res` are Azure Functions-specific automatic variables. They're just file paths, local to the currently executing function. If you start digging around, you'll see them referenced here:

[![afparam](/images/FunctionParam.png)](/images/FunctionParam.png)

- **$req:** The body of a POST request ends up in `$req`. This example code gets the `$req` file's contents, converts from JSON into an object, and then extracts the `name` property to set `$name`.
- **$res:** Azure Functions returns the contents of `$res`, and nothing else. If you see output in your log, but don't get a return in your local console, you may have forgotten to `Out-File $res`.

If you're developing a suite of Azure Functions, it's a good idea to have a Pester test file ensure you remember to `Out-File $res` in each individual function.

> For more info on the Azure Functions runtime environment, see Stefan Stranger's [TechNet post].

### Query parameters

The GET method uses `$req_query_name`. When we did our testing, we used the GET method with `name` as a [query parameter].

```powershell
Invoke-RestMethod -Method Get -Uri "$($YourURI)&name=World!"
```

Query parameters are used _everywhere._ `?` denotes the first parameter, and `&` allows you to append additional parameters.

As an example, [https://www.google.com/search?q=test&hl=en](https://www.google.com/search?q=test&hl=en) hits the /search endpoint, then sets your query = "test" and language = "en" (English).

In Azure Functions, variables are automatically created at runtime for each query parameter supplied. What does that mean?

```powershell
# The user can do this
Invoke-RestMethod -Method Get -Uri "$($YourURI)?food=yes&drink=yes&quantity=yes"

# Whether your Azure Function uses none, one, or all of the following variables:
$req_query_food
$req_query_drink
$req_query_quantity
```

In my limited testing, `$req_query_*` variables seem to be read-only once inside your PowerShell Azure Function.

## A sample script

The code below:

1. Optionally accepts `tag` as a query parameter
2. Queries the [Scripting Guys blog]'s RSS feed for either:
    - The ten most recent posts, or
    - The ten most recent posts containing the given `tag`
3. Returns each post's Date/Title/Description/Link in a JSON collection

What can you learn from this example?

- Param blocks are (apparently?) useless in Azure Functions
    - But it's nice to still notate any expected parameters, somehow
- Reaches outside of Azure, querying content outside of your control
- Proper output of a collection of objects in JSON format
- I can't get Verbose to work
    - So potential debugging is currently a frustrating endeavor

```powershell
# Supported query parameters:
  # tag

# Create an empty list to append results into
$ResultList = New-Object System.Collections.Generic.List[object]

# Set the URI, with or without a user-supplied tag
$BaseURI = 'https://blogs.technet.microsoft.com/heyscriptingguy'
If ($req_query_tag) {
    $BaseURI = "$BaseURI/tag/$req_query_tag"
}

Try {
    # Get the latest 10 posts (RSS feed default is 10 per page)
    $iwr = Invoke-WebRequest -Uri "$BaseURI/feed" -UseBasicParsing
} Catch {
    # Invoke-WebRequest is weird. Just silently fail
}

If ($iwr) {
    # Cast the RSS feed as XML
    [xml]$xml = $iwr.Content

    ForEach ($post in $xml.rss.channel.item) {
        # Assemble the most useful properties in an object
        $newObject = [PSCustomObject]@{
            Date        = $post.pubDate -as [DateTime]
            Title       = $post.title
            Description = $post.description.'#cdata-section'
            Link        = $post.link
        }

        # Append the object into the collection
        [void]$ResultList.Add($newObject)
    }

    # Return the objects in JSON format. Azure Functions likes Out-String
    $return = $ResultList | ConvertTo-Json | Out-String

    # By default, Azure Functions wants to output the contents of $res
    Out-File -Encoding Ascii -FilePath $res -inputObject $return
} Else {
    # Can't get Azure Functions to respect -Verbose, even trying this:
    # https://justingrote.github.io/2017/12/25/Powershell-Azure-Functions-The-Missing-Manual.html#logs-panel-and-verbosedebug-output
    # help, haha
    Write-Verbose 'Invoke-WebRequest returned no results' 4>&1
}
```

Drop that in your "ScriptingGuysGet" function and Save.

[![update](/images/FunctionUpdate.png)](/images/FunctionUpdate.png)

Then, calling it from your local workstation again, just as we did before:

[![finalproduct](/images/FunctionFinal.png)](/images/FunctionFinal.png)

Note that `Invoke-RestMethod` automatically _deserializes_ JSON; that is, it has implicitly converted the JSON response back into the standard PowerShell objects we know and love.

## "Best Practices" from Microsoft

If you're still here, hey! I assume you're pretty interested in writing your own function. You should know Microsoft has some general recommendations on being smart and responsible with your Azure Functions.

[https://docs.microsoft.com/en-us/azure/azure-functions/functions-best-practices](https://docs.microsoft.com/en-us/azure/azure-functions/functions-best-practices)

It includes things like "You can pass the HTTP trigger payload into a queue to be processed by a queue trigger function," which seems like something I would do here, had I bothered to spend 10 minutes looking into it.

I have not. (Yet!)

## Next steps (and diverging paths)

We have a working PowerShell Azure Function! With this, we've completed our first step toward building a Serverless REST API in Azure.

If building a full API doesn't sound interesting or fun...congratulations, you're normal! This post still stands on its own, and there's a lot of awesome directions you can take your first PowerShell-backed Azure Function that don't need a full, formal API front end.

For the abnormal among us, I'm really enjoying building my first formal REST API. Step two is setting up continuous deployment to our new Azure Function App from GitHub...I hope you'll stick around!

---

|[Build a Serverless REST API in Azure]|
|---|
| Part 2: (you are here) |
| Part 3: [GitHub Integration with Azure Functions] |
| Part 4: [Add an API spec in Azure Functions] |
| Part 5: [Azure Functions and Azure API Management] |
| Part 6: [Serverless API Series Conclusion] |



[serverless]: https://azure.microsoft.com/en-us/overview/serverless-computing/

[Azure Functions]: https://azure.microsoft.com/en-us/services/functions/

[official walkthrough]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-azure-function

[mostly free]: https://azure.microsoft.com/en-us/pricing/details/functions/

[REST API verbs]: http://www.restapitutorial.com/lessons/restquicktips.html
[TechNet post]: https://blogs.technet.microsoft.com/stefan_stranger/2017/01/29/powershell-azure-functions-lessons-learned/

[query parameter]: https://docs.microsoft.com/en-us/rest/api/#components-of-a-rest-api-requestresponse

[Scripting Guys blog]: https://blogs.technet.microsoft.com/heyscriptingguy/

[Why?]:                                     /blog/2018/02/26/serverless-api-in-azure/
[Build a Serverless REST API in Azure]:     /blog/2018/02/26/serverless-api-in-azure/
[GitHub integration]:                       /blog/2018/02/28/github-integration-with-azure-functions/
[GitHub Integration with Azure Functions]:  /blog/2018/02/28/github-integration-with-azure-functions/
[Add an API spec in Azure Functions]:       /blog/2018/03/01/azure-functions-swagger-spec/
[Azure Functions and Azure API Management]: /blog/2018/03/02/azure-functions-api-management/
[Serverless API Series Conclusion]:         /blog/2018/03/03/serverless-api-conclusion/
