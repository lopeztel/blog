---
title: Some CLI awesomeness
author: lopeztel
date: 2020-12-17T20:30:53+00:00
url: /2020/12/17/some-cli-awesomeness/
tags:
  - 100DaysToOffload
  - Manjaro

---
I&#8217;ll have to confess I wasn&#8217;t always a fan of CLI applications, I come from a world were all I knew was GUI. I even thought that the heavy use of terminal emulators and CLI apps was snobbish, you know, the feeling was kind of like when (some) people say _&#8220;BTW, I use Arch&#8221;_ I used to roll my eyes internally.

Yet, here I am writing about CLI apps and how awesome they are…

## ncspot

**ncspot** is very good (but it requires a premium subscription), it provides a bloat free ultra lightweight CLI client for Spotify, in my tests it consumed less than 50 MB of RAM:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/Screenshot-from-2020-12-16-13-31-48-1-768x841-me.png#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/Screenshot-from-2020-12-16-13-32-04-768x841-me.png#center)

The official client consumes ~600 MB, probably related to the chromium-like backend and all the social bloat (yes, I&#8217;m anti-social):

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-12-17-15-33-44-768x432.png#center)

The creator provides a footprint reference but I wanted to see for myself and boy, this app delivers, it is just meant to stream music and provide some basic navigation capabilities, which is good enough for me and it actually helps my productivity. The only missing feature is media key bindings but that&#8217;s just me nitpicking.

ncspot is written in rust, I encourage everyone to check the project&#8217;s [repo](https://github.com/hrkfdn/ncspot), it has to be compiled and installed from the AUR (I installed this on my pinebookpro too and it got really toasty) 

## mps-youtube

I used to rely on GUI apps like [smtube](https://www.smtube.org/) and [FreeTube](https://freetubeapp.io/), however these are heavier and bloated, respectively.

Given the success I had with ncspot I ventured into **mps-youtube**, which follows the same principle as ncspot, it lets you search for videos in the CLI and then reproduce them in an external mpv or mplayer window, no bloat or ads, just beautiful:

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-12-17-21-09-24-768x415.png#center)

However there&#8217;s a catch, since it is completely free it won&#8217;t work out of the box, you have to get a Youtube API Key from ~~our evil overlord~~ Google (details of the issue [here](https://github.com/mps-youtube/mps-youtube/issues/551))

It is possible to change the external video player preference, running the app also requires you to change mode _search_music_ to false, and _show_video_ to true.

mps-youtube is written in python, I also encourage everyone to check the project&#8217;s [repo](https://github.com/mps-youtube/mps-youtube), it can be installed from Manjaro&#8217;s official repositories.

## Some thoughts on privacy

Having described the apps and their advantages there are always some downsides:

  * **ncspot** requires your Spotify login information so there is some possible tracking happening but as far as I know it is like using the official client in private mode
  * **mps-youtube** requires a Youtube API key to work properly so there is always the risk of Google linking your account to the requests using your API key, maybe FreeTube is better in this regard

Day 38 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
