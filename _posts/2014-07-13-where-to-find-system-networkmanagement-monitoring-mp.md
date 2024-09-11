---
id: 3465
title: 'Where to find System.NetworkManagement.Monitoring.mp?'
date: '2014-07-13T13:22:13+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3465'
permalink: /2014/07/13/where-to-find-system-networkmanagement-monitoring-mp/
views:
    - '753'
short-url:
    - 'http://bit.ly/1whx1Fl'
categories:
    - Uncategorized
tags:
    - 'Management Pack'
    - SCOM
    - 'Visual Studio'
    - VSAE
---

[![System Center Operations Manager Logo](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb.png "SCOM Logo")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image.png)I am currently working on a Management Pack for SCOM and I have studies a few examples on adding processor and memory counters.

These examples all reference a Management Pack named "System.NetworkManagement.Monitoring.mp" but this Management Pack is not bundled with the [System Center 2012 Visual Studio Authoring Extensions](http://www.microsoft.com/en-us/download/details.aspx?id=30169).

You can find the pack on the SCOM install media in the ManagementPacks folder. However you need a version that matches the version of the other references you’ve made in your Management Pack.

I have chooses to use the MP’s from 2012 RTM for maximum compatibility but my SCOM server is running 2012R2 so I cannot use the MP from my install media.

The solution was to download SCOM 2012 RTM and to save you some time (if you are reading this, you are probably looking for this file) I have downloaded the following versions:

- <font color="#35383d">[2012 RTM (build 7.0.8560.0)](https://remkoweijnen.sharefile.eu/d/sbbae3515cde4caeb)</font>
- <font color="#35383d">[2012 SP1 (build 7.0.9538.0)](https://remkoweijnen.sharefile.eu/d/sb961bb41d1f419c8)</font>
- <font color="#35383d">[2012 R2 (build 7.1.10226.0)](https://remkoweijnen.sharefile.eu/d/s6936ca19df34f629)</font>