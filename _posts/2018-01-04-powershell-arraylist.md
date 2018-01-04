---
layout:     post
title:      PowerShell ArrayList Module
date:       2018-01-04 06:00:00 -0800
summary:    .NET lists in PowerShell syntax
categories: blog
thumbnail:  object-group
author:     brianbunke
image:      /images/arraylist.png
tags:
 - powershell
 - arraylist
 - array
 - code
---

[![aoc](/images/arraylist.png)](/images/arraylist.png)

---

## Table of Contents
{: .no_toc}

- ToC
{:toc}

---

## The Status Quo of @()

You need to capture output within a ForEach loop in your PowerShell code. How do you do it quickly? The way we all learned, of course:

```powershell
# Create an empty array
$Array = @()
1..10 | ForEach-Object {
    # Use the += operator to append
    $Array += $_
}
```

This works, but it's suboptimal. Because this type of array is _immutable,_ it slows down as object counts increase, and modifying array contents--like removing an item--is really just "let's use `Where-Object` to recreate this array."

> This topic has been written about often, so I won't belabor the point. [See more]

## Using .NET classes in PowerShell

In that same Google search, you'll see that the suggestion is to use .NET's ArrayList class, which is what I usually do. However, there are a couple small annoyances with doing it that way:

1. Suppressing output (like with `[void]`) is easy to forget
2. The `AddRange`/`Add` conditional is also easy to forget, when necessary
3. I kept forgetting the full .NET typename :(

I have looked up the syntax for `ArrayList` and `Generic.List` a decent amount in the last year or two, and I figured there's got to be someone else with this same problem.

## Install-Module ArrayList

Embrace the siren song of simple PowerShell syntax, and let's abstract it away. If you'll allow me to borrow from the readme:

> Is `[System.Collections.ArrayList]` already pretty easy to use? Yes.
> 
> Is the point of PowerShell to abstract everything into its most awesome form? ALSO YES, MY FRIEND.

ArrayList is on the [PowerShell Gallery] and [GitHub] for your enjoyment!

### Example module usage

ArrayList v1.0 allows you to create an `ArrayList`/`Generic.List`, append objects into it very quickly, and remove matching objects.

Code is also borrowed from v1 of the readme.

Easy mode:

```powershell
# Capture an empty ArrayList
$10 = New-ArrayList
# Append one/many objects via the pipeline
1..10 | Add-ArrayObject -Array $10
# And review
$10
```

Advanced features:

```powershell
# Create a type-safe List<T> collection
$DirList = New-ArrayList -Type PSCustomObject

$UserGCI = (Get-ChildItem $env:USERPROFILE -Directory).FullName
# Appending into arrays is common within a For/ForEach loop:
ForEach ($folder in $UserGCI) {
    Get-ChildItem $folder -Directory | ForEach-Object {
        [PSCustomObject]@{
            Parent    = $folder
            Directory = $_.Name
        } | Add-ArrayObject $DirList
    }
}
# View the results
$DirList

# Remove all entries of Desktop subfolders
$DirList |
    Where-Object {$_.Parent -like '*desktop'} |
    Remove-ArrayObject $DirList
# And view again
$DirList
```

With implicit module loading, once it's installed, it's easier to remember `New-ArrayList`, `Add-ArrayObject`, and `Remove-ArrayObject` than the .NET syntax.

> _"Abstraction! Abstraction! Abstraction! Abstraction!"_
> 
> \- [Steve Ballmer]

### Why didn't you _?

I wanted to keep it (very) simple to start, and I'm not sure how much interest this module will draw.

Have a good idea? ArrayList is on [GitHub], and I'd love to hear from you!

> (Need a GitHub refresher? Shameless plug for my [101 series])



[See more]: https://www.google.com/search?q=powershell+array+performance

[PowerShell Gallery]: http://www.powershellgallery.com/packages/ArrayList/
[GitHub]: https://github.com/brianbunke/ArrayList

[Steve Ballmer]: https://youtu.be/Vhh_GeBPOhs

[101 series]: /blog/2017/05/08/github-101/
