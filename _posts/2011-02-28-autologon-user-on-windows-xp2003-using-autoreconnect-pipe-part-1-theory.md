---
id: 1430
title: 'Autologon user on Windows XP/2003 using AutoReconnect pipe &#8211; part 1 (theory)'
date: '2011-02-28T10:46:29+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1430'
permalink: /2011/02/28/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-1-theory/
short-url:
    - 'http://bit.ly/dRYhS1'
views:
    - '2737'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows Internals'
    - 'Windows XP'
---

Windows XP introduced the ability to use Fast User Switching (FUS from here on), which is implemented using *Terminal Services*.

But in some cases (i.e. when FUS is not enabled, or when you connect to the console in Windows 2003 server), the Winlogon process in an RDP session needs to transfer credentials to Session 0.

Although not documented in MSDN, the process of transferring credentials is described by Keith Brown in the June 2005 issue of MSDN magazine: [Customizing GINA, Part 2](http://msdn.microsoft.com/en-us/magazine/cc163786.aspx).

[WlxQueryConsoleSwitchCredentials](http://msdn.microsoft.com/en-us/library/aa380577(v=vs.85).aspx) and [WlxGetConsoleSwitchCredentials](http://msdn.microsoft.com/en-us/library/aa380563(v=vs.85).aspx) are used in the transfer with the semi-documented **WLX\_SAS\_TYPE\_AUTHENTICATED** SAS code constant.

Internally, *winlogon.exe* uses a Named Pipe, **\\\\.\\Pipe\\TerminalServer\\AutoReconnect,** to implement both of these functions.

The pipe format is described in this structure:  
  
“&gt;  
The structure mostly repeats [WLX\_CONSOLESWITCH\_CREDENTIALS\_INFO\_V1\_0](http://msdn.microsoft.com/en-us/library/aa381126(v=vs.85).aspx), but you can see some changes in ordering and there are additional fields: **SourceProcessId** (which is used to duplicate the handle) and **TotalLength** (which shows the total length of the structure) have been added.

All pointers within the structure should be normalized, i.e. should be not pointers, but *offsets* from the beginning of the structure. This structure is valid for both 32 and 64 bit windows (of course, with alignment and correct sizes respected).

As you can see, there are some strange things, like declaring pointers as DWORD (**PrivateData**), declaring **UserFlags** as SIZE\_T, which is platform dependent, while in the original structure it’s just ULONG, and so on.

You might think that the **DynamicData** should be really dynamic, but it isn’t: *Winlogon* always creates a variable of **AUTOLOGON\_BUFFER** type in it’s stack or globally. It’s size is always 0x2000, regardless of platform.

That’s why I’ve made a conditional switch for the **DynamicData** size. So never do like Microsoft guys – hardcode the size of the variable – use **sizeof** operator instead :-).

When the credentials have been transferred to the pipe, session 0 *Winlogon* needs to get a notification about it. This happens with a **WM\_LOGONNOTIFY** message with **WPARAM\_AUTHENTICATED** = 14 as WParam.

When *Winlogon* recieves this message, it just translates it to **WLX\_SAS\_TYPE\_AUTHENTICATED** SAS code and sends it to **GINA**, forcing it to query the credentials.

In case of a failure with packing the pipe content or sending it, *Winlogon* uses the undocumented **\_WinstationNotifyDisconnectPipe** function which first verifies the privileges of the calling process and doing the termsrv.dll -&gt;winsrv.dll -&gt; win32k.sys -&gt; winlogon.exe chain, just posts **WM\_LOGONNOTIFY** message with **WPARAM\_DISCONNECT\_PIPE** = 20 as WParam.

This message forces the pipe to be disconnected and start an asynchronous reconnection to the new client.

So, at this point we know everything which is necessary to write our own program, which can simulate credentials transfer from a different session instance to zero session instance, enabling us to logon any user to session 0!

A list of issues and workarounds that I found while writing the program will be described in [part 2](http://192.168.40.25:8081/2011/03/02/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-2-problems-and-workarounds/).

Parts of the source code and the application itself will be published in [part 3](http://192.168.40.25:8081/2011/03/03/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-3-implementation-details/).