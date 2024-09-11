---
id: 215
title: 'Locking a workstation &#8211; part 1'
date: '2008-12-13T23:13:08+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/13/locking-a-workstation-part-1/'
permalink: /2008/12/13/locking-a-workstation-part-1/
views:
    - '7293'
short-url:
    - 'http://bit.ly/hjaAXI'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
---

Win32 API provides only 1 function for locking workstation, named **LockWorkstation**. What does it do and how we can use it?

When a new session is started, *smss.exe* invokes *winlogon.exe*. It registers its process id in *win32k.sys* by calling **RegisterLogonProcess**. It has this prototype:  
“&gt;  
Functional code in win32k.sys for RegisterLogonProcess is very simple:  
“&gt;  
*gpidLogon* is a global variable in win32k.sys. So, only processes with SE\_TCB\_NAME can call it and only once per session (each session has its own instance of win32k.sys).

Later, when winlogon.exe continues its initialization, it creates a hidden window with **‘SAS window’** name and registers it handle using this function:  
“&gt;  
Its functional code is very simple again:  
“&gt;  
So only a logon process is allowed to set logon notify window. Let’s look what does **LockWorkstation** does:  
“&gt;  
Hmm… maybe there are some more messages we can post?

In [next part](http://192.168.40.25:8081/2008/12/23/locking-a-workstation-part-2/) I’ll show how winlogon.exe registers keyboard shortcuts and how we can use them