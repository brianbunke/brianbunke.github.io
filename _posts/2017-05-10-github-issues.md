---
layout:     post
title:      GitHub Issues
date:       2017-05-10 10:00:00 -0700
summary:    Your First GitHub Contribution
categories: blog
thumbnail:  question-circle
author:     brianbunke
tags:
 - gitizen
 - github
 - intro
---

_This blog series started as a lightning talk at the [2017 PowerShell/DevOps Summit] titled "Being an Upstanding Gitizen."_

1. _[GitHub 101]_
2. _[GitHub Lingo]_
3. _(you are here)_
4. _[GitHub Pull Requests]_

---

I've talked about why you should start caring about [GitHub], and what all that silly terminology means. Now, let's get to the fun part: How to make your first contribution!

All you have to do is open a helpdesk ticket.

![Shia](/images/justdoit.jpg)

## User Tier Progression
{: .no_toc}

When a project is born, it has no audience. Slowly, more and more people learn of its existence, starting in pool 1. Today, I'll be your tour guide as you start to get involved.

1. ToC
{:toc}

_"Fun" note for project maintainers here: Fewer and fewer people make it through each level. You can help by removing natural progression barriers, which I'll write about soon™._

---

## Aware

You know the project exists. You checked out the readme to see what it's all about. The readme _should_ have quickly answered these questions for you (among others):

- Why does this project exist?
- Does this project solve a problem I have?
- Where is the documentation?
- How do I download it and get started?

You won't have time to test drive most projects you've briefly heard of, and that's fine. We all wish we had more time. But it would be pretty useless if I stopped here, wouldn't it?

## Test Drive

Whether it's the same day or months later, you've decided to try the project yourself.

Hopefully, in PowerShell land, the project is packaged as a module, and a current version is available on the PowerShell Gallery. A quick `Save-Module` or `Install-Module` is far and away the path of least resistance. If that's not available, maybe you download the repo packaged as a zip, which you'll need to extract to the appropriate folder level. ![Clone or download](/images/github-dl.png)

~~Or maybe the author suggests an `iex` command against a `bit.ly` link to download the~~ GAH I HATE THAT, STOP IT

Anyway. For brevity's sake, I'm assuming you have enough PowerShell knowledge to get the project in question to a spot where you can comfortably `Import-Module asdf`, or whatever else you want to do to actually test the project out.

And you know what? This project is actually perfect, and it just freed up 30 minutes of your work day, forever! For free! Isn't open source amazing? Put your feet up on the desk, take a sip of `$Beverage`, and bask in the warm embrace of our technological utopia.

## Workaround

HAHAHA yeah right. You hit a bug, or you thought of a new feature request, or the examples are subpar. For the purposes of this walkthrough, let's say you ran into an error, and you have no idea why.

You need a workaround. You Google it, but it's still a smaller project, and nobody's written about your specific error. If only there was a place intended to talk about that project...yeah. You're headed to the Issues tab of the repo. From the official [PowerShell Core] repository:
![Issues](/images/github-issues-1.png)

- When you click in to the Issues tab, you only see _open_ issues
  - See both the filter/search box "is:open" and the "741 Open" below it
- Author/Labels/Projects/etc. are additional options for you to filter on
- Issue labels (red/purple/black/etc.) are used for quick categorization
- On the top issue, see that "6.0.0-beta"? That's a milestone
  - It means that the project maintainers won't release version 6.0.0-beta until all issues in the milestone are resolved
- Issues can also be assigned to a user
  - I cropped it out, but you would see a user avatar off to the right, next to the comment count

You'll probably have to search for your error. I do this by removing the "is:open" before typing by error message, because it could exist in an open or closed issue.

![Issue query](/images/github-issues-2.png)

Say you found a matching issue. Gather context for yourself by reviewing the comments, and determining how long ago was it opened and last updated. You can do a couple of things, based on whether it's currently open or closed:

- Open
  - Give a thumbs up
    - To prevent comments like "me too", just giving a thumbs up is encouraged
    - But note that you have to comment if you want to push a notification to participants
  - Is it assigned to someone?
    - Can you tell if it will be fixed soon?
  - Add a comment if something hasn't been covered
   - For example, if you had a potential solution that hadn't been discussed yet
    - Or maybe if you're wondering how high of a priority it is
- Closed
  - Was your bug resolved, or just closed with no action taken?
  - If resolved, is it in a version that has already been released?
    - If you can't tell, feel free to ask this question as a new comment

