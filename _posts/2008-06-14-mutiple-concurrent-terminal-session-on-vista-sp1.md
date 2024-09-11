---
id: 101
title: 'Multiple Concurrent Terminal Server Sessions On Vista SP1'
date: '2008-06-14T23:35:44+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/06/14/mutiple-concurrent-terminal-session-on-vista-sp1/'
permalink: /2008/06/14/mutiple-concurrent-terminal-session-on-vista-sp1/
views:
    - '25422'
short-url:
    - 'http://bit.ly/hsGy1O'
categories:
    - 'Terminal Server'
    - Vista
tags:
    - LinkedIn
---

There are several patched terminal server dll’s floating around in the net to allow multiple concurrent Terminal Server session on Windows Vista with Service Pack 1. But they all have the same limitations:

It’s not possible to start a session to Localhost, this is because the Terminal Server client does a check to see if you are running Personal Terminal Server (Vista/XP) and denies Localhost or 127.0.0.1 if true (127.0.0.2 works though).

It’s not possible to start multiple sessions with the same user. The patch for Vista RTM did allow for this but in Service Pack 1 some Terminal Server code has moved to the Local Session Manager (lsm.exe) so we need to patch this file as well.

Offcourse we need to patch Terminal Server to allow unlimited session on Vista as well.

[VPatch](http://www.tibed.net/vpatch/) files are in the download link below.

[ Vista SP1 Patches (6997 downloads ) ](http://192.168.40.25:8081/download/vista-sp1-patches/?tmstv=1726048918 "Version 1")