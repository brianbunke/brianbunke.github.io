---
layout:     post
title:      Publishing Scripts to the PowerShell Gallery
date:       2017-01-09 08:30:00 +0800
summary:    Share Your Scripts Efficiently!
categories: blog
thumbnail:  cloud-upload
author:     brianbunke
tags:
 - powershell
 - code
 - psgallery
---

In my last post, [Get-VMotion], I mentioned that you can find the script I wrote on the [PowerShell Gallery]. (What's the PS Gallery? [More info.])

Learning the process of publishing a standalone script was like assembling new IKEA furniture: A wave of motivation and confidence, some tentative assembly, frustration because it's not fitting correctly, a rip-and-replace on one side, and some lasting anger at lacking documentation/components.

Gratuitous simile aside, the PS Gallery is really handy, and I'd love to encourage uploading more cool stuff. To that end, I've documented the entire process as of January 2017 here, and I hope it helps you feel confident in publishing your own scripts!

> Full disclosure: I'd love for you to publish a module, too. Modules are the preferred delivery method if you know what you're doing, but there are still use cases for standalone scripts. And, you know, this article isn't about `Publish-Module`. Moving on!

---

## Start to Finish:
{: .no_toc}

- ToC
{:toc}

---

## Create a PowerShell Gallery Account

- [https://www.powershellgallery.com/](https://www.powershellgallery.com/)
- Register (probably with an existing personal account?)
  - Allow access to the PS Gallery app
  - Select a username
  - You're registered!
- All signed in? Now view your profile (by clicking your username in the top right)
- You'll need your API key!

Your profile--where you copy your API key--should look like this:

![PSGalleryProfile](https://brianbunke.github.io/images/PSGalleryProfile.png)

**Important:** Guard your API key like a password! With obnoxious green boxes!


## Create Your Sweet Script

My example for today is [Measure-LastCommand]. `Measure-LastCommand` is a simple script that calculates the total runtime of any command you previously executed. Here's the important stuff:

![Measure-LastCommand](https://brianbunke.github.io/images/Measure-LastCommand.png)

The red arrows aren't important right now; I'm just calling out that I have collapsed both the function's comment-based help and its parameter block. And please forgive the line break...tried to keep things readable in image format.

Finally made your `Get-SimpsonsQuote` script pretty enough that you don't feel self-conscious sharing it? Awesome. Now let's start screwing with it.


## Publish-Script: Take One

![publish-error1](https://brianbunke.github.io/images/publish-script-error1.png)

Oh, ok. It needs `Update-ScriptFileInfo -Force` first.

> Again, I'm assuming you already finished tidying up your masterpiece. `New-ScriptFileInfo` also exists, and assumes you'll start your new script by defining the PS Gallery info first. Which seems unlikely. And it runs into the same problems I complain about below, so I'm ignoring it.

![publish-error2](https://brianbunke.github.io/images/publish-script-error2.png)

Alright, fine.

`Update-ScriptFileInfo -Path $MLC -Force -Description 'testing'` works with no complaints, so let's open things up and...whaaat the hell just happened. Quarantining that to its own Gist:

[Gist 1: Update-ScriptFileInfo results]

Ugh. Unnecessary white space, nonsense trailing spaces, and to really zoom in on this:

![hotgarbage](https://brianbunke.github.io/images/update-scriptfileinfo.png)

1. So many blank lines
2. Why did Description need its own comment block?
3. And now there are duplicate Description fields? How's that work?
4. Is that...is that a redundant, empty `Param ()` block? WHY?!?

brb, going for a long walk.


## Update-ScriptFileInfo, the Right Way

Ok, doing this in three steps. First, to answer the above:

1. I kill those blank lines. _I mean, wth man_
2. It doesn't need its own block
3. Yes to redundant `.DESCRIPTION`, I'll come back to that
4. Gretchen, stop trying to make double `Param ()` block happen. It's not going to happen.

Second, here's a template for using `Update-ScriptFileInfo` as of January 2017. Hopefully it provides an idea both of what options there are to define, as well as how to format most parameters.

[Gist 2: Update-ScriptFileInfo template]

As the template alludes, "Description" in PS Gallery land is similar to "Synopsis" in your comment-based help. Basically, your PSScriptInfo Description is a synopsis for the general public. Synopsis/Description in your help continue to function as expected, and can be more targeted to your niche audience.

> HINT: Need examples of how others did it? Here's a list of [PS Gallery items]; filter to just the Script item type on that page. That shows how Name/Description/etc. are displayed. You can also `Find-Script Measure-LastCommand | Format-List *`, or `Save-Script -Name Measure-LastCommand -Path "$env:USERPROFILE\Desktop"` to ~~steal~~ repurpose anyone's finished product!

> Note that Name is determined by the filename (without file extension) of your script, NOT the function name(s) within.

Third, this is how I published my finished script with reformatted PSScriptInfo:

![psscriptinfo](https://brianbunke.github.io/images/PSScriptInfo.png)

As you tweak the formatting to your preference, use the `Test-ScriptFileInfo` command to ensure everything is still working as expected.


## Publish-Script: Take Two

With PSScriptInfo populated, and with Test-ScriptFileInfo returning no errors, you're ready to go.

```
$MLC = "$env:USERPROFILE\Desktop\Measure-LastCommand.ps1"
$Key = 'asdf-jkl-12345'
Publish-Script -Path $MLC -NuGetApiKey $Key -Verbose
```

No username/password necessary, just done! You'll want to search the [PowerShell Gallery] for your new pride and joy, and you can always `Find-Script`/`Save-Script`/`Install-Script` with it too, just to make sure it works as expected.


## Using Scripts From the Gallery

You have the option to either:

1. `Save-Script`
  - Specify your own directory to store the file
  - Dot-source the file whenever you'd like to call the function
2. `Install-Script`
  - Adds 'C:\Program Files\WindowsPowerShell\Scripts' to your PATH environment variable
  - Which tries to load the script every time you launch PowerShell, automagically
  
I've had mixed results with `Install-Script` working in subsequent sessions. For now, I'm sticking with `Save-Script`, but I hope to find more reliable results when installing in the future, because it's really easy.


## Summary

For you:

- The PS Gallery is an easy way to get and share both modules and scripts with the world
- Use `Update-ScriptFileInfo` responsibly
- `Publish-Script` with your API key
  - And, when publishing updates, ensure your script retains the same GUID as before
- `Save-Script` vs. `Install-Script`
  - Probably `Save` for now...YMMV
- Seriously though, make `Get-SimpsonsQuote` for me!

For both of us?:

- The [PowerShellGet] module was open-sourced in Sept 2016
  - Hopefully most of this article becomes obsolete with some pull requests!

`Measure-LastCommand` can be found wherever fine scripts are available. (He means it's [on the Internet].)



[Get-VMotion]: http://www.brianbunke.com/blog/2017/01/03/get-vmotion/
[PowerShell Gallery]: https://www.powershellgallery.com/
[More info.]: https://msdn.microsoft.com/en-us/powershell/gallery/psgallery/psgallery_gettingstarted
[Measure-LastCommand]: https://www.powershellgallery.com/packages/Measure-LastCommand/
[Gist 1: Update-ScriptFileInfo results]: https://gist.github.com/brianbunke/7a913118b9c7af10cd6456ee5e4c24be
[Gist 2: Update-ScriptFileInfo template]: https://gist.github.com/brianbunke/03e90f28181b373d1df686429163711a
[PS Gallery items]: https://www.powershellgallery.com/items
[PowerShellGet]: https://github.com/PowerShell/PowerShellGet
[on the Internet]: https://www.powershellgallery.com/packages/Measure-LastCommand/
