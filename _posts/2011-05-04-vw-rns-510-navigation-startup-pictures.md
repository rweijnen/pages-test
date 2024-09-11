---
id: 1745
title: 'VW RNS 510 Navigation Startup Pictures'
date: '2011-05-04T21:06:43+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/05/04/vw-rns-510-navigation-startup-pictures/'
permalink: /2011/05/04/vw-rns-510-navigation-startup-pictures/
views:
    - '16048'
short-url:
    - 'http://bit.ly/k3pcbw'
categories:
    - General
tags:
    - Navigation
    - VW
---

A while ago I started exploring the firmware of an RNS510 Navigation unit. The RNS510 is an OEM navigation system, manufactured by Continental Automotive and is used in several models of Volkswagen, Seat and Skoda Cars

![](http://www.volkswagen.nl/media/home/modellen/touran/rns_510.jpg)

When the units is booting it shows a startup logo and I wanted to replace this logo with my own picture.

I have some experience in this area since I did this before on the predecessor of the RNS510, the RNS MFD2 DVD system.

When loading the firmware the logo’s are loaded into the units flash memory (and not to the internal hard drive).

I have found the files that contain the images and how they are encoded. I also managed to extract the images and convert them to a bitmap that can be displayed in Windows.

I also know how to recode a bitmap into the RNS510’s native format however there’s other stuff in the same files that I have not yet deciphered so I am afraid to overwrite something there.

At the moment I don’t have enough time to finish this stuff so I am posting the logo’s here as proof of concept and hopefully someone can continue this or knows more (contact me).

<u>**Volkswagen logo from firmware 2760:**</u>

[![H_TO_HDD2](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_TO_HDD2_thumb.png "H_TO_HDD2")](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_TO_HDD2.png)

**<u>Skoda logo from firmware 2760:</u>**

[![H_SB_HDD](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_SB_HDD_thumb.png "H_SB_HDD")](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_SB_HDD.png)

**<u></u>**

**<u>Skoda logo from firmware 1020:</u>**

[![H_SK_HDD](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_SK_HDD_thumb.png "H_SK_HDD")](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_SK_HDD.png)

**<u></u>**

**<u>Seat logo from firmware 1020:</u>**

[![H_SE_HDD](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_SE_HDD_thumb.png "H_SE_HDD")](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_SE_HDD.png)

**<u>VW Logo for Chinese units:</u>**

[![H_PQ_HDD](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_PQ_HDD_thumb.png "H_PQ_HDD")](http://192.168.40.25:8081/wp-content/uploads/2011/05/H_PQ_HDD.png)

**<u>Dynaudio logo (VW with Dynaudio system only):</u>**

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image12.png)