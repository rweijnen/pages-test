---
id: 60
title: 'More undocumented Terminal Server API&#8217;s'
date: '2007-11-09T22:42:58+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/11/09/more-undocumented-terminal-server-apis/'
permalink: /2007/11/09/more-undocumented-terminal-server-apis/
views:
    - '3163'
short-url:
    - 'http://bit.ly/i0JxJk'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
---

I added some more undocumented API’s to my Jwawinsta unit, the unit is now becoming a collection of the undocumented API’s in winsta.dll.

These are the functions I added:

- WinStationDisconnect
- WinStationGetProcessSid
- CachedGetUserFromSid (exported by utildll.dll)

I also added some more parts of the undocumented structure returned by WinStationQueryInformationW, it now contains:

- Session State
- WinStationName
- SessionId
- ConnectTime
- DisconnectTime
- LastInputTime
- LogonTime
- OutgoingFrames
- OutgoingBytes
- OutgoingCompressedBytes
- IncomingCompressedBytes
- IncomingFrames
- IncomingBytes
- Domain
- Username
- CurrentTime