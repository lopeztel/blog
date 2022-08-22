---
title: Linux and Art mix
author: lopeztel
date: 2020-06-25T20:00:09+00:00
url: /2020/06/25/linux-and-art-mix/
tags:
  - Krita
  - Manjaro
  - Watercolor

---
I just felt the need to document my latest Linux issues:

First, some context, I like painting with watercolor and have been doing it for quite a while now but in the last couple of years material such as paper (I used Arches 300g) has become quite expensive, which made me think that perhaps it was about time to go digital.

After buying a cheapo digitizer tablet (a tiny one by the chinese brand Huion) I came across my first issue: doesn&#8217;t work &#8220;out of the box&#8221; on linux (specifically Manjaro); long story short and after some research my eyes came across the DIGImend project, turns out that these guys have been creating drivers for all sorts of digitizer tablets out there and mine was on the list, but it gets better, it is in the AUR! So I was up and running in a matter of minutes.

Now for the software, at the time of writing it has been a week since Krita 4.3.0 was released featuring new watercolor brushes, after looking at some videos on their new YouTube channel I got very interested because Krita can now emulate pretty decent watercolor, so being the impatient FOSS/FLOSS fan that I am, I decided to install the appimage since the official repositories haven&#8217;t been updated (again, at the time of writing).

Last, another problem, I thought it would be fun to record my screen to document the process, either for my reference or for others (no intention of becoming a content creator though), but after realizing that my first attempt to record resulted in a video that flickered and showed my background at times and further investigation, turns out that recordmydesktop, SimpleScreenRecorder and OBS don&#8217;t play well with Gnome3&#8217;s window manager (called Mutter if I&#8217;m not mistaken). The fix was right in the home page for SimpleScreenRecorder: try removing drivers for 2D acceleration or changing window manager.

Removing xf86-video-intel did the trick and everything works fine now. Working Krita + digitizer tablet + recording setup took some effort but perhaps that is also why I am so satisfied and motivated to paint now, let&#8217;s see if my hands can produce some decent artwork, at least testing the watercolor brushes has been fun so far:

![Painting](https://fosstodon.b-cdn.net/media_attachments/files/005/079/139/original/78a04e37927d9925.png#center) 

EDIT: after some months of on and off work, this is the final result:

![Al-Nasir Muhammad complex](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/Al-Nasir_Muhammad_complex4-768x1086-me.jpg#center)

Some links:  
[**Krita 4.3.0 release notes**][1]  
[**Troubleshooting SimpleScreenRecorder**][2]  
[**DIGImend project**][3]

 [1]: https://krita.org/en/item/krita-4-3-0-released/
 [2]: https://www.maartenbaert.be/simplescreenrecorder/troubleshooting/#the-recording-occasionally-flickers-showing-parts-of-the-desktop-background-instead-of-windows
 [3]: https://digimend.github.io/
