---
layout:     post
title:      Add an API spec in Azure Functions
date:       2018-03-01 06:00:00 -0800
summary:    Use Swagger to define your services
categories: blog
thumbnail:  cog
author:     brianbunke
image:      /images/SwaggerFunctionsSplash.png
tags:
 - powershell
 - azure
 - serverless
 - api
 - functions
 - code
 - swagger
---

Prev: [GitHub integration] \| **[Serverless REST API series]** \| Next: [API Management]

---

Our starting point today is an Azure Functions App with two functions inside.

This is nice if I want to directly query it, using something like `Invoke-RestMethod`. But I wish I could do something more with these functions, like:

- Connect to or from them in [Microsoft Flow] / [Azure Logic Apps]
- Import into [Azure API Management]
- Tie-in 12 other Azure services I don't know about by the time you read this
- See them in a familiar [Swagger] HTML interface
    - Easily consumable endpoint documentation for users
    - Simple "Try it now" buttons in your browser, no code needed
    - Consumption by common tools like [Postman]

With an API specification (spec), your function app has now officially graduated to big kid underwear...and it's a much easier potty training process than the other one you may be familiar with.

[![splash](/images/SwaggerFunctionsSplash.png)](/images/SwaggerFunctionsSplash.png)

---

## Table of Contents
{: .no_toc}

- ToC
{:toc}

---

## Following in footsteps

For the third post in a row, I'm linking [the walkthrough] on Microsoft's official docs site. Just saying that feels like a positive change from the past.

In the linked doc, we're starting at the _"Generate the OpenAPI definition"_ section, and we'll adapt things to our functions as needed.

## Define method verb(s) in functions

The first action described--specifying that our current functions only accept the GET method--now needs to be defined in code, since we enabled GitHub integration in the previous post.

You'll need to do this for the `function.json` file inside _both_ function subfolders:

[![methods](/images/SwaggerFunctions1.png)](/images/SwaggerFunctions1.png)

> If you're doing something different that you don't know how to define to Azure Functions in code, you can quickly figure out most things:
> 
> 1. Function app settings > change edit mode to "Read/Write" (temporarily)
> 2. Make your change(s)
> 3. Function app overview > Download app content

## Generate the API definition template

