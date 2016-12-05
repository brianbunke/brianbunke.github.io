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

You've decided to start a blog, and you've heard that all the cool kids are using [GitHub Pages][GP] these days. (Obviously.) Here's why I chose this path:

__Pros__
- Free hosting (within [limits])
- Site exists within GitHub's central source control
  - a.k.a. no more MySQL database backups
- No dropping everything to patch WordPress again
- Git & Markdown familiarity
  - If you're already familiar, great! And if not, these are good generic skills to gain experience in for the future
  - With Markdown, native support for tech blog staples like code blocks and syntax highlighting

__Cons__
- If you're on Windows, it's far more annoying to preview a post before publishing it
  - [Jekyll on Windows][JekWin] docs
  - Because of this, I have a...ahem..._verbose_ history of [commits] to my master branch while learning the platform (and my theme of choice)
- Less mature platform
  - WordPress has eight addons for every feature you can think of
    - Sure, a fair chunk are startingly insecure, but moving on
  - Some features you're familiar with won't be available
    - For example, I'm not sure how you would insert a poll (if you cared to), because GitHub Pages is a static hosting service with no database required/available. I guess by interfacing with a third-party provider? I'm a blog poll noob.
- And, of course, the topic of the rest of this post...

Selecting a theme for your WordPress blog:
![WordPress](https://brianbunke.github.io/images/theme_wordpress.jpg)

Selecting a theme for your GitHub Pages blog:
![GitHubPages](https://brianbunke.github.io/images/theme_ghp.jpg)

LOTS MORE HERE ABOUT THEMING



[GP]: <https://pages.github.com/>
[limits]: <https://help.github.com/articles/what-is-github-pages/>
[JekWin]: <https://jekyllrb.com/docs/windows/>
[commits]: <https://github.com/brianbunke/brianbunke.github.io/commits/master>
