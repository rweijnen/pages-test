---
id: 2574
title: 'Cookie Error on Citrix XenApp'
date: '2012-03-29T10:02:10+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/29/cookie-error-on-citrix-xenapp/'
permalink: /2012/03/29/cookie-error-on-citrix-xenapp/
views:
    - '1506'
short-url:
    - 'http://bit.ly/Hq8567'
categories:
    - Citrix
tags:
    - Citrix
    - Cookies
    - HDX
    - 'Internet Explorer'
    - XenApp
---

A user reported that the following error while visiting a website on a Citrix XenApp server:

[![You must have cookies enabled in order to user this tool. Please reload the page and try again.](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb26.png "Cookie Error")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image26.png)

I tried adding the site to the Trusted Sites List and adding the url to the Per Site Privacy list:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb27.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image27.png)

But this didn’t work, but I noticed that the site was “flickering” a lot so I suspected that HDX Flash Acceleration was the problem.

I imported the adm file for HDX (HdxFlash-Server.adm) into a GPO and added the site to the Per-URL-blacklist:

[![SNAGHTMLfc0ed9b](http://192.168.40.25:8081/wp-content/uploads/2012/03/SNAGHTMLfc0ed9b_thumb.png "SNAGHTMLfc0ed9b")](http://192.168.40.25:8081/wp-content/uploads/2012/03/SNAGHTMLfc0ed9b.png)

That fixed the problem!