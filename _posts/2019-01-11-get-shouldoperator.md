---
layout:     post
title:      Get-ShouldOperator
date:       2019-01-11 00:00:00 -0800
summary:    Easy discovery of Pester assertions
categories: blog
thumbnail:  pester
author:     brianbunke
image:      /images/getshouldoperator2.png
tags:
 - powershell
 - pester
 - hacktoberfest
---

> [How do I discover Should operators?]

If you've ever tried to learn [Pester], PowerShell's foremost code testing framework, you probably recognize this question.

Hopefully, you found the answers you needed on the [Pester wiki], but that's not an ideal solution. The wiki is not updated with the same frequency as the project, and we want quick answers via our PowerShell console, not on the website.

Enter [Hacktoberfest]. _(And associated nice swag.)_

[![hacktoberfest2018](/images/hacktoberfest2018.jpg)](/images/hacktoberfest2018.jpg)

After submitting some small PRs on other projects, I went hunting for a "[good first issue]" on the Pester repository, and decided I was most interested in solving this problem.

Soon, a [pull request] was born. After some discussion and tweaks, and subsequent testing and acceptance by _(super deserving Microsoft MVP)_ [Jakub], `Get-ShouldOperator` makes its debut in Pester's brand new [4.5.0] release.

The use case is simple: `Get-ShouldOperator` returns every assertion operator available to Pester. By default, the displayed operators ship with Pester, but you would also see any custom registrations you may have added with `Add-AssertionOperator`.

[![getshouldoperator1](/images/getshouldoperator1.png)](/images/getshouldoperator1.png)

Let's say "Contain" sounds like what you're looking for. You can quickly get a synopsis of what it does, and view the example(s) pulled from the Pester wiki:

[![getshouldoperator2](/images/getshouldoperator2.png)](/images/getshouldoperator2.png)

4.5.0 is available on the [PowerShell Gallery]. Run your `Install-Module Pester`/`Update-Module Pester` commands today, and start using `Get-ShouldOperator` to help you get quicker answers!



[How do I discover Should operators?]: https://github.com/pester/Pester/issues/878
[Pester]: https://github.com/pester/Pester
[Pester wiki]: https://github.com/pester/Pester/wiki

[Hacktoberfest]: https://hacktoberfest.digitalocean.com
[good first issue]: https://github.com/pester/Pester/labels/good%20first%20issue

[pull request]: https://github.com/pester/Pester/pull/1116
[Jakub]: https://twitter.com/nohwnd
[4.5.0]: https://github.com/pester/Pester/releases/tag/4.5.0

[PowerShell Gallery]: https://www.powershellgallery.com/packages/Pester
