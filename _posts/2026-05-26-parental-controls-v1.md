---
layout:     post
title:      Parental Controls v1 in 2026
date:       2026-05-26 00:00:00 -0800
summary:    A First Swing
categories: blog
thumbnail:  ban
author:     brianbunke
tags:
 - family
 - parenting
---

## Context

It's 2026 and this article currently applies to 10- and 8-year-old boys.

My kids have a [2015 Intel NUC] in their room. Years ago we got tired of them asking to borrow our phones to play music while cleaning up. I liked the idea of giving them music-on-demand and learning to operate a mouse & keyboard setup. They have a Spotify account on our family plan with explicit content disabled. (Spotify parent rant at end of article.)

The NUC had [Ubuntu Desktop] installed, but the hard drive died this year, and I switched to [Linux Mint] on the NVMe replacement. It has Spotify and Firefox installed with the home page set to their school's [computer science] page. Link included for resource ideas; the current fun is code.org's [Dance Party] using Scratch-style code blocks.

We have left it with no parental controls and passive monitoring for years -- the deviant behavior has just been searching for fart songs.

My kids know I can use my phone to look up answers to their questions, and recently my oldest has asked to use my computer to search for answers while I've been in the room. He just turned 10 and almost immediately discovered he can do the same on his computer, so I'm refreshing on digital parenting in 2026.

## Intent

If you've read [The Anxious Generation], this will be a familiar exercise. Basically, how can we impart freedom and digital literacy while minimizing exposure to addicting screen time and inappropriate online content?

For my part, I prioritize self-sufficiency over most other competing interests. (This tends to carry over as a work ideology as well.) Our job as parents is to teach kids how to learn and thrive in this world without us.

As the book harps on, an unsupervised child online is far more likely to end worse vs. that same child roaming the neighborhood unsupervised, so we need to draw and monitor some boundaries.

There is a lot of worry these days that exposing how you parent incurs silent judgment from other parents. I'm (over-)confident enough with our balance to feel comfortable sharing our current philosophy and strategy. Hopefully it also spurs conversations that help me learn from others.


## New Actions

Here are my first steps. As usual with blogging, I hope this helps you, but I'm also writing this as memory for future me.

1. Mint OS user account is standard. Guest sessions are disabled. Admin account is parents-only.
    * Annoying for applying regular updates, but a necessary evil for now.
1. Firefox settings. No real parental controls here, but we'll do our best.
    1. Disable all suggestions and recommendations.
    2. Disable all AI.
    3. Default to DuckDuckGo for searches.
2. [Cloudflare Families] -- block malware and adult content at DNS.
    * They provide [malware.testcategory.com] and [nudity.testcategory.com] to help verify implementation.
3. [Ubiquiti UDM Pro]
    1. Create Kids network with static DNS servers `1.1.1.3` and `1.0.0.3`.
    2. Create Kids wifi tied to that network. ([Private Pre-Shared Keys] makes this very clean.)
    3. Create Kids content filter and apply it to the Kids network.
        * You can ad block and force a couple safe search settings here.
        * The Enhanced filter redundantly leverages Cloudflare, so I left it on Basic.
        * Explicit allow/block lists; prefer to solve here vs. a browser extension.
            * youtube.com -- YouTube has been a strict "parental supervision only" zone for 10 years, and I'm not ready to change yet.

## Next Steps

My explicit block list is woefully insufficient/naive/etc. This whack-a-mole is a game I will lose and would rather not play. When I'm ready to unblock YouTube and whatever else has joined it, we'll have more conversations with the kids to convey our intents, goals, and boundaries.

I will probably set up a daily/weekly digest of browser history to my email for passive review. This should be the most fun technical problem to solve. If that happens, I'll want to disable it again when they're older to give some privacy back.

I haven't added screen-time limits. There are various ways to solve this, but it's not yet necessary as we handle this in-person.

There are obvious workarounds for everything here; part of learning computers as a teenager is being motivated to clear various hurdles. But again, they're 10 and 8, not doing this yet, and we're physically close enough to monitor the situation.

## Spotify Rant

Spotify added videos and visuals and did not immediately provide controls for blocking them on accounts. There are finally controls now under "Videos and Canvas" to disable this.

One checkbox for "explicit content" was a pathetic lack of control for a long time. Spotify recognized this and added [Managed Accounts]...but you can only auth into a managed account on a mobile app. They do not currently provide a way for managed accounts to log into their desktop applications. Of course, you wouldn't admit to any known limitations in the article -- parents should create the account and then spend 20 minutes wondering how something so easy is very much not.

But for all the complaining I'll happily give you about Spotify...it ain't YouTube.



[2015 Intel NUC]: https://www.intel.com/content/www/us/en/products/sku/83255/intel-nuc-kit-nuc5i5ryh/specifications.html

[Ubuntu Desktop]: https://ubuntu.com/download/desktop
[Linux Mint]: https://www.linuxmint.com
[computer science]: https://sunnyland.bellinghamschools.org/academics/computer-science/
[Dance Party]: https://code.org/en-US/hour-of-code/dance

[The Anxious Generation]: https://www.anxiousgeneration.com/book

[Cloudflare Families]: https://blog.cloudflare.com/introducing-1-1-1-1-for-families/
[Ubiquiti UDM Pro]: https://store.ui.com/us/en/products/udm-pro
[Private Pre-Shared Keys]: https://help.ui.com/hc/en-us/articles/29887064407319-Using-PPSK-RADIUS-for-Multiple-VLANs-On-an-SSID-in-UniFi-Network?utm_source=chatgpt.com

[malware.testcategory.com]: https://malware.testcategory.com
[nudity.testcategory.com]: https://nudity.testcategory.com

[Managed Accounts]: https://support.spotify.com/us/article/managed-accounts/
