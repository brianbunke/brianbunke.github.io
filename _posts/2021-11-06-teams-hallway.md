---
layout:     post
title:      My Hallway is Red During Teams Calls
date:       2021-11-06 00:00:00 -0800
summary:    Teams status controlling Kasa bulb and Slack
categories: blog
thumbnail:  lightbulb
author:     brianbunke
image:      /images/teams-seinfeld.jpg
tags:
 - powershell
 - api
 - teams
 - smarthome
---

Not blogging for over a year has really changed my outlook on titling blog posts.

## tl;dr

**[https://github.com/brianbunke/TeamsStatus](https://github.com/brianbunke/TeamsStatus)**

![teams-hallway](/images/teams-hallway.jpg)
![teams-seinfeld](/images/teams-seinfeld.jpg)

> Why?

So my family knows the answer before they walk down the hall and open the door.

Also because my wife linked [lights with an infrared remote] as a suggestion, and that was the final push I needed to prioritize solving this automagically.

---

## Table of Contents
{: .no_toc}

- ToC
{:toc}

---

## Prerequisites

- PowerShell and Python
  - Walkthrough assumes Windows
- Microsoft Teams is where you take most/all your video calls
- [Kasa KL125] smart bulb on firmware version 1.2
  - I was told 2.0 breaks the local API. (Which has always been undocumented, but previously reverse engineered by some very fine souls.)
- Slack (optional, but I solved for it before I had a bulb)

## Detect Teams Status

### Locally? Why?

- Because, quick and dirty as it is, it works
- Because it doesn't ask you to hold admin rights or learn a new cloud thing
- Because I couldn't figure out querying Teams presence in Microsoft Graph in an unattended fashion
- Because my Teams resides in Government Cloud, which is constant "Am I doing this wrong, or is this just silently absent in GCC High?"

### Teams Code

I spent two minutes trying to rediscover the Github link where I learned about the local log parsing, but failed. Honestly sorry I can't link and give you credit! :(

```powershell
$Time = Get-Date

# Local Teams log FTW
$StatusLog = Get-Content -Path "$env:APPDATA\Microsoft\Teams\logs.txt" -Tail 1000 |
    Select-String -Pattern 'StatusIndicatorStateService: Added' |
    Select-Object -Last 1

# RegEx all the things
[void]($StatusLog -match '(.*?)\sGMT.*StatusIndicatorStateService:\sAdded\s(.*)\s\(.*:\s(.*)\s-\>\s(.*)\)')
$LogTime = Get-Date $Matches[1]
$TeamsStatus = $Matches[2]
$StatusOld = $Matches[3]
$StatusNew = $Matches[4]

# Quit if there is no new log entry
# "NewActivity" overwrites OnThePhone/Presenting, but could happen in or out of a call
  # Ignore NewActivity and continue using the prior status instead
If ($Time.AddMinutes(-1) -gt $LogTime -or $TeamsStatus -eq 'NewActivity') {
    exit
}
```

Yep, [log parsing with RegEx]! By remaining on this site you consent to the use of RegEx and agree that [regex101.com] owns your binding arbitration or you know whatever

### NewActivity

"NewActivity" is an unread chat message or a missed call...or an unread +1 on your message...or you were invited to a group call with a unique combination of people and it spawned a new empty group in chat...

It's trash. It overwrites the "OnThePhone"/"Presenting" statuses (statusii? eh, eh?) we actually care about. I've also had calls where I read the message, but the log kept me on NewActivity until the call ended. For lulz.

It's trash; ignore it.

## Red the Light

### Backstory / Why Kasa

Why the TP-Link [Kasa KL125]? A coworker with a #VerySmartHome had an extra to lend me for testing.

This is my first smart bulb. For those like me that had never investigated, you can drop a smart bulb into a normal ceiling dome and connect to wifi. (You'll have tougher luck if the light fixture is on a dimmer switch.)

My requirements are RGB (color), wifi (not Bluetooth), and local API. It meets my needs* for $15.

\* - _v1.2 disclaimer:_ Kasa reluctantly meets your local API needs, and will not once they start shipping bulbs on v2.0 firmware.

Once Kasa is very broken, the next places I would suggest looking are:

* [LIFX]. I spent all the time doing this 18 months into COVID-19 WFH. It's finally time to defend dropping $35 on a lightbulb one time.
* A [Philips Hue Bridge] to interact with once you reach enough bulbs to justify the cost. It aggregates your bulbs and provides a supported local API.

### Kasa Code

This assumes you have installed Python (I'm on 3.10) and added it to your PATH during installation. You then need to `pip install python-kasa` to use the [python-kasa repo].

I tried solving this in PowerShell to avoid the dependency and failed. Kasa's API is very undocumented, the [python-kasa repo] refers to the work done in Wireshark to figure all this out, and it's not worth diving into deeper if Kasa hopes to break it again anyway.

```powershell
$KasaBulbIP = '192.168.12.34'

If ($TeamsStatus -match 'OnThePhone|Presenting') {
    # call python externally to use python-kasa to change the light
    kasa --bulb --host $KasaBulbIP hsv 0 70 100
} Else {
    # set bulb back to normal
    kasa --bulb --host $KasaBulbIP hsv 0 0 100
}
```

HSV -- While you might equate smart home devices with the Herpes simplex virus, Hue/Saturation/Value is another way of notating the color you prefer. [https://colorpicker.me](https://colorpicker.me)

### Kasa App

The repo allows you to avoid the mboile app, but I used it to set up the light anyway. If you have a v1.2 bulb, skip the prompt to update upon connecting to it.

It's nice for the kids to play with changing colors, and I can still manually set red if I remember after hopping into Zoom instead of Teams.

## Red the Slack

![teams-slack](/images/teams-slack.png)

Before I had a bulb to try, I tried toggling status in my family Slack for my wife. This worked pretty well, it's just not as effective as the bulb. Definitely keeping the work here in case it helps you.

### New Slack app and key

[https://api.slack.com/authentication/basics](https://api.slack.com/authentication/basics)

Go to [https://api.slack.com/apps](https://api.slack.com/apps), create a new app, then go to "OAuth & Permissions" and add `users.profile:write` to "User Token Scopes". Then copy the User OAuth Token for use by your script.

(You may also need `users.read` once to [grab the UserID] you are modifying. That's how I did it, anyway.)

### Slack Code

```powershell
$SlackKey = 'xoxb-Numbers-Numbers-MoreNumbers-LongHexadecimal'
$SlackUserID = 'U03QXXXXX'

If ($TeamsStatus -match 'OnThePhone|Presenting') {
    $SlackStatus = @{
        # slack status and message to set
        status_text  = 'Teams call'
        status_emoji = ':no_entry:'
    }
} Else {
    $SlackStatus = @{
        # slack status will be cleared
        status_text  = ''
        status_emoji = ''
    }
}

# Build Slack API call details
$Splat = @{
    Headers = @{
        Authorization  = "Bearer $SlackKey"
        'Content-type' = 'application/json'
        charset        = 'utf-8'
    }
    Method  = 'Post'
    Uri     = 'https://slack.com/api/users.profile.set'
    Body    = @{
        user    = $SlackUserID
        profile = $SlackStatus
    } | ConvertTo-Json
    ErrorAction = 'Stop'
}

$response = Invoke-RestMethod @Splat
```

## Task Scheduler

100% of Old Windows Dads agree: Just set it as a dumb scheduled task and forget about it. If you don't know how to run a PowerShell script in Task Scheduler, just Google this one.

It should have a recurring trigger every minute.

Most importantly, it should "Run whether user is logged on or not", or you will see a very annoying window pop every minute. They are considering solving this in newer versions of PowerShell 7; see [issue #3028].

This requires that you enter your password upon editing the task, and that your Windows user is a member of the "Log on as Batch Job" group. `lusrmgr.msc`

## In Conclusion

**[https://github.com/brianbunke/TeamsStatus](https://github.com/brianbunke/TeamsStatus)**

Like you care about more words here. I hope it helps!



[lights with an infrared remote]: https://www.amazon.com/HONWELL-Control-Operated-Wireless-Colors-Dimmable/dp/B07B2YLY6D/

[Kasa KL125]: https://www.amazon.com/alexa-smart-light-bulb-wifi/dp/B08TB8QBB1

[log parsing with RegEx]: https://twitter.com/brianbunke/status/1247369403840073728
[regex101.com]: https://regex101.com

[LIFX]: https://www.lifx.com/collections/lightbulbs/products/lifx-color
[Philips Hue Bridge]: https://www.philips-hue.com/en-us/p/hue-bridge/046677458478

[python-kasa repo]: https://github.com/python-kasa/python-kasa

[grab the UserID]: https://api.slack.com/methods/users.list

[issue #3028]: https://github.com/PowerShell/PowerShell/issues/3028
