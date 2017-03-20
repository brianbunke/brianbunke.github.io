---
layout:     post
title:      Introducing Vester
date:       2017-03-07 07:30:00 -0800
summary:    VMware Configuration Management with PowerCLI & Pester
categories: blog
thumbnail:  vmware
author:     brianbunke
tags:
 - vester
 - powershell
 - powercli
 - vmware
 - code
 - psgallery
 - github
---

> Part 2 of this series, [Vester 2: Vest Harder], covers more advanced usage of the module.
>
> Part 3, [Write Your Own Vester Test], covers the individual *.Vester.ps1 test structure, and walks through an example of how to create your own tests.
>
> [Video Vester] is a recorded online presentation walking through this blog series.

This post is all about managing your VMware environment with Vester ([PS Gallery]/[GitHub]). Vester uses PowerCLI and Pester to provide a lightweight configuration management solution for VMware administrators.

Let's start off with a quiz:

- Are you a VMware administrator?
- Can you answer these questions, with certainty, without looking?
    1. Do all of your hosts have the same NTP settings?
    2. How aggressive is your cluster's DRS migration policy?
    3. Would any of your VMs fail a vMotion because an ISO file is currently mounted?
    4. What's the oldest VM snapshot you have hanging around?
- Wouldn't you like to know?

I gravitated to Vester because I am a VMware administrator, and these were very real-world problems I wanted to eliminate. If that also resonates with you, great! Let's dive in.

> Chris Wahl, creator of the project and fellow handsome redhead, also just wrote about the [history and current state of Vester]. You should check that out, too, because I didn't take any screenshots. ;)
>
> You'll find me using "we" throughout this write-up. Vester has a bunch of pretty cool [contributors]. You should stalk them all, and relentlessly compliment their intelligence and taste.

---

### So...what the heck are these projects?

[PowerCLI] is the set of modules VMware provides to interact with vSphere products via PowerShell. Have you ever had to do any manual change to a large set of VMs, maybe like checking the "Check and upgrade VMware Tools before each power on" box? PowerShell and PowerCLI allow you to scale that to 100/1000/10,000 VMs very easily.

[Pester] is a PowerShell module that has two common uses:

1. Testing your PowerShell code to ensure it does what you think
2. Validating that your infrastructure is configured as you think it should be

For the purposes of Vester, we're using Pester in scenario #2 to test our VMware environment. If you're not a developer, it's ok to not know exactly what I mean by #1. If you want me to talk for an hour about using Pester to test your code _(said nobody ever)_, I recommend buying me a coffee sometime.

### Ok, but what about DSC?

I like to acknowledge [DSC] up front, because if you know what it is, it's basically the elephant in the room in this discussion.

If you want something running constantly, to ensure that your vSphere environment never changes from your desired configuration, that's what DSC was made for. It should absolutely be your first choice. [Luc Dekens] ([vSphereDSC]) and [Brandon Olin] ([POSHOrigin_vSphere]) both started VMware-flavored DSC projects, both are pretty cool guys, and you should totally check in with both about their current status and potentially helping out!

Vester, at its core, is doing a whole bunch of `Get` and `Set` operations. (We refer to them as `$Actual` and `$Fix`, but anyway.) So why is this using Pester instead of DSC?

Well, here are some reasons you might choose Vester over a DSC solution:

- You don't know and/or are afraid of DSC
- You can't (or don't want to) set up a push or pull server
- You are a contractor doing an audit of a customer's environment
- You're looking for a very quick way to create your own custom tests

