---
id: 546
title: 'Using Windows Dialogs from Delphi'
date: '2010-03-24T11:48:41+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=546'
permalink: /2010/03/24/using-windows-dialogs-from-delphi/
views:
    - '4320'
short-url:
    - 'http://bit.ly/hw1OFD'
categories:
    - 'Active Directory'
    - Delphi
    - Programming
---

Today I reused a unit I wrote a long time ago for TSAdminEx that shows Resource Dialogs from DLL’s or Executables. I wrote it for a couple of reasons:

- Reusing existing dialogs is conventient since the user already knows it.
- Windows takes care of translating it into the user’s language.
- I am too lazy to recreate them 😉

The code is hardly rocket science and could probably be improved and made more sophisticated but it works for me. I decided to share it since you may find it usefull.

Here is a small usage example that shows the Reset Password dialog from Active Directory Users &amp; Computers. This dialog is in dsadmin.dll (on Windows Vista/7 you will find it in ds.admin.dll.mui in the language subfolder eg %systemroot%\\system32\\en-US but you can load it using just the dll name).

It looks like this:  
“&gt;  
The code to display it in Delphi looks like this:  
“&gt;  
The Constructor takes the following parameters:

- Parent Window Handle
- DLL or Exe name
- Dialog Resource Id
- Optional: A procedure that is called before displaying the dialog where you can init the controls.

My init procedure looks like this:  
“&gt;  
[![ResetPassword](http://192.168.40.25:8081/wp-content/uploads/2010/03/resetpassword-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/03/resetpassword.png)

Have fun with it and if you improve something please let me know!

[ ResDialogs (1445 downloads ) ](http://192.168.40.25:8081/download/resdialogs/?tmstv=1726048918 "Version 1.0")