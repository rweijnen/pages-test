---
id: 1396
title: 'Autologon user on Windows XP/2003 using AutoReconnect pipe &#8211; part 3 (implementation details)'
date: '2011-03-03T13:00:46+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1396'
permalink: /2011/03/03/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-3-implementation-details/
short-url:
    - 'http://bit.ly/i8Uzju'
views:
    - '3651'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows Internals'
    - 'Windows XP'
---

In the previous parts ([part 1](http://192.168.40.25:8081/2011/02/09/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-1-theory/) [part 2](http://192.168.40.25:8081/2011/03/02/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-2-problems-and-workarounds/)) i’ve described the theoretical part and implementation problems. So, now we can write the code:

1\) In case we login the user, we just call [LsaLogonUser](http://msdn.microsoft.com/en-us/library/aa378292(v=vs.85).aspx) to get the token:  
  
“&gt;  
2\) In case we need to create a token, we call [NtCreateToken](http://undocumented.ntinternals.net/UserMode/Undocumented%20Functions/NT%20Objects/Token/NtCreateToken.html). If there are no groups, we create a single group with the Primary SID in this group :  
“&gt;  
3\) In case we want to use a “current” system token, we need to do some adjustments with it – we need to add the group with *SE\_GROUP\_LOGON\_ID* flag to it:  
“&gt;  
4\) Now we can create a buffer:  
“&gt;  
5\) In case of x64 systems we need to copy the whole structure into 64 bit structure:  
“&gt;  
6\) Now we need to open the pipe, send the *WLX\_SAS\_TYPE\_AUTHENTICATED* SAS to *Winlogon* and write the data to the pipe:  
“&gt;  
7\) So, the main procedure will look like this:  
“&gt;  
[ AutoLogonXP 1.0 (1530 downloads ) ](http://192.168.40.25:8081/download/autologonxp-1-0/?tmstv=1726048919 "Version 1.0")

Please note that this sample is free for non-commercial use only. If you require to use it, or it’s source code in your business projects, please send a request using [contact form](http://192.168.40.25:8081/contact/).