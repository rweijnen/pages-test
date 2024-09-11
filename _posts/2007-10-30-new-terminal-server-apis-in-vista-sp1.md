---
id: 40
title: 'New Terminal Server API&#8217;s in Vista SP1'
date: '2007-10-30T22:19:25+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/30/new-terminal-server-apis-in-vista-sp1/'
permalink: /2007/10/30/new-terminal-server-apis-in-vista-sp1/
views:
    - '5863'
short-url:
    - 'http://bit.ly/gPFTI8'
categories:
    - Programming
    - 'Terminal Server'
    - Vista
---

The upcoming Vista SP1 promises 3 new Terminal Server API functions:

- [WTSConnectSession ](http://msdn2.microsoft.com/en-us/library/bb394782.aspx): Connects a Terminal Services session to an existing session on the local computer.
- [WTSStartRemoteControlSession](http://msdn2.microsoft.com/en-us/library/bb394783.aspx): Starts the remote control of another Terminal Services session.
- [WTSStopRemoteControlSession ](http://msdn2.microsoft.com/en-us/library/bb394784.aspx): Stops a remote control session.

If you look in the Windows 2008 beta you can see that the functions are already implemented (in WtsApi32.dll):

![Windows 2008 WtsApi32.dll Exports](http://192.168.40.25:8081/wp-content/uploads/2007/10/exports1.png)

But as you might know Wtsapi32 is just a bunch of wrappers around the actual API’s. If we take a look at (delayed) import sections we can see where the actual API’s are:

![Windows 2008 WtsApi32.dll Imports](http://192.168.40.25:8081/wp-content/uploads/2007/10/import1.png)

So in fact, nothing has changed! These API’s are in Winsta.dll since Windows 2000 but were (as everything else in Winsta.dll) never documented. Why? Maybe because the Terminal Server API was inherited from Citrix? If you know more about the reason let me know!

Oh I’d almost forget… If you can’t wait until SP1 to use shadowing in Vista why not use the commandline shadow tool I made for this article?

Screenshots:

![WTSShadow Screenshot 1](http://192.168.40.25:8081/wp-content/uploads/2007/10/wtsshadow11.png)

![WTSShadow Screenshot 2](http://192.168.40.25:8081/wp-content/uploads/2007/10/wtsshadow21.png)

[ WTSShadow.zip (2225 downloads ) ](http://192.168.40.25:8081/download/wtsshadow-zip/?tmstv=1726048918 "Version 1.0")

As always: It would be nice to leave a comment and take a look in the [Terminal Server Category](http://192.168.40.25:8081/topics/terminalserver/) for related articles.