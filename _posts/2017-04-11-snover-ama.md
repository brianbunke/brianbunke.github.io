---
layout:     post
title:      Jeffrey Snover AMA at PS/DevOps Summit 2017
date:       2017-04-11 08:00:00 -0800
summary:    The Man, The Myth, The Legend
categories: blog
thumbnail:  microphone
author:     brianbunke
tags:
 - powershell
 - summit
 - snover
---

> "Linux becomes more like Windows every year."

---

The [2017 PowerShell/DevOps Summit] is in full swing. Aside from the incredible concentrated knowledge that I am honestly humbled to be around, one of the real highlights so far was the AMA (ask me anything) session with Microsoft Fellow and Esteemed PowerShell Godfather, [Jeffrey Snover].

Because the session wasn't recorded, I took some notes, and made my best effort to transcribe* the highlights here.

\* - I use "transcribe" in the loosest possible sense of the word. I didn't record, and notetaking is not my forte. Please don't take anything here as a word-for-word quote. (Really. Even the "quotes.")

---

> Would you change anything from the [Monad Manifesto] if you rewrote it today?

Honestly, not really. The focus is on automation, and that remains relevant.

> Seeing the way PowerShell has evolved, what is your biggest regret?

Not much from an end-user perspective. The formatting/outputting subsystem is abstracted, complex code that has proven to be difficult to maintain.

The `return` keyword was a misstep. Because any command output prior to `return` is also included, it doesn't behave intuitively.

The [adaptive type system] is an under-utilized feature that I wish more people knew about. And [transactions] have not been embraced by the community at large.

