---
title: Thoughts on the pinebookpro (Part 4)
author: lopeztel
date: 2020-12-06T13:52:02+00:00
url: /2020/12/06/thoughts-on-the-pinebookpro-part-4/
tags:
  - 100DaysToOffload
  - Manjaro
  - pinebookpro

---
This is a follow-up on a previous [entry](https://lopeztel.xyz/blog/2020/11/17/thoughts-on-the-pinebookpro-part-3/)

## Dealing with an external display

A while back I ordered a USB-C dongle that I thought wasn&#8217;t working so I checked a list of USB Combination Devices that are compatible in the [wiki](https://wiki.pine64.org/wiki/Pinebook_Pro_Hardware_Accessory_Compatibility#USB_C_alternate_mode_DP), more specifically I had to wait for this [USB-C dongle from aliexpress](https://www.aliexpress.com/item/32954358411.html) to get here.

Turns out the new dongle didn&#8217;t work either, weird flicker when trying to switch to external display:

![I think it was trying to connect but failed](https://lopeztel.noho.st/piwigo/galleries/blog_media/pinebookpro_external_display.gif#center)

Long story short, the problem was that I had an up to date Manjaro ARM install (linux kernel 5.9), having an up to date system basically screwed me over 😀 The change is documented [here](https://forum.manjaro.org/t/tips-tricks-for-your-new-pinebook-pro/149)

All that was needed from the very beginning was switching to `linux-pinebookpro` from the mainline `linux` kernel.

![External display working (minor glitch on right side)](https://lopeztel.noho.st/piwigo/galleries/blog_media/IMG_20201203_180836171-768x576.jpg#center)

One of those things you have to fail at to get right. The more you know

Day 34 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
