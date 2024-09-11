---
id: 140
title: 'Terminal Server Internals'
date: '2008-11-19T17:55:39+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/11/19/terminal-server-internals/'
permalink: /2008/11/19/terminal-server-internals/
views:
    - '49376'
short-url:
    - 'http://bit.ly/ggSlMm'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
---

Hello, my name is Danila Galimov and i will write here sometimes 🙂

My first post is about communications between Terminal Server sessions and Terminal Server service process (termsrv.exe/dll). Terminal Server service needs to communicate with each session for many tasks, such as sending window message, getting message reply and so on. So, on init, Terminal Server creates a **SmSsWinStationApiPort** port in global namespace and runs a few WinStationLpcThread threads, which are listening on port and are used to process port messages. When csrss.exe is started, it parses its command line, which usually looks like:

> %SystemRoot%\\system32\\csrss.exe ObjectDirectory=\\Windows SharedSection=4096,4096,1024 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=**winsrv***:UserServerDllInitialization,3 ServerDll*=winsrv:*ConServerDllInitialization,2 ProfileControl=Off MaxRequestThreads=16*

and loads the required dlls (winsrv.dll in our case). Initialization of winsrv.dll creates a thread, which connects to **SmSsWinStationApiPort** port and does the loop for processing Terminal Server messages until it receives WinStationTerminate message.

We’ll try to fool Terminal Server and send some messages to the port directly. At first, we need to connect to the port. A structure of 3 DWORDS is used to for connection:  
“&gt;  
Let’s do the connection:  
“&gt;  
Now we’re connected. We need to prepare and send a message. Each message (after standard PORT\_MESSAGE header) has common header:  
“&gt;  
For creating idle winstation, 2 fields are added to this message: WinstationName, which is optional, and OutSessionId, which returns, on success, id of recently created winstation. So the whole structure will look like this:  
“&gt;  
So now we can send this message and check for the results:  
“&gt;  
Disconnect and Reset (logoff messages) are very simple: just target session id is added to the structure.  
“&gt;  
That’s all! Now we can just add command line parameters parsing features, and it’s ready for usage!

As only system is allowed to access SmSsWinStationApiPort port, we need to run our program as system. You can use **[RunAsSys](http://blog.delphi-jedi.net/2008/05/08/runassys-10-preview/)** program for that propose.

Unfortunately, in windows vista/2008 the Terminal Server internal functions have been changed, so this program (IdleWinsta) does not work on vista.

[ IdleWinsta.zip (1675 downloads ) ](http://192.168.40.25:8081/download/idlewinsta-zip/?tmstv=1726048918 "Version 1.0")