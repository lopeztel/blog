---
title: Thoughts on the pinebookpro (Part 2)
author: lopeztel
date: 2020-11-14T14:14:20+00:00
url: /2020/11/14/thoughts-on-the-pinebookpro-part-2/
tags:
  - 100DaysToOffload
  - Krita
  - Manjaro
  - pinebookpro

---
This is a follow-up to this [entry](https://lopeztel.xyz/blog/2020/10/23/thoughts-on-the-pinebook-pro/).

This time I&#8217;ll try to describe more day to day use (at least mine):

## Generic tasks

The most simple task I use a computer for is writing entries for my blog, as I mentioned before I love the Markdown format… I&#8217;ve found a perfectly good FOSS replacement for Typora, which is also available for aarch64, so I am now using [Ghostwriter](https://wereturtle.github.io/ghostwriter/).

Speaking of typing, I&#8217;ve been using the keyboard on the pinebookpro for a while now and it is quite good for the price, no premium feeling to it but it feels sturdy and the keys have decent travel, so that&#8217;s ok. The trackpad is another story, it feels sluggish and some times gestures such as using two fingers for right click take extra effort, I&#8217;d say good enough for the price.

I&#8217;ve also had trouble with WiFi networks, reception is a bit flaky at times, not sure if the antenna/hardware or software/drivers could be the cause (I&#8217;m more inclined to think SW), but both my cellphone and tablet seem to perform better in this regard.

I also found out that it is possible to [watch Netflix on the pinebookpro](https://wiki.pine64.org/index.php/Pinebook_Pro#Watching_DRM_content_.28Netflix.2C_etc..29) but the method is not very elegant, this is achieved by installing a docker container with chromium installed in it (it can be installed by running: `sudo pacman -Sy chromium-docker`). For some reason once installed, chromium also requires to be run as `sudo` or it won&#8217;t even start, I just created an alias in my .bashrc file.

Another problem is that the USB-C dongle (SD, USB 2.0, HDMI, ethernet) I have didn&#8217;t work with my display to I may have to look for another one or just a USB-C to HDMI adapter. 

## More particular tasks

I mentioned it was possible to install VScode, and I am a big fan of [PlatformIO](https://platformio.org/), since it is what I use to play around with my [esp8266](https://en.wikipedia.org/wiki/ESP8266) modules (I&#8217;m also looking into esp32 and nrf52 devices, but that&#8217;s material for another entry), ideally the PlatformIO plugin for VScode should only download the toolchain and flashing tools for your development platform but in this case I&#8217;m assuming the binaries were not compiled for aarch64 so no luck.

However I was able to get started with [rust](https://www.rust-lang.org/learn/get-started) without any issues, more on that later.

Another hobby of mine is digital art, so I was glad to find Krita in the official repositories of Manjaro ARM, another surprise is that my cheapo Huion digitizer tablet seems to work out of the box with it to some extent, pressure sensitivity works (a bit laggy, depends on how complex the effect of the current brush is) but pen buttons (which should be detected as a right click in Krita), seem to glitch, contextual menu is displayed but any further clicks make it disappear without any options being applied.

My x86 computer has the [DIGImend drivers](https://digimend.github.io/) installed and the right click glitch is nowhere to be seen there, so I tried installing them on the pinebookpro (this requires the linux-headers).

That&#8217;s it for now.

Day 28 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
