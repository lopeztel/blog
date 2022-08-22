---
title: The journey to self-hosting (on a Raspberrypi 4)
author: lopeztel
date: 2020-07-07T20:12:55+00:00
url: /2020/07/07/the-journey-to-self-hosting-on-a-raspberrypi-4/
tags:
  - 100DaysToOffload
  - Fediverse
  - OpenMediaVault
  - Raspberrypi
  - Selfhosting

---
Ever since discovering the Fediverse and reading toots from awesome people in [Fosstodon][1], a little idea started to take shape in my head: _perhaps its not that hard to self host, there are tons of tutorials and resources out there&#8230;And I need some alternative to google photos to share albums with my family back home_

## Hardware

After some research and settled for a RaspberryPi server/NAS, now, the purists will say that it is not the ideal platform to host but I think it is perfect to get started and learn the basics or as a hobby, then you can move on to the big boy fancy server hardware.

The first step was to choose the hardware, I settled for this kit because it provides a nice way to attach 2.5&#8243; SATA drives:  
[Dual/Quad SATA HAT, top board HAT and metal case from Radxa][2]

It was easy to assemble and the process was fun:  
![Step 1][3]  
![Step 2][4]  
![3][5] 

## Software

For the software related part, I decided to install the Raspbian lite image from [raspberrypi.org downloads][6]. And then on top of that OpenMediaVault 5 since there are a lot of tutorials on YouTube and it has a nice GUI. [Installation guide][7]

I was up and running after following some instructions in the Radxa Wiki. Ran into some trouble with the Top board hat not displaying any information and RAID because my 4 drives were not detected at first by Open Media Vault and had to set up RAID from terminal- This setup has 4 1Tb HDDs on 2 x RAID 1 configuration &#8211; Just remember that forums are your best friends 😉

Now, you can basically learn everything related to Open Media Vault from [Techno Dad Life][8]. After a few tutorials I had a [piwigo][9] photo gallery up and running, accessible from my local network.

## Hosting over 4G?

For the access through the internet part, the not so fun part: it is a long story, but in a nutshell I have to access the internet using a 4G router because my landlord sucks. Anyway it was a bit of a nightmare, after setting up dynamic DNS on my router with [noip][10] and also setting up OpenVPN (a very nice feature to have on a router if you ask me) I discovered that neither accessing my local network through VPN nor port forwarding were working.

After tedious exchanges with my ISP/carrier (I am using an additional sim card linked to my phone&#8217;s data plan), calls to the store where I got my router from and even TP Link&#8217;s product support; some random hero from my ISP&#8217;s customer support call center said _&#8220;Officially we don&#8217;t support third party routers, that being said I would suggest you change your APN setting to internet.public&#8221;_ and here I am now, a week after following that advice, sharing photos with my family back home through means I trust and manage myself.

I also installed Murmur (The Mumble server component) on the Raspberrypi 4 and was amazed by how easy it was to [set up][11]. My use case is having voice with friends while we play [0 A.D.][12]

Day 4 of <a rel="tag" class="u-tag u-category" href="https://lopeztel.xyz/blog/tags/100daystooffload/">#100DaysToOffload</a>

 [1]: https://fosstodon.org
 [2]: https://wiki.radxa.com/Dual_Quad_SATA_HAT
 [3]: https://fosstodon.b-cdn.net/media_attachments/files/004/780/193/original/91ff9464d0b8e049.png
 [4]: https://fosstodon.b-cdn.net/media_attachments/files/004/832/813/original/eb7791ba7de7e8bf.png
 [5]: https://fosstodon.b-cdn.net/media_attachments/files/005/240/182/original/47756ea4a059218d.jpg
 [6]: https://www.raspberrypi.org/downloads/raspberry-pi-os/
 [7]: https://pcmac.biz/openmediavault-5-on-raspberry-pi/
 [8]: https://www.youtube.com/channel/UCX2Vhc0LIzSS9aMzhGFZ7PA
 [9]: https://piwigo.org/
 [10]: https://www.noip.com/
 [11]: https://pimylifeup.com/raspberry-pi-mumble-server/
 [12]: https://play0ad.com/
