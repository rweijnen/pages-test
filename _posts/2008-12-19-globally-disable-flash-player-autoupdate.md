---
id: 218
title: 'Globally disable Flash Player autoupdate'
date: '2008-12-19T09:06:16+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/19/globally-disable-flash-player-autoupdate/'
permalink: /2008/12/19/globally-disable-flash-player-autoupdate/
views:
    - '7770'
short-url:
    - 'http://bit.ly/fd2sW4'
categories:
    - Citrix
    - General
    - 'Terminal Server'
---

On a Citrix or Terminal Server you will want to disable autoupdate notifications of the flash player.

This can be done by creating a file mm.cfg in the folder where the flash ActiveX control is installed (normally C:\\Windows\\System32\\Macromed\\Flash).

Place the following line in this file (with a text editor like Notepad):

> AutoUpdateDisable=1

Be sure to save the file with UTF-8 encoding, this can be selected in the Save As dialog in Notepad:

![notepad utf8](http://192.168.40.25:8081/wp-content/uploads/2008/12/notepad-utf8.png)

Ofcourse you are aware that only certain Flash versions are supported (and optimized) in Citrix? At this time these versions are: 7a, 8, 8b, 9, 9c, and 9d.