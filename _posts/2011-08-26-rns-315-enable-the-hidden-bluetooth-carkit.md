---
id: 2039
title: 'RNS 315: Enable the hidden bluetooth carkit'
date: '2011-08-26T14:37:13+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/08/26/rns-315-enable-the-hidden-bluetooth-carkit/'
permalink: /2011/08/26/rns-315-enable-the-hidden-bluetooth-carkit/
views:
    - '70129'
short-url:
    - 'http://bit.ly/ohjndM'
categories:
    - General
tags:
    - Bluetooth
    - Carkit
    - RNS315
    - VCDS
    - Volkswagen
---

![](http://t3.gstatic.com/images?q=tbn:ANd9GcQitX_plVMEEHpwnkxI-LcsAGsQmgflLj98KMghV7jwMT5mEPMGjg)

The Volkswagen RNS 315 Navigation has a builtin Bluetooth carkit which is disabled by default. I am not sure why, I presume this is done because VW also sells carkits that integrate into the MFD.

The builtin carkit can be enabled using VCDS like this:

First go to Select Control Module:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/08/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/08/image4.png)

Then select 37-Navigation:

[![SNAGHTML1b76a5](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML1b76a5_thumb.png "SNAGHTML1b76a5")](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML1b76a5.png)

Then select Coding – 07:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/08/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/08/image6.png)

The Bluetooth module is in Byte 3, Bit 5 and is indeed disabled:

[![SNAGHTML2a957b](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML2a957b_thumb.png "SNAGHTML2a957b")](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML2a957b.png)

Remove the checkbox to enable it:

[![SNAGHTML2b1074](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML2b1074_thumb.png "SNAGHTML2b1074")](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML2b1074.png)

Click Exit and then Do It!

The Carkit is available under the Phone Button:

[![IMG_0143](http://192.168.40.25:8081/wp-content/uploads/2011/08/IMG_0143_thumb.jpg "IMG_0143")](http://192.168.40.25:8081/wp-content/uploads/2011/08/IMG_0143.jpg)

Multifunction Steering Wheel and Streaming music over Bluetooth works. There is no integration into the MFD/FIS.

Note that, if not already present, you need to install a microphone.