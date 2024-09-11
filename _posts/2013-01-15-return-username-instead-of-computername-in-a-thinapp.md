---
id: 2993
title: 'Return username instead of computername in a ThinApp'
date: '2013-01-15T11:50:07+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2993'
permalink: /2013/01/15/return-username-instead-of-computername-in-a-thinapp/
views:
    - '1505'
short-url:
    - 'http://bit.ly/UMZ4dD'
categories:
    - Citrix
    - 'Terminal Server'
    - ThinApp
tags:
    - ThinApp
---

![File:VMware ThinApp v4.0 icon.png](http://upload.wikimedia.org/wikipedia/commons/c/c0/VMware_ThinApp_v4.0_icon.png)One of the lesser known features of VMware ThinApp is that you can supply a Virtual Computer name.

This is documented as follows in the [package.ini reference guide](http://www.vmware.com/pdf/thinapp47_packageini_reference.pdf):

**VirtualComputerName Parameter**   
The VirtualComputerName parameter determines whether to rename the computer name, to avoid naming conflicts between the capture process and the deployment process.

The result is that when the virtualised application read the computername via the [GetComputerName](http://msdn.microsoft.com/en-us/library/windows/desktop/ms724295%28v=vs.85%29.aspx) or [GetComputerNameEx](http://msdn.microsoft.com/en-us/library/windows/desktop/ms724301%28v=vs.85%29.aspx) API function the supplied name will be returned.

There are various scenario’s where this parameter is convenient, some examples:

- Applications that have a computer bound license or activation
- Applications that store settings per computer instead of per user

In the first example the solution is simply to fixate the computername. You might also want to fixate the volume serial number. See [License check fails on Citrix XenApp](http://192.168.40.25:8081/2012/12/07/license-check-fails-on-citrix-xenapp/) for an example.

The second example is especially problematic in VDI Environments (user could have a different machine each day) or SBC (load balancing over a farm of servers).

For a natively installed application we can use the Terminal Server Application Compatibility mechanism to [return a username instead of a computername](http://192.168.40.25:8081/2012/12/06/return-username-instead-of-computername-to-applications/).

With ThinApp we can simply set the VirtualComputerName to an environment variable, so we can solve the problem like this:

 “&gt;