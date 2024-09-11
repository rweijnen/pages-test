---
id: 2814
title: 'Application Hangs when Scanning in Citrix XenApp'
date: '2012-12-06T16:10:50+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2814'
permalink: /2012/12/06/application-hangs-when-scanning-in-citrix-xenapp/
short-url:
    - 'http://bit.ly/SJxC0S'
views:
    - '3975'
categories:
    - Citrix
tags:
    - Citrix
    - Twain
---

Another interesting issue today with an application that runs on Citrix XenApp.

**<span style="text-decoration: underline;">Environment</span>**  
Customer has a Citrix XenApp 5 environment running on Windows Server 2003. Clients are all Windows XP and run the Citrix Online Plugin 12.3 full screen.

RES Workspace Extender is used to integrate locally installed application into the XenApp Session. Users have no access to the local desktop.

**<span style="text-decoration: underline;">Symptoms</span>**  
This particular application scans invoices using a USB scanner attached to the client and runs them trough a workflow.  
Whenever the Start scan button was pressed the application froze.

[![SNAGHTML48ec098](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML48ec098_thumb.png "SNAGHTML48ec098")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML48ec098.png)

**<span style="text-decoration: underline;">Troubleshooting</span>**  
Since the application was no longer responding I could only kill the process. However when I did this, the whole session freezed.

The only thing I could do was reset the whole session via the management console.

In order to fix this problem we need to understand how twain redirection is implemented in XenApp.

**<span style="text-decoration: underline;">Twain Redirection</span>**  
When an application running in a XenApp session calls into the Twain API’s in order to obtain an image from a scanner the call is intercepted by a hook dll called twnhook.dll.

So my first step was to verify that twnhook.dll was correctly configured in the registry:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image1.png)

Then I verified that the hook dll was loaded into the target process with Process Explorer (enable the Lower Pane View and set it to DLL’s):

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image2.png)

On the client side a process called CtxTwnPA.exe is launched, this executable calls the TWAIN driver and communicates with the XenApp server through a Virtual Channel.

I verified CtxTwnPA.exe was being launched and that this process had loaded the twain dll:

[![SNAGHTML4f2aa1d](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4f2aa1d_thumb.png "SNAGHTML4f2aa1d")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4f2aa1d.png)

I right-clicked the CtxTwnPA.exe process in Process Explorer and selected **Window | Bring To Front** and that showed this Window:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image3.png)

I am not sure why this window is appearing since there is only one scanner to select but clearly the application was not hanging but was waiting for this dialog to be closed!

In theory this shouldn’t be a problem since the RES Workspace Extender should display this window in the XenApp session but it doesn’t. This happens because the dialog window doesn’t have an associated icon in the taskbar.

This is by design when using the Workspace Extender (RES Virtual Desktop Extender (VDX) doesn’t have this limitation).

I figured that if I could force this dialog to have a taskbar icon it would solve my problem.

<span style="text-decoration: underline;">**Solution**</span>  
I am not going to explain in detail here how resources in c/c++ applications work, but in a nutshell a dialog is composed as a resource script that can be modified with tools such as [Resource Hacker](http://www.angusj.com/resourcehacker/).

I loaded twain\_32.dll into Resource Hacker and found the dialog:

[![SNAGHTML4f66669](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4f66669_thumb.png "SNAGHTML4f66669")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4f66669.png)

In order for an application to have an icon on the taskbar we must make sure that the [Window Style](http://msdn.microsoft.com/en-us/library/windows/desktop/ms632600%28v=vs.85%29.aspx) contains the WS\_POPUP attribute and that the [Extended Window Style](http://msdn.microsoft.com/en-us/library/windows/desktop/ff700543%28v=vs.85%29.aspx) contains the WS\_EX\_APPWINDOW attribute.

So I modified the resource script from:  
“&gt;  
To  
“&gt;  
Then I recompiled the script with the Compile Script button and saved the Executable.

Now the Select Source Dialog has a Taskbar Icon and is displayed correctly in the session by the RES Workspace Extender:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image4.png)

When replacing twain\_32.dll on Windows XP please note that the file is protected by Windows File Protection. You can use my [WfpReplace](http://192.168.40.25:8081/2012/12/05/replacing-wfp-protected-files/) tool to easily overwrite it!