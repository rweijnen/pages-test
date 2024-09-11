---
id: 1196
title: 'Troubleshoot CorrectFilePath shim'
date: '2011-01-16T20:42:03+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1196'
permalink: /2011/01/16/troubleshoot-correctfilepath-shim/
views:
    - '4344'
short-url:
    - 'http://bit.ly/f1mAM2'
categories:
    - 'Application Compatibility'
    - Packaging
    - 'Unattended Installation'
---

A few days ago I was packaging an application that was writing an INI file in the application directory.

If you have read my earlier article, [Using the CorrectFilePaths shim to redirect an ini file to a writable location](/blog/2010/12/28/using-the-correctfilepaths-shim-to-redirect-an-ini-file-to-a-writable-location/), then you will probably think: create a nice shim and redirect that ini file!

But this application had a few challenges, the first being that it writes %COMPUTERNAME%.INI. The application’s developer probably assumed that a user is bound to one pc and that no other user’s use that pc.

To solve it we we need to catch all possible computer names (it would be nice if the CorrectFilePaths shims was able to accept wildcards and environment variables).

But it doesn’t so it means we have to add a parameter for each possible computer name. In my case that was doable because I have only 8 Citrix servers.

So I created a Fix using the Application Compatibility Manager as described in my previous post.

However it didn’t work, so I started to trace what happens.

First step (as usual) is to watch what happens in Process Monitor:

[![GBOSProcMon](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSProcMon_thumb.png "GBOSProcMon")](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSProcMon.png)

Strange, the CreateFile API was called using the exact Path that I used in my parameter.

So we need to check what the Application does exactly and we can do that with a debugger. [WinDbg](http://www.nynaeve.net/?p=142) would be a good option or [Ida Pro](http://www.hex-rays.com/idapro/) but if you are not using a Debugger often then I recommend AutoDebug.

I will describe how to debug this issue with [AutoDebug](http://www.autodebug.com/) because of that reason.

The nice thing about AutoDebug is that it uses [pdb](http://support.microsoft.com/kb/121366) files to be able to show you the exact parameters passed to an API.

To continue Download and Install Autodebug Pro and go to File | Run Process and start the Application you wish to Debug.

After loading the Application Auto Debug show the Function Filter Dialog, here you can enter the functions you want to monitor.

In the Process Monitor Screenshot you can see that the Path was opened with the CreateFile API.

[MSDN Documentation for CreateFile](http://msdn.microsoft.com/en-us/library/aa363858(v=vs.85).aspx) indicates that the function is exported from kernel32.dll and exists in both Ansi and Unicode versions (this is listed in the Requirements section).

So we monitor both CreateFileA and CreateFileW because we don’t know if the Application uses the Ansi- or Unicode version (actually the Ansi version will call the Unicode version but that is out of scope for this Article):

[![GBOSAutoDebug](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSAutoDebug_thumb.png "GBOSAutoDebug")](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSAutoDebug.png)

Press OK to continue and perform the actions in the Application that will write the INI file. In my case it was enough to move the window to another position and exit the Application.

Now inspect the calls to CreateFile until you find the INI file:

[![GBOSAutoDebug2](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSAutoDebug2_thumb.png "GBOSAutoDebug2")](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSAutoDebug2.png)

And there we have the problem!

If you look closely to the lpFileName parameter you can see two backslashes between the Application’s directory and the INI filename:

[![GBOSCreateFIle](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSCreateFIle_thumb.png "GBOSCreateFIle")](http://192.168.40.25:8081/wp-content/uploads/2011/01/GBOSCreateFIle.png)

This is clearly a bug, and one that is quite common and unnoticed because Windows ignores the double backslash and will still open the file.

The CorrectFilePath shim is ruthlessly literal though so if we want to use it here we must redirect using the parameter:

“\\\\ADNRD01\\Apps\\G-BOS\\\\CXNRD05.ini;”H:\\Windows\\G-Bos.ini”

In this sample I redirect the ini file to the redirected Windows directory on the user’s homedrive.

Hope you enjoyed this article and don’t forget to leave a comment if you have questions or simply found it useful!