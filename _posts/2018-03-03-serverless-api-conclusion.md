---
layout:     post
title:      Serverless API Series - Conclusion
date:       2018-02-17 06:00:00 -0800
summary:    Buy all the futures in PowerShell-aaS
categories: blog
thumbnail:  book
author:     brianbunke
tags:
 - powershell
 - azure
 - serverless
 - api
 - functions
---

Prev: [API Management] \| **[Serverless REST API series]** \| Next: ?

It's been a long week of API goodness spamming your Twitter feed. Concluding the series at the end of the APIM post yesterday didn't feel right, so I pulled a few sections over here to help put a bow on everything.

---

## Table of Contents
{: .no_toc}

- ToC
{:toc}

---

## Lessons learned / things to consider

To start with, some small additional things you should know that felt like distractions when I put them in-line with the walkthroughs.

1. Microsoft has published [API design guidance] best practices, which is a good read
    - "HATEOAS" was new to me
2. APIM default products "Unlimited" and "Starter"
    - `Unlimited` is good for personal testing
    - `Starter` imposes rate-limits on each key, which makes it ideal if other people will actually sign up for your service
3. "PowerShell Serverless API API"
    - Just taking the opportunity to make fun of myself again. Know that APIM will append the "API" suffix when presenting
4. Naming my Azure Functions
    - Just use `PowerShellTeam`, not `PowerShellTeamGet`. More on that:

I initially wanted to use the `FunctionNameGet`/`FunctionNamePost` naming scheme, in order to not over-complicate function code. The idea was that APIM could still wrap those two separate functions into one endpoint with GET and POST methods.

In practice, I couldn't get the Swagger spec inside the Azure Functions App to accept me changing the names (maybe a remap is easy and I just don't know how yet?). This meant that Swagger exposed the ugly names, and APIM imported the ugly names.

In limited testing, it _does_ seem like you can manually override that ugliness in APIM, but it would have to be a methodical function-by-function overwrite, potentially breaking on each update, and it broke my existing "Named Values" anyway. To just ship this series and stop thinking about it, I'll have to accept that you all can make fun of me for `/PowerShellTeamGet -Method Get`. :D

## Conclusion? Reinforced conviction

This idea has been kind of an obsession for me over the last month, and it's nice to finally get it out there.

PowerShell-as-a-Service took a silly amount of time to progress from idea to reality. Despite a lot of time spent exploring paths both right and wrong, and staring down the general concept of _"burnout is caused by a lack of progress,"_ the end of this series brings two main encouragements:

- It is now reality
- While struggling with the technical implementation details, at no point did I waver on this idea as the direction to steer toward in the future

Present a single REST API endpoint. Consume it with scripts, web forms, web dashboards, chatops, simple phone apps, etc., and tell other teams to consume it however they like. You now present the same information, regardless of others' preferred tooling, and you can track each disparate source with a different key in case problems arise.

> If you only listen to me once, ever, READ THAT PARAGRAPH ^ once more

I know I'm just scratching the surface here, as it's still pretty new to me, but I'm a true believer. Treat it as a random guy on the street telling you to spend all your money on this technical stock. :D

[![tradingplaces](/images/APIConclusionSplash.jpg)](/images/APIConclusionSplash.jpg)

For now, though, I'm setting this project aside while I prep for the [2018 PowerShell Summit]. ([I'm speaking!])

## Potential future expansions?

Because I still cast loving glances at this concept, I may have the time and motivation to expand on this series at a later date.

In the event I get to circle back down the line, below are some topics I'm interested in. If you can help me out with any of these (or something else I should have considered), hit me up, please! _(...later, haha)_

#### 1) Use Azure API Management to present a custom domain

I'm interested in presenting an API in an ongoing fashion, so I'd love to tie-in an outside subdomain (`api.contoso.com`). I know this is _possible_, but no idea how quick of an implementation it may or may not be.

#### 2) Document a professional way to handle serverless `POST` in Azure-land

In the early stages of creating this series, I had rudimentary working demos of a PowerShell Azure Function integrating with both [Azure Table Storage] and [Azure Cosmos DB]. I decided to ditch them from the series because figuring them out took a lot of time, explaining them was distracting from the overall goal of just shipping a serverless API, and they're not free.

Posting some basic guidelines for endpoints that store data in Azure would be nice, though.

["To ship is to choose."] :)

#### 3) Point a barebones React Native mobile app at the published API

A REST API is a phenomenal source for a simple front-end mobile app to consume. I don't have any experience with [React Native] yet, so this would be a longer-term project, but I think building a basic PoC app with two buttons to interact with API endpoints would be really fun.

## They say I should end with a call to action...

Here are three things you could do that would help make writing this series worth it:

1. Tell me this idea is lame, so I save a lot of future time
2. Now that I've ironed out ~~all~~ ~~most~~ some of the speedbumps, create your own API to play around with! You can tangent from this walkthrough toward your own interests/needs and then share what you did, improving our community knowledge
3. Let me know how to improve upon these steps
    - P.S. - Azure team members should read this as "I am _super thirsty_ to talk more about this with you"

If the idea of PowerShell-as-a-Service has piqued your interest, I'm both apologetic for being contagious, and happy that I'm not isolated-hermit crazy.

We made it, whew. Thanks for sharing interest in this idea...it took me a long time to bring it to fruition. I hope it helps speed up your journey, and if you followed along the whole way, I'd love to hear from you!

Happy creating!

---

|[Build a Serverless REST API in Azure]|
|---|
| Part 2: [PowerShell in Azure Functions] |
| Part 3: [GitHub Integration with Azure Functions] |
| Part 4: [Add an API spec in Azure Functions] |
| Part 5: [Azure Functions and Azure API Management] |
| Part 6: (you are here) |



[API design guidance]: https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design

[2018 PowerShell Summit]: https://powershelldevopsglobalsummit2018.sched.com/
[I'm speaking!]: https://powershelldevopsglobalsummit2018.sched.com/event/CrVR/how-to-love-unit-testing
[Azure Table Storage]: https://azure.microsoft.com/en-us/services/storage/tables/
[Azure Cosmos DB]: https://docs.microsoft.com/en-us/azure/cosmos-db/introduction
["To ship is to choose."]: http://www.jsnover.com/blog/2011/12/18/iranian-drone-hack-and-technical-debt/
[React Native]: https://facebook.github.io/react-native/

[Serverless REST API series]:               /blog/2018/02/26/serverless-api-in-azure/
[Build a Serverless REST API in Azure]:     /blog/2018/02/26/serverless-api-in-azure/
[PowerShell in Azure Functions]:            /blog/2018/02/27/powershell-in-azure-functions/
[GitHub Integration with Azure Functions]:  /blog/2018/02/28/github-integration-with-azure-functions/
[Add an API spec in Azure Functions]:       /blog/2018/03/01/azure-functions-swagger-spec/
[API Management]:                           /blog/2018/03/02/azure-functions-api-management/
[Azure Functions and Azure API Management]: /blog/2018/03/02/azure-functions-api-management/
