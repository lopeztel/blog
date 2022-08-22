---
title: Saving my /home partition with fsck
author: lopeztel
date: 2020-10-12T18:17:14+00:00
url: /2020/10/12/saving-my-home-partition-with-fsck/
tags:
  - 100DaysToOffload
  - Manjaro

---
Let&#8217;s face it, part of the joy of Linux is solving unexpected problems you wouldn&#8217;t have with proprietary Operating Systems like Windows and MacOS. That being said this is my latest adventure:

I turned on m computer and after the grub screen came up and I chose to boot into Manjaro (yes, the horror, I know dual booting Windows is not ideal but a necessary evil in my case). Something like this came up instead of the usual GUI login prompt:

```shell
[FAILED] Failed to start File System &lt;random UUID for my home partition>
[FAILED] Failed to mount /home.
[DEPEND] Dependency failed for Local File Systems.
You are in emergency mode. After logging in, type “journalctl -xb” to view
system logs, “systemctl reboot” to reboot, “systemctl default” or “exit”
to boot into default mode.
```

After the usual _&#8220;Well, crap&#8221;_ moment I decided to search the forums and my initial approach was to boot from a live usb and then to follow this [thread](https://forum.manjaro.org/t/dependency-failed-for-home-and-depedency-failed-for-local-file-systems/25064) but `manjaro-chroot -a` didn&#8217;t seem to work, the terminal propmted to mount Manjaro as option 0 but that didn&#8217;t help (more like I didn&#8217;t really know how to use this). So I attempted something else I found in this [other thread](https://forum.manjaro.org/t/manjaro-no-longer-boots-all-of-a-sudden/12048/11):

`fsck /dev/sda8` (my home partition lives in sda8)

and after following the prompts to repair errors and optimize (said yes to all):

```
~ >>> sudo fsck /dev/sda8                                                                          [32]
fsck from util-linux 2.36
e2fsck 1.45.6 (20-Mar-2020)
/dev/sda8 contains a file system with errors, check forced.
Pass 1: Checking inodes, blocks, and sizes
Inode 1179940 extent tree (at level 1) could be shorter.  Optimize<y>? yes
Inodes that were part of a corrupted orphan linked list found.  Fix<y>? yes
Inode 1180329 was part of the orphaned inode list.  FIXED.
Deleted inode 1185883 has zero dtime.  Fix<y>? yes
Inode 1194474 was part of the orphaned inode list.  FIXED.
Inode 1197451 was part of the orphaned inode list.  FIXED.
Inode 1214730 was part of the orphaned inode list.  FIXED.
Inode 1214937 was part of the orphaned inode list.  FIXED.
Inode 1245329 was part of the orphaned inode list.  FIXED.
Inode 1443168 extent tree (at level 1) could be shorter.  Optimize<y>? yes
Inode 2101262 extent tree (at level 1) could be shorter.  Optimize<y>? yes
Pass 1E: Optimizing extent trees
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
Block bitmap differences:  -4728365 -(5252576--5252593) -(5252832--5252840) -(7281205--7281213) -(28186624--28187647) -(28435982--28435990)
Fix<y>? yes
Free blocks count wrong for group #144 (280, counted=281).
Fix<y>? yes
Free blocks count wrong for group #160 (7611, counted=7638).
Fix<y>? yes
Free blocks count wrong for group #222 (16898, counted=16907).
Fix<y>? yes
Free blocks count wrong for group #860 (5047, counted=6071).
Fix ('a' enables 'yes' to all) <y>? yes to all
Free blocks count wrong for group #867 (6842, counted=6851).
Fix? yes
Free blocks count wrong (1960760, counted=1961830).
Fix? yes
Inode bitmap differences:  -1180329 -1185883 -1194474 -1197451 -1214730 -1214937 -1245329
Fix? yes
Free inodes count wrong for group #144 (0, counted=2).
Fix? yes
Free inodes count wrong for group #145 (0, counted=1).
Fix? yes
Free inodes count wrong for group #146 (8, counted=9).
Fix? yes
Free inodes count wrong for group #148 (1750, counted=1752).
Fix? yes
Free inodes count wrong for group #152 (6483, counted=6484).
Fix? yes
Directories count wrong for group #152 (1042, counted=1041).
Fix? yes
Free inodes count wrong (6279928, counted=6279935).
Fix? yes
/dev/sda8: *** FILE SYSTEM WAS MODIFIED ***
/dev/sda8: 1936641/8216576 files (0.4% non-contiguous), 30896794/32858624 blocks
~ >>>                                                                                               [1]
```

Then after turning off and on my laptop everything went back to normal. In hindsight maybe I could&#8217;ve done that from the terminal I was booting into but it doesn&#8217;t matter now.

Useful reference for `fsck`:

```
 lopeztel@lopeztel-inspiron137359  ~  fsck --help

Usage:
 fsck [options] -- [fs-options] [<filesystem> ...]

Check and repair a Linux filesystem.

Options:
 -A         check all filesystems
 -C [<fd>]  display progress bar; file descriptor is for GUIs
 -l         lock the device to guarantee exclusive access
 -M         do not check mounted filesystems
 -N         do not execute, just show what would be done
 -P         check filesystems in parallel, including root
 -R         skip root filesystem; useful only with '-A'
 -r [<fd>]  report statistics for each device checked;
            file descriptor is for GUIs
 -s         serialize the checking operations
 -T         do not show the title on startup
 -t <type>  specify filesystem types to be checked;
            <type> is allowed to be a comma-separated list
 -V         explain what is being done

 -?, --help     display this help
     --version  display version

See the specific fsck.* commands for available fs-options.
For more details see fsck(8).

```

As I said, there are unexpected problems but there&#8217;s a sense of achievement when you solve them and learn something in the process.

Day 20 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
