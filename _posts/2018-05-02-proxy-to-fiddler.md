---
layout:     post
title:      A Fiddler Proxying Intro
date:       2018-05-02 06:00:00 -0800
summary:    Inspect web requests outside of PowerShell
categories: blog
thumbnail:  cogs
author:     brianbunke
image:      /images/fiddler2.png
tags:
 - powershell
 - fiddler
 - api
---

A friend asks:

> I'm utilizing `Invoke-WebRequest` to post some data to a form. Works great and life is good...but now I have a company writing an application to do the same thing.
> 
> How do I see what my POST looks like outside of PowerShell?

APIs are great because you can bring your own solution. Sometimes, as we're learning a new API, it'd be nice to debug it outside of the console. Or, sometimes, you just need to pass on the request info in a language-agnostic format to another party.

We'll just need two things here:

1. Telerik's [Fiddler] to act as a web proxy
2. An easy public API to post to
  - [http://jsonplaceholder.typicode.com] supports all verbs via HTTP or HTTPS

Some quick notes before we start:

- If you're trying to debug HTTPS traffic, there's an explicit permission in Fiddler's settings you'll need to grant.
- Fiddler will modify your computer's proxy settings. You should know those settings in case you need to revert anything.
  - In Win10: Windows key > type "proxy" > Configure proxy server > LAN settings

Download, install, and run [Fiddler]. Then, via PowerShell:

```posh
$Body = @{abc = 'xyz'; foo = 'bar'}
$URI  = 'http://jsonplaceholder.typicode.com/posts'
# Proxy to Fiddler, default port 8888
Invoke-WebRequest -Method Post -Body $Body -Uri $URI -Proxy 'http://127.0.0.1:8888'
```

Now, open your Fiddler window and highlight your not-grayed-out request.

[![fiddler1](/images/fiddler1.png)](/images/fiddler1.png)

Fiddler captures both your _request_ (top pane) and the _response_ (bottom pane).

For example, here's what my raw request looks like:

[![fiddler2](/images/fiddler2.png)](/images/fiddler2.png)

That's the info that could be passed on to the application team/company.

And the responses tabs detail the headers, the raw response, or just the returned content body in multiple formats (like WebView or JSON).

If you ever need to gain insight into API calls outside of PowerShell, I suggest keeping Fiddler in mind!

---

In search of quicker blog posts (read: not blown out into an insufferable six-part series), I realized I should ~~farm my colleagues for imaginary karma~~ occasionally repurpose day-to-day Q&A into blog format.

Have a question you'd like me to tackle? Get in touch below!



[Fiddler]: https://www.telerik.com/fiddler
[http://jsonplaceholder.typicode.com]: http://jsonplaceholder.typicode.com
