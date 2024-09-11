---
id: 672
title: 'Change Driveletter Commandline Tool'
date: '2010-10-20T14:04:35+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=672'
permalink: /2010/10/20/change-driveletter-commandline-tool/
views:
    - '9538'
short-url:
    - 'http://bit.ly/ek15M4'
categories:
    - Programming
    - Uncategorized
    - VMWare
    - 'Windows 2008'
    - 'Windows 7'
---

Some time ago (2008 actually) I wrote a [tool that can assign driveletters](http://192.168.40.25:8081/2008/12/09/commandline-tool-to-change-drive-letter-assignment/) given a volumename. I use it myself after SysPrep operations to assign the desired drive letters. For instance after cloning a Virtual Machine from a template.

I noticed that I updated this tool sometime after the original post but never uploaded it here. The most important changes are:

- When using ChDrvLetter without parameters it will list the currently assigned Drive Letters and Volumes:

> ChDrvLetter v1.4 (c) Copyright 2008 by Remko Weijnen ([www.remkoweijnen.nl](https://www.remkoweijnen.nl))
> 
> Assigned Letters: \[ACDZ\]
> 
> Volume 0  
> Letter: C:\\  
> Label :  
> Device: \\Device\\HarddiskVolume1  
> Name : [\\\\?\\Volume{c3acae19-dc1f-11df-8130-806e6f6e6963}\\](file://\\?\Volume{c3acae19-dc1f-11df-8130-806e6f6e6963}\)  
> Type : DRIVE\_FIXED
> 
> Volume 1  
> Letter: Z:\\  
> Label :  
> Device: \\Device\\CdRom0  
> Name : [\\\\?\\Volume{c3acae1c-dc1f-11df-8130-806e6f6e6963}\\](file://\\?\Volume{c3acae1c-dc1f-11df-8130-806e6f6e6963}\)  
> Type : DRIVE\_CDROM
> 
> Volume 2  
> Letter: A:\\  
> Label :  
> Device: \\Device\\Floppy0  
> Name : [\\\\?\\Volume{c3acae1d-dc1f-11df-8130-806e6f6e6963}\\](file://\\?\Volume{c3acae1d-dc1f-11df-8130-806e6f6e6963}\)  
> Type : DRIVE\_REMOVABLE
> 
> Volume 3  
> Letter: D:\\  
> Label : DATA  
> Device: \\Device\\HarddiskVolume2  
> Name : [\\\\?\\Volume{d45237f1-dc21-11df-8d8f-005056ae0001}\\](file://\\?\Volume{d45237f1-dc21-11df-8d8f-005056ae0001}\)  
> Type : DRIVE\_FIXED

- <div>There is a new DELETEONLY parameter that can be used to delete an existing Drive Letter assignment</div>

> ChDrvLetter DELETEONLY Letter  
> can be used to delete a drive letter Assignment
> 
> Example:  
> ChDrvLetter DELETEONLY D

- <div>the CDROM parameter can be used to change drive letter assignments for cd/dvd players:</div>

> ChDrvLetter CDROM Letters  
> can be used to assign drive letters to CD and/or DVD drives (because they have  
> no label when no cd/dvd is inserted
> 
> Example:  
> ChDrvLetter CDROM ZYX  
> Assigns Z to the 1st found CD/DVD drive, Y to the next etc.

I hope you like it, find the download below:

[ ChDrvLetter.zip (3204 downloads ) ](http://192.168.40.25:8081/download/chdrvletter-zip/?tmstv=1726048919 "Version 1.3")