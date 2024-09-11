---
id: 3079
title: 'The case of the COM Port Redirection'
date: '2013-03-11T14:53:44+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3079'
permalink: /2013/03/11/the-case-of-the-com-port-redirection/
short-url:
    - 'http://bit.ly/10CSzBN'
views:
    - '7134'
categories:
    - 'Application Compatibility'
    - Citrix
    - 'Windows 2003'
tags:
    - Citrix
    - 'COM Port Redirection'
    - XenApp
---

[![Secutest](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb.png "Secutest")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image.png)One of my colleagues asked me to assist in troubleshooting an application called SmartWare FM running on Citrix XenApp.

This application reads data from an external device called SECUTEST.

The device is connected to a COM port which is redirected to the XenApp session. In contrast to Microsoft Remote Desktop Services COM ports are not automatically redirected in XenApp but need to be mapped via eg a logonscript (NET USE COM1: \\\\Client\\COM1:) or using UEM.

In my case the COM port was mapped with RES Workspace Manager:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image1.png)

 I verified using net use that the com port is actually mapped:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image2.png)

The import in the application fails however with a generic error (Er heeft zich een fout voorgedaan):

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image3.png)

I disconnected the XenApp session and ran Putty locally connected to COM1 9600-N-8-1.

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image4.png)

Pressing enter echoes $24 (apparently some kind of protocol) so the communication between PC and device seems to work:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image5.png)

I then tried to connect with [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) (a lightweight terminal client)from within the XenApp session but got an immediate connection failure:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image6.png)

This message normally appears when a COM port is already in use so I opened [Process Explorer](http://technet.microsoft.com/en-us/sysinternals/bb896653.aspx) and used Find | Find Handle or DLL and searched for COM1:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image7.png)

CdmRedirector is the Client Device Mapping redirector from Citrix so it appears that our COM port is already connected to the psp6164a.exe process.

psp6164a.exe belongs to Philips G2 Speech and seems to be responsible for communication to serial G2 Speech devices. The executable is launched from the HKLM run key:

[![SNAGHTML1ac2ce04](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML1ac2ce04_thumb.png "SNAGHTML1ac2ce04")](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML1ac2ce04.png)

I knew that in this environment only USB connected G2 Speech devices are used but I wanted to check if there was some kind of commandline argument I could set to skip COM1.

I opened psp6164a.exe in Ida Pro and searched for COM1:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image8.png)

Using Ctrl-X (references) I ended up in this code:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image9.png)

Using the registry (HKLM\\SOFTWARE\\Philips Speech\\Control Panel\\6164) we can configure the active port and Port Search Order:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image10.png)

After reconfiguring I retested with Putty and the communication seemed to work from within the XenApp session:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image11.png)

However the application failed with the same error:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image12.png)

Knowing that the connection and protocol were working I monitored with Process Monitor. I usually focus on ACCESS DENIED errors by using the Filter | Highlight option to easily identify them:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image13.png)

There was just one:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image14.png)

Looks like the application tries to write a file to the Program Files folder (why do developers insist on doing this?).

We can of course give the users write permissions on this folder or perhaps even this file but what happens when multiple users try to do this at the same time?

A better solution is to use an Application Compatibility shim to redirect this file to a user specific and user writable location. I choose to redirect it to H:\\Windows\\Temp (H:\\ is the users homefolder).

But we need to verify if there are any other files we need to redirect either by trial and error or search for related files.

I double-clicked the line that resulted in ACCESS DENIED in Process Monitor and switched to the Stack tab. There was only one file from the Applications folder in there called SW2\_FM\_SEC.dll:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image15.png)

I opened Process Explorer, clicked the Applications executable and set the Lower Pane View to DLLs. Then I selected SW2\_FM\_SEC.dll from the list and listed it’s properties, on the Strings tab we can see that besides IMPORT.SEC there is probably a file called ERROR.SEC:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image16.png)

So let’s create the redirect shim (if you are new to Application Compatibility read [this article](http://192.168.40.25:8081/2010/12/28/using-the-correctfilepaths-shim-to-redirect-an-ini-file-to-a-writable-location/) first).

Create a new database and the executable (SW2000.exe in my case):

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image17.png)

Select the CorrectFilePath Shim and click Parameters:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb18.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image18.png)

Note that MSVBVM60.DLL is on stack (in the Process Monitor screenshot above) so we need to add MSVBVM60.DLL to the include modules (see [here](http://192.168.40.25:8081/2010/12/28/using-the-correctfilepaths-shim-with-visual-basic-applications/) for an explanation).

The Command line is:

 “&gt;

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image19.png)

Save and install the database and now everything works correctly:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image20.png)

Of course you need to distribute and install the Shim to your production environment, more about that [here](http://192.168.40.25:8081/2011/01/07/fixing-applications-the-next-step/).