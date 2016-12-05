---
layout:     post
title:      Hello World!
date:       2016-12-04 14:30:00
summary:    A first post to get acquainted with Jekyll.
categories: blog
thumbnail:  heart
tags:
 - blog
 - jekyll
 - github pages
---

I'll be using this blog to write about PowerShell and related technologies, primarily.

I wanted to try out [Jekyll] on [GitHub Pages][GP] for blog hosting, because I write a lot of [Markdown][MD]/[GFM] these days. I'm also excited about the idea of not needing to pay for a website host, maintain database backups, or continuously patch WordPress.

My experience with setting up the blog, selecting a theme, and customizing it as desired will be covered in the next post.[^1]

See the footer for a link to this blog's open source repository. The [readme] contains a lot of the links I used to get started.

```posh
# Oh hey, I can use GitHub Flavored Markdown to specify that a code block should be highlighted in PowerShell syntax!

If ($Author -ne 'lazy') {
    $BlogCount++
}
```

[^1]: One downside of going the Jekyll/GH Pages route: you need to be a little more tech savvy to make theme tweaks. I've done some non-trivial trolling around the code already to make preferred changes. With that time spent, here I am in the footnotes section (which I desperately want to abuse), and there is no special formatting class whatsoever in this theme. Hopefully one day I'll have the motivation to differentiate footnotes, as I won't use them moving forward if they just look like I'm still talking at the end of the post.

[Jekyll]: <https://jekyllrb.com/>
[GP]: <https://pages.github.com/>
[MD]: <http://daringfireball.net/projects/markdown/>
[GFM]: <https://guides.github.com/features/mastering-markdown/>
[readme]: <https://github.com/brianbunke/brianbunke.github.io/blob/master/readme.md>
