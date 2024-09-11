---
id: 2378
title: 'Speed up Windows 7 Resume by 20%'
date: '2012-02-10T21:54:37+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/10/speed-up-windows-7-resume-by-20/'
permalink: /2012/02/10/speed-up-windows-7-resume-by-20/
views:
    - '3731'
short-url:
    - 'http://bit.ly/yIVoE9'
categories:
    - 'Windows 7'
---

![](http://img507.imageshack.us/img507/9686/38779131.jpg)**UPDATE:** See this [new article](http://helgeklein.com/blog/2012/02/what-remains-of-magic-speed-improvements/) by Helge Klein.

Recently Helge Klein wrote a blog titled [How to Speed Up Your Windows 7 Boot Time by 20%](http://helgeklein.com/blog/2012/02/how-to-speed-up-your-windows-7-boot-time-by-20/). He does this by disabling the graphical animation that Windows 7 displays while booting.

After applying this tweak I noticed that a resume from hibernation (which I do far more often than a full boot) still showed the graphical animation (and wasn’t speed up).

So how to disable the animation while resuming?

First you need to determine the GUID of the resumeobject. This GUID is shown when launching bcdedit without parameters:

[![SNAGHTML677d5e6](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML677d5e6_thumb.png "SNAGHTML677d5e6")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML677d5e6.png)

First I tried to set the quietboot option but this is not accepted:

[![SNAGHTML6791575](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML6791575_thumb.png "SNAGHTML6791575")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML6791575.png)

Fortunately we can use bootux disabled:

[![SNAGHTML67a36da](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML67a36da_thumb.png "SNAGHTML67a36da")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML67a36da.png)

And now it’s gone ![Winking smile](http://192.168.40.25:8081/wp-content/uploads/2012/02/wlEmoticon-winkingsmile.png)