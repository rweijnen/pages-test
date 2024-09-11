---
id: 497
title: 'Citrix Workspace Control not working on HP t5540'
date: '2009-12-03T09:33:58+01:00'
author: 'Remko Weijnen'
excerpt: '<p>Citrix Web Interface Client Detection fails on Windows CE with HP Thin Client t5540 and Citrix Workspace control is not working. The reconnect and disconnect buttons are unavailable.</p>'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=497'
permalink: /2009/12/03/citrix-workspace-control-not-working-on-hp-t5540/
views:
    - '5036'
short-url:
    - 'http://bit.ly/fzUDmR'
categories:
    - Citrix
---

Yesterday I was troubleshooting why Workspace Control was not available on an HP t5540 (Windows CE) Thin Client. This was a Citrix Xenapp 5 environment on Server 2008. When logging in through the Web Interface from the Thin Client’s browser we noticed two things: Client Detection failed and the Reconnect and Disconnect buttons were not available: [![NoButtons](http://192.168.40.25:8081/wp-content/uploads/2009/12/nobuttons-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/12/nobuttons.png) I looked into the files in the webinterface folder (*wwwroot/Citrix/XenApp*)and searched for workplace and reconnect. I determined that the Client Detection is done in the **nativeClientDetect.js** (*app\_data/clientDetection/clientscripts*). But what I saw was very strange:

“&gt;

This suggests that on Windows CE with Internet Explorer the Native Client Detection always returns true! This lead me to think that something was misconfigured on the Thin Client rather than in the Webinterface. I logged in as Administrator on the Thin Client and checked the Browser’s settings and found the Culprit there: by default the User Agent string is configured as “Same as Windows XP”:

[![UserAgent1](http://192.168.40.25:8081/wp-content/uploads/2009/12/useragent1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/12/useragent1.png)

I set the User Agent string to “Default (Windows CE)”:

[![UserAgent2](http://192.168.40.25:8081/wp-content/uploads/2009/12/useragent2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/12/useragent2.png)

Then I started the WebInterface again: [![WithButtons](http://192.168.40.25:8081/wp-content/uploads/2009/12/withbuttons-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/12/withbuttons.png)

That’s much better!