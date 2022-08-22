---
title: My blogging journey so far
author: lopeztel
date: 2021-01-21T20:29:13+00:00
url: /2021/01/21/my-blogging-journey-so-far/
tags:
  - 100DaysToOffload
  - Selfhosting
  - Wordpress

---
I remember I started a blog in March last year (2019), it was a simple [Writefreely](https://writefreely.org/) blog,it looked like this:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/Screenshot_2021-01-21-Lopeztels-Blog-768x676-me.png#center)

It was simple, easy to use and you could follow it via mastodon, I wanted to engage a bit more with my fellow fedizens so I started looking into [Wordpress](https://wordpress.org/) around the same time I built my [Raspberrypi 4 server](https://lopeztel.xyz/blog/2020/08/08/my-raspberry-pi-server-with-yunohost/) and I found out that Wordpress was an app Yunohost had listed (it's basically a one-click install but you require a dedicated domain) so I [migrated](https://lopeztel.xyz/blog/2020/08/16/migration-to-wordpress/), at the time that was easy, just downloaded all my previous posts in markdown and added them one by one (there weren't that many). I also discovered the [Indieweb](https://indieweb.org/) and installed a bunch of plugins and theme to make the blog Indieweb compliant, the result was something like this:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/Blog_autonomie-768x1208-me.png#center)

But it&#8217;s been a while and I really don&#8217;t use the Indieweb plugins that much except for a couple. A few days ago when I was going through my RSS feeds, I stumbled upon [Kev Quirk](https://fosstodon.org/@kev)'s [512kb club](https://512kb.club/) (first rule of the 512kb club is you talk about it as much as you can with everyone ;P ), which was a good excuse to make my blog _&#8220;lighter&#8221;_ so I gave myself a challenge: 

>   Let&#8217;s try to make my blog as light as possible with just plugins, no deep diving into PHP, CSS or HTML &#8220;
>
> -- <cite>Silly me</cite>

This is how the blog looks now:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/Blog_2021-768x960-me.png#center)

I can say I barely managed to get in the 512kb club blue team but it was fun! Here&#8217;s the badge of honor (also attached to the blog&#8217;s footer)

![](https://512kb.club/images/blue-team.svg#center)

# My current setup {#my-current-setup}

This is my current setup, I tried to keep the minimum necessary:

  * WordPress theme: [Twenty Twenty-One][1]
  * ActivityPub
  * Akismet Anti-Spam
  * Companion Auto Update
  * Disable Emojis (GDPR friendly)
  * GTmetrix for WordPress
  * Insert Headers and Footers
  * Semantic-Linkbacks
  * Simple LDAP Login
  * Simple Local Avatars
  * Smush
  * Syndication Links
  * Webmention
  * WP Fail2Ban Redux
  * WP Reset

# Static site in the future?

In the future I may try to create a static website with Jekyll or Hugo, self hosting would be a bit of a challenge because all my setup has been automated by Yunohost&#8230; let&#8217;s call that a weekend/side project. I am a complete noob, so it will probably take some time

---

Day 43 of my 2020&#8217;s [#100DaysToOffload](https://lopeztel.xyz/blog/tags/100daystooffload/)

Join [100DaysToOffload](https://100daystooffload.com/)!

 [1]: https://wordpress.org/themes/twentytwentyone/