Summarizing from [the walkthrough] (click through if you'd like screenshots).

- Function app > Platform features tab > API > API definition
- API definition source = "Function (preview)"
- Generate API definition template
    - After 5-10 seconds, a bundle of YAML should appear in the text box
- Save

> Remember what I just said about "Download app content"? If you do it again now, you'll see new `.azurefunctions` > `swagger` directories, with your new `swagger.json` inside.
> 
> Swagger v2.0 specs can be defined in YAML or JSON; it's YAML in the portal, but it looks like the Save process converts it to JSON on the back end.

Also of note: "Generate API definition template" immediately populates 6 "Cross-Origin Resource Sharing" (CORS) rules in your functions app. This allows `functions.azure.com`, `portal.azure.com`, and variations therein to immediately communicate with your function app.

## Modify the API spec

Ok, ignore my aside about the code download, get back in the Azure portal! We'll stay there for consistency's sake, but granted that you could do these steps in either place.

Two quick asides before we begin:

- This walkthrough uses the [Swagger v2.0 spec]. I'd love to use the successor, [OpenAPI v3.0], but I couldn't get Azure to accept it as a valid spec. Don't know if that's my fault or not, and/or a potential timeline to support it.
- The [Swagger v2.0 spec] (linked again for extra obnoxious clickability) is useful if you'd like to supplement properties in this section...which I will. New stuff I've added either came from Microsoft's walkthrough or the v2.0 docs.

### Review the YAML changes

Instead of a massive screenshot or code block, I'll link to the GitHub repo highlighting the difference from start (generation) to finished product.

#### --> **[Link to full diff]** <--
{: .no_toc}

Remember, Azure Functions expects a .json definition within a subfolder, so this linked YAML _will not_ change anything. It's there only to provide a permalink to the YAML evolution, for this blog post's reference.

[![sidebyside](/images/SwaggerFunctions2.png)](/images/SwaggerFunctions2.png)

### Summarize the changes

To give brief context to changes made, and help point out things you can consider when modifying your API spec:

- [3-9]\: You can add things to the `info` object, based on the Swagger v2.0 definition
- [18-23]\: Describe your PowerShellTeamGet function
    - `summary`, `description`, `x-ms-summary` provide friendly text and help integrate with the Azure platform:
        - [Extend an OpenAPI definition for a custom connector]
    - `operationId` is a unique identifier
    - A `GET` only produces and doesn't consume. Our example outputs only JSON, but you can support multiple formats, like `application/xml`
- [25-32]\: Defining parameters
    - See the [Swagger parameter object] docs
    - Again, the `x-ms-*` attributes are for Azure integrations
- [35-36]\: Providing friendly text around a successful response (200)
    - Here is where you could provide helpful error messages for other error codes (404, etc.) if you're getting serious
- [39-56]\: Defining the properties returned in a 200 response
    - My objects return Date/Title/Description/Link properties, so I briefly acknowledge each
    - Note that `Date`'s type is different. Type/Format options are here:
        - [Swagger data types]
- [68-99]\: Repeat 16-58 for other functions in your function app

## Take a test drive

This step is concisely described in the official doc's [Test the OpenAPI definition] section. The brief, no screenshot version:

- Copy your function's default host key, for authorization
- In the "preview" pane of the API definition screen:
    - Security > apikeyQuery
    - Change Authentication
    - Paste the key in and Authenticate
- Scroll down to one of your GET functions and "Try this operation"

Hopefully you see something like this!

[![greatsuccess](/images/SwaggerFunctions3.png)](/images/SwaggerFunctions3.png)

## To be continued

That's it! Your Azure Functions App now has two GET functions, committed via a GitHub repository, defined and documented in a Swagger API spec, and presented to other Azure services.

On the next episode of "Why Did I Agree to This? Someone Please Send Help", we'll tie in to [Azure API Management], and learn how it complements our serverless API mission.

---

|[Build a Serverless REST API in Azure]|
|---|
| Part 2: [PowerShell in Azure Functions] |
| Part 3: [GitHub Integration with Azure Functions] |
| Part 4: (you are here) |
| Part 5: [Azure Functions and Azure API Management] |
| Part 6: [Serverless API Series Conclusion] |



[Microsoft Flow]:       https://flow.microsoft.com/en-us/
[Azure Logic Apps]:     https://azure.microsoft.com/en-us/services/logic-apps/
[Azure API Management]: https://azure.microsoft.com/en-us/services/api-management/
[Swagger]:              https://swagger.io/
[Postman]:              https://www.getpostman.com/

[the walkthrough]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-openapi-definition#generate-the-openapi-definition

[Swagger v2.0 spec]: https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md
[OpenAPI v3.0]: https://github.com/OAI/OpenAPI-Specification

[Link to full diff]: https://github.com/brianbunke/PowerShellAPI/commit/4664ea648c681c8f7084e5dcde7658414a4e0c92
[3-9]: https://github.com/brianbunke/PowerShellAPI/commit/4664ea648c681c8f7084e5dcde7658414a4e0c92#diff-ded16f88e1db6c3579828bb6d46945b6R3
[18-23]: https://github.com/brianbunke/PowerShellAPI/commit/4664ea648c681c8f7084e5dcde7658414a4e0c92#diff-ded16f88e1db6c3579828bb6d46945b6R18
[Extend an OpenAPI definition for a custom connector]: https://docs.microsoft.com/en-us/connectors/custom-connectors/openapi-extensions
[25-32]: https://github.com/brianbunke/PowerShellAPI/commit/4664ea648c681c8f7084e5dcde7658414a4e0c92#diff-ded16f88e1db6c3579828bb6d46945b6R25
[Swagger parameter object]: https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#parameter-object
[35-36]: https://github.com/brianbunke/PowerShellAPI/commit/4664ea648c681c8f7084e5dcde7658414a4e0c92#diff-ded16f88e1db6c3579828bb6d46945b6R35
[39-56]: https://github.com/brianbunke/PowerShellAPI/commit/4664ea648c681c8f7084e5dcde7658414a4e0c92#diff-ded16f88e1db6c3579828bb6d46945b6R39
[Swagger data types]: https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#data-types
[68-99]: https://github.com/brianbunke/PowerShellAPI/commit/4664ea648c681c8f7084e5dcde7658414a4e0c92#diff-ded16f88e1db6c3579828bb6d46945b6R68

[Test the OpenAPI definition]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-openapi-definition#test-the-openapi-definition

[Serverless REST API series]:               /blog/2018/02/26/serverless-api-in-azure/
[Build a Serverless REST API in Azure]:     /blog/2018/02/26/serverless-api-in-azure/
[PowerShell in Azure Functions]:            /blog/2018/02/27/powershell-in-azure-functions/
[GitHub integration]:                       /blog/2018/02/28/github-integration-with-azure-functions/
[GitHub Integration with Azure Functions]:  /blog/2018/02/28/github-integration-with-azure-functions/
[API Management]:                           /blog/2018/03/02/azure-functions-api-management/
[Azure Functions and Azure API Management]: /blog/2018/03/02/azure-functions-api-management/
[Serverless API Series Conclusion]:         /blog/2018/03/03/serverless-api-conclusion/
