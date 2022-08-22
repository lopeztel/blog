---
title: Fooling around with bash scripts
author: lopeztel
date: 2021-02-02T19:13:02+00:00
url: /2021/02/02/fooling-around-with-bash-scripts/
tags:
  - 100DaysToOffload
  - Bash
  - Manjaro
  - Selfhosting
  - yunohost

---
I thought I would share 2 shell scripts that I&#8217;ve found useful, the first one allows me to to use my DSLR camera as a webcam and the second one allows me to batch resize and optimize images

## Using DSLR as webcam {#using-dslr-as-webcam}

This is probably most useful in COVID-19 times, found it a while back on [YouTube](https://www.youtube.com/watch?v=TsuY4o2zLVQ) (not a fan of the big G? [Invidious](https://invidious.site/) or [FreeTube](https://freetubeapp.io/) are your friends), also here&#8217;s a [blog post](https://medium.com/nerdery/dslr-webcam-setup-for-linux-9b6d1b79ae22) for more details.

Dependencies:

  * gphoto2
  * v4l-utils
  * v4l2loopback-dkms
  * ffmpeg

Script:

```
#!/bin/bash
modprobe v4l2loopback exclusive_caps=1 max_buffers=2
gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420 -threads 0 -f v4l2 /dev/video0
```

Leave this running and the device should be accessible from your videoconferencing software of choice or web browser webRTC app

Here&#8217;s a list of [gphoto2&#8217;s supported cameras](http://www.gphoto.org/proj/libgphoto2/support.php)

## Batch resizing, compressing and renaming photos {#batch-resizing-compressing-and-renaming-photos}

For this one there&#8217;s a bit of a background, our story begins when I was trying to get in the [512kbclub](https://512kb.club/). A big part of my blog&#8217;s home page reduction was related to compressing images, at the time a web service was suggested to me: [shortpixel](https://shortpixel.com/online-image-compression) but being the nerd I am I thought:

> &#8220;There should be a way to do this locally on all the pictures in a directory&#8221;
>  
> --me

So I started looking online and did a bit pf patchwork to create a bash script that served this purpose.

I also plan to use this for my Nextcloud instance in my Yunohost install because I&#8217;ve noticed that I&#8217;ve been trying to share links to albums full of DSLR ORIGINAL SIZED IMAGES \*facepalms\* (something ridiculous like 5000 pixels wide). No wonder my folks kept complaining that their cellphones took forever to load the images&#8230;

Dependencies:

  * imagemagick
  * jpegoptim
  * jhead

Script:

```
#!/bin/sh
mkdir -p compressed_images
mkdir -p resized_images

for file in ./*.jpg ./*.jpeg ./*.JPG; do
         echo "resizing and optimizing $file ..."
         convert "$file" \
                 -resize '1900x1080&gt;' \
                 "./resized_images/$file"
         jpegoptim "./resized_images/$file" \
                 -d './compressed_images' \
                 --max 65 \
                 --all-progressive \
                 -p
         done

echo "renaming pictures according to creation date..."
for i in ./compressed_images/*; do
    jhead -n%Y%m%d-%H%M%S $i
done

echo "deleting transient directory..."
rm -rf resized_images
echo "done!"
```

The process is quite simple, navigate to a directory containing the pictures, run the script and it will generate a directory with the compressed pictures, I&#8217;m also renaming them from creation date contained in exif data for organization purposes.

This is actually the first time I try to make one of these so I may have some errors or possible improvements, all constructive feedback is welcome.

---

Day 47 of my 2020&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
