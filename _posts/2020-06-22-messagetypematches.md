---
layout:     post
title:      New-InboxRule -MessageTypeMatches
date:       2020-06-22 00:00:00 -0800
summary:    Advanced email "filing" via EXOPS
categories: blog
thumbnail:  calendar-times
author:     brianbunke
image:      /images/owa-rule-splash.png
tags:
 - powershell
 - exchange
 - exops
---

A coworker asked for an Outlook rule that deleted all meeting responses. [This walkthrough] works if you're sitting in front of Outlook, but there has to be a way to do it with Exchange Online PowerShell (EXOPS), right?

```powershell
New-InboxRule -Mailbox 'me@brianbunke.com' -Name 'Delete meeting responses' -MessageTypeMatches CalendaringResponse -DeleteMessage:$true -StopProcessingRules:$true
```

Just a quick writeup, because I was surprised I couldn't find this while Googling. To figure it out, I had to `Get-Help New-InboxRule -Full` and scan through... `(Get-Help New-InboxRule -Full).parameters.parameter.Count` ...72 parameters to find roughly what I was looking for.

```
PS C:\> Get-Help New-InboxRule -Parameter MessageTypeMatches

-MessageTypeMatches <AutomaticReply | AutomaticForward | Encrypted | Calendaring | CalendaringResponse | PermissionControlled | Voicemail | Signed | ApprovalRequest | ReadReceipt | NonDeliveryReport>
[...]
```

This parameter does not implement a `[ValidateSet()]` attribute, so the choices do not auto-populate. I had actually found this a few minutes prior and hoped `Ctrl + Space` (open the Intellisense menu) would solve my problem, to no avail.

**One big gotcha:** Using `New-InboxRule`, whether on yourself or others, prevents the rule from being visible in the Outlook client. You must use Outlook Web Access (OWA) or EXOPS to see the rule you just created.

Outlook client:

[![outlook-rule-warning](/images/outlook-rule-warning.png)](/images/outlook-rule-warning.png)

OWA ([https://outlook.office365.com]):

[![owa-rule](/images/owa-rule.png)](/images/owa-rule.png)

Keep this in mind if you're administratively populating rules, as it won't be fun to learn after the fact. From my sleuthing, there is no fix for this today (June 2020).



[This walkthrough]: https://www.extendoffice.com/documents/outlook/1894-outlook-remove-meeting-responses.html
