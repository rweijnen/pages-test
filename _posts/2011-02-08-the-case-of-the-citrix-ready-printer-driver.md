---
id: 1402
title: 'The Case of the Citrix Ready Printer Driver'
date: '2011-02-08T21:47:44+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1402'
permalink: /2011/02/08/the-case-of-the-citrix-ready-printer-driver/
views:
    - '5855'
short-url:
    - 'http://bit.ly/h82Oak'
categories:
    - Citrix
    - PowerShell
    - 'Terminal Server'
---

I had a very interesting issue today on a new Citrix XenApp 5 farm. We went into production yesterday and we noticed a number of issues:

- <span style="color: #35383d;">Printing in general was slow, especially when a user connects to a printer for the first time.</span>
- User Profiles were rapidly growing in size (from the expected 1-2 MB to over 40 MB).
- Logons took much longer then in the testing period (and since we use a Full Screen Desktop the user doesn’t see any progress).
- Performance monitoring showed CPU spikes in Word, Excel and IE processes.

<span style="color: #4c4c4c;">I took a look at the profiles first and noticed that the size growth was due to a Xerox subfolder in %APPDATA%:  
</span>

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image2.png)

An even bigger problem is the huge amount of files in this folder (almost 9000) which of course causes the delay in the logon (while the server is loading the profile).

This is very bad considering that this driver, the Xerox Global Print Driver is advertised by Xerox as the Terminal Server/Citrix ready printer driver!

What makes it even worse is that the driver exhibiting this bug, is still the one offered for download on the [Xerox Site](http://www.support.xerox.com/support/global-printer-driver/downloads/enus.html?operatingSystem=wins2003&fileLanguage=en).

For the record: I found this bug in version 5.185.19 dated oct. 14, 2010 in the PCL5 and PCL6 versions for both x86 and x64.

The good news is that I found a newer version that was linked on the Xerox forum, version 5.185.32 dated jan. 5, 2011.

I tested this version and the bug was fixed there, I cannot yet tell if the CPU Spikes issue was also fixed.

Here are the links:

- [Xerox Global Print Driver x86 PCL5](http://www.support.xerox.com/support/_all-products/file-download/enus.html?contentId=114411)
- [Xerox Global Print Driver x64 PCL5](http://www.support.xerox.com/support/_all-products/file-download/enus.html?contentId=114412)
- [Xerox Global Print Driver x86 PCL6](http://www.support.xerox.com/support/_all-products/file-download/enus.html?contentId=114415)
- [Xerox Global Print Driver x64 PCL6](http://www.support.xerox.com/support/_all-products/file-download/enus.html?contentId=114416)

But after installing this driver we also need to cleanup the (roaming) profiles to get rid of the Xerox folder.

I wrote a very simple PowerShell script for that:  
“&gt;