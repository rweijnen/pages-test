---
id: 149
title: 'Executing a Fast User Switch programmatically &#8211; part 1'
date: '2008-11-26T15:23:37+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/11/26/executing-a-fast-user-switch-programmatically-part-1/'
permalink: /2008/11/26/executing-a-fast-user-switch-programmatically-part-1/
views:
    - '31395'
short-url:
    - 'http://bit.ly/hKUQHV'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
---

I think many of you have got experience with multiple Terminal Server Sessions in windows XP, also called Fast User Switching (FUS). Let’s get inside this cool feature.

How does FUS work? Each session has its own winlogon.exe. It draws the **same** interface which looks like the screenshot below:

[](http://192.168.40.25:8081/wp-content/uploads/2008/11/default.png)

[![multiple-users-logged-on](http://192.168.40.25:8081/wp-content/uploads/2008/12/multiple-users-logged-on-2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/multiple-users-logged-on-2.png)

1\) When the current session is empty (no user is logged in), and you clicking on a user name, the system just logs you on to the current session. This case is very clear.

| [![user-new](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-2.png) | [![user-new-loggin-in](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-loggin-in-6-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-loggin-in-6.png) | [![user-new-logged-on](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-logged-on-4-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-logged-on-4.png) |
|---|---|---|

2\) If you click on an already logged in user, you are connected to the target user session using the *WinStationConnect* API. It’s prototype is  
“&gt;  
The current session is just disconnected (if it contains logged on user) or even eliminated (if it doesn’t contain a logged on user and if its not session number 0, which is never disposed).

| [![new-session-old-user](http://192.168.40.25:8081/wp-content/uploads/2008/12/new-session-old-user-4-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/new-session-old-user-4.png) | [![user-new-logged-on](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-logged-on-5-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-logged-on-5.png) |
|---|---|

3\) But what happens when you click on a user other than yourself and you are already logged into the current session? Visually it looks almost the same – but look, there is a quick blank switch (you may not notice this as it disappears very quickly). This is a session disconnect and re-connect:

| [![user-new](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-3-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-3.png) | [![blank-screen](http://192.168.40.25:8081/wp-content/uploads/2008/12/blank-screen-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/blank-screen-1.png) | [![user-new-loggin-in](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-loggin-in-7-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-loggin-in-7.png) | [![user-new-logged-on](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-logged-on-6-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/user-new-logged-on-6.png) |
|---|---|---|---|

So what is happening? After verifying your credentials, msgina.dll starts a *Credential Server*, and disconnects the current session from the console. Because the console session is an auto-reconnected session (if you disconnect it, you’ll get a new session attached to the console (Terminal Server has even a “hardcode” in termsrv.dll to reconnect console session faster, if the current gina is msgina.dll)), a new session will be created and attached to the console. Msgina.dll perfoms some checks on startup: if it’s attached to the console session, and the Credential Server is running, it will read the credentials, and perform the login as if the credentials were typed manually.

But how we can use this programmatically? Msgina.dll has an undocumented, exported, function named *ShellStartCredentialServer*. It’s exported from msgina.dll by ordinal #23.

It’s full prototype is  
“&gt;  
The function doesn’t validate anything; it just packs the data, writes it to a pipe and disconnects the console session. However, if the credentials are invalid, you will not get FUS working, so it is a good idea to validate them before sending. The domain parameter is ignored (it seems that FUS was initially designed to work with domain users as well, however this support was apparently dropped on some stage). If you pass valid credentials data, but the user is already logged on, you will just switch to this session. So you can use this function whenever you need to switch the console. You can even call this function from an RDP session but it will still affect the console session only.

This function will fail if the Welcome screen is disabled; in that case you’ll just get your session disconnected and locked. The caller is required to have SE\_TCB\_NAME privilege (“Act as part of the operating system”) enabled. Actually, this is not enough since the default security of the registry hive *HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon\\Credentials* allows read and write access to system only.

In the [next part](http://192.168.40.25:8081/2008/11/26/executing-a-fast-user-switch-programmatically-part-2/) I’ll show you how to create your own implementation of the Credential Server.