> (I don't remember the question here, but he ended up talking about trying not to support workflows from 6.0 forward)

There is an [open RFC (#23)] on the topic of PowerShell-native parallel operations. Because PowerShell is a scripting language, general programming language concepts like multithreading were not a top priority. However, it is currently a consideration, and the PowerShell Team is actively soliciting feedback!

**"In general, taking everything forward is an error."** In the context of future workflow support, making everything backward compatible, forever, can put an incredible burden on product teams. (I'm injecting my own paraphrasing here:) Sometimes, you have to cut ties with the worst 10%, apologize, and promise that the next thing will be even better.

Also of note: If you, the reader, are freaking out about a potential future with no workflows, reach out to the PowerShell team (there's even a [monthly community call]!) and explain your use case.

> How can I get started with contributing to the [PowerShell open source projects]?

The code base is massive, and it will take people a lot of time to get up to speed with it. In the meantime, if you'd like to help, here are some easy wins where we always need more eyes:

- File issues. New bug reports are valuable, whether you have the means to fix them yourself or not.
- **Error messages.** Everyone complains about generic errors. There is always more work to be done writing error messages, and increasing their specificity.
- **Documentation.** Help files could always be better, and most of them could use more examples. ([PowerShell-Docs GitHub repo])

> What are some of your favorite community projects?

Visual Studio Code and Pester.

He doesn't believe [VS Code] is a copout answer. While it's maintained by Microsoft, it is fully open source with significant community contributions, and Microsoft doesn't make any money from it. "If every project was going as well as VS Code, I'd be a very happy man."

[Pester] is a community project that the team thought was so important, they fought to bring it (an open source project) into the Windows 10 build.

> What's the next big thing?

Well, the big, big thing was getting PowerShell open sourced and onto GitHub. The majority of PowerShell 6.0 work is on the PowerShell Core side. (The [6.0 roadmap] is publicly available.)

The team is working with groups that have written their PowerShell modules in C# to port their code to [.NET Standard 2.0], which can produce .dll files that work against both the full .NET framework and .NET Core. 

> What's new with Nano?

Two major pieces of feedback from Nano Server:

- It's too small.
- It's too big.

He's not directly overseeing the Nano Server team right now, but has suggested that the team focus on analyzing & reducing boot times, allowing for more ephemeral VM operations (quicker spin-up/tear-down).

> What is a practical application of PowerShell classes (introduced in v5)?

Right now, they serve two primary purposes: Enable DSC, and give developers more recognizable syntax. He mentioned that he had a very valuable conversation with an internal team that refused to use PowerShell back in the day, and the lack of developer-familiar semantics as a hindrance to that team's adoption. Classes, and the effort to provide a more enforcable contract, were partly borne from that feedback. (I'm drastically simplifying here, sorry.)

The team's work on improving and extending classes continues.

> Can the PowerShell Gallery start segregating modules? (e.g. Linux-compatible vs. Windows-only)

Tags will be the primary way to solve that problem at this point. There are discussions around other potential solutions (like harvesting the `#requires` lines from scripts), but nothing concrete.

> What do you see as a primary use case for PowerShell on Linux?

He prefaced this--with a laugh--as being controversial, but he also believes it:

**"Linux becomes more like Windows every year."**

To explain: Linux is an entire file system based around ASCII text docs. Microsoft tested out that approach in Windows back in the day...and abandoned it pretty quickly (try to `grep` the registry sometime). So Windows is married to structured data objects and APIs.

More and more, bits and pieces of the Linux ecosystem are migrating to the REST API + JSON document model. In this scenario, PowerShell's object-oriented approach becomes more valuable.

> What's next for [JEA]?

"My experience with security technologies: Everyone loves them, but nobody actually deploys them." While he said that jokingly, they don't have great metrics around adoption, so they're not sure how much the community has embraced it. Meanwhile, he currently oversees the Cloud Automation area, and isn't directly involved with the day-to-day JEA operations and roadmap. Those with questions should seek out the relevant product managers.

> Bad guys use PowerShell. Should I turn off PowerShell?

"Bad guys use PowerShell for the same reason you do: Because it's _freaking awesome_." PowerShell itself isn't the exploit; that exploit exists in whatever technology PS is interacting with, and would be available with or without PowerShell. Additionally, v5 brought many security-related improvements (like enhanced auditing), and the good guys are leveraging PowerShell for some impressive forensics work as well.

> What languages most influenced the design of PowerShell?

Certainly Bash, but also [VMS DCL]. Terrible language to learn, but incredibly powerful to those familiar with it.

PowerShell was always intended to bridge the gap between administrators and developers. The Verb-Noun format, readable parameters, and `-WhatIf`/`-Confirm` were early must-haves. Jim Truher and Bruce Payette played a large part in getting PowerShell off the ground and solving early design challenges.

> Will we see other teams open sourcing their product's modules? (e.g. Active Directory)

That's always a case-by-case basis, and a singular decision won't be dictated to all teams. OSS is just another business model; Linus needs to make money, too. Determining what should remain proprietary to Microsoft is a neverending conversation.

> And, finally, a few quotes that I really enjoyed, but have no idea what the questions were that spawned them:

- Positive feedback is great, and motivating, _but negative feedback is actionable._
  - "PowerShell sucks!" Great! I'd love to know why, maybe I can fix it!
    - "When I do _x_, clearly _y_ should be an option, and it should output _z."_ You know what, you're right! I'm writing this down!
    - "I don't like using dollar signs to declare variables." Well, ok. You can't please all the people all the time, and language aesthetics especially invoke strong opinions that aren't always solvable problems.

"The goal of an open beta isn't to find and fix bugs. The goal is to find them...and know about them. Well, wait...what?" Bug fixes during a beta typically result in introducing new bugs, due to feeling a little more rushed during that period. If you approach an open beta with the goal of only finding bugs, you can continue to scope & prioritize them appropriately.

---

I really enjoyed the insights, and hope I didn't lose too much in translation. I'm appreciative that he took the time, and am looking forward to the rest of the Summit!



[2017 PowerShell/DevOps Summit]: https://powershell.org/summit/
[Jeffrey Snover]: https://twitter.com/jsnover
[Monad Manifesto]: http://www.jsnover.com/blog/2011/10/01/monad-manifesto/
[adaptive type system]: https://blogs.msdn.microsoft.com/powershell/2008/09/06/hate-add-member-powershells-adaptive-type-system-to-the-rescue/
[transactions]: https://msdn.microsoft.com/en-us/powershell/reference/5.1/microsoft.powershell.core/about/about_transactions
[open RFC (#23)]: https://github.com/PowerShell/PowerShell-RFC/issues/85
[monthly community call]: https://blogs.msdn.microsoft.com/powershell/2017/03/30/regular-cadence-for-powershell-core-community-call/
[PowerShell open source projects]: https://github.com/PowerShell
[PowerShell-Docs GitHub repo]: https://github.com/PowerShell/PowerShell-Docs
[VS Code]: https://code.visualstudio.com/
[Pester]: https://github.com/pester/Pester
[6.0 roadmap]: https://github.com/PowerShell/PowerShell/issues/3046
[.NET Standard 2.0]: https://blogs.msdn.microsoft.com/dotnet/2016/09/26/introducing-net-standard/
[JEA]: https://msdn.microsoft.com/en-us/powershell/jea/overview
[VMS DCL]: https://en.wikipedia.org/wiki/DIGITAL_Command_Language
