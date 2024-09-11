---
id: 1299
title: 'Querying a user token under 64 bit version of 2003/XP'
date: '2011-01-29T22:17:52+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1299'
permalink: /2011/01/29/querying-a-user-token-under-64-bit-version-of-2003xp/
short-url:
    - 'http://bit.ly/fJUjtb'
views:
    - '5026'
categories:
    - Delphi
    - Programming
    - 'Strange Error'
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows Internals'
    - 'Windows XP'
---

If you want to obtain a user’s token in a Terminal Server or Citrix session (eg to launch a process in a session) you can call the [WTSQueryUserToken](http://msdn.microsoft.com/en-us/library/aa383840(v=vs.85).aspx) function.

On the x64 versions of Windows XP and Server 2003 this function fails however and returns **ERROR\_INSUFFICIENT\_BUFFER** (“*The data area passed to a system call is too small*.”) when called from a 32 bit process.

Internally WTSQueryUserToken calls the undocumented function [WinstationQueryInformationW](http://msdn.microsoft.com/en-us/library/aa383827(v=vs.85).aspx) with the **WinStationUserToken** class (14) and passing a [WINSTATIONUSERTOKEN](http://msdn.microsoft.com/en-us/library/cc248658(PROT.10).aspx) struct, filled with caller ProcessId and ThreadId.

But on x64 Windows the size of this structure is 24 bytes, while on 32 bit Windows the size of the structure is 12 bytes!

Ok, let’s try to call [WinstationQueryInformationW](http://msdn.microsoft.com/en-us/library/aa383827(v=vs.85).aspx) function directly:  
“&gt;  
But this call returns **ERROR\_INSUFFICIENT\_BUFFER** again! Why?

The reason is that [WinstationQueryInformationW](http://msdn.microsoft.com/en-us/library/aa383827(v=vs.85).aspx) does some buffer checks/adjustments, before calling the real function, [RpcWinStationQueryInformation](http://msdn.microsoft.com/en-us/library/cc248834(PROT.10).aspx).

Unfortunately, in some cases the buffer size is (incorrectly) hardcoded while the receiver (termsrv.dll) in many cases allows any value greater then required.

The buffer check is done in an internal function called ValidUserBuffer:  
“&gt;  
So, as you can see, there is no way to succeed using the original winsta.dll!

The only workaround is to patch it (and possibly distribute it with your app) or use [RpcWinStationQueryInformation](http://msdn.microsoft.com/en-us/library/cc248834(PROT.10).aspx) directly.

The code below works fine:  
“&gt;  
However to use the RpcWinStationQueryInformation you need the MIDL compiler, which supports only C/C++, to generate the code.

Of course it can be done using Delphi but that is out of scope for this article.