If your exact problem has been fixed, maybe your resolution is as simple as `Update-Module asdf`. If the issue is still open, maybe you can ignore your problem while you wait for it to be fixed, or maybe you stop working with the module until the update shows up.

Here's what you need to take away from this section:

> Even a thumbs up or a comment on an existing issue is helpful.

As a maintainer trying to triage issues, knowing how many people are encountering a bug is helpful. Input on potential solutions, and reactions to potential solutions, can be helpful. And on newer repos, it can even serve as a small indicator that someone is actually consuming the project and cared enough about it to cruise the Issues page.

Well I guess we're done, right? Run along now :)

## Reporter

But wait--what if you searched for an issue and didn't find any matches?

### Issues as helpdesk tickets

Yesterday, I said that [issues are like helpdesk tickets]. If you have an IT Ops background, you know that a ticketing system is mandatory, and vague tickets waste time and effort.

We also expect users to submit tickets instead of wasting time on workarounds. Because a lot of the time, you don't know about the problem if it isn't reported, right?

A logical conclusion of this analogy, then, is:

> Project maintainers want you to open new issues.

Issues are an easy entry point for you to give back to the project. You can log a quick bug without having the time or knowledge to dive into the code base and fix it yourself.

Don't think of it like you're creating more work. This is the first step in a stronger project. If you have this problem, others do, too!

### Reasons to create

- Find something wrong? Issues can be bug trackers
- Need something more? Issues can be feature requests
- Got stuck? Issues can request help docs to cover your use case
  - You don't need to log an issue for quick documentation fixes, like typos
- Have a question? Issues can be discussions, or your question could easily morph to bug/feature request/closed
  - Some projects also have chat channels like Slack/Gitter for quick, informal questions
  - If so, it should be listed somewhere on the project's `readme`

### What to include

Here's an old issue I filed that I purposely went overboard on for my own amusement: [PSJira #49]. Maintainers hope for issues this detailed, but that much effort certainly isn't expected. However, I do like to start a new issue with something like this:

> First, thanks so much for this project!

Issues, like helpdesk tickets, are usually negative (something is wrong!). Why not start with some gratitude and positivity?

It's certainly recommended for projects to use issue templates to pre-populate the text field. But if you're staring at an empty box, there are some basics to avoid "My email doesn't work..."

Here are some example issue templates from PowerShell projects I've seen recently. [[PS Core], [PoshBot]] You'll notice some common threads from these and other good templates:

- Current behavior
  - If a bug, steps to reproduce
- Expected behavior
- Possible solution
- Other context
- Your environment
  - OS, PowerShell version, module versions, etc.

Welcome to QA, haha.

## Key Takeaways

If you followed along with a project of your choice, congratulations! You've just made your first contribution to the world of open source software.

Just like a smoothly operating IT department, "ticket in, ticket out" is the sign of a healthy open source project.

Creating issues is an important step in the process, and requires no prior knowledge with the code. If you've used the project and found anything that's not perfect, you have the expertise to contribute!

## Up Next

If you also want to fix your problem, great! We'll cover pull requests (PRs) [next time].

---

#### Acknowledgments
{: .no_toc}

- [@BrandonPadgett] & [@devblackops] for making issues an issue ([1], [2])

|Being an Upstanding Gitizen|
|---------------------------|
| Part 1: [GitHub 101] |
| Part 2: [GitHub Lingo] |
| Part 3: (you are here) |
| Part 4: [GitHub Pull Requests] |



[2017 PowerShell/DevOps Summit]: https://powershell.org/summit/
[GitHub 101]: http://www.brianbunke.com/blog/2017/05/08/github-101/
[GitHub Lingo]: http://www.brianbunke.com/blog/2017/05/09/github-lingo/
[GitHub Pull Requests]: http://www.brianbunke.com/blog/2017/05/12/github-pr/

[GitHub]: https://github.com
[PowerShell Core]: https://github.com/PowerShell/PowerShell

[issues are like helpdesk tickets]: http://www.brianbunke.com/blog/2017/05/09/github-lingo/#issues

[PS Core]: https://github.com/PowerShell/PowerShell/blob/master/.github/ISSUE_TEMPLATE.md
[PoshBot]: https://github.com/poshbotio/PoshBot/blob/master/.github/ISSUE_TEMPLATE.md

[next time]: http://www.brianbunke.com/blog/2017/05/12/github-pr/

[@BrandonPadgett]: https://twitter.com/BrandonPadgett
[@devblackops]: https://twitter.com/devblackops
[1]: https://twitter.com/BrandonPadgett/status/859613452477898754
[2]: https://twitter.com/devblackops/status/859657767690264576
