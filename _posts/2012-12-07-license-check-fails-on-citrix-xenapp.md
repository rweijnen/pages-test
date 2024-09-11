---
id: 2870
title: 'License Check fails on Citrix XenApp'
date: '2012-12-07T16:30:07+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2870'
permalink: /2012/12/07/license-check-fails-on-citrix-xenapp/
short-url:
    - 'http://bit.ly/1276JbC'
views:
    - '1595'
categories:
    - Citrix
tags:
    - Citrix
    - XenApp
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image14.png)Today I was asked to assist in getting an Excel Add-In to work on Citrix XenApp.

The application was packaged into a Thinapp by one of our package engineers.

However when testing the Add-In on Citrix XenApp the following message appeared:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image15.png)

Apparently this application does a license check that fails when run from another server (how bad).

**<span style="text-decoration: underline;">Disclaimer</span>**  
Before we go on: I would like to make clear that my goal is not to be able to use an application without license. I am just trying to make it work within the customer’s environment.

**<span style="text-decoration: underline;">Analysis  
</span>**The application consisted of an xla (Excel Add-In Sheet) and two Dll’s called analyse.dll and analyse12.dll.

It seemed logical to me that the license check was in the Dll’s and that there were two versions: one for Excel 2003 and one for Excel 2007 (version 12) and possibly 2010.

I loaded analyse.dll in [Ida Pro](http://www.hex-rays.com/products/ida/index.shtml) so I could analyze it.

The exports Tab shows a few exports that are common to [XLL Files](http://support.microsoft.com/kb/178474):

[![SNAGHTML14624cb3](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML14624cb3_thumb.png "SNAGHTML14624cb3")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML14624cb3.png)

Inside the Dll there was a lot of trace/debug output, like this:  
“&gt;  
So I looked into sub\_9A6E9AD to see if we could output this trace info:  
“&gt;  
sub\_9ADA1F6 is a function for registry access:  
“&gt;  
From this code I concluded that if a DWORD registry value Dumplog (Value 1) exists in the application’s registry key (HKLM\\SOFTWARE\\Analyse-it\\Analyse-it for Excel\\1.0) a logfile would be written to the Temp directory.

The logfile was helpful in determining what happended:

checking registry access  
“&gt;  
That confirmed the computer check, but where does it come from?

Again the debug output was helpful:  
“&gt;  
Let’s have a look inside the check, sub\_9A6AFDB (only showing the relevant piece):  
“&gt;  
The location of the Windows directory is retreived and then the volume serial number is requested for that volume.

The obtained serial number is compared to the serial that was saved during activation:  
“&gt;  
If we analyze the code above we can determine that a registry key is accessed in the HKEY\_CLASSES\_ROOT hive (-2147483648 = 0x80000000 in HEX which is the constant for HKEY\_CLASSES\_ROOT)

The key is Excel.App and the value is Flags32d.

The value of this key on the machine that had been activated matched 14065cba and was also the machine’s volume serial.

So the solution was there: I placed the Flags32d value in the ThinApp and in the package.ini I added the following line:  
“&gt;  
Now the application worked nicely on my testmachine (even though it has a different volume serial number):

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image16.png)

On the production XenApp however the Thinapp failed!

It took me some time to realize why, rember this code?  
“&gt;  
It retrieves the Windows directory and application that are not [Terminal Server aware](http://192.168.40.25:8081/2012/12/04/file-not-found-error-when-scanning-using-twain-redirection-in-citrix-xenapp/) have the Windows directory redirected to their User Profile!

In this case the User Profile was on the H drive so I change the package.ini to:  
“&gt;  
And not it worked nicely!

**<span style="text-decoration: underline;">And a small PS to developers</span>**: First of all: License check’s don’t work at all, removing them is often very easy. Secondly license checks often do more harm than good. If a bug in the license check hurts a paying customer what have you accomplished? In this case there are at least two bugs (or bad design) in only a few lines of code!