---
layout:     post
title:      A VMworld 2017 Retrospective
date:       2017-09-05 06:00:00 -0700
summary:    People, Pictures, and Code
categories: blog
thumbnail:  vmware
author:     brianbunke
image:      /images/vmworld2017speaking.jpg
tags:
 - vmware
 - vmworld
 - vmwarecode
 - hackathon
 - conference
---

As I write this, we VMworld US attendees are thankful that Labor Day provides a long holiday weekend to recover from the hectic festivities. There is a lot going on, and my desk job is not offering much preparation for 25k steps daily.

I previously shared my [VMworld 2017 Tips], and I wanted to encapsulate how things actually went as well. In case you haven't read that post, I went in to VMworld focusing on connecting with the community, primarily around PowerCLI and hackathon activities.

## VMworld Keynotes

Getting this out of the way: I'm not going to write anything about news or announcements. Many many people cover these, and can provide much better context to the headlines.

In case you don't know, Scott Lowe always does great work liveblogging the keynotes ([Mon]/[Tue]). If you're out of the loop, I encourage you to Google around to catch up.

## The vBrownBag/VMTN presentations

The vBrownBag booth was great. Having VMTN partnership, which allowed for the sessions to appear in the Schedule Builder for the first time, really helped raise awareness and drive attendees.

The vBrownBag team did an amazing job manning it, because that was an unrelenting amount of content that was both livestreamed and almost immediately published to YouTube.

Speaking of YouTube: vBrownBag's [2017 TechTalks at VMworld] playlist.

## Becoming a VMworld speaker

[![speaking](/images/vmworld2017speaking.jpg)](/images/vmworld2017speaking.jpg)

I had a blast giving my first (ten-minute vBrownBag) presentation at VMworld!

I've updated prior post [Video Vester] with a new link to the presentation. It's also hiding within the 72-video playlist link above.

## The VMware {code} hackathon

[VMware Code] tried something new by lining up four pre-hackathon training sessions, meant to get attendees a little more familiar with outlying VMware technologies.

The VMware Clarity session was really interesting. I hadn't used Clarity yet, and I have no immediate use cases...but it's a really appealing project, and I've definitely filed it away for future consideration when UI needs arise.

Clarity links: [repo] / [seed] / [video] / [slide deck]

---

As far as the actual 8pm-midnight hackathon:

[Steve Trefethen] did me an unintentional solid by grabbing a panorama of the entire hackathon room while I was presenting our team's results. Really couldn't have asked for a better picture!

[![hackathon](/images/vmworld2017hackathon.jpg)](/images/vmworld2017hackathon.jpg)

