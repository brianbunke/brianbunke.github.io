---
layout:     post
title:      Get-VMotion
date:       2017-01-03 10:00:00 +0800
summary:    Report on VMware vMotion events
categories: blog
thumbnail:  exchange
author:     brianbunke
tags:
 - powershell
 - code
 - powercli
 - vmware
---

As a VMware administrator, I occasionally field requests about VM performance. Troubleshooting involves asking the question "What changed?" a lot, and a vMotion (or Storage vMotion) can fall under that category.

I got really tired of parsing VM logs for vMotion events, and figured there could be a much better way. And, spoiler alert, there is: Get-VMotion.

> tl;dr - You can [find Get-VMotion] on the [PowerShell Gallery]! You can also grab it directly from [my GitHub], if that's more your style.

Here's how to get up and running:

```posh
Install-Script 'Get-VMotion'
# One-time step: Install it from the PowerShell Gallery with PS v5
# Requires an admin PowerShell window
# You may be prompted to add 'C:\Program Files\WindowsPowerShell\Scripts' to environment variable Path

# And load the Get-VMotion function into the current session by dot-sourcing it
# Specifying no path here works if your Scripts folder above shows in $env:Path
. Get-VMotion.ps1
# Get-VMotion requires PS v3 and PowerCLI module VMware.VimAutomation.Core
# Meaning PowerCLI v6 or higher must be installed

Get-Help Get-VMotion
Get-Help Get-VMotion -Examples

# Connect to your vCenter server
Connect-VIServer 'your-vcenter-server'

# View all vMotions in the last day. Or try something else from the examples!
Get-VMotion
```

![vMotion pic](https://brianbunke.github.io/images/get-vmotion.png)

The examples have some more advanced use cases (sorry, I went all-in on the Tron flavoring):

```posh
Get-VMotion
# By default, searches $global:DefaultVIServers (all open Connect-VIServer sessions).
# For all datacenters found by Get-Datacenter, view all s/vMotion events in the last 24 hours.
# VM name, type of vMotion (compute or storage), and vMotion start time & duration are returned by default.

Get-VMotion -Verbose | Format-List *
# For each s/vMotion event found in Example 1, show all properties instead of the default four.
# Verbose output tracks current progress, and helps when troubleshooting results.

Get-Cluster '*arcade' | Get-VMotion -Hours 8 | Where-Object {$_.Type -eq 'vmotion'}
# For the cluster Flynn's Arcade, view all vMotions in the last eight hours.
# NOTE: Piping "Get-Datacenter" or "Get-Cluster" will be much faster than an unfiltered "Get-VM".

Get-VM 'Sam' | Get-VMotion -Days 30 | Format-List *
# View hosts/datastores the VM "Sam" has been on in the last 30 days,
# when changes happened, and how long any migrations took to complete.
# When supplying VM objects, a generic warning displays once about result speed.

$Grid = $global:DefaultVIServers | Where-Object {$_.Name -eq 'Grid'}
Get-VM -Name 'Tron','Rinzler' | Get-VMotion -Days 7 -Server $Grid
# View all s/vMotion events for only VMs "Tron" and "Rinzler" in the last week.
# If connected to multiple servers, will only search for events on vCenter server Grid.

Get-VMotion | Select-Object Name,Type,Duration | Sort-Object Duration
# For all s/vMotions in the last day, return only VM name, vMotion type, and total migration time.
# Sort all events from fastest to slowest.
# Selecting < 5 properties automatically formats output in a table, instead of a list.
```

s/o to lucd ([link][link1]) and alanrenouf/sneddo ([link][link2]) for prior work in this area.

You can easily raise an issue on [my GitHub] if you find a bug, or just let me know if you found it helpful!

[find Get-VMotion]: https://www.powershellgallery.com/packages/Get-VMotion/
[PowerShell Gallery]: https://www.powershellgallery.com/
[my GitHub]: https://github.com/brianbunke/vCmdlets
[link1]: http://www.lucd.info/2013/03/31/get-the-vmotionsvmotion-history/
[link2]: https://github.com/alanrenouf/vCheck-vSphere
