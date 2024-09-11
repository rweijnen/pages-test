---
id: 3702
title: 'Enable Developer mode on VW Discover Pro with VCDS'
date: '2016-05-17T22:46:21+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3702'
permalink: /2016/05/17/enable-developer-mode-on-vw-discover-pro-with-vcds/
views:
    - '8983'
short-url:
    - 'http://bit.ly/1YzQf8v'
    - 'http://bit.ly/1YzQf8v'
categories:
    - Automotive
tags:
    - Adaptation
    - DiscoverPro
    - VCDS
    - VW
---

If you try to enable “Developer mode” on the VW Discover Pro navigation with VCDS you will get the following error: “Request out of range:”

[![VCDS | Discover Pro | Adaptation | Developer Mode | Request out of range](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-5.png "VCDS release 15.7.4")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-5.png)

This happens because VCDS uses a type 0x02 (Programming) session but this adaptation needs type 0x4F (Developer) session.

Ross-Tech has just released a new [Beta Version 16.5.0](http://www.ross-tech.com/vcds/download/beta/current.php) that enables a switch to Developer session.

[![image](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-6.png)

Here’s how it works: Go to Control Module 5F-Information Electronics and click Security Access, then use code S12345:

[![Secuirty Access S12345](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-7.png "VCDS Beta 16.5.0")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-7.png)

Select Channel “Developer mode” and set New value to “activated”:

[![Developer Mode](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-8.png "VCDS 16.5.0")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-8.png)

Click “Do it” and you should get “Controller accepted the request”:

[![Controller accepted the request. Will now read the channel again](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-9.png "VCDS")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-9.png)

Followed by success!

[![Developer Mode|Activated](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-10.png "VCDS Beta 16.5.0")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-10.png)