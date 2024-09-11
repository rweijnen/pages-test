---
id: 2429
title: 'Change CD/DVD drive letter with RES Automation Manager'
date: '2012-02-17T15:04:34+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/17/change-cddvd-drive-letter-with-res-automation-manager/'
permalink: /2012/02/17/change-cddvd-drive-letter-with-res-automation-manager/
views:
    - '1524'
short-url:
    - 'http://bit.ly/xx4fCc'
categories:
    - RES
tags:
    - 'Automation Manager'
    - RES
---

I needed to change the drive letter assigned to the cd/dvd station from an Automation Manager project.

[![DVD Drive Icon](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb2.png "DVD Drive")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image2.png)Although most systems only have one cd/dvd drive, some machines might be equipped with multiple drives.

A couple of years ago I wrote a tool called [ChDrvLetter](http://192.168.40.25:8081/2010/10/20/change-driveletter-commandline-tool/) that can assign a specific drive letter to a partition given it’s volumename. In that tool I also included an option for CD/DVD drives.

Using the CDROM \[Letters\] parameter you can assign specific letters to the CD/DVD drives.

So let’s used this with RES Automation Manager. First create a new Resource:

[![RES Automation Manager | Add / Edit Resource | ChDrvLetter](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb3.png "Add / Edit Resource")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image3.png)

After adding it, note down the GUID:

[![RES Automation Manager | Add / Edit Resource | ChDrvLetter | GUID](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb4.png "Add / Edit Resource")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image4.png)

Now create a new Module:

[![RES Automation Manager | Add / Edit Module| Force CD/DVD to D:\](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb5.png "RES Automation Manager | Add / Edit Module")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image5.png)

On the Tasks tab click Add, Command (Execute) and use the Command line: $Worspace{GUID} CDROM Letters:[![RES Automation Manager | Add / Edit Module | Execute Command | CommandLine](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb6.png "RES Automation Manager | Add / Edit Module | Execute Command")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image6.png)

In my case I used the commandline “$Workspace{C9CED3CE-32C2-42BA-BDEC-1AB13DFFDF2E} CDROM DZY”.

This will assign the letter D to the first CD/DVD drive (if present), Z to the second CD/DVD drive (if present) and Y to the third CD/DVD drive (if present).