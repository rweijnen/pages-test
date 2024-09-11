---
id: 14
title: 'Launch RDP from commandline'
date: '2007-10-17T22:47:53+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/17/launch-rdp-from-commandline/'
permalink: /2007/10/17/launch-rdp-from-commandline/
views:
    - '77225'
short-url:
    - 'http://bit.ly/gPlDuj'
categories:
    - 'Terminal Server'
---

![Command Prompt Icon](http://192.168.40.25:8081/wp-content/uploads/2007/10/cmd1.png)A little while ago I wrote a little commandline tool that starts an RDP connection (with mstsc.exe) because mstsc doesn’t allow you to use the login credentials (username, password) as commandline arguments.

Arguments are:

1. Servername (string)
2. Port (integer, usually 3389)
3. Username (string)
4. Domain (string)
5. Password (string)
6. Console (integer, specify 0 for false and 1 for true)
7. RedirectDrives (integer, specify 0 for false and 1 for true)
8. RedirectPrinters (integer, specify 0 for false and 1 for true)

Examples:  
LaunchRDP MyServer 3389 User Domain Password 0 0 0

For v6 Terminal Server client it also fixes the annoying “do you trust the computer dialog” (shown below) by adding a registry key for the computer you connect to. So next time you connect you won’t be bothered!

[![Annoying popup](http://192.168.40.25:8081/wp-content/uploads/2007/10/trustdialog.thumbnail1.jpg)](http://192.168.40.25:8081/wp-content/uploads/2007/10/trustdialog1.jpg "Annoying popup")

[ LaunchRDP (18105 downloads ) ](http://192.168.40.25:8081/download/launchrdp/?tmstv=1726048918 "Version 1.0")

<span style="color: #ff0000;">Note: Well, lot’s of people have asked for minimize/maximize buttons and/or the connection bar. No one has made a donation (through the Paypal donate button in the right most sidebar).</span>

<span style="color: #ff0000;">So I decided to make this a little challenge: **when I receive at least 5 donations I will create an update with the connection bar in it!**</span>

<span style="color: #ff0000;">**/Update: An updated version is [here](http://192.168.40.25:8081/2009/11/06/small-launchrdp-update/)** </span>