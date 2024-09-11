---
id: 517
title: 'displayNotification: Out of memory error when starting Delphi 2010'
date: '2010-02-17T11:16:29+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/02/17/displaynotification-out-of-memory-error-when-starting-delphi-2010/'
permalink: /2010/02/17/displaynotification-out-of-memory-error-when-starting-delphi-2010/
views:
    - '8735'
short-url:
    - 'http://bit.ly/fUjl8G'
categories:
    - Delphi
---

Delphi 2010 crashed when starting, it was clear that this was happening when opening the welcome page.

Just before the crash an error message “Message from webpage, displayNotification: Out of memory” was displayed.

[This post](https://forums.codegear.com/thread.jspa?threadID=30193&tstart=0) on the Embarcadero Developer Network which was one of the first hits in Google showed that the solution was to clear Internet Explorer’s Browsing History (Temporary Internet Files). This fixed it for me.