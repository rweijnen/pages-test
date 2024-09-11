---
id: 404
title: 'Modifying Microsoft Updates and/or hotfixes 2'
date: '2009-07-21T22:54:54+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/07/21/modifying-microsoft-updates-andor-hotfixes-2/'
permalink: /2009/07/21/modifying-microsoft-updates-andor-hotfixes-2/
views:
    - '14850'
short-url:
    - 'http://bit.ly/gyoOVa'
categories:
    - General
---

In a [previous post](http://192.168.40.25:8081/2009/05/12/modifying-microsoft-updates-andor-hotfixes/) I wrote about patching update.exe to allow installing updates with modified .inf files.

A commenter asked how to do this for another build of update.exe, specifically version 6.3.4.1 as is distributed with Windows 2003 SP2 (now what would he want to do with it?).

This is actually a very easy task with the knowledge of the previous post, so let me explain it here step by step.

First we open the target file in [Ida](http://www.hex-rays.com/idapro/idadownfreeware.htm) and wait for the Autoanalysis to finish. Then go to the Functions window and look for the function IsInfFileTrusted:

[![Ida1](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida1.png)

Now doubleclick this function and watch the disassembly in the IDA View-A tab:

[![Ida2](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida2.png)

In this case we don’t really care what the function actually does, we just want to make it return always True.

In Ida options make it show the opcode bytes:

[![Ida3](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida3-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida3.png)

Make a note or screen dump of the opcode bytes.

Now go to the Edit menu | Patch program | Assemble and make it return True:

[![Ida4](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida4-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida4.png)

[![Ida5](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida5-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida5.png)

We can see the needed changes now:

[![Ida6](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida6-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/07/ida6.png)

Now make the changes with your favorite Hex Editor. For the really lazy people I attached the dUP2 file below.

[ Patch for Update.exe 6.3.4.1 (1104 downloads ) ](http://192.168.40.25:8081/download/patch-for-update-exe-6-3-4-1/?tmstv=1726048918 "Version 1.0")