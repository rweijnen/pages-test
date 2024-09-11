---
id: 4
title: 'Atheros AR5007EG &#8211; Bad WLAN performance'
date: '2007-10-16T17:55:54+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=4'
permalink: /2007/10/16/toshiba-satellite-l40/
views:
    - '82855'
short-url:
    - 'http://bit.ly/fj6kwQ'
categories:
    - Vista
tags:
    - slow
    - Vista
    - WLAN
---

A friend bought a new Toshiba Satellite L40 laptop which came with Windows Vista preinstalled. The wireless connection (via the built in Atheros AR5007EG card) was very unstable and sometimes unable to connect to the access point. When connected internet speed was very slow, sometimes unable to open pages at all. First we tried replacing the preinstalled Toshiba drivers with the latest from the Toshiba site and later on the most recent from Atheros (which can be found [here](http://www.atheros.cz/ "here")). Both drivers didn’t improve the speed.   
Solution:

To fix the speed issue:  
Update the driver to v7.3.1.109 v1.15, you can find it [here](http://laptopvideo2go.com/drivers/wlan/atheros_v7.3.1.109_v1.15.exe "here").  
Mirror: [ Atheros Driver (23993 downloads ) ](http://192.168.40.25:8081/download/atheros-driver/?tmstv=1726048918 "Version v7.3.1.109_v1.15")

To improve stability and fix the connection issues:  
Install the hotfixes below from Microsoft addressing WLAN issues with Vista:

[You cannot connect to a wireless network on a Windows Vista-based computer](http://support.microsoft.com/kb/935222 "You cannot connect to a wireless network on a Windows Vista-based computer")  
[Windows Vista incorrectly detects the miniport edge of PassThru as a WLAN adapter when you use the PassThru Intermediate Driver from the Windows Driver Kit](http://support.microsoft.com/kb/932030 "Windows Vista incorrectly detects the miniport edge of PassThru as a WLAN adapter when you use the PassThru Intermediate Driver from the Windows Driver Kit")  
[Several problems occur on a Windows Vista-based computer when you work in a wireless network environment](http://support.microsoft.com/kb/932063 "Several problems occur on a Windows Vista-based computer when you work in a wireless network environment")