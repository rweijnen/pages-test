---
id: 453
title: 'Save Explorer settings without Logging off'
date: '2009-11-16T16:42:12+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=453'
permalink: /2009/11/16/save-explorer-settings-without-logging-off/
views:
    - '1945'
short-url:
    - 'http://bit.ly/gI6l4O'
categories:
    - Vista
    - 'Windows 2008'
---

This is more a note to self because I always forget. Explorer holds all it’s settings in memory so if you change a settings through the GUI (like in Folder Options) you cannot use a tool like Process Monitor to see what the corresponding registry entry is.

Fortunately there is a (well known) trick: click with the RIGHT mouse button while holding both Shift and Ctrl near the logoff button. A small popup menu appears with an Exit Explorer option:

![ExitExplorer](http://192.168.40.25:8081/wp-content/uploads/2009/11/exitexplorer.png)

If you use it Explorer will end and save it’s settings (it’s convenient to keep taskmanager or a cmd prompt open so you easily relaunch it).

The trick works on Vista and higher.