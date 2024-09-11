---
id: 437
title: 'Small LaunchRDP Update'
date: '2009-11-06T22:44:27+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/11/06/small-launchrdp-update/'
permalink: /2009/11/06/small-launchrdp-update/
views:
    - '628'
short-url:
    - 'http://bit.ly/fiDs4r'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
---

For a long time now people have asked for a version of [LaunchRDP](http://192.168.40.25:8081/2007/10/17/launch-rdp-from-commandline/) that includes the Connection Bar. The original version was originally written for a very specific purpose and I never anticipated so many people would want to use it. But it seems that a lot of people like the Connection Bar (I hate it, especially with sessions in sessions, so that’s why I am using [RDPWithLocalTaskbar](http://192.168.40.25:8081/2008/11/02/rdp-session-with-local-taskbar-visible/)).

I really wanted to do a complete rewrite of LaunchRDP and make lots of improvements, especially to the Command Line Parameters but I never found time to get that started. So I finally decided to publish an interim version with the Connection Bar instead (and thus full screen). I have moved over to Delphi 2010 so I had to make a few minor adjustments to make it compile but the downside of Delphi 2010 is that the resulting exe is much bigger (92 KB -&gt; 306 KB).

Hope you like it.

[ LaunchRDP2.zip (8584 downloads ) ](http://192.168.40.25:8081/download/launchrdp2-zip/?tmstv=1726048918 "Version 1.2")