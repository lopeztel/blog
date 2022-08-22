---
title: "Migrating from WordPress to Hugo"
author: lopeztel
date: 2022-08-22T19:37:10+02:00
url: /2022/08/22/from-wordpress-to-hugo/
tags:
  - 100DaysToOffload
  - selfhosting
  - blog
---

I've been toying with the idea for a while, and now I finally found time to get to it
(more like I used the project as an excuse to not go out and exercise :P)

The whole thing started with finding [this blog
post](https://blog.arkadi.one/p/how-to-use-hugo-with-yunohost/) and realizing I
could just host my own static website from my yunohost raspberrypi setup, so the
journey began

I was gladly surprised by the fact that Hugo is easy to install in Arch or Arch based
Linux distros (such as my main desktop's Manjaro install): 

`sudo pacman -Syu hugo`

Then I started by going into the [quick start
guide](https://gohugo.io/getting-started/quick-start/) and picked the [papermod
theme](https://themes.gohugo.io/themes/hugo-papermod/) from the [big list of
available themes](https://themes.gohugo.io/)

For the record I liked themes [Archie](https://themes.gohugo.io/themes/archie/)
and [Terminal](https://themes.gohugo.io/themes/hugo-theme-terminal/) too

## First tests

I started with a demo website importing the theme as a git submodule:
```shell
hugo new site lopeztel_blog -f yml
cd lopeztel_blog
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod 
```
After this I spent a lot of time playing with the configuration file `config.yml`

## Exporting blog posts

Then it was time to look for a way to export all my posts from WordPress and
found some [alternatives](https://gohugo.io/tools/migrations/#wordpress), I
chose
[wordpress-to-hugo-exporter](https://github.com/SchumacherFM/wordpress-to-hugo-exporter)
because of the ease of use, it was easy to download the repo as a .zip and
upload to my WordPress instance, then load it as a plugin, activate it, go to
the dashboard -> Tools -> Export to Hugo

To be honest I panicked a little when I saw the message Nginx not found or
something on my web browser but upon ssh-ing into my serve I saw that the files
were exported successfully under `/tmp/`

## Fixing WordPress "inline HTML garbage"

After inspecting the .md files and copying them under `/content/posts/` and
reading on how to set up the [blog's
archive](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-features/#archives-layout),
I realized that all the posts were available through the tags link but not
listed in the archive.  All because of the added WordPress type. Also, my links
were broken because link notation is not markdown(I call it inline HTML, but I
may be wrong), example:

```markdown
---
title: 'Making another split keyboard: Lily58 QMK edition'
author: lopeztel
<!-- this is the line that caused the most issues, wordpress specific -->
type: post
date: 2022-05-17T19:32:55+00:00
url: /2022/05/17/making-another-split-keyboard-lily58-qmk-edition/
mf2_syndication:
  - 'a:0:{}'
categories:
  - Uncategorized
tags:
  - 100DaysToOffload
  - SplitKeyboard

---
With the previous success (see <a
href="https://lopeztel.xyz/2021/06/02/making-a-bluetooth-split-keyboard-part-1/"
target="_blank" rel="noreferrer noopener">part1</a>, <a
href="https://lopeztel.xyz/2021/07/05/making-a-bluetooth-split-keyboard-part-1-a/"
target="_blank" rel="noreferrer noopener">part1-a</a> and <a
href="https://lopeztel.xyz/2021/09/15/making-a-bluetooth-split-keyboard-part-2/"
target="_blank" rel="noreferrer noopener">part2</a>) in making a bluetooth
keyboard, I decided to make another keyboard with a different approach:

```

So I had to delete that line from every file, using the live grep functionality
from [Neovim Telescope live
grep](https://github.com/nvim-telescope/telescope.nvim#usage) proved useful,
not too bad.

Another problem was the link format I got from WordPress, had to fix it by
using some clever but cumbersome regex. Examples:

```
<a href="https://github.com/pfefferle/Autonomie" target="_blank" rel="noreferrer noopener">Autonomie theme</a> </figcaption></figure>
<a rel="tag" class="u-tag u-category" href="https://lopeztel.xyz/tag/100daystooffload/">#100DaysToOffload</a>
```

both change to:
```
[Autonomie theme](https://github.com/pfefferle/Autonomie)
[#100DaysToOffload](https://lopeztel.xyz/tag/100daystooffload/)
```

After issuing the following command in neovim:

```
:%s/<a.\{-}href="\([^*"]*\)\{-}>\([^*<]*\).\{-}>/[\2](\1)/g
```
Another example:

```
<pre class="wp-block-code"><code>#!/bin/bash
modprobe v4l2loopback exclusive_caps=1 max_buffers=2
gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420 -threads 0 -f v4l2 /dev/video0</code></pre>
```

Changes to:

```
#!/bin/bash
modprobe v4l2loopback exclusive_caps=1 max_buffers=2
gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420 -threads 0 -f v4l2 /dev/video0
```

After issuinf the following commands in neovim:

```
:%s/<pre .*<code>/```\r
:%s/<\/code><\/pre>/\r```
```

## Using my own Piwigo instance as a CDN

To serve the pictures and media I just uploaded everything to a public album
in my piwigo instance

---

Day 3 of my 2022's [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)
