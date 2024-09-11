---
id: 81
title: 'Vista SP1 changes to Terminal Server API'
date: '2008-03-02T15:31:55+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/03/02/vista-sp1-changes-to-terminal-server-api/'
permalink: /2008/03/02/vista-sp1-changes-to-terminal-server-api/
views:
    - '4995'
short-url:
    - 'http://bit.ly/hjiaxe'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
    - Vista
---

In a previous [article](http://192.168.40.25:8081/2007/12/19/why-tsadmin-crashes-on-windows-vista/) I wrote about changes in utildll in vista that breaked compatibality for Terminal Server. Even though release notes for Service Pack 1 don’t indicate changes or fixes in this area my testing shows that Microsoft has taken over the Windows 2008 implementation of utildll to Vista.

This is a good thing, because applications depending on utildll work again. I have updated JwaWinsta for SP1, all Vista versions of the utildll functions are renamed to VistaRTM and all Safe functions were updated to check for SP1. This means that the Safe functions can be used on all OS versions and Service packs! You are strongly advised to use only the Safe functions.

Some observations with SP1:

- I quickly tested TSAdmin as well and it seems to work again, only noticable flaw is that the console sessions returns an idle time of 17642 days (Reported Last Input Time is 01-01-1601 but utildll’s ElapsedTimeString function doesn’t account for dates this long in the past).
- WTSApi32.dll contains some new functions like WTSStartRemoteControlSession and WTSStopRemoteControlSession (which are wrappers to WinStationShadow).
- The WTSWaitSystemEvent bug I wrote about [earlier](http://192.168.40.25:8081/2008/01/25/using-wtswaitsystemevent/) is still present. I advise to check for winsta.dll version &gt;= 6.0.6000.20664 in code when using this API and advise user to install the Hotfix.

<font color="#ff0000">Update:</font> I just tried to install hotfix KB941561 but this fail with the error: The update does not apply to your system. If you do want to get this bug fixed you need to manually replace winsta.dll (take ownership and set permissions to full control).[ winsta.dll from hotfix KB941561 (X86) (2477 downloads ) ](http://192.168.40.25:8081/download/winsta-dll-from-hotfix-kb941561-x86/?tmstv=1726048918 "Version 6.0.6000.20664")