So, that's the DSC caveat. I've done my best to talk you out of caring about this. (I write code because I wouldn't be able to sell a wad of cash.) Still with me? Excellent. Let's do the thing.

### Getting started in five minutes

Vester is available on the [PS Gallery]. This walkthrough assumes:

- You're on Windows
  - I have no idea right now what does and does not work cross-platform, sorry
- You have access to the [PowerShell Gallery]
  - via PS v5, or you've manually installed PowerShellGet on v3/v4
- For the **first run only**, you're running PowerShell as admin
  - Default PSGet install directory is `C:\Program Files\WindowsPowerShell\Modules\`
  - `Install-Module` and `New-VesterConfig` will need access to write there
- You have installed [PowerCLI] 6.5

I'm sure all of those scenarios have exceptions, but a guy's gotta provide simple instructions somehow, right?

```posh
# Your vCenter server
$vCenter = 'your.vcenter.com'

Install-Module Vester
Import-Module Vester
# Module requirements "Pester" & "VMware.VimAutomation.Core" automatically load into the session

# Do you care about Distributed Switches?
# PowerCLI doesn't do implicit module loading yet, so manually import any other needed modules
Import-Module VMware.VimAutomation.Vds

Connect-VIServer $vCenter

# Help is available:
Get-Help about_Vester
Get-Help New-VesterConfig
Get-Help Invoke-Vester
```

Excellent. Now you're ready to generate a config file by gathering the current values of our environment. The idea here is that you specify one host/VM/etc. to provide the base values to get started, so that you can get started as quickly as possible.

Vester uses a `Config.json` file to store all desired settings for one vCenter. By default, this is stored in the `\Config\` directory within the module folder. (If you have multiple vCenters you'd like to test, you'll need to have one `Config.json` for each.)

```posh
New-VesterConfig
# Upon completion, Config.json now exists in the module's \Config folder
```

You can open the `Config.json` file and manually edit any settings you prefer. You may not care at this point, which is fine. :) If you always want a test to be skipped, you can change the value to `null`, or just delete that line completely.

Now, with the fresh config, Vester knows what you _want_, based on the one host/VM/etc. you chose. Will the rest of your inventory conform? Let's run some tests and find out!

_(NOTE: You're still not changing anything. This step will just take each `*.Vester.ps1` test, pull its value from each applicable object, compare those results to your desired value stored in the config, and report back.)_

```posh
# By default, run all included *.Vester.ps1 tests using the newly created config
Invoke-Vester
```

That's it! How'd you do?

### Investigating results

Ok, I lied. A couple screenshots. This is from a section of the Host tests I just ran.

![Host Tests](/images/vesterhosttests.png)

1. `Syslog-Server.Vester.ps1` didn't actually execute its tests. This is due to my config file having a `null` value
  - You could confirm that the skip is intentional by running `Invoke-Vester -Verbose`
2. `TPS-ForceSalting.Vester.ps1` passed all tests
3. `VIB-AcceptanceLevel.Vester.ps1` passes on host `esxi01`, but fails 02 & 03

Logically, the next question is: What is VIB-AcceptanceLevel? Well, let's go take a quick look at the description in the test file itself. It's a Host test, because the test is prefaced with "Describing Host Configuration." So within your Vester module folder, it's here:

`\Tests\Host\VIB-AcceptanceLevel.Vester.ps1`

![Vester Test](/images/vestertest.png)

I'm going to break down the `.Vester.ps1` file structure in a future post. For now, the top of the test gives you a few good ideas as to why this test exists. It references the vSphere 6.0 Hardening Guide, and the Title/Description provide additional information.

The `$Actual` script block is the line that pulls the current value from each host. (If you were really curious, you could run that command manually by supplying a normal VMHost object instead of relying on Vester's `$Object` parameter.)

Making this investigation process a little easier is a decent idea for future work. ;)

### Remediation

If you failed any tests, the beauty of Vester is that you can fix them right away if you prefer.

> This is the part where you start changing your VMware environment! Proceed with appropriate caution.

I always recommend using `-WhatIf` prior to executing any commands that will affect your environment. We have made an effort to exhaustively support `-WhatIf` and provide useful, detailed messages.

```posh
Invoke-Vester -Remediate -WhatIf
```

Still like what you see? Pull the trigger! This will attempt to immediately fix every instance of failed tests by changing the values to those specified in your config.

```posh
Invoke-Vester -Remediate
```

There are more advanced uses, but I'll have to leave those for a follow-up blog post. If you made it down here, I'm glad you tagged along! I'm happy to answer any questions or listen to any feedback.

### More info

- Vester is open-source, maintained on [GitHub]
- The docs are published on [ReadTheDocs]
  - Admittedly, they're out of date at the moment :(
  - We are working hard to update them very soon!
- You can find us on [VMware {code} Slack] (open to all), in the **#vester** channel

Seriously, if nothing else, dip your toe in to the Slack channel! We think we're friendly, anyway. ;)

Part 2: [Vester 2: Vest Harder] | Part 3: [Write Your Own Vester Test]



[Vester 2: Vest Harder]: http://www.brianbunke.com/blog/2017/03/08/vester-2-vest-harder/
[Write Your Own Vester Test]: http://www.brianbunke.com/blog/2017/03/09/write-your-own-vester-test/
[Video Vester]: http://www.brianbunke.com/blog/2017/03/20/video-vester/
[PS Gallery]: https://www.powershellgallery.com/packages/Vester
[GitHub]: https://github.com/WahlNetwork/Vester
[PowerCLI]: https://code.vmware.com/web/dp/tool/vsphere_powercli/6.5
[Pester]: https://github.com/pester/Pester
[Chris Wahl]: https://twitter.com/chriswahl
[history and current state of Vester]: http://wahlnetwork.com/2017/03/03/vsphere-configuration-management-with-vester/
[contributors]: https://github.com/WahlNetwork/Vester/graphs/contributors
[DSC]: https://msdn.microsoft.com/en-us/powershell/dsc/overview
[Luc Dekens]: https://twitter.com/LucD22
[vSphereDSC]: https://github.com/lucdekens/vSphereDSC
[Brandon Olin]: https://twitter.com/devblackops
[POSHOrigin_vSphere]: https://github.com/devblackops/POSHOrigin_vSphere
[PowerShell Gallery]: https://www.powershellgallery.com/
[ReadTheDocs]: http://vester.readthedocs.io/en/latest/index.html
[VMware {code} Slack]: https://code.vmware.com/join
