---
title: My Raspberry Pi server with yunohost
author: lopeztel
date: 2020-08-08T20:37:35+00:00
url: /2020/08/08/my-raspberry-pi-server-with-yunohost/
tags:
  - 100DaysToOffload
  - Raspberrypi
  - Selfhosting
  - yunohost

---
I will try to make this entry resemble a tutorial, hopefully this ends up being useful to someone.

## Why start over?

I had been playing around with Open Media Vault before (see this [entry][1] for details), but it was mainly based on docker images and I could never get Nextcloud up and running (the closest I came was installing [nextcloudpi][2], but I could never get the docker image to run using external storage).

Also I never got to the part of using [letsencrypt][3] to access everything through https, pretty sure there&#8217;s a tutorial out there somewhere.

Then tragedy happened, for some reason my SD card just died and I was left with nothing.

After that I decided that my setup needed some improvements, and to be completely honest I also wanted to try something different, in yunohost almost everything is a one-click step when it comes to installing apps so that was a huge point in favor, here I am telling you my story:

## Initial setup

After the SD card incident, booting from an external drive seemed like the best option. There are 2 prerequisites for that to happen:

  1. update the pi&#8217;s bootloader in eeprom (detailed steps [here][4], see section: _Update the bootloader_)
  2. burn the OS image to the external drive, and use your favorite file browser to copy & replace some files to the `boot` partition (no need to do this if you&#8217;re using 64 bit Raspbian OS). This is the list of files that need to be replaces (just get them from the [repo][5]):
      * fixup.dat
      * fixup4.dat
      * fixup4cd.dat
      * fixup4db.dat
      * fixup4x.dat
      * fixup_cd.dat
      * fixup_db.dat
      * fixup_x.dat
      * start.elf
      * start4.elf
      * start4cd.elf
      * start4db.elf
      * start4x.elf
      * start_cd.elf
      * start_db.elf
      * start_x.elf

In my particular case the base was Raspberry Pi OS lite, I know, you may be thinking: _&#8220;Aren&#8217;t there yunohost images already out there? save yourself some trouble!&#8221;_ and my answer is documented [here][6].  
Then after updating, it&#8217;s possible to [install][7] yunohost (4.0.3 at the time of writing) with a curl one liner: `curl https://install.yunohost.org | bash`

Post intsall steps are pretty straightforward, here&#8217;s a [link][8].

I ended up using a noho.st domain as it was graciously offered for free.

## My multi-HDD setup

**Note: What this section describes has to be done after yunohost is up and running in the system, I tried doing it the other way around and that caused my SATA HAT to stop working altogether, no HDDs were detected, fan stopped spinning and the little LCD display showed nothing**

A while back I got an awesome RADXA Quad-SATA HAT that houses 4x1TB HDDs. It is not detected or working out of the box but the installation process is detailed [here][9]. So my system has 5 HDDs (4 in the SATA-HAT and the one I boot from connected one of the pi&#8217;s USB ports).

I also wanted to have some sense of security for my data so naturally I set up 2x RAID1 arrays (`mdadm` is your best friend, here&#8217;s a useful [guide][10])

All this sounds very nice, doesn&#8217;t it? There&#8217;s always a catch: since the external drives are on the SATA HAT it is not possible to just edit `fstab` and auto-mount them on the system, there is a service associated with the SATA HAT that needs to be running to &#8220;see&#8221; the drives. My hacky solution for now is [mounting the drives][11] everytime I restart the system (yes, by hand, like a caveman).

## App installing and configuration

Installing apps was surprisingly easy, [Gitea][12], [WordPress][13], [Nextcloud][14] and [Piwigo][15] are one clic installs except for [Synapse][16], the fix is just installing from testing branch: `sudo yunohost app install https://github.com/YunoHost-Apps/synapse_ynh/tree/testing --debug`

Since the apps are installed in the boot drive I decided to move or reference content to/from the other drives. My Nextcloud instance has access to the external storage so I&#8217;m able to &#8220;upload&#8221; pictures to my external drives. then I just made symlinks from `/home/yunohost.app/piwigo/.galleries` to reference the album directories in the external drives. Here&#8217;s an useful [link][17], also, hopefully you don&#8217;t screw up like me but here&#8217;s a [link][18] on how to remove symlinks. Hacky solution, yes, but it works.

So far my idea is to &#8220;upload&#8221; content to external storage through Nextcloud and follow the same approach (for blogposts, videos, etc …)

I am also testing communication through my Synapse instance and the Nextcloud Talk plugin with my folks back home.

## Improvements

Because what kind of engineer would I be if I was satisfied? Improvements:

  * I recently bought a domain from Namecheap, so I&#8217;ll be using that for my blog in the future
  * Deciding between Writefreely or WordPress to migrate this blog (writefreely has federation and I don&#8217;t know if the [activity pub plugin][19] for wordpress is mature enough)
  * There is a problem with outgoing port 25 so my self-hosted email address can receive but not send any emails and there&#8217;s an issue with reverse DNS so basically even if I could send them they&#8217;d be rejected or best case sent to spam. Apparently this is a router issue, my ISP has replied that they don&#8217;t block anything on their side
  * Deciding if I move my inner circle from Signal to Element (through my Synapse instance) or just use Nextcloud Talk

Well, that&#8217;s all, any suggestions are greatly appreciated.

Day 10 of <a rel="tag" class="u-tag u-category" href="https://lopeztel.xyz/blog/tags/100daystooffload/">#100DaysToOffload</a>

 [1]: https://lopeztel.xyz/blog/2020/07/07/the-journey-to-self-hosting-on-a-raspberrypi-4/
 [2]: https://ownyourbits.com/nextcloudpi/#content_start
 [3]: https://letsencrypt.org/
 [4]: https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md#usbmassstorageboot
 [5]: https://github.com/raspberrypi/firmware/tree/master/boot
 [6]: https://lopeztel.xyz/blog/2020/08/06/thank-you-yunohost/
 [7]: https://yunohost.org/#/install_manually
 [8]: https://yunohost.org/#/postinstall
 [9]: https://wiki.radxa.com/Dual_Quad_SATA_HAT
 [10]: https://www.digitalocean.com/community/tutorials/how-to-create-raid-arrays-with-mdadm-on-ubuntu-16-04#creating-a-raid-1-array
 [11]: https://yunohost.org/#/external_storage
 [12]: https://gitea.io/en-us/
 [13]: https://wordpress.org/
 [14]: https://nextcloud.com/
 [15]: https://piwigo.org/
 [16]: https://matrix.org/docs/projects/server/synapse
 [17]: https://hackernoon.com/symbolic-links-did-not-work-as-expected-6a3af628da53
 [18]: https://linuxize.com/post/how-to-remove-symbolic-links-in-linux/#remove-symbolic-links-with-unlink
 [19]: https://wordpress.org/plugins/activitypub/
