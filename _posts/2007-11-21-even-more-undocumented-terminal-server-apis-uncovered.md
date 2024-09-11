---
id: 62
title: 'Even more undocumented Terminal Server API&#8217;s uncovered'
date: '2007-11-21T18:18:37+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/11/21/even-more-undocumented-terminal-server-apis-uncovered/'
permalink: /2007/11/21/even-more-undocumented-terminal-server-apis-uncovered/
views:
    - '4348'
short-url:
    - 'http://bit.ly/gaGrzx'
categories:
    - Uncategorized
---

I was contact by Danila Galimov a while ago because he was working with my JwaWinsta unit. Together we were able (and are still working on) uncovering more of the undocumented API’s in winsta.dll.

 We found several new classes for WinStationQueryInformationW that return lots of information:

- The user’s password (under special circumstances).
- The Windows Product ID (server and client’s).
- Client Info such as Timezone information.

We got the following API’s working:

- WinStationGetAllProcesses
- WinStationGetTermSrvCountersValue (“QWinsta /Counter”)
- WinStationFreeGAPMemory
- WinStationSendMessage
- WinStationCloseServer
- WinStationDisconnect
- WinStationReset
- WinStationShutdownSystem

Further testing is needed to determine if the functions work on different OS versions and produce the same results.