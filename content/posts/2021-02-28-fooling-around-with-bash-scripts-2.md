---
title: Fooling around with bash scripts 2
author: lopeztel
date: 2021-02-28T13:20:27+00:00
url: /2021/02/28/fooling-around-with-bash-scripts-2/
tags:
  - 100DaysToOffload
  - Bash
  - Selfhosting
  - Tutorial

---
This is a follow-up to my previous entries: [Fooling around with bash scripts](https://lopeztel.xyz/blog/2021/02/02/fooling-around-with-bash-scripts/), which I used as a base for [Migration from Google Photos to Nextcloud and Piwigo](https://lopeztel.xyz/blog/2021/02/13/migration-from-google-photos-to-nextcloud-and-piwigo/)

Since some of my old picture albums have old .mov, .mpg and .avi videos in them I decided to also convert them to .mp4 (piwigo can show them in the albums), so once again I tried came up with something to automate the task

Dependencies:

  * ffmpeg

Script:

```bash
#!/bin/sh
mkdir -p converted_videos

for file in ./*.MOV ./*.mov ./*.MPG ./*.AVI;  do
    echo "converting file $file ..."
    ffmpeg -i "$file" -vcodec h264 -acodec mp2 "./converted_videos/$file".mp4
done

#Assuming there are mp4 files in the directory move them all to the same directory
mv *.mp4 converted_videos/

echo "Finished video conversion..."
```

I also further tweaked my resize and compress script to do its task for sub directories in the current directory and also call the video converting script:

```bash
#!/bin/sh
for D in *; do
    if &#91; -d "$D" ]; then
        cd "$D"
        echo "Current directory: ${D}"

        mkdir -p compressed_images
        mkdir -p resized_images

        echo "Resizing and compressing pictures"
        for file in ./*.jpg ./*.jpeg ./*.JPG; do
                echo "resizing and optimizing $file ..."
                convert "$file" \
                        -resize '1920x1080&gt;' \
                        "./resized_images/$file"
                jpegoptim "./resized_images/$file" \
                        -d './compressed_images' \
                        --max 65 \
                        --all-progressive \
                        -p
        done
        echo "done!"

        echo "Renaming pictures according to creation date..."
        for i in ./compressed_images/*; do
                jhead -n%Y%m%d-%H%M%S $i
        done
        echo "done!"

        echo "Deleting temporal resized_images directory"
        rm -rf resized_images
        echo "done!"

        echo "converting videos..."
        ~/mov_to_mp4.sh
        echo "done!"

        echo "Deleting temporal converted_videos directory"
        rm -rf converted_videos
        echo "done!"
        cd ../
    fi
done
```

I am using this script to before uploading for all my photo albums and I thought it would be nice to share it

---

Day 53 of my 2020&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!
