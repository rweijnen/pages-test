---
id: 59
title: 'Terminal Server Client annoyances'
date: '2007-11-09T21:39:50+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/11/09/terminal-server-client-annoyances/'
permalink: /2007/11/09/terminal-server-client-annoyances/
views:
    - '3210'
short-url:
    - 'http://bit.ly/frCM2c'
categories:
    - 'Terminal Server'
---

If you want to get rid of this message:

Remote Desktop cannot verify the identity of the computer you want to connect to. This problem can occur if:

1\) The remote computer is running a version of Windows that is earlier than Windows Vista.  
2\) The remote computer is configured to support only the RDP security layer.

Contact your network administrator or the owner of the remote computer for assistance.

Do you want to connect anyway?

Set the DWORD value AuthenticationLevelOverride of HKCU\\Software\\Microsoft\\Terminal Server Client\\AuthenticationLevelOverride to 0.

Read more on [Scott Forsyth’s blog](http://weblogs.asp.net/owscott/archive/2006/11/10/Vista_2700_s-Remote-Desktop-Prompt.aspx)