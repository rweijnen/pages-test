---
id: 3421
title: 'Dumping passwords in a VMware .vmem file'
date: '2013-11-25T08:54:34+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3421'
permalink: /2013/11/25/dumping-passwords-in-a-vmware-vmem-file/
views:
    - '12914'
short-url:
    - 'http://bit.ly/18AKQa1'
    - 'http://bit.ly/18AKQa1'
categories:
    - VMWare
tags:
    - Hack
    - mimikatz
    - Passwords
    - Security
    - VMWare
---

![image](http:///192.168.40.25:8081/wp-content/uploads/2013/06/image_thumb2.png?resize=63%2C62 "image")[Benjamin Delpy](https://twitter.com/gentilkiwi) the author of the well known mimikatz toolkit has released a very cool extension to WinDbg today.

In summary the extension can extract Windows passwords from memory dumps, hibernation files and Virtual Machine .vmem files (paging, snapshots).

Especially the ability to extract passwords from .vmem files was very interesting. So I decided to to test this out, so let’s see how it works!

First you need to download and install the [Debugging Tool for Windows](http://192.168.40.25:8081/2013/06/13/debugging-tools-for-windows-direct-download/) (WinDbg). Then we’ll need MoonSols [Windows Memory toolkit](http://www.moonsols.com/windows-memory-toolkit/) (Free edition suffices) and finally you’ll need to download [mimikatz](http://blog.gentilkiwi.com/mimikatz).

Extract bin2dmp.exe from Windows Memory Toolkit and use it to convert a .vmem file to a .dmp file:

[![SNAGHTML14833be2](http://192.168.40.25:8081/wp-content/uploads/2013/11/SNAGHTML14833be2_thumb.png "SNAGHTML14833be2")](http://192.168.40.25:8081/wp-content/uploads/2013/11/SNAGHTML14833be2.png)

Now start WinDbg and load the generated dump file via File -&gt; Open Crash Dump. Load the mimilib.dll file that corresponds to the dump file (32 bit lib for x86 dumps and 64 bit lib for x64 dumps).

eg: .load mimilib.dll

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/11/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/11/image.png)

Now search for the lsass process (!process 0 0 lsass.exe) and use the returned address:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/11/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/11/image1.png)

Finally enter !mimikatz and wait for the magic to happen:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/11/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/11/image2.png)

I have just one word, WOW. Great job by Benjamin again!