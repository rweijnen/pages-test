---
id: 3516
title: 'Get Actual CPU Clock Speed with PowerShell'
date: '2014-07-18T16:29:24+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3516'
permalink: /2014/07/18/get-actual-cpu-clock-speed-powershell/
short-url:
    - 'http://bit.ly/1nTbRwK'
views:
    - '6528'
categories:
    - PowerShell
tags:
    - 'Clock Speed'
    - CPU
    - PowerShell
    - VDI
---

To get the best performance out of Virtual Desktops it is essential that the power configuration in the system BIOS and the HyperVisor are configured for maximum performance.

Many people have blogged about the importance of these settings like, [Andrew Wood](http://blog.atlantiscomputing.com/2013/08/powering-vdi-performance-best-practices-for-optimal-virtual-desktop-performance/), [Helge Klein](http://helgeklein.com/blog/2013/05/the-effects-of-power-savings-mode-on-vcpu-performance/) and [Didier Van Hoye](http://workinghardinit.wordpress.com/2011/06/20/consider-cpu-power-optimization-versus-performance-when-virtualizing/). So I will not go into details again.

But how do you check from a Virtual Machine if you are actually running at full clock speed or not?

I have written a PowerShell script to do just that (requires at least PowerShell v3).

Here are some screenshots:

Running with "High Performance profile":

[![CPU Clock Speed with d"High Performanced" Power Profile](http://192.168.40.25:8081/wp-content/uploads/2014/07/clip_image002_thumb.jpg "Windows PowerShell")](http://192.168.40.25:8081/wp-content/uploads/2014/07/clip_image002.jpg)

Running with "Balanced" power profile:

[![CPU Clock Speed with High Performance Balanced Profile](http://192.168.40.25:8081/wp-content/uploads/2014/07/clip_image0025_thumb.jpg "Windows PowerShell")](http://192.168.40.25:8081/wp-content/uploads/2014/07/clip_image0025.jpg)

Here is the script (download link at the bottom):

 “&gt;

[Download](https://remkoweijnen.sharefile.eu/d/sbeb8ee084c343aca)