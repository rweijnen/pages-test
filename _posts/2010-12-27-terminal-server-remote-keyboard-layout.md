---
id: 956
title: 'Terminal Server Remote Keyboard Layout'
date: '2010-12-27T16:38:55+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/12/27/terminal-server-remote-keyboard-layout/'
permalink: /2010/12/27/terminal-server-remote-keyboard-layout/
views:
    - '13011'
short-url:
    - 'http://bit.ly/ev6VJc'
categories:
    - 'Terminal Server'
    - 'Windows 2003'
---

Today I shadowed a user’s session in Citrix and when I wanted to type something I noticed that the keyboard layout was incorrect.

This is and old “friend” that I always tend to forget about. So hopefully this post will help me to remember it :D.

You can prevent this by adding a value “IgnoreRemoteKeyboardLayout” to the registry key HKLM\\System\\CurrentControlSet\\Keyboard Layout:

> reg add “HKLM\\SYSTEM\\CurrentControlSet\\Control\\Keyboard Layout” /v IgnoreRemoteKeyboardLayout /t REG\_DWORD /d 0x00000001 /f

This option has been present since Windows 2000 but was broken in Windows 2003. For Windows 2003 there are two related hotfixes, see [kb 842136](http://support.microsoft.com/kb/842136 "The IgnoreRemoteKeyboardLayout registry entry has no effect in Windows Server 2003") and [kb 917910](http://support.microsoft.com/kb/917910 "The terminal server IME keyboard layout differs from the client computer when you remotely log on to a Windows Server 2003 SP1-based terminal server").

If you want to change the keyboard layout that is used before logging in (“at the logon screen”) you need to modify the key HKEY\_USER\\.DEFAULT\\Keyboard Layout\\Preload:

[![PreLoad](http://192.168.40.25:8081/wp-content/uploads/2010/12/preload-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/preload.png)

In the screenshot above the locale id 0413 (Dutch) but you can even add more than one entry and cycle between them with ALT-SHIFT.

A description of the Locale ID’s (LCID’s) can be found in [kb 262283](http://support.microsoft.com/kb/262283 "The input locale does not type the chosen keyboard mapping in the MS-DOS window").