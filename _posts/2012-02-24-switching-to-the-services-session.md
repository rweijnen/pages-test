---
id: 2464
title: 'Switching to the Services Session'
date: '2012-02-24T14:49:21+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/24/switching-to-the-services-session/'
permalink: /2012/02/24/switching-to-the-services-session/
views:
    - '3692'
short-url:
    - 'http://bit.ly/wZNhCO'
categories:
    - Delphi
    - script
    - Vista
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
---

Just read a tweet from [@andyjmorgan](http://twitter.com/andyjmorgan) about [Interactive Service Detection](http://twitter.com/andyjmorgan/statuses/173033102514462720). This made me remember that it’s possible to switch to the Session 0 with an undocumented api in winsta.dll.

For this API to work you must have the Interactive Services Detection (UI0Detect) service running.

The signatures are:

 “&gt;  
Since these procedures require no arguments we can also call them from the commandline:

“&gt;

When you switch to the services desktop you will automatically get a Dialog Box that enables you to Return to the normal desktop (this just calls WinStationRevertFromServicesSession).

[![The program is no longer requesting attention | The message might have been one that required no action, such as a quick status notification. Windows will notify you if the problem reappears | Return now](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb20.png "Interactive Services Detection")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image20.png)

Or you can manually return with this commandline:

“&gt;