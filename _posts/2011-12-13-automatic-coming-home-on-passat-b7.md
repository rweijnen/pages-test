---
id: 2247
title: 'Automatic Coming Home on Passat B7'
date: '2011-12-13T20:01:50+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/12/13/automatic-coming-home-on-passat-b7/'
permalink: /2011/12/13/automatic-coming-home-on-passat-b7/
views:
    - '5685'
short-url:
    - 'http://bit.ly/tw7uq1'
categories:
    - Automotive
tags:
    - 'Coming Home'
    - Passat
    - VCDS
---

![Passat - B7](http://www.kufatec.de/shop/images/categories/380.png)I am not sure why Volkswagen has altered the behavior of the Coming Home function but on recent B6 and all B7 Passats it works differently.

Previously the Coming Home function was activated automatically either when taking out the ignition key or when leaving the car (depending on coding).

In newer B6 and all B7 models you need pull the high beam stalk towards you before leaving the car which is annoying and looks very silly.

So how can we restore the old behavior?

Open VCDS and go to Control Module 9, Central Electronics:

[![VCDS | Select Control Module | 09 Central Electronics](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0b435f_thumb.png "VCDS | Select Control Module")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0b435f.png)

Then go to Option 7, Long Coding and set the following:

- Set the checkbox for Byte 12 Bit 0 (Coming Home mode Automatic)
- Deselect Byte 12 Bit 2 (Optional, activates Coming Home when opening the door instead of when taking out the ignition key)

 [![SNAGHTML7acd7c0](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML7acd7c0_thumb.png "SNAGHTML7acd7c0")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML7acd7c0.png)

In the same screen activate Byte 17, Bit 5 (Coming Home Active):

[![SNAGHTML7ac748b](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML7ac748b_thumb.png "SNAGHTML7ac748b")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML7ac748b.png)