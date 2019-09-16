---
layout:     post
title:      REST Client in VS Code
date:       2019-09-16 00:00:00 -0800
summary:    A lightweight solution for quick API calls
categories: blog
thumbnail:  cogs
author:     brianbunke
image:      /images/restclient1.png
tags:
 - vscode
 - api
---

Last year I [blogged] how to install [Fiddler] and view raw HTTP request/response data when interacting with a remote REST API.

Enter [REST Client], a Visual Studio Code extension that solves this use case (and many more) in a much lighter fashion. After trying it out, I thought it was useful enough to deserve a quick write-up.

Start to finish in four quick steps:

1. [VS Code] > Extensions pane > Install [REST Client]
2. Open a new file tab and set the language (`Ctrl + K, M`) to HTTP
3. `POST https://jsonplaceholder.typicode.com/posts`
  - [API reference] for the test site
4. A "Send Request" link appears directly above that line; click it and the response tab appears to the right

Here's a screenshot demonstrating how to send a body with your POST request:

[![restclient](/images/restclient1.png)](/images/restclient1.png)

Really, the hardest part is digging through the massive readme to figure out how to do what you want amid all the other crazy features. There's support for GraphQL, various authentication methods (Azure AD?!), proxies, cookies...it's an impressive list. Go check it out!

---

_Credit to [@tiberriver256] for sharing._



[blogged]: /blog/2018/05/02/proxy-to-fiddler
[Fiddler]: https://www.telerik.com/fiddler
[REST Client]: https://marketplace.visualstudio.com/items?itemName=humao.rest-client
[VS Code]: https://code.visualstudio.com/
[API reference]: https://jsonplaceholder.typicode.com

[@tiberriver256]: https://twitter.com/tiberriver256/status/1172113350047797249
