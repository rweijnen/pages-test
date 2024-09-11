---
id: 29
title: 'How to launch a process in a Terminal Session'
date: '2007-10-20T22:45:24+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/20/how-to-launch-a-process-in-a-terminal-session/'
permalink: /2007/10/20/how-to-launch-a-process-in-a-terminal-session/
views:
    - '18934'
short-url:
    - 'http://bit.ly/dYTgbS'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
---

This is an often asked question but the solution is simple:

So how does it work?  
First we obtain the user’s primary access token with the [WtsQueryUserToken ](http://msdn2.microsoft.com/en-us/library/aa383840.aspx) API call. To call this function successfully, the calling application must be running within the context of the LocalSystem account and have the SE\_TCB\_NAME privilege (LocalSystem has this privilege by default). Since the function returns a primary acces token we can just pass this to CreateProcessAsUser and voila!

  
“&gt;  
I specified WtsGetActiveConsoleSessionID as the SessionID, this function retrieves the Terminal Services session currently attached to the physical console. The physical console is the monitor, keyboard, and mouse. Note that it is not necessary that Terminal Services be running for this function to succeed. In Windows 2000 the console is always session 0 but for Windows XP/2003/Vista this can be any number. So if you want to launch a process in session 5 the code is:  
“&gt;  
WtsQueryUserToken is defined in the unit JwaWtsApi32 and WtsGetActiveConsoleSessionID is defined in the unit JwaWinBase. Both units are part of the [Jedi APILibrary](http://jedi-apilib.sourceforge.net/).

This will launch notepad in the console session but offcourse you can replace the function WtsGetActiveConsoleSessionId with a specific SessionID. Just remember that only the system account is allowed to use WtsQueryUserToken. So you will need to launch this code from a service.

Do you find this article usefull, why not leave a comment then?