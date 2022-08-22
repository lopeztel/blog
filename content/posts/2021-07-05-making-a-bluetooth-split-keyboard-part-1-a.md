---
title: Making a bluetooth split keyboard (Part 1-a)
author: lopeztel
date: 2021-07-05T19:00:20+00:00
url: /2021/07/05/making-a-bluetooth-split-keyboard-part-1-a/
tags:
  - 100DaysToOffload
  - SplitKeyboard
  - nRF52

---
A bit of a followup on a previous [entry](https://lopeztel.xyz/blog/2021/06/02/making-a-bluetooth-split-keyboard-part-1/), I’ve succeeded in turning a Chinese STM32F103 clone into a Black Magic Probe :

I found some instructions in [blackmagic-bluepill](https://github.com/koendv/blackmagic-bluepill) to follow

I’ll try to summarize:

  1. wiring is important!

![Wiring diagram][1] </figure> 

  2. Since the whole point of making the BMP flasher/debugger was to flash the nRFmicro, I followed the instructions on the [nRFmicro repo](https://github.com/joric/nrfmicro/wiki/Bootloader#bluepill) got their supplied blackmagic binaries
  3. For some reason I had to fiddle around and hold the reset button before attaching a device (search STM32 bluepill connect under reset for more on this) , it was a hit and miss thing … (`arm-none-eabi-binutils`, `arm-none-eabi-gcc` and `arm-none-eabi-gdb` are installed at this point, this is the list of commands to follow:
  4. I couldn’t load .bin files so I converted the downloaded binaries to .hex files

```bash
arm-none-eabi-objcopy -I binary -O ihex blackmagic_dfu.bin blackmagic_dfu.hex
arm-none-eabi-objcopy -I binary -O ihex blackmagic.bin blackmagic.hex

```

```bash
arm-none-eabi-gdb
target extended-remote /dev/ttyBmpGdb 
#/dev/ttyACM0 or /dev/ttyACM1 if the device rules are not installed
monitor swdp_scan
#this should show the target board voltage and Something like
#No. Att Driver
#1      M4

#up until this point I was holding the target board's reset button
#I had to try multiple times to do the following step and releasing the reset
#button at the same time
attach 1
load ./blackmagic_dfu.hex 0x8000000
load ./blackmagic.hex 0x8002000
quit
```

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/flahing_bmp-me.png#center)

Issuing `lsusb` yields:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/lsusb_bmp-me.png#center)

So yes, I now have a fully operational **blackmagic-bluepill** 😀

---

Day 16 of my 2021’s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!

 [1]: https://raw.githubusercontent.com/koendv/Connecting-Black-Magic-Probe-and-Blue-Pill/master/bmp_bp.svg
