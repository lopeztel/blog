---
title: 'Making another split keyboard: Lily58 QMK edition'
author: lopeztel
date: 2022-05-17T19:32:55+00:00
url: /2022/05/17/making-another-split-keyboard-lily58-qmk-edition/
tags:
  - 100DaysToOffload
  - SplitKeyboard
  - ProMicro
---
With the previous success (see
[part1](https://lopeztel.xyz/blog/2021/06/02/making-a-bluetooth-split-keyboard-part-1/),
[part1-a](https://lopeztel.xyz/blog/2021/07/05/making-a-bluetooth-split-keyboard-part-1-a/)
and
[part2](https://lopeztel.xyz/blog/2021/09/15/making-a-bluetooth-split-keyboard-part-2/))
in making a bluetooth keyboard, I decided to make another keyboard with a
different approach:

My excuse for building this keyboard was that my blue switches keyboard may be
too noisy for the office, and the work desktop has no bluetooth (I know, sad,
right?). The plan for this one was:
- Quieter [Red gateron
  switches](https://www.aliexpress.com/item/1005003375712469.html?algo_exp_id=f391bbab-781f-48fc-949d-321524883df2-0&pdp_ext_f=%7B%22sku_id%22%3A%2212000025488463074%22%7D&pdp_npi=2%40dis%21NOK%21%218.91%21%21%21%21%21%400b0a555f16527735507182444ee7f3%2112000025488463074%21sea)
  rather than blue
- USB powered rather than battery power, which allows for [WS2812B
  LEDs](https://www.aliexpress.com/item/32682015405.html?algo_exp_id=3d875cbb-d97d-468e-aa53-f0df6dd59d08-0&pdp_ext_f=%7B%22sku_id%22%3A%2212000026799811814%22%7D&pdp_npi=2%40dis%21NOK%21190.6%2174.93%21%21%21%21%21%40210318bb16527733258817716e0440%2112000026799811814%21sea)
  and [OLED
  display](https://www.aliexpress.com/item/32672229793.html?algo_exp_id=f8b12bd7-fe32-43be-a437-974d3360e951-2&pdp_ext_f=%7B%22sku_id%22%3A%2265874427314%22%7D&pdp_npi=2%40dis%21NOK%21%2113.22%21%21%219.79%21%21%400b0a556b16527733814697343e73b4%2165874427314%21sea)
- Not a lot of assembly required (nRFmicro was a bit of a pain), so I chose the
  [Pro Micro](https://www.sparkfun.com/products/12640) as a controller board
  instead. Also, since this is going to be wired anyway, no need for bluetooth
  either
- Since the ProMicro is an 8-bit microcontroller and not supported by
  [ZMK](https://zmk.dev/docs/hardware), I had to go for
  [QMK](https://docs.qmk.fm/#/) for the firmware, which supports a [wide range
  of
  boards](https://github.com/qmk/qmk_firmware/blob/master/docs/compatible_microcontrollers.md)

## Building the keyboard

Basically tried to follow the guide
[here](https://kata0510.github.io/Lily58-Document/Lily58_Lite_BG.html), which
is in Japanese, but the pictures are more than enough guidance... Anyway, here
are some pictures from the process:

![Controller boards](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20220514-172133-me.jpg#center)

![Red gateron switches](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20220515-175352-me.jpg)

![WS2812B LED strip](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20220516-185712-me.jpg#center)

## Flashing QMK onto the controller boards

The fun part was flashing the keyboard's controller boards, and while all the
information is on the documentation page, what's a project without some issues?

First, I tried to use the qmk package from the community repos, i.e. `sudo
pacman -S qmk` and followed the documentation on [building
firmware](https://docs.qmk.fm/#/newbs_building_firmware), but I ran into
problems:

```shell-session
╰─ qmk compile -kb lily58/rev1 -km default                                                                                                                   ─╯
Ψ Compiling keymap with make --jobs=1 lily58/rev1:default


QMK Firmware 0.16.9
Making lily58/rev1 with keymap default

avr-gcc (GCC) 12.1.0
Copyright (C) 2022 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Generating: .build/obj_lily58_rev1/src/info_config.h                                                [OK]
Generating: .build/obj_lily58_rev1/src/default_keyboard.h                                           [OK]
Generating: .build/obj_lily58_rev1/src/layouts.h                                                    [OK]
Compiling: keyboards/lily58/lib/rgb_state_reader.c                                                  [OK]
Compiling: keyboards/lily58/lib/layer_state_reader.c                                                [OK]
Compiling: keyboards/lily58/lib/logo_reader.c                                                       [OK]
Compiling: keyboards/lily58/lib/keylogger.c                                                         [OK]
Compiling: keyboards/lily58/lily58.c                                                                [OK]
Compiling: keyboards/lily58/rev1/rev1.c                                                             [OK]
Compiling: keyboards/lily58/keymaps/default/keymap.c                                                [OK]
Compiling: quantum/quantum.c                                                                        [OK]
Compiling: quantum/send_string.c                                                                    [OK]
Compiling: quantum/bitwise.c                                                                        [OK]
Compiling: quantum/led.c                                                                            [OK]
Compiling: quantum/action.c                                                                         [OK]
Compiling: quantum/action_layer.c                                                                   [OK]
Compiling: quantum/action_tapping.c                                                                 [OK]
Compiling: quantum/action_util.c                                                                    [OK]
Compiling: quantum/eeconfig.c                                                                       [OK]
Compiling: quantum/keyboard.c                                                                       [OK]
Compiling: quantum/keymap_common.c                                                                  [OK]
Compiling: quantum/keycode_config.c                                                                 [OK]
Compiling: quantum/sync_timer.c                                                                     [OK]
Compiling: quantum/logging/debug.c                                                                  [OK]
Compiling: quantum/logging/sendchar.c                                                               [OK]
Compiling: quantum/bootmagic/magic.c                                                                [OK]
Compiling: quantum/matrix_common.c                                                                  [OK]
Compiling: quantum/matrix.c                                                                         [OK]
Compiling: quantum/debounce/sym_defer_g.c                                                           [OK]
Compiling: quantum/split_common/split_util.c                                                        [OK]
Compiling: quantum/split_common/transport.c                                                         [OK]
Compiling: quantum/split_common/transactions.c                                                      [OK]
Compiling: quantum/main.c                                                                           [OK]
Compiling: quantum/crc.c                                                                            [OK]
Compiling: drivers/oled/ssd1306_sh1106.c                                                            [OK]
Compiling: quantum/process_keycode/process_magic.c                                                  [OK]
Compiling: quantum/process_keycode/process_grave_esc.c                                              [OK]
Compiling: quantum/process_keycode/process_space_cadet.c                                            [OK]
Compiling: platforms/avr/drivers/i2c_master.c                                                      In function 'i2c_start_impl',
    inlined from 'i2c_start' at platforms/avr/drivers/i2c_master.c:104:18:
platforms/avr/drivers/i2c_master.c:61:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   61 |     TWCR = 0;
      |          ^
platforms/avr/drivers/i2c_master.c:63:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   63 |     TWCR = (1 << TWINT) | (1 << TWSTA) | (1 << TWEN);
      |          ^
In file included from /usr/avr/include/avr/io.h:99,
                 from platforms/avr/pin_defs.h:18,
                 from platforms/pin_defs.h:22,
                 from quantum/config_common.h:20,
                 from ./keyboards/lily58/config.h:21,
                 from <command-line>:
platforms/avr/drivers/i2c_master.c:66:14: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   66 |     while (!(TWCR & (1 << TWINT))) {
      |              ^~~~
platforms/avr/drivers/i2c_master.c:73:11: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   73 |     if (((TW_STATUS & 0xF8) != TW_START) && ((TW_STATUS & 0xF8) != TW_REP_START)) {
      |           ^~~~~~~~~
platforms/avr/drivers/i2c_master.c:73:47: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   73 |     if (((TW_STATUS & 0xF8) != TW_START) && ((TW_STATUS & 0xF8) != TW_REP_START)) {
      |                                               ^~~~~~~~~
platforms/avr/drivers/i2c_master.c:78:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   78 |     TWDR = address;
      |          ^
platforms/avr/drivers/i2c_master.c:80:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   80 |     TWCR = (1 << TWINT) | (1 << TWEN);
      |          ^
platforms/avr/drivers/i2c_master.c:83:14: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   83 |     while (!(TWCR & (1 << TWINT))) {
      |              ^~~~
platforms/avr/drivers/i2c_master.c:90:30: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
   90 |     uint8_t twst = TW_STATUS & 0xF8;
      |                              ^
In function 'i2c_stop',
    inlined from 'i2c_transmit' at platforms/avr/drivers/i2c_master.c:166:5:
platforms/avr/drivers/i2c_master.c:299:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
  299 |     TWCR = (1 << TWINT) | (1 << TWEN) | (1 << TWSTO);
      |          ^
In function 'i2c_stop',
    inlined from 'i2c_receive' at platforms/avr/drivers/i2c_master.c:188:5:
platforms/avr/drivers/i2c_master.c:299:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
  299 |     TWCR = (1 << TWINT) | (1 << TWEN) | (1 << TWSTO);
      |          ^
In function 'i2c_stop',
    inlined from 'i2c_writeReg' at platforms/avr/drivers/i2c_master.c:203:5:
platforms/avr/drivers/i2c_master.c:299:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
  299 |     TWCR = (1 << TWINT) | (1 << TWEN) | (1 << TWSTO);
      |          ^
In function 'i2c_stop',
    inlined from 'i2c_writeReg16' at platforms/avr/drivers/i2c_master.c:222:5:
platforms/avr/drivers/i2c_master.c:299:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
  299 |     TWCR = (1 << TWINT) | (1 << TWEN) | (1 << TWSTO);
      |          ^
In function 'i2c_stop',
    inlined from 'i2c_readReg' at platforms/avr/drivers/i2c_master.c:255:5:
platforms/avr/drivers/i2c_master.c:299:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
  299 |     TWCR = (1 << TWINT) | (1 << TWEN) | (1 << TWSTO);
      |          ^
In function 'i2c_stop',
    inlined from 'i2c_readReg16' at platforms/avr/drivers/i2c_master.c:292:5:
platforms/avr/drivers/i2c_master.c:299:10: error: array subscript 0 is outside array bounds of 'volatile uint8_t[0]' {aka 'volatile unsigned char[]'} [-Werror=array-bounds]
  299 |     TWCR = (1 << TWINT) | (1 << TWEN) | (1 << TWSTO);
      |          ^
cc1: all warnings being treated as errors
 [ERRORS]
 |
 |
 |
make[1]: *** [builddefs/common_rules.mk:456: .build/obj_lily58_rev1_default/i2c_master.o] Error 1
Make finished with errors
make: *** [Makefile:413: lily58/rev1:default] Error 1
```

After that, I had to go back to the the [getting started
guide](https://docs.qmk.fm/#/newbs_getting_started) and use Windows (the
horror), turns out that [QMK MSYS](https://msys.qmk.fm/) is basically the same
thing, so I just used that to create a new keymap and since I was already using
windows I figured I should also use the [QMK
Toolbox](https://github.com/qmk/qmk_toolbox/releases)

Also, I figured out that it is possible to make the already soldered ProMicro
boards go into bootloader mode by pressing the **shift** + **B** keys on the left
half and **shift** + **N** keys on the right half of the keyboard. Another fun
fact is that you flash the same .hex file onto both boards, so no need to have
per half binaries (as opposed to the last keyboard)

![QMK Tools](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/qmk-tools-me.png#center)

[Here](https://github.com/lopeztel/QMK-config) are my configuration files, in
case anyone finds them useful

Mandatory glamour shot of the finished product:

![Finished keyboard](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/20220516-212419-me.jpg#center)

Day 2 of my 2022's [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)
