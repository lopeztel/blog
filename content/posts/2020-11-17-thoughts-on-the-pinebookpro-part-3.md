---
title: Thoughts on the pinebookpro (Part 3)
author: lopeztel
date: 2020-11-17T18:24:11+00:00
url: /2020/11/17/thoughts-on-the-pinebookpro-part-3/
tags:
  - 100DaysToOffload
  - COVID-19
  - Manjaro
  - pinebookpro

---
This is a follow-up on a previous [entry](https://lopeztel.xyz/blog/2020/11/14/thoughts-on-the-pinebookpro-part-2/)

One step closer to daily driver!!

## Working from home is possible with the pinebookpro

I managed to connect to my workplace&#8217;s VPN, however it took some digging around: first of all, we have a Linux VPN package, so that&#8217;s already something, (kudos for showing Linux some love); you can choose to either install a .deb package, a .rpm package or build from source for either **x86_64** or **armhf** architectures (basically just copies over some pre-compiled binaries), so far so good, everything great.

However being the curious person I am, I wanted to try to use my pinebookpro in a daily driver scenario so I tried to build the VPN client for armhf (from what I know **aarch64** should be backwards compatible with **armhf**, if one of you, my dear readers knows otherwise, let me know), which failed to even build and would&#8217;ve required digging into the provided makefile or .sh files…

Thankfully Manjaro ARM/Arch official repositories contain [openconnect](http://www.infradead.org/openconnect/index.html), a multi protocol VPN client compatible with my Workplace&#8217;s VPN, so I just installed it, took a look at the manual to provide the right arguments and I was able to connect (only downside is it has to be run as `sudo`). Just for reference:

`sudo openconnect --protocol=<protocol> <vpn.your.workplace/> -u <username>`

It&#8217;s possible to use the `-p` flag to provide the password but I would advise against it. To avoid typing all this I just added an alias to my .bashrc.

I use [remmina](https://remmina.org/) as a remote desktop client, it works perfectly fine and is also available in Manjaro ARM&#8217;s official repositories.

## One more thing (see what I did there?)

I noticed one more quirk on the keyboard, apparently delete is a secondary function of the backspace key, not horrible but takes some getting used to.  
I have the ANSI US keyboard.

Day 28 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
