---
id: 221
title: 'Locking a workstation &#8211; part 2'
date: '2008-12-23T17:13:37+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/23/locking-a-workstation-part-2/'
permalink: /2008/12/23/locking-a-workstation-part-2/
views:
    - '10290'
short-url:
    - 'http://bit.ly/hgDOGP'
categories:
    - Delphi
    - Programming
---

In [part 1](http://192.168.40.25:8081/2008/12/13/locking-a-workstation-part-1/) I showed how *winlogon.exe* registers its process and main window handle.

In the *SasCreate* function, winlogon.exe registers hotkeys like this:  
“&gt;  
Did you notice the MOD\_SAS constant? It’s an undocumented value which can be successfully used only by *the logon process* (read [part 1](http://192.168.40.25:8081/2008/12/13/locking-a-workstation-part-1/)). As you see, ANY hotkey combination can be used as SAS (Secure Attention Sequence) combination; a special behavior of SAS is that it enables input after a call of *BlockInput*, so it cannot be recorded or played back by Journal Hook and cannot be simulated with the *SendInput* API.

So, how we can use it? *winlogon.exe* runs on the secure *Winlogon* desktop. So we need to be running as system! At first, we need to find the target window. I do not want to bother with *SetThreadDesktop*, so we’ll just do a cycle in *EnumDesktopWindows*:  
“&gt;  
Now we can send the messages directly:  
“&gt;  
Windows XP allows you even to unlock the workstation by sending a message:  
“&gt;  
Windows 2000 cannot be unlocked this way for now. Maybe… later? 😉

[ Winstation Locker (2026 downloads ) ](http://192.168.40.25:8081/download/winstation-locker/?tmstv=1726048918 "Version 1.3")

You can download the sample program with included sources. As a bonus, it allows remote execution on the target machine.

P.S. In Windows Vista and higher the logon mechanism has been changed to RPC interfaces, so this program will NOT work on these platforms.