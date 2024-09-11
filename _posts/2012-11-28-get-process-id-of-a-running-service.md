---
id: 2767
title: 'Get Process Id of a running Service'
date: '2012-11-28T12:20:40+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2767'
permalink: /2012/11/28/get-process-id-of-a-running-service/
views:
    - '2809'
short-url:
    - 'http://bit.ly/10UBnVY'
categories:
    - Delphi
tags:
    - Delphi
    - Service
---

Sometimes you need to know the Process Id (PID) of a running service. Since Windows 2003 you can use the tasklist.exe tool with the /SVC switch. But how to do this programmatically?

The [QueryServiceStatusE](http://msdn.microsoft.com/en-us/library/ms684941%28VS.85%29.aspx)x API returns a [SERVICE\_STATUS\_PROCESS](http://msdn.microsoft.com/en-us/library/ms685992(v=vs.85).aspx) structure that contains the PID.

The code is not very complicated:

 “&gt;