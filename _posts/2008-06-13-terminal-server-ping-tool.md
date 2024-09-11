---
id: 100
title: 'Terminal Server Ping Tool'
date: '2008-06-13T13:34:46+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/06/13/terminal-server-ping-tool/'
permalink: /2008/06/13/terminal-server-ping-tool/
views:
    - '10203'
short-url:
    - 'http://bit.ly/eEhMqV'
categories:
    - Citrix
    - 'Terminal Server'
tags:
    - LinkedIn
---

A while ago I included a new undocumented API into my JwaWinsta unit which is called WinStationServerPing. This API “pings” a Terminal or Citrix server and verifies that Terminal Server is up and running. It is not the same as a regular networking ping! This API actually makes a connection to a (remote) Terminal Server and verifies that Terminal Server runs and accepts connections.

I wrote a small cmdline tool that uses this API to ping a Terminal Server which can be used to quickly determine if a Terminal Server is up and running. I named it WTSPing.

So how does it work? Open up a command prompt (Start -&gt; Run -&gt; cmd) and type WTSPing /? to see the help:

WTSPing (c) Remko Weijnen 2008, [www.remkoweijnen.nl](https://www.remkoweijnen.nl)  
Pings a Terminal Server with the WinStationServerPing API.

WTSPing \[/SERVER:servername\] \[-n count\]

/SERVER:servername Specifies the Terminal server (default is current).  
-n count Number of echo requests to send (default is 3).

If you just type WTSPing it will ping the local Terminal Server 3 times:

![wtsping1](http://192.168.40.25:8081/wp-content/uploads/2008/06/wtsping1.png)

You can specify the number of pings with the -n parameter (eg -n 5). If you want to ping a remote server specify the server name with the /SERVER: parameter (eg /SERVER:MYSERVER).

Please note that Vista and Server 2008 do not support this API call from or to remote systems.

Have fun with it and let me know if you like it!

Download: [ WTSPing (2144 downloads ) ](http://192.168.40.25:8081/download/wtsping/?tmstv=1726048918 "Version 1.0")