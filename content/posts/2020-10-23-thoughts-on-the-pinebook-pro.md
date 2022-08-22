---
title: Thoughts on the pinebook pro
author: lopeztel
date: 2020-10-23T18:32:59+00:00
url: /2020/10/23/thoughts-on-the-pinebook-pro/
tags:
  - 100DaysToOffload
  - Manjaro
  - pinebookpro

---
Back in September I pulled the trigger and ordered a pinebook pro.

Long story short: I declare myself a fanboy, the [pinebook pro](https://www.pine64.org/pinebook-pro/) from [pine64](https://www.pine64.org/) is a very capable little machine.

There are a lot of better written and more detailed blog posts/reviews of the pinebook pro out there already but just in case:

For the uninitiated the pinebook pro is an ARM powered laptop (yes, pine64 did it before it was cool) based on the ROCKPro64 single board computer &#8211;_Think Raspberrypi on steroids_&#8211; here are the specs:

  * Rockchip RK3399 SOC with Mali T860 MP4 GPU
  * 4GB LPDDR4 RAM
  * 1080p IPS Panel
  * Magnesium Alloy Shell body
  * Bootable Micro SD Slot
  * 64GB of eMMC (Upgradable)
  * PCIe x4 to m.2 NVMe SSD Slot (requires optional adapter)
  * SPI Flash 128Mbit
  * HD Digital Video Out via USB-C
  * USB 2.0 Host
  * USB 3.0 Host
  * USB-C (Data, Power and Video out)
  * Lithium Polymer Battery (10000mAH)
  * Stereo Speakers
  * WiFi 802.11 AC + Bluetooth 5.0
  * Headphone Jack
  * Microphone
  * Front-Facing Camera (1080p)
  * ISO & ANSI Keyboard Variants
  * Privacy Switches for Camera, Microphones and BT/WiFi (**Meta + F12/F10/F11 respectively. Silly me had trouble figuring out this one**)
  * Large Trackpad
  * UART Access via Audio Jack (**This requires opening the laptop and flipping a switch**)
  * Barrel Power (5V 3A) Port

The [wiki](https://wiki.pine64.org/index.php?title=Pinebook_Pro) is a lot more detailed. It includes OS and overall usage information as well, be sure to check it out.

## Experience with Manjaro ARM so far

The pinebook pro ships with [Manjaro ARM](https://manjaro.org/download/#ARM) pre-installed and I haven&#8217;t tested any other OS (yet), but I really like it and it is what I run on my old laptop as well so…

I really like to write my blog posts in markdown, so one of the first things I noticed was that [Typora](https://typora.io/) (thanks again [@kev](https://fosstodon.org/@kev)) was listed as an AUR install in pamac. That was a lie, all I got was something like _&#8220;Typora is not supported for aarch64&#8221;_, the alternative is to use VSCode to write blog posts but that&#8217;s like killing a fly with a bazooka, there&#8217;s always the option to build from source but that is not very elegant 🙁

[FreeTube](https://freetubeapp.io/) is another app I like to use to avoid all the ads and tracking from YouTube, but I was lucky and there is a zip file with a portable version for aarch64, not as elegant but all you need is to unzip and run the executable. 🙂

VScode, [QtCreator](https://www.qt.io/) and [Element](https://element.io/) are available from the official repositories. 🙂

I couldn&#8217;t find [timeshift](https://github.com/teejee2008/timeshift), my preferred backup solution. However I found [DejaDup](https://wiki.gnome.org/Apps/DejaDup) and was gladly surprised to find out that it is possible to backup to a server, which is why I used my Nextcloud instance on my yunohost server, which was cool (DejaDup can access via WebDAV) 🙂

That&#8217;s it for now, will continue testing, I&#8217;d like to thank [@pine64](http://@PINE64@fosstodon.org) for their hard work and community.

Here are some pictures of my unit with some fosstodon swag:

![see the cool pine meta key ?](https://lopeztel.noho.st/piwigo/galleries/blog_media/8669a61761419a96-768x576.jpg#center)

![Fosstodon sticker put to good use](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/aad1ee691183b703-768x1024-me.jpg#center)

EDIT: As pointed out by [@schrofi](https://digitalcourage.social/@schrofi), turns out Typora only publishes parts of the source code (Themes) in [GitHub](https://github.com/typora/), so building from source is not an option in this case.

I really like Typora but may have to look at other options&#8230; [Kwrite](https://apps.kde.org/en/kwrite) has markdown syntax highlighting so that&#8217;s something&#8230; Or I could go back to killing flies with a bazooka. 

Day 23 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
