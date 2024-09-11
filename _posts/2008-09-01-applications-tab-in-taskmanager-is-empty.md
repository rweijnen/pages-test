---
id: 114
title: 'Applications tab in taskmanager is empty'
date: '2008-09-01T16:46:21+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/09/01/applications-tab-in-taskmanager-is-empty/'
permalink: /2008/09/01/applications-tab-in-taskmanager-is-empty/
views:
    - '32629'
short-url:
    - 'http://bit.ly/i0EJMC'
categories:
    - Citrix
---

Today I noticed something strange: on a Citrix (Presentation Server 4.5) server taskmanager does not show anything in the applications tab.

![taskmgr](http://192.168.40.25:8081/wp-content/uploads/2008/09/taskmgr.png)

I tested this on the other Citrix Servers in the farm and they all had the same problem (and non Citrix servers did not). As you might know taskmanager fills the applications tab by enumerating all top level windows. That’s why I suspected Citrix because it places several hooks (multi monitor support, speedscreen etc.).

Turned out that by setting the Flag value of the HKEY\_LOCAL\_MACHINE\\Software\\Citrix\\CtxHook\\AppInit\_Dlls\\Multiple Monitor Hook from 4 to 0 fixed the issue.