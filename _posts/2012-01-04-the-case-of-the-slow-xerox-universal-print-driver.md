---
id: 2319
title: 'The case of the Slow Xerox Universal Print Driver'
date: '2012-01-04T22:54:29+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/04/the-case-of-the-slow-xerox-universal-print-driver/'
permalink: /2012/01/04/the-case-of-the-slow-xerox-universal-print-driver/
views:
    - '7885'
short-url:
    - 'http://bit.ly/wSStzt'
categories:
    - Citrix
    - General
    - 'Windows 2003'
tags:
    - Citrix
    - XenApp
    - Xerox
---

[![Xerox Logo](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb9.png "Xerox Logo")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image9.png)Earlier this week I was asked to investigate a problem with the Xerox Universal Printer Driver. Users complained that printing to a Xerox printer was much slower than printing to an HP printer.

[![Excel 2007 Icon](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb10.png "Excel 2007 Icon")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image10.png)I received a reference document from a user, a rather complex Excel sheet. When selecting multiple tabs it took almost a minute to generate a print preview in Excel 2007 running on Windows 2003 with XenApp 5.

I was aware of a bug in the Xerox Universal Driver where almost 9.000 files were copied into the user’s profile directory (I wrote about that in an [earlier post](http://192.168.40.25:8081/2011/02/08/the-case-of-the-citrix-ready-printer-driver/)). But this seemed to be another problem.

I made a trace with Process Monitor while generating the print preview and this made clear that the driver tries to create the key **HKLM\\Software\\Xerox**. This is denied because a user has of course no permissions to write in HKLM:

[![Process Monitor Trace | HKLM\Software\Xerox | ACCESS DENIED](http://192.168.40.25:8081/wp-content/uploads/2012/01/clip_image002_thumb.jpg "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2012/01/clip_image002.jpg)

The Registry Summary shows us that the driver tries to access HKLM\\Software\\Xerox almost 8.000 times before finally giving up:

[![Registry Summary | HKLM\Software\Xerox](http://192.168.40.25:8081/wp-content/uploads/2012/01/clip_image0026_thumb.jpg "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2012/01/clip_image0026.jpg)

I changed the permissions on the Xerox key to see what the driver writes to this key. It creates a subkey with the SID of the User under HKLM\\Software\\Xerox\\V5.0\\Low.

Then it gives the user special permissions on this key and subkeys so the user is allowed to write there.

Finally it stores a few settings in that key:

[![HKLM\Software\Xerox](http://192.168.40.25:8081/wp-content/uploads/2012/01/clip_image0028_thumb.jpg "Registry Editor")](http://192.168.40.25:8081/wp-content/uploads/2012/01/clip_image0028.jpg)

![](http://farm1.staticflickr.com/113/282707058_02305d3cce_z.jpg?zz=1)This looks like a serious design flaw because not only is a standard user not allowed to create keys under the HKLM hive but it also means that user settings are stored on a single machine and are not roaming with the user.

The good news was that the time to generate the print preview went back to around 4 seconds which is acceptable considering the complexity of the reference Excel sheet.

I tested in total 3 versions of the Xerox Universal Print Driver and they all exhibited this particular issue:

- 5.185.42.0N 2011.03.02
- 5.216.12.0N 2011.06.10
- 5.246.7.0 2011.12.07