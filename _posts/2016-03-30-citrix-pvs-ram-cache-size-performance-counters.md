---
id: 3655
title: 'Citrix PVS RAM cache size Performance Counters'
date: '2016-03-30T14:58:54+02:00'
author: 'Remko Weijnen'
excerpt: 'Free tool to add Performance Monitor counters for Citrix PVS RAM Cache'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3655'
permalink: /2016/03/30/citrix-pvs-ram-cache-size-performance-counters/
short-url:
    - 'http://bit.ly/1RIuKA9'
views:
    - '5346'
categories:
    - Citrix
tags:
    - Citrix
    - Performance
    - PVS
    - 'RAM Cache'
---

[![clip_image002](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image002_thumb.jpg "clip_image002")](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image002.jpg)

There has long [been a debate](http://andrewmorgan.ie/2015/08/accurately-checking-the-citrix-pvs-cache-in-ram-overflow-to-disk-ram-cache-size/) about how to accurately view the size of your Citrix Provisioning Services ram cache size. SO much so that even [Citrix clarified](https://www.citrix.com/blogs/2015/08/19/digging-into-pvs-with-poolmon-and-wpa/) on how to view this detail using yet another tool

The thing is, this is all fine and well, but it’s a bit of a pig to actually get this data when you need it, or in an automated way. Wouldn’t it be better if we could have something easier?

Lately, [Andrew Morgan](http://www.andrewmorgan.ie/) and I decided to sit down and create an easy to use, Windows performance counter for the key metrics in a PVS cache and provide them to the community for use.

These counters turned out to be fascinating, as they really show how the cache works.

Our latest counters (which can be downloaded below) provide the following counters for easy access:

[![Performance Monitor](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image004_thumb.png "PVS Ram cache size (MB) | PVS metadata size (MB) | PVS Write Cache VHD disk size (MB) | PVS Ram Cache Percent used")](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image004.png)

- PVS Ram cache size (MB)
- PVS metadata size (MB)
- PVS Write Cache VHD disk size (MB)
- PVS Ram Cache Percent used. \*
  *\* As there is no accurate way to detect how much ram is assigned to cache via Citrix Provisioning services, this value must be provided or this performance counter is missing.*


   
The end result, is a simple MSI. Install this MSI on your PVS target golden image and when the image is in “Cache in RAM, overflow to disk” the performance counters will appear in perfmon as below:
[![Performance Counters for Citrix PVS RAM Cache](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image006_thumb.png "Performance Monitor | Add Counters")](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image006.png)

The Service on start-up will check the cache type and if correct, the counters will be created an update every 500 MS. On service stop, the counters will be deleted. Simple stuff.

These counters are available in library form, should you wish to implement these counters in your own monitoring solution. Please reach out directly If you would like them.

In order for the “PVS RAM Cache Percent Used %” to appear. You must define the following registry value:

[![HKEY_LOCAL_MACHINE\SOFTWARE\PVSCOUNTER](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image008_thumb.png "Registry Editor")](http://192.168.40.25:8081/wp-content/uploads/2016/03/clip_image008.png)

HKEY\_LOCAL\_MACHINE\\SOFTWARE\\PVSCOUNTER   
Type: REG\_SZ   
Name: CacheSizeMB

Value: e.g. 1024

**Download:**

You can find a download link [here.](https://app.box.com/s/25tt2xs62hyv48e62daat44cy1hrlhd1)