---
title: Optimizing my yunohost wordpress blog
author: lopeztel
date: 2020-08-22T10:07:13+00:00
url: /2020/08/22/optimizing-my-yunohost-wordpress-blog/
tags:
  - 100DaysToOffload
  - Raspberrypi
  - Selfhosting
  - Wordpress
  - yunohost

---
After my last update about my [blog
migration](https://lopeztel.xyz/blog/2020/08/16/migration-to-wordpress), basically
all I did was install [LiteSpeed Cache
plugin](https://wordpress.org/plugins/litespeed-cache/) and mess around with
it.

For reference, lots of documentation goodness [here](https://docs.litespeedtech.com/)

The most important part to note is that I kept getting bugged by both
Google&#8217;s PageSpeed Insights and GTmetrix to enable text compression.

After some time reading about compression and [Marco Saric&#8217;s
blogpost](https://markosaric.com/speed-up-wordpress/), I tried to enable
compression by adding some lines of code at the bottom of the .htaccess file
(accessible by the plugin through: LiteSpeed Cache->Toolbox->Edit .htaccess):

```
<ifModule mod_gzip.c>
mod_gzip_on Yes
mod_gzip_dechunk Yes
mod_gzip_item_include file .(html?|txt|css|js|php|pl)$
mod_gzip_item_include handler ^cgi-script$
mod_gzip_item_include mime ^text/.*
mod_gzip_item_include mime ^application/x-javascript.*
mod_gzip_item_exclude mime ^image/.*
mod_gzip_item_exclude rspheader ^Content-Encoding:.*gzip.*
</ifModule>
```

This didn&#8217;t work and realized that gzip doesn&#8217;t come enabled by
default (most things were commented out) in yunohost&#8217;s nginx
installation. So I referred to this
[link](https://varvy.com/pagespeed/enable-compression.html) and edited the file
_/etc/nginx/nginx.conf_ :

```
gzip on;

add_header Content-Encoding "gzip2";
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_buffers 16 8k;
gzip_http_version 1.1;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/x-javascript
```

The line 

`add_header Content-Encoding "gzip2";`

is the most important, after that my blog was tested for compression using:
[https://www.giftofspeed.com/gzip-test/](https://www.giftofspeed.com/gzip-test/)
and [https://varvy.com/tools/gzip/](https://varvy.com/tools/gzip/), both test
websites see compression enabled.

I&#8217;m still not sure about [Google&#8217;s PageSpeed
Insights](https://developers.google.com/speed/pagespeed/insights/) but
[GTmetrix](https://gtmetrix.com/) sees it as well. Here are my results (for
now):

![Google&#8217;s PageSpeed Insights](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-08-22-11-56-47-768x504.png#center)

![Gtmetrix](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-08-22-11-57-16-768x481.png#center)

Just for fun (haven&#8217;t looked too much into this yet) I am adding the
score for [Website carbon](https://www.websitecarbon.com/) as well:

![Website carbon](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-08-22-12-01-09-768x511.png#center)

So far I am more or less happy with the results and there is room for
improvement. Some settings seem to break my Indieweb and ActivityPub plugins.

Day 15 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
