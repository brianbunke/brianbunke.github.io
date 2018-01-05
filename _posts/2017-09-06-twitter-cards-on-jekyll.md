---
layout:     post
title:      Twitter Cards on Jekyll
date:       2017-09-06 06:00:00 -0700
summary:    If you're reading this, it worked ;)
categories: blog
thumbnail:  twitter
author:     brianbunke
image:      
tags:
 - blog
 - githubpages
 - jekyll
 - twitter
---

You have a [Jekyll] blog (most likely [GitHub Pages]), and you wrote an awesome post! Once it's published, it's time to tweet the link, lay in the hammock, and watch the visitors roll in.

[![before](/images/twittercard-before.png)](/images/twittercard-before.png)

...oh. That won't do. You can barely tell I shared a link! Nobody scrolling by is going to stop for that.

[![after](/images/twittercard-after.png)](/images/twittercard-after.png)

Ah, there we go. That's the good stuff.

This post covers how I added support for "Twitter Cards" to my Jekyll blog, even though my theme didn't have anything pre-built for me.

---

## Table of Contents:
{: .no_toc}

- ToC
{:toc}

---

## tl;dr (Too long; didn't read)

1. Paste [code] into `/_includes/head.html`
2. Ensure it refers to variables that exist:
  - "site.name" variables are in your top-level `_config.yml` file
  - "page.name" variables are in your individual blog post's [YAML front matter]
3. Tweak as desired
4. Refer to the [Twitter Cards] documentation when you get stuck
5. Use the [Validator Tool] before you tweet!

## Figure out the basics

I'll skip the part where I didn't know the proper name, and was Googling things like `why doesn't stupid twitter preview my stupid links`. But step one is always to figure out the silly marketing name of the thing you're trying to do. (Slack calls their link previews "unfurling.")

[Twitter Cards] documentation demonstrates the different [card types] available. I'm not doing any video right now, so I'm looking at two card types:

1. [Summary Card]
2. [Summary Card with Large Image]

Looking through both types' properties, you'll notice that the required properties are the same (card, title, description). Quite handy.

## Google around for examples

Armed with the proper term, now I can send Google queries like `twitter cards jekyll`. Two useful hits that I ~~stole~~ borrowed from:

- [Defining Twitter Cards in your Jekyll Template] \[alligator.io]
- [Supporting Twitter Cards with Jekyll] \[davidensinger.com]

Both links show the final code used, which proved helpful to pick and choose from. But only the first link says where to put the code: the _"head.html include file",_ which was `\_includes\head.html` for my site. Bingo.

## Ctrl + V like a boss

This is what my head.html file looked like: [Old head.html]

Paste your new code in there, basically anywhere. I decided to just paste in the code block from the first site into the very bottom of the file (right before `</head>`). However, VS Code's syntax highlighting parses that line 26 oddly, so I ended up inserting the new code at line 26 and bumping the last "paragraph" down. Whatever works.

But there's some basic stuff in our paste job that we need to fix. So...

## Tweak to fit your needs

I'll start with the final code I used, and then break down why I did each section that way, so you can make similar considerations when you tweak.

```html
{% raw %}
<!-- Twitter cards -->
<meta name="twitter:site"    content="@{{ site.twitter_username }}">
<meta name="twitter:creator" content="@{{ page.author }}">
<meta name="twitter:title"   content="{{ page.title }}">

{% if page.summary %}
<meta name="twitter:description" content="{{ page.summary }}">
{% else %}
<meta name="twitter:description" content="{{ site.description }}">
{% endif %}

{% if page.image %}
<meta name="twitter:card"  content="summary_large_image">
<meta name="twitter:image" content="{{ site.url }}{{ page.image }}">
{% else %}
<meta name="twitter:card"  content="summary">
<meta name="twitter:image" content="{{ site.title_image }}">
{% endif %}
<!-- end of Twitter cards -->
{% endraw %}
```

### Variables

Variable syntax is `{% raw %}{{ variablename }}{% endraw %}`. In Jekyll's case, you'll have `{% raw %}{{ site.variablename }}{% endraw %}` and `{% raw %}{{ page.variablename }}{% endraw %}`.

- **Site** variables are defined in your site's `_config.yml` file
  - For example, my site's [_config.yml] specifies a "title\_image", which I can refer to with `{% raw %}{{ site.title_image }}{% endraw %}`
