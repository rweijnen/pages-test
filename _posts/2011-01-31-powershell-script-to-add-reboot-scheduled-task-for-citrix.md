---
id: 1381
title: 'PowerShell Script to add reboot scheduled task for Citrix'
date: '2011-01-31T22:53:47+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1381'
permalink: /2011/01/31/powershell-script-to-add-reboot-scheduled-task-for-citrix/
views:
    - '5076'
short-url:
    - 'http://bit.ly/eFGy7t'
categories:
    - Citrix
    - PowerShell
---

I wanted to create a Scheduled Task on my Citrix Servers to have the reboot every other night.

The idea is that half of the servers will reboot in a night and the other half the following night.

The [TSSHUTDN](http://support.microsoft.com/kb/320188) tool is handy since it can issue a warning to logged on users, log them out after a certain period and finally issue the reboot.

Since I needed to add a scheduled task to many servers I wanted to do this with a script.

WMI Exposes the [Win32\_ScheduledJob](http://msdn.microsoft.com/en-us/library/aa394399(v=VS.85).aspx) Class and it’s [Create Method](http://msdn.microsoft.com/en-us/library/aa389389(v=vs.85).aspx).

The parameters, especially StartTime, are constructed very odly and I can never remember them.

So I wrote a very simple wrapper in PowerShell to make it a little easier for the next time:  
“&gt;