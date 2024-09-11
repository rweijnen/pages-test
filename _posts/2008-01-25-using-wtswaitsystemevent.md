---
id: 68
title: 'Using WTSWaitSystemEvent'
date: '2008-01-25T18:02:13+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/01/25/using-wtswaitsystemevent/'
permalink: /2008/01/25/using-wtswaitsystemevent/
views:
    - '5146'
short-url:
    - 'http://bit.ly/eJyOj8'
categories:
    - Citrix
    - Delphi
    - Programming
    - 'Terminal Server'
    - Vista
tags:
    - bug
    - Citrix
    - 'Terminal Server'
    - Vista
    - wts_event_flush
    - WTSWaitSystemEvent
---

If you develop an application for Terminal Server you might want to react on session events. This means that your application is notified when a user logs on, logs off or becomes idle. This can be done with the [WTSWaitSystemEvent](http://msdn2.microsoft.com/en-us/library/aa383856(VS.85).aspx) function. Implementing it is rather simple and could look something like this:  
“&gt;  
Notice that you would probably do this from a seperate thread otherwise you will block the main thread. To stop waiting for Events you send a special event:  
“&gt;  
Please note that there are at least 2 issues with this API, one with Windows 2000 and one with Windows Vista. On Windows 2000 events are reported twice for each actual event. Microsoft’s resolution?

*The application should expect the event twice, and filter out the second occurrence.*

Now how do we solve this? I would suggest introducing a small delay after an event trigger, this way you will probably not receive the duplicate event.

On Windows Vista there’s another issue: After you set the value of the EventMask parameter to WTS\_EVENT\_FLUSH in the WTSWaitSystemEvent function, no pending calls to the function return on a Windows Vista-based computer. Now what does this mean? It means that after sending WTS\_EVENT\_FLUSH the thread never responds! So there’s actually no nice way to end the thread, the only escape is a call to TerminateThread.

Microsoft does offer a hotfix, so my suggestion is a check on startup that will notify the user that he/she needs to install the hotfix. A version check can be done on winsta.dll, the version before the fix is 6.0.6000.16386. Hotfix version is 6.0.6000.20664. According to this [article](http://technet2.microsoft.com/WindowsVista/en/library/20184cb6-7038-4e82-a32c-4bc10ffe56ab1033.mspx?mfr=true) the fix will be included in Vista SP1.

References:

- [BUG: WTSWaitSystemEvent Returns Terminal Services Event Twice](http://support.microsoft.com/kb/249315)
- [After you set the value of the EventMask parameter to WTS\_EVENT\_FLUSH in the WTSWaitSystemEvent function, no pending calls to the function return on a Windows Vista-based computer](http://support.microsoft.com/kb/941561/en-us)
- [Hotfixes and Security Updates included in Windows Vista Service Pack 1](http://technet2.microsoft.com/WindowsVista/en/library/20184cb6-7038-4e82-a32c-4bc10ffe56ab1033.mspx?mfr=true)