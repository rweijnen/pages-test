---
id: 4133
title: 'Get Windows 10 Version Number with PowerShell'
date: '2017-04-04T16:54:02+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4133'
permalink: /2017/04/04/get-windows-10-version-number-powershell/
views:
    - '5710'
categories:
    - PowerShell
tags:
    - PowerShell
    - 'Windows 10'
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image.png)As you may heard, the API’s returning the Operating System version have [changed](https://msdn.microsoft.com/windows/compatibility/operating-system-version-changes-in-windows-8-1), started with Windows 8.1 and Server 2012 R2.

The reason for this change is Application Compatibility but let’s take a little closer look into this why.

As an application developer there may be a need to check the version of the OS you’re running on. A typical example is when you are using an API that only works on a specific Windows version (and up). Or the other round, you’re not supporting an older version of Windows (say Windows XP as an example).

A common error in such version checks is to check for a specific Windows version but forget to take new (not yet released) versions into account.

Similar things happened with applications checking if they had a specific version of windows components such as Internet Explorer.

Before Windows 10 Microsoft mitigated these errors with Application Compatibility shims which could be set per user via application properties:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-1.png)

Or for all users with the Application Compatibility Toolkit:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-2.png)

However this meant you needed to target specific applications that are at fault.

The new approach is basically the reverse, you can mark your application as being aware of the changes in the GetVersion(Ex) Api by adding a special manifest to your application. If you have such a manifest then you will get the real Windows version information. If you do not have such a manifest then you’ll get 6.2.9200 (Windows 8).

So why is this relevant? Let’s check!

In PowerShell.exe we get the right answer:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-3.png)

What about PowerShell ISE?

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-4.png)

Let’s check some popular editors for PowerShell. Here’s PowerGUI Script Editor:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-5.png)

And here’s SAPIEN PowerShell Studio:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-6.png)

So why are these editors producing different results? We now have 10.0.15063, 6.2.9200 and 6.3.9600.

The answer is in the manifest as discussed earlier, we can see this if we inspect PowerShell.exe with Resource Hacker:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-7.png)

If you run scripts on your own systems only and you’re not using 3rd party tools and you’re not using Windows 8.1 (in which PowerShell.exe doesn’t have the manifest) you’re basically fine.

If however you share your PowerShell scripts or run them from other tools (e.g. your favorite deployment tool or editor) then you may need to use an alternative method for checking the Windows version.

I’ve seen various create methods to get the Windows version without manifest like reading from the Registry, WMI queries, calling `RtlGetVersion` or `VerifyVersionInfo`.

Please allow me to add my own method which is to read the version from `KUSER\_SHARED\_DATA`.

Now you may wonder what `KUSER\_SHARED\_DATA` is. Geoff Chappell does a pretty good job to describe it [here](http://www.geoffchappell.com/studies/windows/km/ntoskrnl/structs/kuser_shared_data.htm). In a nutshell it is a kernel mode structure that is shared on a fixed address (readonly) with every user mode process since Windows NT 3.51.

It is defined as a global constant `SharedUserData` and documented in the DDK since Windows 2000.

In usermode it’s at the address `0x7FFE0000` for both 32- and 64 bit addresses.

This means we can read directly from this address without needing to revert to C# and P/Invoke.

The Major and Minor Windows versions are stored at offsets 0x026C and 0x0270:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-8.png)

And specific for Windows 10 we have the buildnumber:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-9.png)

Here is an example:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-10.png)

Of course we can write this a little bit more compact:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-11.png)

Or even:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/04/image_thumb-12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/04/image-12.png)

Have fun and PowerShell away!