- **Page** variables are defined in each post's [YAML front matter]
  - [An example post] shows the variables (title, summary, author, etc.) at the very top. Therefore, `{% raw %}{{ page.title }}{% endraw %}` will grab the post's title

### Site, Creator, Title

`twitter:site` - I already had a Twitter username defined in my site's config, so I'm just referencing it here to reduce hard-coded values.

`twitter:creator` - Not required, but interesting to add in case I have a guest author. I can easily refer to a different "author" at the top of the post.

Twitter's looking for a Twitter handle in "@username" format here, so I added "@" to the front of both strings.

`twitter:title` - The individual post's title. Hopefully pretty self-explanatory. :)

### Description

`twitter:description` - The sub-headline. I found that the theme I adopted was using "summary" instead of "description" for this, but either way is easy enough to solve for.

```html
{% raw %}
<!-- Did I include a "summary" variable in this post's front matter? -->
{% if page.summary %}
<!-- I did! Use it -->
<meta name="twitter:description" content="{{ page.summary }}">
{% else %}
<!-- Nope! Use the "description" defined in the site's _config.yml instead -->
<meta name="twitter:description" content="{{ site.description }}">
{% endif %}
{% endraw %}
```

### Image, Card

Here's where I got a little fancy. I don't always include an image in every post, so I decided to leverage a simple if/else to check for an image on the post, and only use the Large Image type when there's something useful to display.

```html
{% raw %}
<!-- Did I include an "image" variable in this post's front matter? -->
{% if page.image %}
<!-- I did! Select the "large image" card type, and supply the image path -->
<meta name="twitter:card"  content="summary_large_image">
<meta name="twitter:image" content="{{ site.url }}{{ page.image }}">
{% else %}
<!-- Nope! Select the "summary" card type, supplying the site's title image -->
<meta name="twitter:card"  content="summary">
<meta name="twitter:image" content="{{ site.title_image }}">
{% endif %}
{% endraw %}
```

Note that none of my posts had an "image" property defined! I'm adding it selectively to new posts from here on out. Therefore, it's just as easy for you to create whatever you want to help support your goal.

You could easily just define a full image path in each post, instead of copying my `site.url + page.image` string concatenation.

## Use the Validator Tool

Finished adding your changes? Twitter helpfully provides a [Validator Tool] to figure out whether your cards are working before tweeting 20 times.

## Adding Slack support

Just kidding! Slack surprised me by totally cooperating with "unfurling" activities after I added Twitter meta tags. Thanks, Slack!

## Summary

No link previews = bad. Link previews = excellent.

If you're here, I hope that means you were successful, and you learned a little about Jekyll's inner workings along the way. Reach out if you have suggestions or questions!

---

| Previous Jekyll posts:
| [Personalizing GitHub Pages]
| [Jekyll on Win10]



[Jekyll]: https://jekyllrb.com
[GitHub Pages]: https://pages.github.com

[code]: /blog/2017/09/06/twitter-cards-on-jekyll/#tweak-to-fit-your-needs

[Twitter Cards]: https://dev.twitter.com/cards/overview
[card types]: https://dev.twitter.com/cards/types
[Summary Card]: https://dev.twitter.com/cards/types/summary
[Summary Card with Large Image]: https://dev.twitter.com/cards/types/summary-large-image

[Defining Twitter Cards in your Jekyll Template]: https://alligator.io/jekyll/twitter-cards/
[Supporting Twitter Cards with Jekyll]: http://davidensinger.com/2013/04/supporting-twitter-cards-with-jekyll/

[Old head.html]: https://github.com/brianbunke/brianbunke.github.io/blob/0f67b89109f6035744f786e6e7b1c049902ff276/_includes/head.html

[_config.yml]: https://github.com/brianbunke/brianbunke.github.io/blob/7e2bfdf8a0ff9f15f2680f7fe3d5966ad213cdc5/_config.yml#L12
[YAML front matter]: https://jekyllrb.com/docs/frontmatter/
[An example post]: https://raw.githubusercontent.com/brianbunke/brianbunke.github.io/7f3261932826652d0111c1a6b7708daf44aba86d/_posts/2017-09-05-vmworld-2017-wrapup.md

[Validator Tool]: https://cards-dev.twitter.com/validator

[Personalizing GitHub Pages]: http://www.brianbunke.com/blog/2016/12/08/personalizing-github-pages/
[Jekyll on Win10]: http://www.brianbunke.com/blog/2017/05/24/jekyll-win10/
