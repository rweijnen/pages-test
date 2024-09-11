---
id: 1570
title: 'STOP: 0x0000005D when booting Windows PE'
date: '2011-03-15T12:47:13+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/15/stop-0x0000005d-when-booting-windows-pe/'
permalink: /2011/03/15/stop-0x0000005d-when-booting-windows-pe/
views:
    - '4450'
short-url:
    - 'http://bit.ly/hW3uEF'
categories:
    - Altiris
    - VMWare
tags:
    - Altiris
    - VMWare
    - 'Windows PE'
---

I was booting a new VMWare Virtual Machine with Windows PE through Altiris for initial deployment but Windows PE halted with a BSOD:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb22.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image22.png)

[0x0000005D](http://msdn.microsoft.com/en-us/library/ff559072(v=VS.85).aspx) means UNSUPPORTED\_PROCESSOR (defined in [bugcodes.h](http://msdn.microsoft.com/en-us/library/ff542347(v=VS.85).aspx)) so I expected there was a x86 vs x64 problem.

The VM was configured for a 32 bit OS:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image19.png)

The Altiris Job was configured to use Auto Select:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image20.png)

But instead of the x86 version of Windows PE, Altiris attempts to boot the x64 version and this explains the BSOD: VMWare prevents the CPU from going to x64 mode and thus Windows has no choice but to halt.

Workaround is to change the Automation pre-boot environment in Altiris to x86:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb21.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image21.png)

Note that it’s no problem to deploy an x64 OS using the x86 version of Windows PE so I don’t see any real problems with this workaround.