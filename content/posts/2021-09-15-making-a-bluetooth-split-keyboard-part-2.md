---
title: Making a bluetooth split keyboard part 2
author: lopeztel
date: 2021-09-15T18:45:53+00:00
url: /2021/09/15/making-a-bluetooth-split-keyboard-part-2/
tags:
  - 100DaysToOffload
  - nRF52
  - SplitKeyboard

---
This is a followup on a previous [entry](https://lopeztel.xyz/blog/2021/07/05/making-a-bluetooth-split-keyboard-part-1-a/)

It has been a while since I last wrote about this but I&#8217;ve been busy:

## Flashing the first nRFmicro {#flashing-the-first-nrfmicro}

The very fist attempt at flashing the [bootloader](https://github.com/adafruit/Adafruit_nRF52_Bootloader/releases/download/0.2.11/pca10056_bootloader-0.2.11_s140_6.1.1.hex) to my nRFmicro board failed miserably because I botched some pins on the main module so I <del>used this as an excuse and</del> bought [Pine64&#8217;s hammerhead kit](https://pine64.com/product/pinecil-preheater-hammer-head-tip/) to repair my soldering poor job, which seemed to work

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210717-115635-me.jpg#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210717-115844-me.jpg#center)


After this, mass erasing and flashing was possible and went smoothly:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/victory-me.png#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/victory2-me.png#center)

## Keyboard Assembly {#keyboard-assembly}

The rest of the process was just about waiting for components and assembling

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210718-170341-me.jpg#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210718-183056-me.jpg#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210723-175545-me.jpg#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210913-205138-me.jpg#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210913-211100-me.jpg#center)

## Final Product {#final-product}

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210915-195214-me.jpg#center)

{{< rawhtml >}} 
<video width=100% controls autoplay>
    <source src="https://lopeztel.noho.st/piwigo/galleries/blog_media/VID_20210913_214823382_HDR.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>
{{< /rawhtml >}}


## ZMK {#zmk}

I chose to use ZMK firmware for my keyboard and the setup is quite straightforward, as described in their [website](https://zmk.dev/docs/user-setup), the best part is that no local environment is needed and nRFmicro is supported out of the box

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/victory3-me.png#center)

Next I&#8217;m going to be playing around with the [keymap](https://zmk.dev/docs/features/keymaps/) 😀

---

Day 19 of my 2021&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
