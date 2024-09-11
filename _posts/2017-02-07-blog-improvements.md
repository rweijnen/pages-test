---
id: 3764
title: 'Blog improvements'
date: '2017-02-07T15:56:21+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=3764'
permalink: /2017/02/07/blog-improvements/
views:
    - '237'
categories:
    - General
    - Uncategorized
tags:
    - NetScaler
    - NGINX
    - Optimize
    - PageSpeed
    - Security
    - SSL
    - Wordpress
---

When I started this blog in 2007 (wow that’s almost 10 years ago) I went for a cheap web hoster with a reasonable performance to host it.

In the beginning performance was acceptable but over the years it has degraded and of course user experience standards have changed.

I decided it was time to do something about it so I’ve moved the blog from a shared platform to my own server.

This server is running on optimized flash storage where most writes are DeDuplicated and never actually hits the flash disks:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image.png)

It was a fun experience to optimize the webserver (NGINX) and WordPress for performance resulting in a PageSpeed index of 93:

[![www.remkoweijnen.nl pagespeed 93/100](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb-1.png "Page Speed Insights")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image-1.png)

For the people with eye for detail: there are still improvements to be made for Mobile.

I also decided to implement SSL and move over to SSL only fronted by Citrix Netscaler (with http to https redirection).

[![SSL Labs Overall Rating A+](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb-2.png "SSL Report www.remkoweijnen.nl")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image-2.png)

I had a lot of fun, frustations and learning on this journey so I hope this makes your stay at this blog more pleasant.

I am planning a write up to share these experience but the work isn’t finished yet so you may notice some outtages as I continue to optimize and improve!