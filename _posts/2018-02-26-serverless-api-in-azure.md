---
layout:     post
title:      Build a Serverless API in Azure
date:       2018-02-26 06:00:00 -0800
summary:    ...because it's pretty awesome
categories: blog
thumbnail:  lightbulb
author:     brianbunke
image:      /images/ServerlessSplash.png
tags:
 - powershell
 - azure
 - serverless
 - api
---

[![icons](/images/ServerlessIcons.png)](/images/ServerlessIcons.png)

In the beginning, there was an idea.

I've interacted with a lot of REST APIs in the past few years. Wonky implementations aside, I really appreciate that method of providing a service overall.

However, the services I manage don't follow the same model, exposing API endpoints for others to consume. From an IT Operations standpoint, my customers can query Active Directory...and most other things require a request for direct access to the product our team uses.

Indirectly, it begged the question: **_How do I build a REST API?_**

This remained as an idle question in the back of my head for the last year or so. At the end of 2017, `$Job` onboarded to O365 and Azure. That made me think a little harder about the possibility, but I hadn't yet made it a priority.

### The real catalyst

Last month, I read a rant that lodged itself in my brain and _will. not. leave._

[Stevey's Google Platforms Rant]

Steve Yegge worked for Amazon for ~6.5 years, and then Google for another ~6.5 when he wrote this in October 2011. (Yes, I'm apparently ~6.5 years behind the curve. I learned of this old rant because he just left Google, and [wrote about why].)

The big takeaway is that in ~2002, Jeff Bezos sent a memo to Amazon employees introducing "service-oriented architecture" (SOA). Summarizing:

1. All teams must expose data and functionality through service interfaces
2. Teams must communicate with each other through these interfaces
3. No other interprocess communication (backdoors) allowed
4. Use whatever back-end technology your team prefers to accomplish this
5. All service interfaces must be designed from the ground up as _"externalizable."_ Expect to expose your interface to the public sometime in the future
6. Anyone who doesn't do this will be fired.

Implementing SOA, team-by-team across the company, led to a bunch of small discoveries about what not to do. Those small discoveries became collective organizational knowledge, and _yada yada yada,_ you can introduce AWS to the world.

### Embarking on a journey

Afterwards, I couldn't stop thinking about applying the premise of SOA to my own situation...through the prism of my original question.

**_How, exactly, do I design, build, publish, and maintain a REST API, so others can pull our team's data when and how they see fit?_**

Can I do it "serverless," so there's no OS for me to maintain, and I only pay for runtime? I'm most comfortable in PowerShell, and most info my team would provide is processed via PowerShell; is there a way to use it as my language? How can I provide a user-friendly, Swagger-type portal to present my service contract?

I was facing some stark realities:

- I don't yet know how to do this
- This may be easy in other languages, but using PowerShell for this is not something I can easily Google
- I don't personally know anyone who can help teach this to me
- Azure's documentation is pretty good when following a happy path, but filling in the blanks takes a lot of blind trial and error (at least for me)
- Maintaining a production API isn't easy, but it seems worth learning by doing
- I'm not yet able to erase ideas from my brain

So I did what I had to do: I began investigating how in the cloudy, cloudy world I can make this happen.

It took about a month, but I made it, and I'm documenting the journey in case anyone else finds this idea half as cool as I do.

### On tinkering and cloud platforms

A quick aside before we get started. I know that cost prohibits some people from wanting to explore The Cloud™.

There isn't a free tier for every single service, and this two-steps-forward, one-step-back learning process cost me some money. (About $50 USD, I think?)

There are two options for you:

- Ask your employer to cover/supplement the cost. This helps your learning stay current, and provides more familiarity when discussing projects to move the company forward
- Presonally adopt the [CapEx to OpEx] budgetary shift that most companies are making in technology, and accept that you'll spend a little money each month instead of periodic larger purchases to refresh/supplement your home lab

Hopefully I'm stating the obvious, but check the pricing of each service for free tier options, and remember to delete what you no longer need.

### Why Azure?

While this idea of a serverless REST API is language-agnostic, my example functions (and potential applications at `$Job`) are written in PowerShell.

As far as serverless platforms go, PowerShell is not yet available in AWS Lambda or Google Cloud Functions. It currently exists as an unsupported, experimental language in Azure Functions. In other words: good enough for some blogging. ;)

In addition, my company is on O365 and Azure. If you're here for the PowerShell flavoring especially, there's a pretty good chance you learned PowerShell because your company runs a lot of Microsoft, and Azure will the lowest common denominator in that scenario.

### Next steps

In its current state, this is a six(!)-part series that will roll out this week. Part six will cover my current lessons learned, and some potential (longer-term) areas for expansion.

Part two, writing your first Azure Function in PowerShell--and highlighting the quirks of coding in a language with "experimental" support--drops tomorrow. Hopefully I've piqued your interest with what's possible, and I'll see you on the other side!

---

|Create a Serverless REST API in Azure|
|---|
| Part 2: [PowerShell in Azure Functions] |
| Part 3: [GitHub Integration with Azure Functions] |
| Part 4: [Add an API spec in Azure Functions] |
| Part 5: [Azure Functions and Azure API Management] |
| Part 6: [Serverless API Series Conclusion] |



[Stevey's Google Platforms Rant]: https://plus.google.com/+RipRowan/posts/eVeouesvaVX
[wrote about why]: https://medium.com/@steve.yegge/why-i-left-google-to-join-grab-86dfffc0be84

[CapEx to OpEx]: http://www.bmc.com/blogs/capex-vs-opex/

[PowerShell in Azure Functions]:            /blog/2018/02/27/powershell-in-azure-functions/
[GitHub Integration with Azure Functions]:  /blog/2018/02/28/github-integration-with-azure-functions/
[Add an API spec in Azure Functions]:       /blog/2018/03/01/azure-functions-swagger-spec/
[Azure Functions and Azure API Management]: /blog/2018/03/02/azure-functions-api-management/
[Serverless API Series Conclusion]:         /blog/2018/03/03/serverless-api-conclusion/
