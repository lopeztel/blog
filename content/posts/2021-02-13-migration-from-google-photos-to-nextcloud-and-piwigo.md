---
title: Migration from Google Photos to Nextcloud and Piwigo
author: lopeztel
date: 2021-02-13T18:59:15+00:00
url: /2021/02/13/migration-from-google-photos-to-nextcloud-and-piwigo/
tags:
  - 100DaysToOffload
  - Nextcloud
  - piwigo
  - Raspberrypi
  - Tutorial
  - yunohost

---
## Getting the pictures {#getting-the-pictures}

First step was downloading all my pictures from Google Photos, so I went to [Google Takeout](https://takeout.google.com) which is the way to download all the data the big G has on you. For now I selected only the data in Google Photos but if I had wanted to I could&#8217;ve downloaded potentially everything.

Just for context I didn&#8217;t have that much in Google Photos, around 45 GB total, not bad

## Organization {#organization}

One of the most common comments I get from family and friends is that photos in either web browser through Nextcloud shared links or piwigo app take forever to load, and it makes sense, full sized glory usually meant 17+ MB per file&#8230; _*me mentally kicking myself_

While I will be backing up my pictures in all their full sized glory, I decided to also create a compressed images directory for easy sharing through Nextcloud shared links and/or faster viewing in my Piwigo private, password protected photo albums both on a web browser and the mobile app.

The setup is a bit weird, while my [raspberry pi yunohost server](https://lopeztel.xyz/blog/2020/08/08/my-raspberry-pi-server-with-yunohost/) boots up from an external HDD, and every Yunohost app lives there, I want most of the content to live in one of my 2 x 1 TB RAID1 arrays (All in all I have 5 SATA drives attached to the raspberry pi server).

Nextcloud was not a problem since it supports adding external drives, it lists my 2 RAID arrays as storage0 and storage1, however the other apps were tricker to setup. Both Piwigo and WordPress don&#8217;t play well with external drives or they don&#8217;t offer any option to use them so, teaks&#8230;

Without going too much into details, most of the Yunohost apps follow the path `/var/www/<app_name>` so I created symlinks pointing to directories in either storage0 or storage1 for both Piwigo and WordPress, that way I get to see the files in Nextcloud as well. Piwigo example:

I navigated to `/var/www/piwigo/galleries`, which is the directory scanned to add new albums in Piwigo, then I did:

```
sudo ln -s /home/admin/media/storage0/Photos/<insert_album_name> <insert_album_name>
```

Which basically follows the convention `sudo ln -s <existing_directory> <name_of_the_link>`

I haven&#8217;t found a way to batch process this, I could set up a single symlink to a single directory in storage0 and make piwigo scan subfolders but that would make a huge album with a ton of sub-albums, so I do this on a directory by directory basis by hand

## Compression {#compression}

The approach is simple, just run my compression script recursively through all the album directories

### Compression comparison: Shortpixel vs my own script {#compression-comparison-shortpixel-vs-my-own-script}

As I mentioned before, I was using [Shortpixel](https://shortpixel.com/online-image-compression) to compress pictures for my blog posts, but my entire photo library is a pretty big task more suited to do in batches, which is why decided to use my own [optimization/resizing script](https://lopeztel.xyz/blog/2021/02/02/fooling-around-with-bash-scripts/).

Just for fun and because [@ndanes](https://fosstodon.org/@ndanes) gave me the idea I thought I would compare both approaches:

| Original Image size | compressed with script | shortpixel lossy | shortpixel glossy | shortpixel lossless |
| ------------------- | ---------------------- | ---------------- | ----------------- | ------------------- |
| 17.7 MB             | 186.2 KB               | 64 KB            | 124 KB            | 149.6 KB            |

Only did one picture but this gives a rough idea

Wanna see the files? here&#8217;s a [link](https://lopeztel.noho.st/nextcloud/s/37ELj2ss6CSrdmY) (WordPress may compress images further and this is the only way I found to share the image results)

Some findings:

  * I discovered shortpixel has a limit of 18.27MB per file
  * Lossy and Glossy compression seem to mess with the white balance, slightly bluer. Which is a no no if you&#8217;re trying to show that perfect picture you took your sweet time taking and editing
  * Script is slightly less efficient but retains a high level of quality and doesn&#8217;t mess with white balance, results are comparable to lossless compression in short pixel

Given the fact that I am going to be processing loads of files the script way seems good enough, plus it is also possible to tweak further to shave off some KBs but I&#8217;m quite happy with the current results.

## Upload {#upload}

Now, this is probably the most inefficient step of the process because I haven&#8217;t been able to find the time to setup FTP access to my files to I usually did access to Nextcloud (which in turns writes/reads to/from storage0 and storage1) by webdav or web interface to upload/download files. Needless to say this is a time consuming task

That&#8217;s it, questions, comments, improvements? (I&#8217;d really appreciate those)

---

Day 49 of my 2020&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
