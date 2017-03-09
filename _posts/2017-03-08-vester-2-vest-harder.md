---
layout:     post
title:      "Vester 2: Vest Harder"
date:       2017-03-08 07:30:00 -0800
summary:    Even Deeper into VMware Configuration Management
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

> Part 1 of this series, [Introducing Vester], covers the basics of what Vester is and how to get started.
>
> Part 3 of this series will cover the individual *.Vester.ps1 test structure, and walk through an example of how to create your own tests.

Relevant Vester links, conveniently up top for your reference:
- The project is published on the [PowerShell Gallery] and maintained, open-source, on [GitHub]
- Documentation is available on [ReadTheDocs]
  - Sorry, they're currently a bit outdated. Refresh coming very soon!
- Chat with users & maintainers on [VMware {code} Slack], in the **#vester** channel

And maybe a Table of Contents would be useful for quick jumps?
1. ToC
{:toc}

---

With the [happy path] covered, now I'm going to get a little more in-depth on common use cases.

### Config scope

By default, Vester assumes that all objects are created equal. This is ok in the general sense, because Vester isn't ensuring that VMs have the same name, same memory allocation, or anything crazy like that.

However, it's easy to pare back that behavior, as well. For example, "[brownfield]" scenarios are very common in the real world. Maybe you have a shiny new cluster you'd like to enforce uniformity on, but you have an older cluster in the same vCenter that can't be treated the same way.

Vester handles this by allowing each individual config file to limit object scope. Want to test your shiny new cluster right now, but leave the older cluster as a special snowflake?

![Shiny](/images/shinycluster.png)

As seen in the screenshot, the prompt provides examples of how PowerCLI will use your input. Test the commands yourself, and craft the scope string accordingly.

On the other hand, maybe you do want to run Vester against the old cluster, but you need to define different config settings. (To account for different shared storage, perhaps?) Well, what you'd probably want is...

### Multiple config files

Config files are intended to correspond 1:1 with vCenter servers, but that's not the only possibility.

If heading down this path, you'll need to start caring about two additional parameters.

If using `New-VesterConfig` to generate another file, you should use the `-OutputFolder` parameter to specify a different folder to drop the new `Config.json` file into.

```posh
# Dump it to the desktop for easy edit/rename/move
New-VesterConfig -OutputFolder "$env:USERPROFILE\Desktop\"
```

When you call `Invoke-Vester`, by default the `-Config` parameter uses `\Configs\Config.json` within your module folder. If you have multiple configs and/or you store them in a different folder, you'll need to specify that here.

```posh
Invoke-Vester -Config "C:\Vester\Config.json"
```

With code parameter explanations out of the way, here are a couple scenarios that would warrant multiple config files.

#### Config scenario 1: Multiple vCenter servers

If you're responsible for maintaining `vcenter-uswest.domain.com` and `vcenter-useast.domain.com`, you'll need a separate config file for each. Naming those files `config-uswest.json` and `config-useast.json` would be an easy way to differentiate.

#### Config scenario 2: Prod & Dev environments

You can also have multiple config files for the same vCenter server. Remember when I covered limiting your scope? You know, literally two minutes ago?

That idea can easily be applied to a `prd.json` vs. a `dev.json`. Limit your scope however you see fit, and then you can have Production and Development co-exist in vCenter while still wearing whatever different values you deem necessary.

### Manually editing a config

Here's a screenshot showing part of my `config.json` file.

![Config](/images/vesterconfig.png)

If you want to edit any values, just open it up in your favorite text editor and change away! Saying that you want DRS enabled is as easy as changing `false` to `true`. Changing your aggression level from 3 to 4? Go for it, ace.

Any test with a `null` value will be skipped on every run. You can also just delete that line from the config file for the same effect, if you really like keeping things tidy.

It's easy for me to tell you that you can change drslevel from "FullyAutomated" to "Manual", but what if you don't know that? Open the test file and inspect it yourself; it'll provide guidance on what the test does, and what the valid values are. I touched on how to review a Vester test file in [Introducing Vester].

After editing and saving, just run `Invoke-Vester` again to observe the change in test results.

### Narrowing the test suite

Want to save a lot of time? Run only the tests you care about. There are a few ways to do that, but here's where I start to get lazy and just shamelessly regurgitate `Get-Help Invoke-Vester -Examples`.

```posh
# Run all *.Vester.ps1 tests in a given directory, recursively
Invoke-Vester -Test C:\Tests\

# Supply Get-ChildItem results into the -Test parameter
$splat = @{
    Path = 'C:\Program Files\WindowsPowerShell\Modules\Vester'
    Filter = '*dns*.Vester.ps1'
    File = $true
    Recurse = $true
}
$DNS = Get-ChildItem @splat
Invoke-Vester -Test $DNS
```

And, as mentioned above, you can also `null` out values in your config file as another way of limiting the number of tests that are verifying results.

### Advanced Pester features

Because we're leveraging Pester, we can call some functionality that is already baked into Pester.

Two examples that are currently implemented are:

1. `$Results = Invoke-Vester -PassThru`
    - Want to capture test results in a variable to refer to later? PassThru sends results to `Write-Output`, instead of just `Write-Host`.
2. `Invoke-Vester -XMLOutputFile C:\vester.xml`
    - Pester can provide test results in [NUnit XML format]. This is helpful if you want to rig Vester up to a Continuous Integration (CI) system, like [GitLab] or [AppVeyor].

There are other options from Pester that we could support, but it needs to be done explicitly, on a case-by-case basis. We can't just flip the switch and bring in everything at once.

Because of that, if you have a use case where you need an extra feature from Pester, ask if it's possible! We figured any additional support could be inserted as demand is demonstrated.

### Troubleshooting and reporting bugs

If you think you've run into something weird, trying again with the Verbose flag active will give you some more helpful details. They say you need to hear a phone number three times to remember it, so if you'll forgive me:

```posh
Invoke-Vester -Verbose
Invoke-Vester -Verbose
Invoke-Vester -Verbose
```

Ran in verbose and think you've found a bug? (Don't worry, I believe you, there are probably a bunch kicking around.) [Open an issue] on GitHub! What were you trying to do, what did you expect to happen, and what actually happened?

You can also open an issue to make a feature request, or ask if something would be feasible. And, again, you can hit up the small group on [VMware {code} Slack] if you have more informal questions or feedback.

At the time of this writing, March 8th, 2017, we have an open issue on the project [soliciting feedback] on Vester's 1.0 functionality. We'd be happy to hear your thoughts on the current status.

Look for part 3 (writing your own tests) tomorrow. Happy Vesting!



[Introducing Vester]: http://www.brianbunke.com/blog/2017/03/07/introducing-vester/
[PowerShell Gallery]: https://www.powershellgallery.com/packages/Vester
[GitHub]: https://github.com/WahlNetwork/Vester
[ReadTheDocs]: http://vester.readthedocs.io/en/latest/index.html
[VMware {code} Slack]: https://code.vmware.com/join
[happy path]: https://en.wikipedia.org/wiki/Happy_path
[brownfield]: https://en.wikipedia.org/wiki/Brownfield_(software_development)
[NUnit XML format]: https://github.com/pester/Pester/wiki/Showing-Test-Results-in-CI-%28TeamCity%2C-AppVeyor%29
[GitLab]: https://gitlab.com
[AppVeyor]: https://www.appveyor.com
[Open an issue]: https://github.com/WahlNetwork/Vester/issues
[soliciting feedback]: https://github.com/WahlNetwork/Vester/issues/91
