---
id: 213
title: 'Commandline tool to change drive letter assignment'
date: '2008-12-09T15:13:27+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/09/commandline-tool-to-change-drive-letter-assignment/'
permalink: /2008/12/09/commandline-tool-to-change-drive-letter-assignment/
views:
    - '9806'
short-url:
    - 'http://bit.ly/dLbQkO'
categories:
    - General
    - Programming
---

One of the side effects of using sysprep to prepare an image is that drive letter assignments are reset to default. This behaviour is documented in a [knowledge base article](http://support.microsoft.com/kb/928386).

I previously solved this by running a diskpart script but that needed a custom script for each system (if the disk or partition order differs the script needed to be adjusted). So I needed to run a restore with sysprep determine the drive layout after sysprep, change the script, test by restoring again. So I wrote a commandline tool that can change a drive letter assignment based on the volume label.

eg. the partition with the Apps label is assigned D and the partition with the label Profiles is assigned E and so on.

This the syntax:

> ChDrvLetter LABEL Letter
> 
> Example:  
> ChDrvLetter DATA D
> 
> Remarks:  
> You cannot change the drive letter for the boot/system partition.  
> LABEL is case insensitive and the first match is used.
> 
> ChDrvLetter DELETEONLY Letter can be used to delete a drive letter  
> Assignment

[ ChDrvLetter.zip (3204 downloads ) ](http://192.168.40.25:8081/download/chdrvletter-zip/?tmstv=1726048918 "Version 1.3")