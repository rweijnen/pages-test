---
id: 1089
title: 'Paging file and Memory Dump'
date: '2011-01-07T13:45:55+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/01/07/paging-file-and-memory-dump/'
permalink: /2011/01/07/paging-file-and-memory-dump/
views:
    - '2469'
short-url:
    - 'http://bit.ly/dJPnJY'
categories:
    - Citrix
    - General
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows XP'
---

I often hear that people configure the Paging File (on Citrix or Terminal Servers) on a seperate volume but, the reasons is either performance or the chance that the Paging File might corrupt the volume.

However if at some point you would like to create a Memory Dump you must have a paging file on the boot volume.

For a Small memory dump you need at least 2MB Paging File on the Boot Volume but for a Full Memory Dump you need a Paging File that is sufficient to hold all the physical RAM plus 1 megabyte (MB).

Side Note: with the increasing ram of today’s servers, how long does it take for a full memory dump to be saved when you have lots of gigabytes?

See also: [Overview of memory dump file options for Windows Vista, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003, Windows XP, and Windows 2000](http://support.microsoft.com/kb/254649).