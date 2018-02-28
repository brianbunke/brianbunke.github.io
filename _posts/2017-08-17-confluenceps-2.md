---
layout:     post
title:      ConfluencePS v2
date:       2017-08-17 06:00:00 -0700
summary:    Bigger, Badder, Meaner
categories: blog
thumbnail:  file-alt
author:     brianbunke
tags:
 - powershell
 - confluenceps
 - confluence
 - atlassian
 - documentation
---

Today, [ConfluencePS] version 2.0.0 was published on the [PowerShell Gallery]!

ConfluencePS is a PowerShell module that interacts with Atlassian's wiki product, [Confluence]. It helps automate bulk operations and repetitive tasks from the command line. For example:

- Create new pages for each row in a CSV
- Move all pages below "Parent Page A" to "Parent Page B" instead
- Delete all pages labeled `deleteme`
- When your environment changes, update the relevant wiki page(s)

There will be a future blog post with a walkthrough of ConfluencePS 2.0, featuring use cases and code examples. But that'll have to wait until sometime after [VMworld 2017] :)

If you are a current user of v1.0, please review the [changelog] for hopefully all of the details anyone could want.

### History and Perspective

In November 2015, I started writing ConfluencePS. I had three main motivations:

1. Automating documentation was a huge pain point I was feeling at work
2. I hadn't written/maintained a big-boy PowerShell module yet, and wanted to learn the ins and outs
3. I don't think I'd interacted with any REST API endpoints yet

Let me tell you, writing a module that hits a REST API is a pretty good way to learn how to write modules and how to interact with REST APIs.

In November 2016, ConfluencePS 1.0 finally appeared on the PowerShell Gallery. I held out because I was being a stubborn purist, and wanted to create an automated deployment procedure to publish when the repo was updated. (SPOILER: It worked, but it wasn't worth the long wait. You should manually publish your repos until that process becomes painful enough to be automated.)

In April 2017, [@lipkau] found the module. He was already familiar with [JiraPS], and started overhauling the ConfluencePS code base (after receiving my enthusiastic blessing). This removed repeat code, made what was left more testable, and aligned it more closely with current-day best practices for API module design. v2.0 is here because of his efforts, which are greatly appreciated.

I was in charge of reviewing the changes and updating the documentation...and here we are in the middle of August, haha.

### AtlassianPS

This summer, [@lipkau] also led the charge to form the [AtlassianPS] organization, in order to streamline future open-source development efforts of all Atlassian-based PowerShell modules.

(Like the open source modules, the org is not affiliated with Atlassian in any way.)

This has been excellent. Friends of the blog know that I love diversity in open source projects and sustainability built into teams, and this change has definitely helped both of those areas.

I'm excited to already see the potential of the AtlassianPS modules coming under a shared stewardship, and am looking forward to what's next with 2.0 finally out in the world!

### Test Drive 2.0

```posh
# PowerShell version 5 provides cmdlets to interact with the Gallery:
Install-Module ConfluencePS
# or Update-Module ConfluencePS

Get-Command -Module ConfluencePS
Get-Help about_ConfluencePS
Get-Help Set-ConfluenceInfo -Full

Set-ConfluenceInfo -BaseUri 'https://wiki.example.com' -PromptCredentials
Get-ConfluenceSpace
```

For bug reports or feature requests, feel free to [file an issue on GitHub], or join the [AtlassianPS Slack team].

Hope you enjoy, and we'd love to hear from you if you end up trying it out!



[ConfluencePS]: https://github.com/AtlassianPS/ConfluencePS
[PowerShell Gallery]: https://www.powershellgallery.com/packages/ConfluencePS
[Confluence]: https://www.atlassian.com/software/confluence

[VMworld 2017]: http://www.brianbunke.com/blog/2017/07/18/vmworld-2017/
[changelog]: https://github.com/AtlassianPS/ConfluencePS/blob/master/CHANGELOG.md

[@lipkau]: https://github.com/lipkau
[JiraPS]: https://github.com/AtlassianPS/JiraPS
[AtlassianPS]: https://atlassianps.org

[file an issue on GitHub]: https://github.com/AtlassianPS/ConfluencePS/issues
[AtlassianPS Slack team]: https://atlassianps.org/slack/
