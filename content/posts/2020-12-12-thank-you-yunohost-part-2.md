---
title: Thank you Yunohost (Part 2)
author: lopeztel
date: 2020-12-12T09:52:12+00:00
url: /2020/12/12/thank-you-yunohost-part-2/
tags:
  - 100DaysToOffload
  - Selfhosting
  - yunohost

---
This is a bit of a continuation on my previous [thank you post](https://lopeztel.xyz/blog/2020/08/06/thank-you-yunohost/), since I received a lot of help and it deserves proper credit, this post is to be taken as part thank you note and part summary of the whole SSL certificate issue ordeal:

First things first, in short Yunohost is an easy solution to get started with self hosting without dealing with the more technical side of things, it&#8217;s easy to install apps and I&#8217;ve been running a [yunohost server on my raspberry pi 4](https://lopeztel.xyz/blog/2020/08/08/my-raspberry-pi-server-with-yunohost/) for months now.

The problem was that my SSL certificate from <a href="https://letsencrypt.org/" target="_blank" rel="noreferrer noopener">Let&#8217;s Encrypt</a> expired at the beginning of November and I wasn&#8217;t able to renew it. Yunohost provides a way to renew or install SSL certificates via CLI or admin webpage, both methods were failing: **SSL Certificates Expired: Challenge did not pass for xmpp-upload.maindomain.tld**

The problem is very well documented [here](https://forum.yunohost.org/t/ssl-certificates-expired-challenge-did-not-pass-for-xmpp-upload-maindomain-tld/13508) if it is of any interest 😛

Yesterday I was thinking of using the nuclear option: nuking the whole server installation and starting over from scratch (as humanity should), I voiced my opinion on Mastodon but then [@yunohost](https://mastodon.social/@yunohost) offered some help and after some intensive ~~debugging~~ copy-pasting commands prompted by some hero (Aleks, if you&#8217;re reading this you&#8217;re awesome) on the [yunohost support matrix room](https://app.element.io/#/room/#freenode_#yunohost:matrix.org) we were able to find the problem:

Turns out that my &#8220;blog optimization&#8221; (documented [here](https://lopeztel.xyz/blog/2020/08/22/optimizing-my-yunohost-wordpress-blog/)), in particular this code block added to `nginx.conf`:

```
gzip on;

add_header Content-Encoding "gzip2";
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_buffers 16 8k;
gzip_http_version 1.1;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript
```

kind of broke the whole setup to automatically renew the SSL certificate. This is quite funny because they provide a way to regenerate `nginx.conf`, I incorrectly assumed `yunohost tools regen-conf nginx --force` would fix this but then again, I didn&#8217;t really check so… \*facepalm\*

The problem was solved by adding `gzip off;` to `/etc/nginx/conf.d/acme-challenge.conf.inc`:

```
{
        default_type "text/plain";
        alias /tmp/acme-challenge-public/;
        gzip off;
}
```

this fix will be part of a future release of yunohost, 4.1 if I&#8217;m not mistaken (see this [commit](https://github.com/YunoHost/yunohost/commit/c5d06af20e97e5939bb338ca81e6739e32ae943d) to their github repo) to prevent others from running into the same problem 😀

This has been a bit frustrating but a learning experience, I can&#8217;t thank you enough [@yunohost](https://mastodon.social/@yunohost)!

Day 36 of [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)
