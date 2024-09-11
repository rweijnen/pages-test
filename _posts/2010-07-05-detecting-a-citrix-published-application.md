---
id: 615
title: 'Detecting a Citrix Published Application'
date: '2010-07-05T21:48:28+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=615'
permalink: /2010/07/05/detecting-a-citrix-published-application/
views:
    - '2822'
short-url:
    - 'http://bit.ly/f4jUhQ'
categories:
    - Citrix
---

While browsing through my old projects folder I found a little commandline tool that I wrote about a year ago. I needed to detect a certain published application on a Citrix environment in the loginscript.

The tool detect the current Citrix published applicationname or if you are running Terminal Server aka Remote Desktop Services the Initial Program name and stores this in an environment variable (APPNAME).

There are no parameters and there are no special dependancies (such as MFCom).

[ CtxPubApp (2474 downloads ) ](http://192.168.40.25:8081/download/ctxpubapp/?tmstv=1726048918 "Version 1.0")