---
id: 2381
title: 'Hibernation fails in Windows 7'
date: '2012-02-10T22:40:46+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/10/hibernation-fails-in-windows-7/'
permalink: /2012/02/10/hibernation-fails-in-windows-7/
views:
    - '1755'
short-url:
    - 'http://bit.ly/Ar0WCK'
categories:
    - 'Windows 7'
---

![](http://4.bp.blogspot.com/_xZlqc9CDR-Q/TTmOZOl7OSI/AAAAAAAAADU/0N5ruIdeUcg/s1600/hibernate.gif)A while ago my Windows 7 laptop suddenly refused to go into Hibernation. The strange thing was that the whole process of saving memory to the hibernate file seemed to work correctly. The screen would go black and there was lots of disk activity. Then after the disk activity finished the system would return to the logon screen.

A Google on this issue learned that the most likely cause was a driver preventing the system from going into hibernation. Using the cmdline “*powercfg -DEVICEQUERY wake\_armed*” we can check if there are any devices that can wake the system. Another useful parameter is *-ENERGY* which generates an html report file.

But in my case this lead to nothing.

I even uninstalled all recently added Windows Updates but again no success. So I was at the point that I thought of reinstalling Windows when I read [Helge Klein’s blog post](http://helgeklein.com/blog/2012/02/how-to-speed-up-your-windows-7-boot-time-by-20/) about removing the graphical animation that is shown at startup.

But when I tried to edit the Boot Configuration I got the following error: “*The boot configuration data store could not be opened.*“

[![The boot configuration data store could not be opened | The system cannot find the file specified](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb.png "bcdedit")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image.png)

I checked the contents of the C:\\Boot directory and to my surprise the whole directory was empty!

It seemed logical that this was the cause of my hibernation issue since some data is written to the bootloader at the end of the hibernation process so that it knows it has to resume on next boot.

It was unclear though how Windows managed to boot into the OS without a working bootloader. So I opened Disk Management and I noticed that instead of my OS partition, the OEM Dell partition was marked as active.

I set my OS partition to active, booted from the Windows 7 Install DVD and selected Repair.

From the Recovery Console I used [BootRec /RebuildBcd](http://support.microsoft.com/kb/927392) to rebuild the Boot Configuration Datastore and now Hibernation was working again!