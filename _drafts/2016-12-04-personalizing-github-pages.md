---
layout:     post
title:      Personalizing GitHub Pages
date:       2016-12-04 14:30:00
summary:    You've decided to use GitHub Pages. Now what?
categories: blog
thumbnail:  rss-square
tags:
 - blog
 - jekyll
 - github pages
 - carte noire
---

[Forfty percent][forfty] of all blogs' second posts are about how that blog was created, and tips & tricks learned during the process. I don't want to break from tradition during the holiday season, so here we are!

## What we're covering in class today:
{:.no_toc}
{toc}

## Why GitHub Pages?
You've decided to start a blog, and you've heard that all the cool kids are using [GitHub Pages][GP] these days. (Obviously.) Ultimately, I'm interested in authoring in Markdown and not worrying about standard hosting headaches.

Here's the good and bad from my point of view, if you find yourself thinking about the same decision:

__Pros__
- Free hosting (within [limits])
- Static site generation
  - I don't know PHP. Diving into CSS to edit themes is as far as I want to go
  - No back-end database like MySQL
- Site exists within GitHub's central source control
  - a.k.a. no more MySQL database backups
- No worrying about patching WordPress this week
- Git & Markdown familiarity
  - If you're already familiar, great!
  - And if not, these are good generic skills to gain experience in for the future
  - With Markdown, native support for tech blog staples like code blocks and syntax highlighting

__Cons__
- If you're on Windows, it's far more annoying to preview a post before publishing it
  - [Jekyll on Windows][JekWin] docs
  - Because of this, I have a...ahem..._verbose_ history of [commits] to my master branch while learning the platform (and my theme of choice)
- Less mature platform
  - WordPress has eight addons for every feature you can think of
    - Sure, a fair chunk are startingly insecure, but moving on
  - Some features you may be familiar with won't be available
    - Research your must-haves before making the leap!
- And, of course, the topic of the rest of this post...

## How to Create a GHP Blog

You just need a free GitHub account!

[pages.github.com](https://pages.github.com/) has a super easy walkthrough to create your _username_.github.io repository and sync a quick index.html file to get started.

I really like [@jmcglone's in-depth walkthrough][jmcglone]. However, most people (myself included) don't want to build an entire blog from scratch. The less barriers to begin writing, the more content we all get to consume. If you'd rather just copy an existing theme (again, like I did), feel free to skim that walkthrough and we'll move on to the next section!

## Pimp My Ride

Selecting a theme for your WordPress blog:

![WordPress](https://brianbunke.github.io/images/theme_wordpress.jpg)

Selecting a theme for your GitHub Pages blog:

![GitHubPages](https://brianbunke.github.io/images/theme_ghp.jpg)

LOTS MORE HERE ABOUT THEMING



[forfty]: <https://youtu.be/CpmDIP3Fn2Y>
[GP]: <https://pages.github.com/>
[limits]: <https://help.github.com/articles/what-is-github-pages/>
[JekWin]: <https://jekyllrb.com/docs/windows/>
[commits]: <https://github.com/brianbunke/brianbunke.github.io/commits/master>
[jmcglone]: <http://jmcglone.com/guides/github-pages/>