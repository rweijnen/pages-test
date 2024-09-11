---
id: 1515
title: 'Office Communicator 2007 R2 crashes after sign on'
date: '2011-03-10T15:45:45+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/10/office-communicator-2007-r2-crashes-after-sign-on/'
permalink: /2011/03/10/office-communicator-2007-r2-crashes-after-sign-on/
views:
    - '8063'
short-url:
    - 'http://bit.ly/gAbYoC'
categories:
    - General
tags:
    - Communicator
    - Office
---

After I uninstalled Office 2010 64 bit and installed Office 2010 32 bit I had a problem with Office Communicator 2007 R2.

After entering my password and clicking sign in it crashed every time:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image.png)

In the EventLog an Application Error was recorded with some additional error info:

> Event Type: Error Event Source: Application Error Event Category: (100) Event ID: 1000 Date: 10-3-2011 Time: 15:20:52 User: N/A Computer: remkolaptop Description: Faulting application name: communicator.exe, version: 3.5.6907.221, time stamp: 0x4cddcd9f Faulting module name: KERNELBASE.dll, version: 6.1.7601.17514, time stamp: 0x4ce7bafa Exception code: 0xc06d007e Fault offset: 0x0000b727 Faulting process id: 0xf94 Faulting application start time: 0x01cbdf2e592fc53c Faulting application path: C:\\Program Files (x86)\\Microsoft Office Communicator\\communicator.exe Faulting module path: C:\\Windows\\syswow64\\KERNELBASE.dll Report Id: 9a4e3adf-4b21-11e0-8f0f-c0cb38a92f9b For more information, see Help and Support Center at http://go.microsoft.com/fwlink/events.asp.

The exception code is 0xc06d007e which is defined in WINERROR.h as ERROR\_MOD\_NOT\_FOUND, the error description is: “The specified module could not be found”.

Basically this means that Communicator attempted to load a DLL with a call to LoadLibrary(Ex) and it failed.

So I did a trace with Process Monitor:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image1.png)

So I concluded that mapi32.dll was the missing dll and after obtaining the DLL from another system Communicator worked again.