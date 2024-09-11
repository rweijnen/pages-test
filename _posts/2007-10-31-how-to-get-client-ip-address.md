---
id: 45
title: 'How to get Client IP Address?'
date: '2007-10-31T11:10:10+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/31/how-to-get-client-ip-address/'
permalink: /2007/10/31/how-to-get-client-ip-address/
views:
    - '13264'
short-url:
    - 'http://bit.ly/e7qqFT'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
---

When a client is connected to a Terminal Server Session you can use the Terminal Server API to retrieve the client’s local IP address.

Start by enumerating all sessions with [WtsEnumerateSessions](http://msdn2.microsoft.com/en-us/library/aa383833.aspx) and then for each session get the ClientAddress with a call to [WTSQuerySessionInformation](http://msdn2.microsoft.com/en-us/library/aa383838.aspx) with the WTSClientAddress parameter. Sound simple, no?

WTSQuerySessionInformation returns a pointer to a [WTS\_CLIENT\_ADDRESS](http://msdn2.microsoft.com/en-us/library/aa383857.aspx) structure. You need to know that the IP address is located at on *offset of 2 bytes* in the Address member of WTS\_CLIENT\_ADDRESS.

So here’s a sample:

  
“&gt;  
  
Screenshot:  
![Client IP demo app screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/11/clientipscreenshot.png)  
[ ClientIP.zip (3423 downloads ) ](http://192.168.40.25:8081/download/clientip-zip/?tmstv=1726048918 "Version 1.0")