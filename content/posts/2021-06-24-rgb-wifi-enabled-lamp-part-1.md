---
title: RGB WiFi enabled lamp (Part 1)
author: lopeztel
date: 2021-06-24T18:47:13+00:00
url: /2021/06/24/rgb-wifi-enabled-lamp-part-1/
tags:
  - 100DaysToOffload
  - esp8266
  - RGBLamp

---
Following up on hobby projects, this past few weeks I&#8217;ve been working a lot on my abandoned WiFi enabled RGB lamp

## The inspiration {#the-inspiration}

On one of those days you end up searching for random stuff across the internet I came across a project called [**Twisted WiFi-controlled Desk Lamp**](https://www.thingiverse.com/thing:4129249) on Thingiverse (awesome site, check it out), this was back when a friend offered to print stuff for me so I jumped at the opportunity and had him 3D print the parts

## My own personal touch {#my-own-personal-touch}

Since I had already been tinkering with [ESP8266 modules](https://www.microchip.ua/wireless/esp01.pdf) and looked into manipulating [WS2812B](https://www.digikey.com/en/datasheets/parallaxinc/parallax-inc-28085-ws2812b-rgb-led-datasheet) based LED strips (such as the Adafruit Neopixel) and reverse engineered a bit of the Arduino library for my needs, it only made sense to integrate all into this project. There are some instructions that the creator of the original project kindly provides but I wanted a more barebones/DIY thing so instead of Arduino and HTTP requests, I am using [PlatformIO](https://docs.platformio.org/en/latest/boards/espressif8266/esp01_1m.html) and ESP8266-nonos-sdk to implement [MQTT](https://mqtt.org/) in plain old C. I also didn&#8217;t want Amazon Alexa integration so I replaced the [Weemos D1 mini](https://www.weemos.cc/en/latest/d1/d1_mini.html) and the microphone the original project uses with a generic ESP-01 module and a [ESP-01 RGB board](https://www.aliexpress.com/item/32843759597.html)

## Before {#before}

Before starting this project I already had some work done with my custom MQTT Dashboard project on raspberrypi 3B:

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/Demo_compressed.gif#center)

Simple solid color change on a NeoPixel ring (16 LEDs)

## After {#after}

I added some more custom effects to my code and tweaked the lamp base to have access to the reset button, the array is made up of 8 strips with 9 LEDs each (72 in total), which also required a power supply instead of batteries, I went with standard 5V/3A power supply

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210617-105718-me.jpg#center)

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210621-221439-me.jpg#center)

This is the final product:

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/dashboard_demo.gif#center)

In part two I&#8217;ll go into details for some (kind of crappy) low level C programming and RGB LED effects

More details in my [github repo](https://github.com/lopeztel/esp8266_NeoPixel), also available from a mirrored repo on my self-hosted [gitea instance](https://lopeztel.noho.st/gitea/lopeztel/esp8266_NeoPixel)

---

Day 12 of my 2021&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
