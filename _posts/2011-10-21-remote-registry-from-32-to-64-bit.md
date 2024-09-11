---
id: 2146
title: 'Remote Registry from 32 to 64 bit'
date: '2011-10-21T09:05:11+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/10/21/remote-registry-from-32-to-64-bit/'
permalink: /2011/10/21/remote-registry-from-32-to-64-bit/
views:
    - '3511'
short-url:
    - 'http://bit.ly/rjeDWe'
categories:
    - script
    - Vista
    - 'Windows 2003'
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows XP'
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/10/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/10/image5.png)Yesterday I needed to set a few registry keys remotely from a 32 bit windows machine to a 64 bit machine.

I used reg.exe to set the key but even though it returned success the key wasn’t altered.

As I suspected the key was written to the Wow6432Node. In the help I couldn’t find any switch to force reg.exe to use the 64-bit view.

On a 64 bit machine this is not a problem since both 32- and 64 bit versions of reg.exe exists. The 32 bit version of reg.exe defaults to the 32 bit view and the 64 bit version defaults to the 64 bit view.

But luckily reg.exe has a switch (that is not listed in the help) to force the View:

- <font color="#35383d">/reg:64 forces reg.exe to use the 64 bit view</font>
- <font color="#35383d">/reg:32 forces reg.exe to use the 32 bit view</font>

Example:

 “&gt;

**Note: Windows XP, 2003, Vista and Server 2008 require hotfix** [**kb948698**](http://support.microsoft.com/kb/948698).