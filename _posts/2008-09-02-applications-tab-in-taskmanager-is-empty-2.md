---
id: 115
title: 'Applications tab in taskmanager is empty #2'
date: '2008-09-02T09:05:25+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/09/02/applications-tab-in-taskmanager-is-empty-2/'
permalink: /2008/09/02/applications-tab-in-taskmanager-is-empty-2/
views:
    - '1316'
short-url:
    - 'http://bit.ly/hatxJ3'
categories:
    - Citrix
---

As a followup to the previous [article](http://192.168.40.25:8081/2008/09/01/applications-tab-in-taskmanager-is-empty/):

It might be better to just Exclude taskmanager because settings the Flag value to 0 might disable multi monitor support. To do this Create a new REG\_SZ (string) under HKEY\_LOCAL\_MACHINE\\Software\\Citrix\\CtxHook\\AppInit\_Dlls\\Multiple Monitor Hook and name it Exclude. It’s value should be taskmgr.exe (case sensitive!).

I’ve also seen some issue where starting a Remote Desktop (RDP) session from within a Citrix session has some troubles with the RDP client’s window (the window “sticks” to the upper left corner or cannot be maximized). So it might be a good idea to include mstsc.exe as well in the Exclusion list (seperate values with ;).

The issues seem to have appeared with Hotfix Rollup 2.