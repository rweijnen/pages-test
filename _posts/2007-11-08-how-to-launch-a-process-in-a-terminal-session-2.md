---
id: 56
title: 'How to launch a process in a Terminal Session #2'
date: '2007-11-08T13:48:18+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/11/08/how-to-launch-a-process-in-a-terminal-session-2/'
permalink: /2007/11/08/how-to-launch-a-process-in-a-terminal-session-2/
views:
    - '9188'
short-url:
    - 'http://bit.ly/ezKWNN'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
---

![Command Prompt Icon](http://192.168.40.25:8081/wp-content/uploads/2007/11/cmd.png)A little while ago I wrote an article on launching a process in another Terminal Session (<http://192.168.40.25:8081/2007/10/20/how-to-launch-a-process-in-a-terminal-session/>).

The article didn’t have a demo app yet so I’ve attached it here.

***How do you use it?***

It’s a commandline tool and it’s called RunInSession. You need to specify at least the SessionId in which you want to launch the process and which process you want to launch. Optional is servername if you want to launch on a remote server. If you run it without parameters a dialog with possible parameters is shown:

![Run In Session Screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/11/runinsessionscreenshot.png)

Currently supported OS versions are Windows XP, 2003, Vista and 2008.

***How does it work?***

The program needs to run in the context of the Localsystem user, therefore it temporarily installs itselfs as service and start itsself. With the [WTSQueryUserToken ](http://msdn2.microsoft.com/en-us/library/aa383840.aspx)it obtains the Primary User token of the requested Terminal Session. Finally the process is launched with [CreateProcessAsUser](http://msdn2.microsoft.com/en-us/library/ms682429.aspx) and the service deletes itsself.

[ RunInSession.zip (3240 downloads ) ](http://192.168.40.25:8081/download/runinsession-zip/?tmstv=1726048918 "Version 1.0")