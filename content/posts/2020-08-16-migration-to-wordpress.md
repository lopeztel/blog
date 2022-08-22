---
title: Migration to WordPress
author: lopeztel
date: 2020-08-16T11:15:25+00:00
url: /2020/08/16/migration-to-wordpress/
tags:
  - 100DaysToOffload
  - Federation
  - Indieweb
  - Selfhosting
  - Wordpress
  - yunohost

---
In recent days I&#8217;ve been busy trying to get my blog up and running on my cheapo [Raspberry Pi + Yunohost server setup][1]. The process was fun, so here&#8217;s a blog entry about migrating my blog (blogception?) from a writefreely instance to my self hosted wordpress instance.

## Why though?

To put it simply: because I can.

On a more serious note, my previous blog was hosted at [qua.name][2] and there was nothing wrong with it, but I really wanted to be able to have a bit more interaction,  easy theming, and an overall a plug and play solution with a GUI. Now, theming is possible in writefreely instances with a bit of CSS magic but I wanted to stay away from that, maybe in the future&#8230; As for the interaction side of things, the ol&#8217; leave a comment kind of thing is good but there are a lot more possibilities.

One of the advantages of writefreely is the built in federation support, and finding out that there is an [ActivityPub][3] plugin (in beta) for WordPress was interesting and another point in favor of the change.

## My setup

The first step was buying the cheapest domain name I could find on namecheap.com (after all this is a budget server setup), and if I remember correctly, the deal was $8.5 for 2 years.

Then, obtaining a SSL certificate from [Let&#8217;s Encrypt][4] and the WordPress app installation through yunohost was straightforward through the admin GUI.

Importing posts as plain text files from my writefreely instance was straightforward as well, luckily WordPress seems to support Markdown in its block editor, so it was a matter of copy and paste, updating links to the old blog posts and changing the published date. Also, I had less than 15 posts so&#8230; not a massive effort.

## Federation and Indieweb

I installed the ActivityPub plugin and began testing, had a little trouble with the entire posts being dumped as a single toot in the timeline, but that was fixed tweaking the settings:
![](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-08-16-12-20-17-768x386.png)

Which makes it possible to have a neat timeline like this:

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/Screenshot-from-2020-08-16-12-16-35-e1597573445687.png#center)

While testing this, another concept came to light: [Indieweb][5], I opened another can of worms, so of course I liked the idea and I had to make my blog Indieweb compliant by installing the corresponding plugins.

Here&#8217;s a list of the active plugins on my blog:

  * ActivityPub
  * Akismet Anti-Spam
  * Classic Editor (this one is important, post kinds and micropub need it, new block editor is not fully compatible)
  * Companion Auto Update
  * IndieAuth
  * IndieWeb (this is one of the first ones to install, it provides how to get started information and provides a list of other relevant plugins)
  * Micropub
  * Microformats 2
  * Post Kinds
  * Semantic-Linkbacks
  * Simple LDAP Login
  * Simple Local Avatars (To avoid using Gravatar)
  * Syndication Links
  * Webmention
  * WebSub/PubSubHubbub
  * WP Fail2Ban Redux
  * Yarns Microsub Server

## Last details

Apparently the theme I am using doesn&#8217;t display my [h-card][6] correctly through the available widget so after reading [Kev Quirk&#8217;s post][7], my h-card was manually added by editing the footer .php file, this is nice because it&#8217;s just sitting there not changing the appearance of the blog.

This is also efficient because my yunohost installation defaults the wordpress user email to the selfhosted email, and that is not working at the moment (content for another blog post maybe)

The test in [indiewebifyme][8] displays it correctly:

![](https://lopeztel.noho.st/piwigo/_data/i/galleries/blog_media/h-card-me.png#center)

Also I discovered that the default settings of the Webmentions plugin make it so that every kind of interaction is displayed as an avatar and not under the comments section so I just changed this:

![](https://lopeztel.noho.st/piwigo/galleries/blog_media/webmentions-300x289.bk.png#center)

The results are shown in this [post][9]

There are some more details related to the plugins and using [bridgy][10] to link your social media interactions to posts, but this pretty much covers the basics and having a look into the plugin settings gives a better idea on how they work.

Kudos to [Ricardo][11], [hs0ucy][12], [pixelroiber][13], and [Robby][14] for helping me test webmentions and federation on my previous dummy site and/or the current one.

Next steps will be looking into optimization and such, for now I think the Lazyload plugin messes up my h-card somehow.

Day 12 of <a class="u-tag u-category" href="https://lopeztel.xyz/blog/tags/100daystooffload/" rel="tag">#100DaysToOffload</a>

&nbsp;

 [1]: https://lopeztel.xyz/blog/2020/08/08/my-raspberry-pi-server-with-yunohost/
 [2]: https://qua.name
 [3]: https://wordpress.org/plugins/activitypub/
 [4]: https://letsencrypt.org/
 [5]: https://indieweb.org/
 [6]: https://indieweb.org/h-card
 [7]: https://kevq.uk/how-to-create-an-indieweb-profile/
 [8]: https://indiewebify.me
 [9]: https://lopeztel.xyz/blog/2020/03/02/into-the-fediverse/
 [10]: https://brid.gy/
 [11]: https://mstdn.social/@rmdes
 [12]: https://mastodon.sdf.org/@hs0ucy
 [13]: https://mastodon.social/@pixelroiber
 [14]: https://fosstodon.org/@Zambyte
