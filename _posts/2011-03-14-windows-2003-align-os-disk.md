---
id: 1561
title: 'Windows 2003 align OS disk'
date: '2011-03-14T15:11:48+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/14/windows-2003-align-os-disk/'
permalink: /2011/03/14/windows-2003-align-os-disk/
views:
    - '4067'
short-url:
    - 'http://bit.ly/fkqUgD'
categories:
    - VMWare
    - 'Windows 2003'
tags:
    - align
    - VMWare
---

If you read one of VMWare’s Best Practices Guides (in my case [this](http://www.peppercrew.nl/?p=1637) one) then you may have read that it’s important to align guest partitions.

We can do this (for Windows OS) using the DiskPart tool that comes with the OS since Windows 2003 SP1 (there is a [hotfix](http://support.microsoft.com/kb/923076) for earlier versions).

On Windows 2008, and higher, all partitions are automatically aligned to a [1 MB boundary](http://frankdenneman.nl/2009/05/windows-2008-disk-alignment/).

But how to do this for the OS disk on Server 2003?

My first thought was to open a [command prompt during setup](http://support.microsoft.com/kb/242380), right before creating the partitions and then use diskpart.

However the OS partition is created during the Text portion of the install process and even though we can get a cmd prompt using SHIFT-F10 we get the recovery console (which has a builtin diskpart but cannot align).

So I used a Windows PE bootdisk. Any version with Diskpart should do but I used a bootdisk from Symantec Backup Exec System Recovery that I’ve customized to my own needs.

If you boot the original Symantec disk you can open a command prompt by accessing a hidden feature: move the mouse above the “S” from Symantec until you get a Hand icon and press the left mouse button:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image12.png)

Now we can launch DiskPart and create the partition (see [here](http://192.168.40.25:8081/2010/11/04/programmatically-create-aligned-partitions/) for a scripted way):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image13.png)

After formatting I used the Partition Table Operations tool to verify the alignment:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image14.png)

I then created an Image of this partition using Altiris RapiDeploy because in my situation this was a requirement.

I checked with the ImageExplorer tool if the alignment (Start Sector) was preserved and this was ok:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image15.png)

And of course I checked if the alignment would be preserved when restoring but it doesn’t:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image16.png)

In the [documentation](http://www.symantec.com/connect/forums/getting-around-partitions) I found the -retainstart switch and when I used that the start sector was ok:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image17.png)

PS: there is also a -align switch but this is not used for aligning the partition!