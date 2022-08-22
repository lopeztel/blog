---
title: Making a bluetooth split keyboard (part 1)
author: lopeztel
date: 2021-06-02T19:04:16+00:00
url: /2021/06/02/making-a-bluetooth-split-keyboard-part-1/
tags:
  - 100DaysToOffload
  - nRF52
  - SplitKeyboard

---
It&#8217;s been a while since I last posted something remotely technical so I thought this would do this entry

All the work is based on the [Lily58 split keyboard](https://github.com/kata0510/Lily58), we will be using the [nRFmicro](https://github.com/joric/nrfmicro/) board, which in turn will be the brain of each keyboard half

The first step was to make the nRFmicro board using the [soldering guide](https://github.com/joric/nrfmicro/wiki/nRFMicro-1.4#soldering) in the repo, needless to say the process was complicated but a very good excuse to use my new [Pinecil](https://wiki.pine64.org/wiki/Pinecil)

![](https://fosstodon.b-cdn.net/media_attachments/files/106/325/921/459/803/737/original/f4818e5652c4838f.jpg#center)

![](https://fosstodon.b-cdn.net/media_attachments/files/106/325/922/036/958/684/original/b7b179fa62229abd.jpg#center)

The plan is to use the [ZMK firmware](https://zmkfirmware.dev/) but to do so, flashing the board is necessary, so far I&#8217;ve looked at 2 different approaches:

## nRF Command Line Tools in Linux {#nrf-command-line-tools-in-linux}

This is the process that was needed to use a [nRF52DK](https://www.nordicsemi.com/Software-and-tools/Development-Kits/nRF52-DK) as a debugger/programmer (it has a [J-Link OB](https://www.segger.com/products/debug-probes/j-link/models/j-link-ob/), which is why it can be used to program or debug other boards ) to flash a bootloader on the nRFmicro

According to [official documentation](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fug_nrf52840_dk%2FUG%2Fdk%2Fhw_debug_out.html&cp=4_0_4_7_9) and this [devZone post](https://devzone.nordicsemi.com/f/nordic-q-a/57636/programming-external-nrf52832-board-via-nrf52-dk-without-powering-external-device) it is possible to use P20 or P19 to connect external boards (needs shorting solder bridge SB47)

I&#8217;m using P20 according to the following diagram (**minus the GND DETECT connection**):

![](https://devzone.nordicsemi.com/cfs-file/__key/communityserver-discussions-components-files/4/layout.png#center)

Once the connections are sorted out the first step of the process is downloading and installing [nRF Command Line Tools](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Command-Line-Tools/Download#infotabs)

Following the link provided will point to a downloads page, since I use Manjaro, I had to download the corresponding linux zip file and uncompress it, I also uncompressed the tar files within the root directory (Debian based distro users can just use the .deb packages):

```
.
├── nRF-Command-Line-Tools_10_12_2_Linux-amd64
│   ├── JLink_Linux_V688a_x86_64.deb
│   ├── JLink_Linux_V688a_x86_64.tgz
│   ├── mergehex
│   ├── nRF-Command-Line-Tools_10_12_2_Linux-amd64.deb
│   ├── nRF-Command-Line-Tools_10_12_2.tar
│   ├── nrfjprog
│   └── README.txt
├── nRF-Command-Line-Tools_10_12_2_Linux-amd64.tar.gz
├── pynrfjprog-10.12.2.zip
└── ReadMe.txt
```

According to the README, directories _mergehex_ and _nrfjprog_ need to be copied to the _/opt_ directory:

  * copy directories to _/opt_

```
sudo cp -r <path_to_download>/nRF-Command-Line-Tools_10_12_2_Linux-amd64/nRF-Command-Line-Tools_10_12_2_Linux-amd64/nrfjprog /opt/
sudo cp -r <path_to_download>/nRF-Command-Line-Tools_10_12_2_Linux-amd64/nRF-Command-Line-Tools_10_12_2_Linux-amd64/mergehex /opt/
```

  * Make symbolic link for nrfjprog in _/usr/local/bin_

```
sudo ln -s /opt/nrfjprog/nrfjprog /usr/local/bin/nrfjprog
```

  * Make symbolic link for mergehex in _/usr/local/bin_

```
sudo ln -s /opt/mergehex/mergehex /usr/local/bin/mergehex
```

nRF command line tools expects the Jlink files to be installed under _/opt/SEGGER/JLink_, otherwise errors about JLink DLL not found pop up in the terminal, I followed the guide at [Eclipse embed CDT](https://eclipse-embed-cdt.github.io/debug/jlink/install/#linux_1):

```
sudo mkdir -p ~/opt/SEGGER
cd ~/opt/SEGGER
sudo tar xf /<path_to_download>/nRF-Command-Line-Tools_10_12_2_Linux-amd64/JLink_Linux_V688a_x86_64.tgz
chmod a-w ~/opt/SEGGER/JLink_Linux_V688a_x86_64
mv JLink_Linux_V688a_x86_64 JLink
```

Also an important part of this is to copy the device rules

```
sudo cp ~/opt/SEGGER/JLink/99-jlink.rules /etc/udev/rules.d
```

Funnily enough I think the installation was successful but I couldn&#8217;t flash the nRFmicro board, still debugging why &#8230;

## Creating a Black Magic Probe using a STM32 Blue Pill board {#creating-a-black-magic-probe-using-a-stm32-blue-pill-board}

As an alternative I tried to follow a [tutorial](https://github.com/koendv/blackmagic-bluepill) to create another programmer/debugger

I installed stm32flasher from AUR

```
yay -S stmflasher
```

And then followed the instructions but I have run into trouble with either my FTDIRS232 module or the Blue pill not responding UART requests in bootloader mode&#8230; a bit frustrating and also, still debugging&#8230;

---

Day 10 of my 2021&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
