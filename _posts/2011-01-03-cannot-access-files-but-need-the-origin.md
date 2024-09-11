---
id: 1034
title: 'Cannot Access Files, But Need the Origin?'
date: '2011-01-03T09:52:20+01:00'
author: Chris
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1034'
permalink: /2011/01/03/cannot-access-files-but-need-the-origin/
views:
    - '9469'
short-url:
    - 'http://bit.ly/fmECSf'
categories:
    - Delphi
    - Programming
    - Vista
    - 'Windows 7'
---

Have you developed an application that accesses files and may stop because a file cannot be accessed but you need to?

Since Windows Vista it is possible to find out the name of the application which holds open a file.

And we have created a solution for you that doesn’t require a driver, nor does it need Administrator rights! Just plain user source code for you to use instantly.

We have developed a solution in Delphi that can show your user which application stalls your application. Look at these screenshots:

[![](http://192.168.40.25:8081/wp-content/uploads/2010/12/screens-300x181.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/screens.png)

In fact, these dialogs are only for demonstration purposes. But you can get them in addition as a Delphi project!

The code itself is fairly easy to use. We have developed an extension to the official IFileIsInUse Interface from Microsoft.  
“&gt;  
All you have to do it to call this function provides by us:  
“&gt;  
The name of the app is returned by the method *GetAppName*. And if the other application supports *IFileIsInUse* (call method *GetCapabilities*) interface you even can close the file or switch to the window.

You’ll see how it works in the demonstration project (images above) accompanied by the function *GetFileInUseInfo*.

Currently, we only offer a Delphi solution. C++ may available in future or when there’s enough demand for it.

Please use the [Contact Form](/blog/contact) to get more information about how to obtain the solution and conditions.