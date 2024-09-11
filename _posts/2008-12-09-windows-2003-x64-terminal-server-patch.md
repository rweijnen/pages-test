---
id: 212
title: 'Windows 2003 X64 Terminal Server Patch'
date: '2008-12-09T13:53:39+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/09/windows-2003-x64-terminal-server-patch/'
permalink: /2008/12/09/windows-2003-x64-terminal-server-patch/
views:
    - '31248'
short-url:
    - 'http://bit.ly/i0ypiF'
categories:
    - General
    - 'Terminal Server'
---

A while ago I published a [patch for Windows 2003 Terminal Server](http://192.168.40.25:8081/2008/08/31/patch-windows-2003-terminal-server-to-allow-more-than-2-concurrent-sessions/) that allows more than 2 concurrent sessions in Remote Administration mode.

Today I publish the same patch but for Windows Server 2003 X64. The patched function (CRAPolicy::Logon) is the same as in the original patch.

The patch was tested with build 5.2.3790.3959, English language.

This patch uses a new patch engine which I described [here](http://192.168.40.25:8081/2008/12/09/new-universal-patch-method/). If you have tested the patch on a different language and/or build of termsrv.dll please report back if it’s working.

[ Windows 2003 X64 Terminal Server Patch (4222 downloads ) ](http://192.168.40.25:8081/download/windows-2003-x64-terminal-server-patch/?tmstv=1726048918 "Version 1.0")