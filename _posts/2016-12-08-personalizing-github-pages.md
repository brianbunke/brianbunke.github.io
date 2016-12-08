---
layout:     post
title:      Personalizing GitHub Pages
date:       2016-12-08 05:00:00
summary:    You've decided to use GitHub Pages. Now what?
categories: blog
thumbnail:  rss-square
tags:
 - blog
 - jekyll
 - github pages
 - carte noire
---

[Forfty percent] of all blogs' second posts are about how that blog was created, and tips & tricks learned during the process. I don't want to break from tradition during the holiday season, so here we go!

> Sorry, today's safety standards dictate that I need to read a warning message before we jump without looking. This walkthrough assumes you have very basic Git/GitHub familiarity. If not, refer to a guide like GitHub's [Hello World].

---

## What we're covering in class today:
{: .no_toc}

- ToC
{:toc}

---

## Why GitHub Pages?
You've decided to start a blog, and you've heard that all the cool kids are using [GitHub Pages] these days. (Obviously.) Ultimately, I'm interested in authoring posts in Markdown, and not worrying about standard hosting headaches.

Here's the good and bad from my point of view, if you find yourself thinking about the same decision:

**Pros**

- Free hosting (within [limits])
- Static site generation
  - I don't know PHP. Diving into CSS to edit themes is as far as I want to go
  - No back-end database like MySQL
- Site exists within GitHub's central source control
  - No more MySQL database backups or annoying imports during host transfers
- No worrying about patching WordPress this week
- Git & Markdown familiarity
  - If you're already familiar, great!
  - And if not, these are good generic skills to gain experience in for the future
  - With Markdown, native support for tech blog staples like code blocks and syntax highlighting

**Cons**

- If you're on Windows, it's far more annoying to preview a post before publishing it
  - [Jekyll on Windows] docs
  - Because of this, I have a...ahem..._verbose_ history of [commits] to my master branch while learning the platform (and my theme of choice)
- Less mature platform
  - WordPress has eight addons for every feature you can think of
    - Sure, a fair chunk are startingly insecure, but moving on
  - Some features you may be familiar with won't be available
    - Research your must-haves before making the leap!
- If the traffic limits listed above scare you, maybe you won't like being locked into GHP when you need to start paying
  - As a humble PowerShell nerd and meme repeater, I have absolutely no experience to offer here
- And our topic du jour, themes
  - Theme hunting in GHP is far more basic than WordPress's nice directory
  - Hopefully this post will help navigate that gap!

## How to Create a GHP Blog

You just need a free GitHub account!

