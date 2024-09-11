---
id: 65
title: 'Undocumented API&#8217;s from Utildll'
date: '2007-12-09T11:05:04+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/12/09/undocumented-apis-from-utildll/'
permalink: /2007/12/09/undocumented-apis-from-utildll/
views:
    - '1597'
short-url:
    - 'http://bit.ly/gbIiES'
categories:
    - Citrix
    - Delphi
    - Programming
    - 'Terminal Server'
---

Several of Microsoft’s Terminal Server tools use undocumented API’s from Utildll.dll. For instance Terminal Server Admin uses it to get a localised connect state string and to format time strings like idle time, logon time etc.

Functions below seems to be the most usefull ones, I will add those to the JwaWinsta unit:

- function StrConnectState (returns localised string of the given ConnectState)
- DateTimeString (returns formatted date timestring according to user’s timesettings)
- function CurrentDateTimeString (like the name says)
- function ElapsedTimeString (returns formatted string with elapsed time as in TSAdmin)
- function CalculateElapsedTime (returns elapsed time in seconds)
- function CalculateDiffTime (returns time difference in seconds)
- function GetUnknownString (returns localised “unknown” string)

Citrix has it’s own version of this DLL called CUtildll.dll which is similar but uses (Citrix) MUI for localising strings.