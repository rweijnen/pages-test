---
id: 1994
title: 'GetUserObjectInformation fails with Access Denied'
date: '2011-08-16T21:11:01+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/08/16/getuserobjectinformation-fails-with-access-denied/'
permalink: /2011/08/16/getuserobjectinformation-fails-with-access-denied/
views:
    - '2668'
short-url:
    - 'http://bit.ly/r0mMFm'
categories:
    - Delphi
    - Programming
    - Vista
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
tags:
    - Delphi
    - GetUserObjectInformation
    - OpenWindowStation
---

[![Logon SID](http://192.168.40.25:8081/wp-content/uploads/2011/08/image_thumb2.png "Logon SID")](http://192.168.40.25:8081/wp-content/uploads/2011/08/image2.png)Today I was reusing some old (pre vista) code the retrieves the Logon SID that I wrote a few years ago. The Logon SID is a special SID that identifies a logon session that has the form S-1-5-5-X-Y.

You can view your Logon SID with Process Explorer, right click a GUI process, select Properties and goto the Security Tab:

[![Process Explorer|Security Tab|Logon SID](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML1b84fe6b_thumb.png "explorer.exe:4484 Properties")](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML1b84fe6b.png)

My code called [OpenWindowStation](http://msdn.microsoft.com/en-us/library/ms684339(VS.85).aspx) and then passed the obtained handle to [GetUserObjectInformation](http://msdn.microsoft.com/en-us/library/ms683238(VS.85).aspx) with the UOI\_USER\_SID index (error handling left out):

 “&gt;

![Windows 7 Logo](http://t2.gstatic.com/images?q=tbn:ANd9GcRtG2z72wTQFqFSwNplYm_QZnRLenLi4KkP_8Pt2bL2aHjbtrqV "Windows 7")On my Windows 7 machine the call to GetUserObjectInformation failed however. GetLastError returns ERROR\_ACCESS\_DENIED (error 5) with description *Access is denied*.

The handle returned from OpenWindowStation was valid so I assumed that the ACCESS\_MASK was the problem. I replaced READ\_CONTROL with WINSTA\_READATTRIBUTES and then it worked fine:

“&gt;