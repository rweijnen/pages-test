---
id: 117
title: 'Patch Windows 2008 Terminal Server to allow more than 2 concurrent sessions'
date: '2008-10-21T13:56:23+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/10/21/patch-windows-2008-terminal-server-to-allow-more-than-2-concurrent-sessions/'
permalink: /2008/10/21/patch-windows-2008-terminal-server-to-allow-more-than-2-concurrent-sessions/
views:
    - '95531'
short-url:
    - 'http://bit.ly/hqUCRk'
categories:
    - 'Terminal Server'
    - 'Windows 2008'
tags:
    - LinkedIn
---

Well it took some time but I patched Terminal Server for Windows 2008 to allow unlimited sessions in Remote Administration mode.

This patch is for 32 bit English version. In order to install it you need to perform the steps below. Before you start please check if using this patch is allowed according to your country’s law and your license agreement.

- Install v-patch
- From the vpatch directory launch vpatchprompt.exe
- vpatchprompt will ask you for the following files:  
    – Patch file (the .pat file).  
    – Source file (termsrv.dll).  
    – Destination file (the patched termsrv.dll).
- Stop the “Terminal Services” service.
- Take ownership of c:\\windows\\system32\\Termsrv.dll
- Give Administators full control of this file and rename it to Termsrv.dll.old
- Place the patched file in the system32 folder
- Restart “Terminal Services” service

<font color="#ff0000">EDIT: The download has been replaced with a new file containing the proper md5 hashes (see noone’s comments)</font>

[VPatch](http://www.tibed.net/vpatch/) file: [ Windows Server 2008 VPatch file (27744 downloads ) ](http://192.168.40.25:8081/download/windows-server-2008-vpatch-file/?tmstv=1726048918 "Version 1.0") (of termsrv.dll build 6.0.6001.18000 English language)

See also:

- [www.remkoweijnen.nl/blog/2008/08/31/patch-windows-2003-terminal-server-to-allow-more-than-2-concurrent-sessions/](http://192.168.40.25:8081/2008/08/31/patch-windows-2003-terminal-server-to-allow-more-than-2-concurrent-sessions/ "Terminal Server Patch for Server 2003")
- [www.remkoweijnen.nl/blog/2008/06/14/mutiple-concurrent-terminal-session-on-vista-sp1/](http://192.168.40.25:8081/2008/06/14/mutiple-concurrent-terminal-session-on-vista-sp1/ "Terminal Server Patch for Windows Vista")

Upcoming:

- Terminal Server Patch for Windows XP X64