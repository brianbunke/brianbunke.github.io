---
layout:     post
title:      GitHub Integration with Azure Functions
date:       2018-02-28 06:00:00 -0800
summary:    Sync your commits to Azure, automagically
categories: blog
series:     azureapi
thumbnail:  code-branch
author:     brianbunke
image:      /images/GitHubFunctionSplash.png
tags:
 - powershell
 - azure
 - serverless
 - api
 - functions
 - code
 - github
---

Yesterday, we created an Azure Functions App with a "ScriptingGuysGet" function inside. It's pretty rad...but we had to manually paste that code into a text box in Azure.

If you're iterating on your code in source control, at some point you're going to wish those commits would automatically update your Azure Function.

Wish no more, my friend.

[![splash](/images/GitHubFunctionSplash.png)](/images/GitHubFunctionSplash.png)

---

## Table of Contents
{: .no_toc}

- ToC
{:toc}

---

## No experience with GitHub / source control?

This walkthrough assumes you know the basics of creating a repository on GitHub and committing files to it.

If that's new to you, that's ok, we've all gotta learn sometime! I previously wrote a [GitHub 101 series], because I'd like more of my IT Ops friends to get familiar with that workflow. IMHO: All that is cool in technology these days relies on source control as a foundation, so I hope you consider it.

## Downloading your function app

Since we have a function app with one function already in place, the blueprint of what Azure Functions expects is already there. We need to "Download app content," so we can get it into a new GitHub repo.

[![download](/images/GitHubFunction0.png)](/images/GitHubFunction0.png)

This comes down as a .zip file to your browser's download location.

If you "Include app settings in the download," you just get an extra `local.settings.json` file. Not needed in this walkthrough, but nice if you get serious and want to define everything from source control.

Here's the entire contents of the .zip file you download:

- `host.json`
- ScriptingGuysGet
    - `function.json`
    - `run.ps1`

So dump those into your new GitHub repo!

## Connect your function app to GitHub

Once again, the [official doc] was pretty easy to follow. Here are some screenshots of my process, if you'd like to make sure you're following along:

bbpowershellapi Function App > Platform features > Deployment options > Setup

Allow Azure to access your GitHub account:

[![authorize1](/images/GitHubFunction1.png)](/images/GitHubFunction1.png)

[![authorize2](/images/GitHubFunction2.png)](/images/GitHubFunction2.png)

(Note that private repos are included as well)

Select the `organization + repository + branch` to sync with:

[![options](/images/GitHubFunction3.png)](/images/GitHubFunction3.png)

Once it's created, it'll sync pretty quickly. I was impatient the first time, and kicked it off manually. Either way.

[![sync](/images/GitHubFunction4.png)](/images/GitHubFunction4.png)

It found my commit, and shows details of the sync process:

[![details](/images/GitHubFunction5.png)](/images/GitHubFunction5.png)

Now you can browse back to your Azure Function, and something's different:

[![readonly](/images/GitHubFunction6.png)](/images/GitHubFunction6.png)

I was really excited to see "Your app is currently in read only mode." Of course, that's because I thought everything had worked perfectly...

...but "read-only mode" happens whether your repo is laid out properly or not. Mine wasn't at first, and I had to flail for a while to learn why further commits weren't changing anything.

Turns out, I didn't know about the "Downloading your function app" step above, and Azure Functions was too polite to point out that my zipper was down.

But you don't have that problem! Let's keep rolling.

## Commit a new function to your repo

With the sync working, I'd like to add another function to check the [PowerShell Team blog].

Your Azure Functions App and your GitHub repo map 1:1. There can be many functions inside a functions app, so there can be many functions inside a repo as well.

> Remember: Azure Functions expects each function to have its own folder, containing `function.json` and `run.ps1`.

I have copied `ScriptingGuysGet` into new function `PowerShellTeamGet`. I just needed to change the hard-coded BaseURI, because both blogs helpfully run on the same backend system. This is great because it's super low effort, and less great because I'm grossly violating [DRY].

Who cares, it's an example. ;)

[![newcommit](/images/GitHubFunction7.png)](/images/GitHubFunction7.png)

Without needing to manually sync, the new commit's details are already available. We see that it is now active, and has marked the previous commit as inactive. (The ScriptingGuysGet function is unaffected, since it already existed in our repo and we didn't change that file/folder.)

[![twofunctions](/images/GitHubFunction8.png)](/images/GitHubFunction8.png)

All's well that ends well.

> For reference, my [PowerShellAPI repo] is available on GitHub.

## Onward and upward

We have an Azure Functions App, two functions inside it, and the ability to automatically update contents with a simple GitHub commit. That's preeetty cool.

But what good is a cool function app if you can't connect it to the rest of the Azure ecosystem? Well, allow me to definitively answer that eternal question once and for all: _Kind of good._

To level up, next time we'll generate a Swagger v2.0 API specification file from our current state, and peek inside the doorways it opens for us.



[GitHub 101 series]: /blog/2017/05/08/github-101/

[official doc]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-continuous-deployment

[PowerShell Team blog]: https://blogs.msdn.microsoft.com/powershell/
[DRY]:                  https://en.wikipedia.org/wiki/Don%27t_repeat_yourself
[PowerShellAPI repo]:   https://github.com/brianbunke/PowerShellAPI/
