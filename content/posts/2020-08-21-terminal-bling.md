---
title: Terminal bling
author: lopeztel
date: 2020-08-21T18:15:46+00:00
url: /2020/08/21/terminal-bling/
tags:
  - 100DaysToOffload
  - Manjaro

---
Followup on my previous entry related to my wallpaper. I just wanted my terminal to look nice since I spend quite some time on it. I use zsh by the way, but there are similar instructions for bash as well.

I&#8217;ll just document the steps I followed (2 alternatives)

## powerline-shell

Install **[powerline-shell](https://github.com/b-ryan/powerline-shell)** (more configuration options in the Github repo):

`sudo pip install powerline-shell`

Add these lines to .zshrc:

```bash
function powerline_precmd() {
    PS1="$(powerline-shell --shell zsh $?)"
}

function install_powerline_precmd() {
  for s in "${precmd_functions&#91;@]}"; do
    if &#91; "$s" = "powerline_precmd" ]; then
      return
    fi
  done
  precmd_functions+=(powerline_precmd)
}

if &#91; "$TERM" != "linux" ]; then
    install_powerline_precmd
fi
```

## ohmyzsh

Install **[ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)**:

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)`

Replace the theme in line 

`ZSH_THEME="robbyrussell"`

by one of the themes in this [list](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes), I chose agnoster.

## Color scheme (doesn&#8217;t work completely with powerline-shell)

Install **[pywal](https://github.com/dylanaraps/pywal)**:

<pre class="wp-block-code"><code>sudo pip install pywal</code></pre>

Then run

`wal -i "/path/to/img.jpg`

Which I pointed to my pacwall generated wallpaper, you should see a change in color scheme now. This is my result:<figure class="wp-block-image size-large">

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-08-21-19-19-18-768x432.png#center)

Finally make the color scheme part of the .zshrc and persist between reboots (by modifying .xinitrc) (based on these [instructions](https://github.com/dylanaraps/pywal/wiki/Getting-Started#applying-the-theme-to-new-terminals), here are my .zshrc additions:

```bash
(cat ~/.cache/wal/sequences &)
neofetch
```

And my .xinitrc addition:

`wal -R`

Day 14 of <a rel="tag" class="u-tag u-category" href="https://lopeztel.xyz/blog/tags/100daystooffload/">#100DaysToOffload</a>
