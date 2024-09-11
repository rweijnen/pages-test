---
id: 2139
title: 'Prevent additional IP addresses from being registered in DNS'
date: '2011-10-18T15:01:33+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/10/18/prevent-additional-ip-addresses-from-being-registered-in-dns/'
permalink: /2011/10/18/prevent-additional-ip-addresses-from-being-registered-in-dns/
views:
    - '8548'
short-url:
    - 'http://bit.ly/mZJtA4'
categories:
    - 'Windows 2008 R2'
    - 'Windows 7'
---

[![SNAGHTML1aaf7885](http://192.168.40.25:8081/wp-content/uploads/2011/10/SNAGHTML1aaf7885_thumb.png "SNAGHTML1aaf7885")](http://192.168.40.25:8081/wp-content/uploads/2011/10/SNAGHTML1aaf7885.png)In Windows 7 and 2008 R2 all IP Addresses are by default registered in DNS.

If you don’t want certain IP addresses to appear in DNS you can alter this behavior with Netsh using the **skipassource** flag.

Use the following syntax to add an additional IP Address with **skipassource** flag:

  
“&gt;  
If you want to check if the skipassource flag is set, you can use the following syntax:  
“&gt;  
**<span style="text-decoration: underline;">Notes</span>**

- You need either SP1 or [Hotfix kb2386184](http://support.microsoft.com/kb/2386184).
- If you change the IP settings from the GUI the **skipassource flag is cleared** (install [Hotfix kb2554859](http://support.microsoft.com/kb/2554859)).
- See [kb975808](http://support.microsoft.com/?kbid=975808) for Vista and Server 2008.
- See [kb246804](http://support.microsoft.com/kb/246804) for Windows 2000 and 2003.