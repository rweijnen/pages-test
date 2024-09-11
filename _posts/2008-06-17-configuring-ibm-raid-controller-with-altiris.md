---
id: 102
title: 'Configuring IBM Raid controller with Altiris'
date: '2008-06-17T21:41:58+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/06/17/configuring-ibm-raid-controller-with-altiris/'
permalink: /2008/06/17/configuring-ibm-raid-controller-with-altiris/
views:
    - '3842'
short-url:
    - 'http://bit.ly/eCj48m'
categories:
    - Uncategorized
---

Today I was deploying some IBM x3550 and x3650 servers with Altiris Deployment Server. IBM Delivers a toolkit for Altiris that contains amongst others jobs for configuring raid arrays.

To do this you need to create a raid policy file and deploy this. I created this policy file:

> \[Policy.RAID-1\]
> 
> AppliesTo.1 = t:ServeRAID-8k-l,d:4
> 
> Array\_Mode = CUSTOM  
> Array.A = 1,2  
> Array.B = 3,4
> 
> Logical\_Mode = CUSTOM  
> Logical.1 = A:FILL:1  
> Logical.2 = B:FILL:1

As you can see the policy only applies to the type of array controller in my servers (t:ServeRAID-8k-l). This way we prevent applying the policy to other configurarions. I have a 4 disk configuration (d:4) and want to create to RAID 1 arrays (A &amp; B). On each array one Logical drive with the maximum size (FILL parameter).

Looks allright no?

But on deploying I received the following error: CFGRAID: Policies file does not exist or syntax error in policies file. Return Code=2.

As it turns out, the script copies the config file to the ramdisk with the XCopy command but Altiris’ Freedos image doesn’t contain XCopy!

I solved it by editing the cfgraid.bat file (\\sgdeploy\\sgtk\\examples\\cfgraid.bat). Change this line:

> echo Y | xcopy %RD\_PATH%\\%RD\_FILE% %RAMDSK%\\ &gt; nul

to:

> copy %RD\_PATH%\\%RD\_FILE% %RAMDSK%\\ /y