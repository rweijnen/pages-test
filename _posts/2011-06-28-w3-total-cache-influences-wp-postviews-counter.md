---
id: 1955
title: 'W3 Total Cache influences WP-PostViews counter'
date: '2011-06-28T21:14:10+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/06/28/w3-total-cache-influences-wp-postviews-counter/'
permalink: /2011/06/28/w3-total-cache-influences-wp-postviews-counter/
views:
    - '3439'
short-url:
    - 'http://bit.ly/lwL5MF'
categories:
    - General
tags:
    - W3TotalCache
    - Wordpress
    - WP-PostViews
---

I have used the [WP-Postviews](http://lesterchan.net/portfolio/programming/php/) WordPress plugin for a while to keep track of the number of times my blog posts are being read.

But after installing the [W3 Total Cache](http://www.w3-edge.com/wordpress-plugins/w3-total-cache/) plugin I noticed that the read counters weren’t properly updated anymore. I figured this was a consequence of using a cache.

But today I played around a little and it seems that the counter are not correctly updated when using the “Disk (enhanced)” Page Cache. But when I switched it to “Disk (basic)” the counters seems to be correctly updated again.

[![Page Cache Method | Disk (basic) | Disk (enhanced)](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb33.png "W3 Total Cache")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image33.png)

Don’t forget to empty the cache after changing this setting!