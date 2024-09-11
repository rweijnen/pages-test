---
id: 98
title: 'Script to logoff all Terminal Server sessions'
date: '2008-06-09T13:48:50+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/06/09/script-to-logoff-all-terminal-server-sessions/'
permalink: /2008/06/09/script-to-logoff-all-terminal-server-sessions/
views:
    - '14090'
short-url:
    - 'http://bit.ly/igt6sQ'
categories:
    - Uncategorized
---

I needed a script to logoff all running Terminal Server sessions in order to rollout an install package. As you might know there is a commandline tool to logoff a session, it’s called logoff.exe.

These are the commandline options:

LOGOFF \[sessionname | sessionid\] \[/SERVER:servername\] \[/V\]

sessionname The name of the session.  
sessionid The ID of the session.  
/SERVER:servername Specifies the Terminal server containing the user  
session to log off (default is current).  
/V Displays information about the actions performed.

No option to logoff all sessions is there?

On a Terminal Server there is a special session called the Listener session, you can see it with TSAdmin in the sessions tab:![Listener](http://192.168.40.25:8081/wp-content/uploads/2008/06/listener-1.png)

A Listener is associated with a protocol (Microsoft RDP by default) and is used to setup new sessions. If you logoff a Listener session it will logoff all session that were created through it. Great, just what we need!

So Logoff 65536 will do the trick? Let’s try:

![logoff](http://192.168.40.25:8081/wp-content/uploads/2008/06/logoff.png)

So Logoff is smart enough to ask for confirmation, we can prevent this by using the following commandline:

Echo Y ! Logoff 65536