[pages.github.com](https://pages.github.com/) has a super easy walkthrough to create your _username_.github.io repository and sync a quick index.html file to get started.

I really like [@jmcglone's in-depth walkthrough][jmcglone]. However, most people (myself included) don't want to build an entire blog from scratch. The less barriers to begin writing, the more content we all get to consume. If you'd rather just copy an existing theme (again, like I did), feel free to skim that walkthrough and we'll move on to the next section!

## Pimp My Ride

Selecting a theme for your WordPress blog:

![WordPress](https://brianbunke.github.io/images/theme_wordpress.jpg)

Selecting a theme for your GitHub Pages blog:

![GitHubPages](https://brianbunke.github.io/images/theme_ghp.jpg)

"Well, that settles it, WordPress it is." Yeah, I heard you. But hold on! We may be taking your training wheels off, but I'm running behind you holding the seat of the bike.

> Totally on topic: Dad held me steady for the first two houses before letting me ride off down the road...and I promptly forgot what brakes were and ran into the front of a parked car. But I was fine, and you will be too. ;)

Let's find a theme! Check out [this list] and tab open as many demos as you can. (No, there are no ratings or filtering to help. Yes, it's a bummer.) I prefer a dark blog, and those options are always much more limited, so the evaluation process goes a little quicker for me.

Find one you like? Sweet. Whatever will we do with it? And is that a sufficient cliffhanger that makes you want to turn the page into the next chapter?

> Now is a good time to mention that [installing] GitHub Pages supported themes is super easy, and that they have certified [ONE WHOLE THEME] for your cultivated tastes. So if you love Minima, I guess you're done!

## Ok, But Actually Pimp My Ride This Time

Alright. From here on out, I'm referencing the theme I chose, [Carte Noire], and some commits I made to this repo to apply and tweak it. If your theme has detailed install instructions, that's awesome, but mine and most others do not.

First off, you need to download the entire repository of the theme you want to use. In the [Carte Noire] repo, see that green "Clone or download" button in the top right? "Download ZIP" from there. Once you unzip, you now have your blog's basic structure on your local computer. Note that files like `_config.yml` and `index.html` should be at the top level.

Now, before applying to your site, you need to edit or remove some files.

- `CNAME` (delete)
  - This can be used to forward a personal domain to _username_.github.io, so your blog can still be maxpower.com.
    - More on [CNAME] in GHP
  - I'd recommend just deleting this right now. If it's something you want, get the basics working first before troubleshooting that
- `LICENSE` (edit?)
  - Normally, editing a LICENSE file is very poor form, and I wouldn't advocate doing so
  - In my case, the first half of the LICENSE file had a disclaimer about the original site's personal content, and the second half was a GPLv3 license covering the theme. Given that scenario, I removed the first half
- `README` (edit)
  - If there's a readme, it's definitely not useful for you. Edit that bad boy
  - You can [refer to mine][README] if you need inspiration 
- `_config.yml` (edit)
  - If the theme is well coded, most of the customization you need to do (aside from updating posts) will be contained within this file
  - Site settings (title, description), maybe social usernames / a footer to use across all pages / etc.

Once you've made those edits, you have the option to delete all posts in the `_posts` folder and start with no content. This keeps your .xml feed clean (yay, automatic Atom/RSS feed support!) at the expense of not giving you any example posts to view if you do some CSS tinkering later.

Whether or not you deleted all posts, now we can upload!

Because you have already created your _username_.github.io repo, and because you're using Git, you should already have cloned a _username_.github.io folder to your local computer. You know what goes in that folder? _Everything._

See what I added with [this commit]? Just dump it all in there, nice and cozy. Now, commit and sync (push) your changes up into GitHub.

GitHub Pages now uses Jekyll to immediately start generating your page. This happens very quickly (in ~5 seconds for me, most of the time), but you may occasionally experience a ~30-60 second delay while your site is built.

Anyway, in less than a minute, you can browse to https://_username_.github.io and your copied theme should be up and running. /successkid

## Impose Your Will

a.k.a. Edit that theme

I don't want to make this section too long, because it will be specific to your individual theme. Suffice to say, however, that I'm picky. I decided diving into the code right away was a worthwhile investment up front, versus just beginning to write and sorting it out later.

While reviewing my site's new theme, I made a note of three main things I wanted to change (I cannot overemphasize here how picky I am):

1. I wanted to add a Reddit icon to the existing social link options
2. The menu's width was too wide
3. Tags currently don't do anything (like, you can't click on them to view all posts with the same tag)
  - Noted as an [open issue] in the source repo

Adding Reddit took some bouncing around different files to follow all the dependencies, but I got there. However, after that didn't work right away, I also found that external dependency [Font Awesome] needed to be using its newest version in order to realize that a Reddit icon exists. See commits [1]/[2]/[3]/[4]/[5] if interested. Also, Font Awesome is kinda awesome.

> Little known fact: If you fix something generic that others might find useful, you can score bonus karma by submitting a [Pull Request] on the original theme's repo to help out everyone else!

Have you ever heard the [$10,000 hammer][10k] story? Well, five more commits later, I had finally figured out how to shrink the menu width as desired. ([a]/[b]/[c]/[d]/[e].) Moral of the story: Don't hire me to efficiently grasp 900-line CSS files. Well, and Chrome's Inspect is so freaking amazing and powerful.

And tags? Well, anticipating the amount of work I may need to do to make them behave as expected, I didn't start on tags. I may or may not get to them, ever. Maybe you'll use Carte Noire, consider my advice on pull requests, and solve the problem for yourself and everyone else at the same time? Eh? Eh?

## Burying the Lede

You know how this is a walkthrough on why GitHub Pages is cool? Well, here's my favorite part: **Everything is open source.** Do you want to know how to insert a link, an image, or some nested bullets? How about that Table of Contents up at the top of the page? (Which--side note--requires you to be using "Kramdown" in your `_config.yml` file.)

Well, this link below me ("suggest an edit!") takes you straight to the source code, and you can follow along with the Markdown file I committed to generate this page. You can even submit a Pull Request on this post to fix my typos, which you should totally do, because I _hate_ typos.

Come to the dark side. We have this walkthrough, and we most definitely have cookies.



[Forfty percent]: <https://youtu.be/CpmDIP3Fn2Y>
[Hello World]: <https://guides.github.com/activities/hello-world/>
[GitHub Pages]: <https://pages.github.com/>
[limits]: <https://help.github.com/articles/what-is-github-pages/#usage-limits>
[Jekyll on Windows]: <https://jekyllrb.com/docs/windows/>
[commits]: <https://github.com/brianbunke/brianbunke.github.io/commits/master>
[jmcglone]: <http://jmcglone.com/guides/github-pages/>
[this list]: <https://github.com/jekyll/jekyll/wiki/Themes>
[installing]: <https://help.github.com/articles/adding-a-jekyll-theme-to-your-github-pages-site/>
[ONE WHOLE THEME]: <https://pages.github.com/themes/>
[Carte Noire]: <https://github.com/jacobtomlinson/carte-noire>
[CNAME]: <https://help.github.com/articles/using-a-custom-domain-with-github-pages/>
[README]: <https://github.com/brianbunke/brianbunke.github.io/blob/master/readme.md>
[this commit]: <https://github.com/brianbunke/brianbunke.github.io/commit/2c81c8c20706b6202e217b869010e67984b9808e>
[1]: <https://github.com/brianbunke/brianbunke.github.io/commit/9f3370c7f4e2b27e9811e270400202c9839a246d>
[2]: <https://github.com/brianbunke/brianbunke.github.io/commit/62c8e96fee3edca3d32df49ae4e5a1b0a1ae7a33>
[3]: <https://github.com/brianbunke/brianbunke.github.io/commit/c0f22940f9f5f5b96f39a25f5d5bd4b716af368e>
[4]: <https://github.com/brianbunke/brianbunke.github.io/commit/5af0ef45656473d10506866fdbaec11416c36ba2>
[5]: <https://github.com/brianbunke/brianbunke.github.io/commit/cb7ea71fc62269689ff32dcd04f621c195b6d01e>
[Font Awesome]: <http://fontawesome.io/icons/>
[Pull Request]: <https://github.com/jacobtomlinson/carte-noire/pull/77>
[10k]: <http://michaelhamburger.com/10000-hammer/>
[a]: <https://github.com/brianbunke/brianbunke.github.io/commit/631581489453923ae3b77b165831432f65cb3335>
[b]: <https://github.com/brianbunke/brianbunke.github.io/commit/c3df41bce179740fba62eb6a682b02f7ba5b28b7>
[c]: <https://github.com/brianbunke/brianbunke.github.io/commit/0144f406f080a7265ca03e0d9b4d0acd3c6414ee>
[d]: <https://github.com/brianbunke/brianbunke.github.io/commit/668d0da05e99b17f5a555626d2247fd910555064>
[e]: <https://github.com/brianbunke/brianbunke.github.io/commit/5f5ab15fc80cdfa878d757cba4e6ae009c288eab>
[open issue]: <https://github.com/jacobtomlinson/carte-noire/issues/62>
