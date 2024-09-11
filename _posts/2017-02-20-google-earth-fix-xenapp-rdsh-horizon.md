---
id: 4004
title: 'Google Earth fix for XenApp, RDSH &amp; Horizon'
date: '2017-02-20T20:26:51+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4004'
permalink: /2017/02/20/google-earth-fix-xenapp-rdsh-horizon/
views:
    - '3776'
categories:
    - Citrix
    - Uncategorized
    - VMWare
tags:
    - Citrix
    - Google
    - 'Google Earth'
    - Horizon
    - RDSH
    - XenApp
---

[![Google Earth Logo](http://192.168.40.25:8081/wp-content/uploads/2017/02/Google_Earth_logo_thumb.png "Google Earth Logo")](http://192.168.40.25:8081/wp-content/uploads/2017/02/Google_Earth_logo.png)Both Google Earth and Google Earth Enterprise do not work correctly for multiple users on shared Hosted Shared Desktops (I still prefer to call it Server Based Computing but that’s likely because I’m an oldtimer).

**Problem summary**  
So let’s look at the actual issue: the first user on a server is able to launch Google Earth but for any subsequent users on the same server Google Earth fails silently.

**<u>Problem details</u>**  
Google Earth uses various synchronization objects such as Events and Mutexes but registers those in the **\\Global** namespace instead of the **\\Local** namespace.

MSDN is quite clear on [Kernel object namespaces](https://msdn.microsoft.com/en-us/library/aa382954(v=vs.85).aspx):

> “*A Remote Desktop Services server has multiple namespaces for the following named kernel objects: events, semaphores, mutexes, waitable timers, file-mapping objects, and job objects. There is a global namespace used primarily by services in client/server applications. In addition, each client session has a separate namespace for these objects, such as in Windows Vista*.”

And

> “*The separate client session namespaces **enable multiple clients to run the same applications without interfering with each other***“.

That last line is key, as that’s exactly what’s happening here!

**<u>Api Hooking  
</u>**The Application Compatibility Toolkit has a shim to redirect Global to Local objects, called the [LocalMappedObject](http://technet.microsoft.com/en-us/library/cc749287(WS.10).aspx) but it is only applied for file mapping objects so this will not help us here.

The easiest solution therefore is to modify the api call using a mechanism called [Api Hooking](https://en.wikipedia.org/wiki/Hooking#In_Depth_API_Hooking). From an Api Hook the **\\Global** namespace can be replaced with **\\Local**.

The most common method for Api Hooking is [Dll Injection](https://en.wikipedia.org/wiki/DLL_injection).

**<u>Security</u>**  
![Security, Code Signing Certificate, Shield](https://www.digicert.com/images/code-signing-shield.png)Although Api Hooking can be used for doing Evil (e.g. a rootkit) it’s also essential for virtualization. Application virtualization (e.g. ThinApp or App-V) relies on Api Hooking and couldn’t exist without.

There is of course a risk that an Api Hook mechanism could be abused (e.g. someone replaces the Dll that is injected and thus the payload) or that antivirus programs will prohibit the injection.

**<u>Google Earth Fix v1</u>**  
In the first version of my Google Earth fix I avoided Dll Injection by using a loader program. This loader would emulate a debugger and launch Google Earth as a child process (a debuggee).

As a debugger, it is perfectly valid to do typical debugger operations such as settings breakpoints, changing memory and writing in the memory space of a process that is being debugged.

This approach worked well but was more difficult to maintain as newer OS versions such as Server 2012 and Server 2016 had subtle changes in the assembly code of Windows Api functions.

In short: my fix broke on new OS releases and had to be modified each time.

**<u>Google Earth Fix v2</u>**  
For this reason, I started thinking about a smarter, easier to maintain, approach and these were the design criteria:

- <span style="color: #35383d;">No more loader, the fix should be transparent (make it as easy as possible)</span>
- <span style="color: #35383d;">Should be secure (protect against abuse)</span>
- <span style="color: #35383d;">Should work on Windows vNext (especially with the current release speed in Windows)</span>
- <span style="color: #35383d;">Reusable for other Application Compatibility Fixes</span>

This has resulted in a solution based on 2 components:

- A (kernel mode) driver that is used to be notified when a new process is created.
- A Dll that is injected by the driver into selected processes.

The driver needs be signed with a, kernel mode, code signing certificate. This is a normal Windows security requirement since Windows Vista.

**This protects against any alterations of the driver**

For further security, the driver will only inject a Dll that has been signed by the *same code signing certificate* as the driver itsself.

**This protects against any alterations or replacements of the Dll**

**<u>[![Google Earth Compatibility Fix](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb-5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image-5.png)Easy Installation</u>**  
To make it eas easy as possible to use the fix I have created an MSI installer for it. The installer will install the driver and configure it to inject only into the Google Earth process.

The installation directory is C:\\Program Files (x86)\\Remko Weijnen\\Application Compatibility\\ and in this directory you will find the driver and dll.

Uninstallation is of course supported and will stop and remove the driver before removing the files.

**<u>Verify  
</u>**If you want to verify the driver installation you can check if it’s running with sc query= AppCompatDriver

[![sc queryex AppCompatDriver](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb-6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image-6.png)

Using [Process Explorer](https://technet.microsoft.com/en-us/sysinternals/processexplorer.aspx) you can easily verify if the Dll has been injected into Google Earth. Active the Lower Pane with the View Dll’s options (Ctrl-D) and select the Google Earth process:

[![Process Explorer | Lower Pane | DLL View](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb-7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image-7.png)

**<u>Supported Platforms</u>**  
The fix has been tested on the following platforms:

- <span style="color: #35383d;">Citrix XenApp</span>
- <span style="color: #35383d;">Microsoft RDSH</span>
- <span style="color: #35383d;">VMware Horizon (RDSH)</span>

[ Google Earth Compatibility Fix for Horizon, RDSH and XenApp (3749 downloads ) ](http://192.168.40.25:8081/download/google-earth-compatibility-fix-horizon-rdsh-xenapp/?tmstv=1726048920 "Version 1.0")