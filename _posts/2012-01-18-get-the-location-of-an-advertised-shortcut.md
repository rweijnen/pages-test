---
id: 2334
title: 'Get the location of an advertised shortcut'
date: '2012-01-18T13:28:36+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/18/get-the-location-of-an-advertised-shortcut/'
permalink: /2012/01/18/get-the-location-of-an-advertised-shortcut/
views:
    - '4618'
short-url:
    - 'http://bit.ly/AnbLii'
categories:
    - script
tags:
    - MSI
    - VBS
---

Installers can create so called Advertised Shortcuts in the Start Menu. I wanted to check the Target Path of such an shortcut but Explorer doesn’t show it:

[![Microsoft Visio 2010 Properties | Shortcut Properties | Target Path](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb13.png "Microsoft Visio 2010 Properties")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image13.png)

But we can easily retrieve the path using a small VB Script:  
“&gt;  
The scripts accepts the shortcut filename as a parameter so you can simply drag the shortcut to the script and it will show the Target Path and Icon Location:

[![Target Path  : C:\WINDOWS\Installer\{90140000-0057-0000-0000-0000000FF1CE}\visicon.exe |Icon Location: C:\WINDOWS\Installer\{90140000-0057-0000-0000-0000000FF1CE}\visicon.exe,0](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb14.png "Windows Script Host")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image14.png)

Bonus Chatter: if you want to prevent an installer from creating Advertised Shortcuts set the DISABLEADVTSHORTCUTS MSI property to 1 (MsiExec /I MyMsi.msi DISABLEADVTSHORTCUTS=1).