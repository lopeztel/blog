---
title: Infinitime 1.2-0 on PineTime, from scratch
author: lopeztel
date: 2021-06-26T19:00:00+00:00
url: /2021/06/26/infinitime-1-2-0-on-pinetime-from-scratch/
tags:
  - 100DaysToOffload
  - nrf52
  - pinetime

---
It seems that it is normal for me to buy stuff/parts/devkits and let them accumulate some dust before getting back to the project pile &#8230;

Just recently I read on the Pine64 Blog [June update](https://www.pine64.org/2021/06/15/june-update-new-hardware-and-more-on-the-way/) that Infinitime 1.2 was coming soon&#8230; Then I remembered I had ordered a [PineTime](https://www.pine64.org/pinetime/) devkit (part of the first batch ever actually) so I decided to find it and flash working firmware on it&#8230;

I attempted to flash it with a [STM bluepill Black Magic Probe](https://github.com/koendv/blackmagic-bluepill) first:<figure class="wp-block-image">

![wiring diagram][1] </figure> 

Image taken from this [forum thread](https://forum.pine64.org/showthread.php?tid=8816&pid=57095)

Guide to program using arm-noneabi-gdb and BMP [here](https://bluetun.serverbox.ch/2020/01/10/flashing-the-nrf52840-with-a-blackmagic-probe-swd-jtag-programmer/)

Apparently I managed to delete the application firmware because the dummy GUI that showed the charging state was gone&#8230;but I didn&#8217;t manage to make it show anything, I was left with a paperweight, just for fun, here&#8217;s the log:

```shell
╰─ arm-none-eabi-gdb                                                         ─╯
GNU gdb (GDB) 10.2
Copyright (C) 2021 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later &lt;http://gnu.org/licenses/gpl.html&gt;
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "--host=x86_64-pc-linux-gnu --target=arm-none-eabi".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
&lt;https://www.gnu.org/software/gdb/bugs/&gt;.
Find the GDB manual and other documentation resources online at:
    &lt;http://www.gnu.org/software/gdb/documentation/&gt;.

For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb) target extended-remote /dev/ttyACM0
Remote debugging using /dev/ttyACM0
(gdb) monitor swdp_scan
Target voltage: 2.96V
Available Targets:
No. Att Driver
 1      Nordic nRF52 M4
 2      Nordic nRF52 Access Port 
(gdb) attach 1
Attaching to Remote target
warning: No executable has been specified and target does not support
determining executable automatically.  Try using the "file" command.
0xfffffffe in ?? ()
(gdb) mon erase_mass
erase..
(gdb) mon hard_srst
(gdb) detach
Detaching from program: , Remote target
&#91;Inferior 1 (Remote target) detached]
(gdb) monitor swdp_scan
Target voltage: 2.94V
Available Targets:
No. Att Driver
 1      Nordic nRF52 M4
 2      Nordic nRF52 Access Port 
(gdb) attach 1
Attaching to Remote target
warning: No executable has been specified and target does not support
determining executable automatically.  Try using the "file" command.
0xfffffffe in ?? ()
(gdb) file ~/Downloads/Infinitime/1.2.0/merged-internal.hex
A program is being debugged already.
Are you sure you want to change the file? (y or n) y
Reading symbols from ~/Downloads/Infinitime/1.2.0/merged-internal.hex...
(No debugging symbols found in ~/Downloads/Infinitime/1.2.0/merged-internal.hex)
(gdb) load
Loading section .sec1, size 0x54a0 lma 0x0
Loading section .sec2, size 0x8000 lma 0x8000
Loading section .sec3, size 0x10000 lma 0x10000
Loading section .sec4, size 0x10000 lma 0x20000
Loading section .sec5, size 0x10000 lma 0x30000
Loading section .sec6, size 0x10000 lma 0x40000
Loading section .sec7, size 0x9970 lma 0x50000
Start address 0x00000000, load size 355856
Transfer rate: 39 KB/sec, 969 bytes/write.
(gdb) compare-sections
Section .sec1, range 0x0 -- 0x54a0: matched.
Section .sec2, range 0x8000 -- 0x10000: matched.
Section .sec3, range 0x10000 -- 0x20000: matched.
Section .sec4, range 0x20000 -- 0x30000: matched.
Section .sec5, range 0x30000 -- 0x40000: matched.
Section .sec6, range 0x40000 -- 0x50000: matched.
Section .sec7, range 0x50000 -- 0x59970: matched.
(gdb) mon hard_srst
(gdb) detach
Detaching from program: /home/lopeztel/Downloads/Infinitime/1.2.0/merged-internal.hex, Remote target
&#91;Inferior 1 (Remote target) detached]
(gdb) quit
```

After consulting with the guys at the [PineTime-dev Matrix channel](https://matrix.to/#/#pinetime-dev:matrix.org), I was glad to confirm I was flashing the right file: merged-internal.hex from the [releases page](https://github.com/JF002/InfiniTime/releases/tag/1.2.0) in [InfiniTime github repo](https://github.com/JF002/InfiniTime) actually contains both the bootloader and the application firmware in a single file, so no complaints there&#8230;

But, someone also pointed out that the first devices to ship had locked bootloaders, so turns out I had to unlock it with a Segger J-Link Debug Probe (aka [NRF52DK](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fug_nrf52832_dk%2FUG%2Fnrf52_DK%2Fintro.html) I had around here)

Pin connections:<figure class="wp-block-table">

| Pin on PineTime | Pin on NRF52DK P20 |
| --------------- | ------------------ |
| SWDIO           | SWDIO              |
| SWDCLK          | SWDCLK             |
| 3.3V VCC        | VTG                |
| GND             | GND DETECT         |</figure> 

Then it was just a matter of issuing a couple of [nrfjprog](https://www.nordicsemi.com/Products/Development-tools/nrf-command-line-tools) commands as stated in the wiki article [Reprogramming the PineTime](https://wiki.pine64.org/wiki/Reprogramming_the_PineTime#STM32_.28Blue_Pill.29):

```shell
nrfjprog -f NRF52 --recover
nrfjprog -f NRF52 --program <path-to-hex>/merged-internal.hex --sectorerase
nrfjprog -f NRF42 -f NRF52 --reset
```

Which finally did the trick:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20210624-105138-me.jpg#center)

Now, I only need to do something about the stuck button&#8230;I attempted to seal the device with superglue and it spilled everywhere

Huge shoutout to the nice people in the PineTime-dev Matrix channel, they were very patient and helpful!

---

Day 14 of my 2020&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!

 [1]: https://matrix-client.matrix.org/_matrix/media/r0/download/matrix.org/TTthxkqwegUAPqePnQWXjLmC