_(Team leaders had 90 seconds to present. Something something world's fastest panorama)_

I'm happy to report that we achieved our three primary goals:

1. Add 6.5 Security Config Guide tests
2. Add vSAN tests, a previously unexplored area
3. Each team member creates (and commits to GitHub!) at least one test

Alas, the judges did not reward us with a victory, but we did get a spontaneous round of applause (mid-presentation) for every member making a commit to the public GitHub repo! Some members created their first Git commit and/or first GitHub pull request, which is absolutely my favorite tidbit to report.

The [VesterHackathon2017] repository on GitHub serves as a snapshot of our evening; there's more info in the readme if you're interested.

Thanks _so much_ to Kanji, Becky, Matt, Robin, Aaron, Tony, and Mark. I honestly couldn't have asked for a better, friendlier, or more cooperative team, and I'm proud of what we accomplished!

_See also: [Justin Sider] and [Nick Korte] have already written about their experiences, as of Labor Day._

## PowerCLI sessions

[There were a lot]. Personally, I attended three standard breakout sessions:

"Power Hour: 10th Birthday" and "PowerCLI 201" were both voted top daily sessions, and [the recordings] are available on the VMworld website.

The Power Hour was a fun refresher on how far PowerCLI has come (and some accumulated stats) over the last ten years. On the back half, Alan and Luc talked REST APIs, and then in the spirit of reminiscing, teaching old scripts new tricks (like recursion).

PowerCLI 201 was predominantly code examples, which should be available on GitHub after VMworld Barcelona.

Unfortunately, "PowerCLI What's New" was not one of the top sessions, and a recording isn't publicly available. However, [Derek Seaman] was benevolent enough to liveblog the session's contents. Highlighting:

- If you haven't heard, [PowerCLI 6.5.1 Tips]
- Focusing on shorter release cycles
- Jake recited an incomplete list of awesome things:
  - _Note that VMware cannot confirm if/when any of these might be worked on and/or delivered. Standard disclaimer applies_
  - Parameter value tab completion, with a shoutout to [Matt Boren]
    - e.g. `Get-VM ASuperLongVMName` _<-- That tab completes, based on your inventory_
  - More REST API efforts
  - Providing DSC resources (config mgmt tool agnostic)
  - An Onyx refresh
  - PowerCLI compatibility with PowerShell's 6.0 release
    - Mac & Linux users: This is your bullet
- And Alan highlighted PowerCLI community projects:
  - [OpBot]

And there's a new PowerCLI issues page to log bugs and feature requests! Check it out now, create an account, and vote on the items that affect you!

> **[http://vmwa.re/powercli](http://vmwa.re/powercli)**

## Looking back

A couple small items best symbolize my conference:

[![objects](/images/vmworld2017objects.jpg)](/images/vmworld2017objects.jpg)

Much love to [Jake Robinson] for 3D printing the PowerCLI ~~poker chips~~ coins for PowerCLI speakers on his own time.

Of course, VMworld isn't about the swag, but the people. Being more involved with the online community over the past 12 months definitely afforded me many more opportunities to connect with new friends. I'm grateful for being able to meet so many of you, and am sorry there wasn't more time in the day. We should absolutely meet again (or for the first time!) in 2018. Until then, I'm leaving Vegas motivated to continue helping and expanding the community.

One more thing: I highly recommend hanging around Thursday night, grabbing a couple friends, and finding a nice restaurant.

[![yellowtail](/images/vmworld2017yellowtail.jpg)](/images/vmworld2017yellowtail.jpg)



[VMworld 2017 Tips]: /blog/2017/07/18/vmworld-2017-tips/

[Mon]: https://blog.scottlowe.org/2017/08/28/vmworld-2017-day-1-keynote/
[Tue]: https://blog.scottlowe.org/2017/08/29/vmworld-2017-day-2-keynote/

[VMware Code]: https://code.vmware.com
[repo]: https://github.com/vmware/clarity
[seed]: https://github.com/vmware/clarity-seed
[video]: https://youtu.be/8J4b_GqHN90
[slide deck]: http://bit.ly/vmworld-clarity

[Steve Trefethen]: https://twitter.com/stevetrefethen/status/904078820663148544
[VesterHackathon2017]: https://github.com/brianbunke/VesterHackathon2017
[Justin Sider]: https://invoke-automation.blog/2017/08/29/what-horse-did-you-ride-in-on-vmwarecode-hackathon/
[Nick Korte]: https://community.spiceworks.com/topic/2039632-vmworld-notebook-an-inside-look-at-the-vmworld-hackathon

[2017 TechTalks at VMworld]: https://www.youtube.com/watch?v=R05nnH7wcwo&list=PL2rC-8e38bUVLHcoiomo8Yt0R-aZ5s62h
[Video Vester]: /blog/2017/03/20/video-vester/

[There were a lot]: https://blogs.vmware.com/PowerCLI/2017/08/powercli_vmworld_us_2017.html
[the recordings]: https://www.vmworld.com/en/us/video/top-sessions.html
[Derek Seaman]: https://www.derekseaman.com/2017/08/vmworld-powercli-whats-new.html
[PowerCLI 6.5.1 Tips]: /blog/2017/06/02/powercli-651/
[Matt Boren]: https://twitter.com/mtboren
[OpBot]: http://try.opvizor.com/opbot/

[Jake Robinson]: https://twitter.com/jakerobinson
