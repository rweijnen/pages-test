---
id: 1344
title: 'Using Fast User Switching on domain XP computers'
date: '2011-01-30T20:42:20+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1344'
permalink: /2011/01/30/using-fast-user-switching-on-domain-xp-computers/
short-url:
    - 'http://bit.ly/gJIK37'
views:
    - '15844'
categories:
    - script
    - 'Terminal Server'
    - 'Windows Internals'
    - 'Windows XP'
---

As you may know, Fast User Switching (FUS) is not available (disabled) on Windows XP computers joined to a domain, Microsoft confirms this in [kb280758](http://support.microsoft.com/kb/280758).

However, Microsoft doesn’t tell us there’s an undocumented registry value that allows us to have FUS when joined to a domain!

To enable FUS you need to set the **DWORD** registry value *HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon\\ForceFriendlyUI*.

It can also be set by Group Policy at *HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Policies\\System.*

When the value is set to 1, and *LogonType* key is also set to 1, it allows you to use a Friendly UI on a computer joined in a domain:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb22.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image22.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb23.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image23.png)

You can even set *AllowMultipleTSSessions* value to 1, which will allow you to have multiple users logged on!

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb24.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image24.png)

The only problem is that the “Friendly UI” doesn’t always allow to logon as a domain user. Only when no-one is logged on you can press the **Ctrl+Alt+Delete** sequence twice to get to the Classis Logon Screen where you can logon with Domain Credentials:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb25.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image25.png)

If we disconnect the console session, for example, by using the [tsdiscon.exe](http://msdn.microsoft.com/en-us/library/cc770592(v=ws.10).aspx) tool we are also able to go to the Classic Logon Screen.

I’ve created a few files, which you can place, say to the All Users Desktop (*CSIDL\_COMMON\_DESKTOPDIRECTORY)* folder (i.e. “C:\\Documents and Settings\\All Users\\Desktop”) to make them available to all users:

There is also a slightly modified version of “[Switch User](http://192.168.40.25:8081/2008/11/26/executing-a-fast-user-switch-programmatically-part-2/)” application, which now adjust the value of *AllowMultipleTSSessions, LogonType* and *ForceFriendlyUI* keys programmatically (if required) for the time needed to logon the user and then reverts them back.

It also supports a **/Server=ServerName** option, which allows you to execute it on another machine.

Please note that you cannot log on domain users in this way!

[ Switch User 1.21 (2146 downloads ) ](http://192.168.40.25:8081/download/switch-user-1-21/?tmstv=1726048919 "Version 1.21")

P.S. “Bugs”

1\) If you’ve logged on as a domain user, and are set the logon type to friendly UI, be careful: if you lock your current session, you won’t be able to return to it in a normal way – “Friendly UI” doesn’t support domain logged users and you just won’t be listed in a users list! But, of course, you can log in as a different user, start Task Manager, click on “Users” tab, and connect to your original session manually. You can also use “”Turn Friendly UI Off.bat” file to switch Friendly UI off and be able to lock your workstation normally.

2\) If you connect via RDP session, you’ll still get a “Friendly UI”. It won’t be fully functional as it was not designed to be used from RDP session.

3\) Explorer doesn’t show you “Switch user” option when you try to logoff the current user, but you can lock workstation by using “Window+L” shortcut or use the batch file above.