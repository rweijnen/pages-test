---
id: 1395
title: 'Autologon user on Windows XP/2003 using AutoReconnect pipe &#8211; part 2 (problems and workarounds)'
date: '2011-03-02T00:10:02+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1395'
permalink: /2011/03/02/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-2-problems-and-workarounds/
short-url:
    - 'http://bit.ly/gxO76y'
views:
    - '3395'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows XP'
---

In [part 1](http://192.168.40.25:8081/2011/02/09/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-1-theory/) I’ve described the theoretical parts needed for a custom autologon application implementation.

But there are some practical problems which I will describe here.

1\) I use the [LsaLogonUser](http://msdn.microsoft.com/en-us/library/aa378292(v=vs.85).aspx) function to log in the user. However, if I do not pass not null for the *LocalGroups* parameter, msgina.dll fails to process the logon.

Why? Because it looks for the **SE\_GROUP\_LOGON\_ID** SID and treat it as logon SID. So we have to add the logon SID manually:  
  
“&gt;  
2\) In case we want to create a token using [NtCreateToken](http://undocumented.ntinternals.net/UserMode/Undocumented%20Functions/NT%20Objects/Token/NtCreateToken.html), we need to add the *Everyone* Group (otherwise W*inlogon* will fail to load the user profile):  
“&gt;  
3\) On the X64 versions of Windows XP and all version of Windows 2003 a service process doesn’t have the [SeCreateTokenName](http://msdn.microsoft.com/en-us/library/bb530716(VS.85).aspx) privilege. But we can open *lsass.exe* process and copy it’s token:  
“&gt;  
4\) In case we create the token manually, we have to specify the logon session. As there is no way to create a session without injecting into *lsass.exe*, I use a SYSTEM luid as a logon session luid. It has the side-effect of showing the user as SYSTEM regardless of the actual used SID.

5\) Sometimes opening the pipe fails (it may have been connected to some other session’s *winlogon*). I try to open the pipe with short timeout and post a disconnect pipe message:  
“&gt;  
6\) In Windows 2003 (or on XP x64) you should read the BOOL variable from the pipe after you’ve written to it. But it may not work as *Winlogon* simply writes the value and then disconnects the pipe, so sometimes the data can not be read (subject to threading issue). I simply ignore the read error, since even if it shows that the credentials were accepted by *Winlogon*, they may have been rejected by *gina*  
“&gt;  
7\) In some extremely rare cases our process may exit before *Winlogon* has read the credentials and duplicated them (actuallyI was able to repeat this only under debugging *winlogon*).

To be sure that our credentials have been correctly processed, I create a different thread and wait for console logon event:  
“&gt;  
8‌) If Fast User Switching is enabled and we are on a Workstation OS, W*inlogon* ignores the *WLX\_SAS\_TYPE\_AUTHENTICATED* message. However, before execution we can disable the *AllowMultipleTSSessions* key, and enable it again after the execution. Of course, we do have to remember that under WOW we need to use the special registry flag to access real HKLM hive.

9\) In case session 0 has a logged-on user, W*inlogon* also ignores the *WLX\_SAS\_TYPE\_AUTHENTICATED* message. I use [WTSQueryUserToken](http://msdn.microsoft.com/en-us/library/aa383840(v=vs.85).aspx) to get the token of the logged on user. If the function succeeds, then a user is logged on. There are some problems calling this function on 64 bit systems, and they are described in [Querying a user token under 64 bit version of 2003/XP](http://192.168.40.25:8081/?p=1299)

10\) If the screensaver is running, we’ll have to wait until someone breaks the screensaver. So we need to stop screensaver programmatically. MSDN recommends posting the WM\_CLOSE message to the foreground window, so let’s do it:  
“&gt;  
11\) Although you can send an empty username, some GINA’s, at least *msgina.dll* require it to be not-null. So we can use an empty string (#0) instead. You may need to overwrite this procedure to increase compatibility with your GINA:  
“&gt;  
12\) There is incompatibility with some custom GINA’s, for example, with [FMLogin](http://www.frontmotion.com/FMLogin/), which subclasses “Sas Window”. I’ve tested the sample with original msgina.dll, so the further compatibility should be tested with the exact gina.

13\) Getting a session process list on x64 may fail if you do not allocate buffers correctly – described [here](http://192.168.40.25:8081/2011/01/20/enumerating-session-process-with-ntquerysysteminformation/)

So we’re ready to go on to the implementation which will be described in the [part 3](http://192.168.40.25:8081/2011/03/03/autologon-user-on-windows-xp2003-using-autoreconnect-pipe-part-3-implementation-details/).