---
id: 684
title: 'Automatically Accept Shadow Request'
date: '2010-10-22T17:04:00+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/10/22/automatically-accept-shadow-request/'
permalink: /2010/10/22/automatically-accept-shadow-request/
views:
    - '3051'
short-url:
    - 'http://bit.ly/h8TGFh'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
---

When you request Shadow (Remote Control) of a Remote Desktop (Terminal Server) or Citrix session the user gets a Dialog where he can Accept or Deny the Shadow Request.

It looks something like this:

![ShadowRequest](http://192.168.40.25:8081/wp-content/uploads/2010/10/shadowrequest.png)

It’s possible to change the default settings and remove the need for this permission but I think this is a bad idea since it violates the user’s privacy.

But sometimes it would be convenient to automatically accept, for instance for when a user is away or when you want to shadow a session that is “yours” but runs under another account.

I wrote a tool to do just that 😀  
It’s a commandline tool that must be run in the target session (tip: use my [RunInSession](http://192.168.40.25:8081/2007/11/08/how-to-launch-a-process-in-a-terminal-session-2/) tool). When you launch it, it will “listen” in a console window:

[![ShadowAccept1](http://192.168.40.25:8081/wp-content/uploads/2010/10/shadowaccept1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/shadowaccept1.png)

When a Shadow Request appears it is automatically Accepted and this is logged in the Console Window:

[![ShadowAccept2](http://192.168.40.25:8081/wp-content/uploads/2010/10/shadowaccept2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/shadowaccept2.png)

As usual have fun with it!

[ Shadow Accept (1778 downloads ) ](http://192.168.40.25:8081/download/shadow-accept/?tmstv=1726048919 "Version 1.0")