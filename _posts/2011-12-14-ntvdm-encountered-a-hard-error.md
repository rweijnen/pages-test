---
id: 2254
title: 'NTVDM encountered a hard error'
date: '2011-12-14T21:41:22+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2254'
permalink: /2011/12/14/ntvdm-encountered-a-hard-error/
short-url:
    - 'http://bit.ly/seUJxZ'
views:
    - '15302'
categories:
    - C++
    - Citrix
    - 'Terminal Server'
    - 'Windows 2003'
tags:
    - Citrix
    - DOS
---

[![MS-Dos Logo](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb7.png "MS-Dos Logo")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image7.png)Today I troubleshooted an old DOS application that needed to run on a 32 bit Citrix XenApp Server. The last time I saw an actual DOS application in a production environment must be years ago.

When starting the application, the WOW subsystem (NTVDM) crashed with the message: “NTVM encountered a hard error.”:

[![NTVDM encoutered a hard error](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb8.png "ntvdm.exe - System Error")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image8.png)

After spending some time troubleshooting I remembered a similar issue from a few years ago where a DOS application worked fine from the Console but refused to work from an RDP or ICA session.

And indeed the application works perfectly when run from the Console but not from a Console <u>session</u>. I noticed that the application switched to full screen mode after it was launched (even when I set it to Windowed mode) and presumably this is why ntvdm errors: full-screen mode is disallowed for DOS apps in RDP (and ICA) sessions as documented in [Q192190](http://support.microsoft.com/?kbid=192190).

I looked for a way to force the application to run in windowed mode but I was unable to find such a solution. So I decided to test the application in [DOSBox](http://sourceforge.net/projects/dosbox/), an x86 PC emulator.

And that worked perfectly, no changes were needed at all to make the application run.

As an added bonus, DOSBox takes care of typical issues with DOS applications running on Citrix XenApp such as [keyboard polling and 100% cpu usage](http://support.citrix.com/article/CTX846521).

I was even more impressed that the application **runs fine with DOSBox on my Windows 7 64 bit machine**!

There is one thing I didn’t like though, DOSBox always shows a Splashscreen that fades in and out:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image9.png)

This is typically something that is not desirable on a XenApp (or RDS) environment because it causes many unnecessary screen updates. This may be a non issue on a fast LAN but on a slower WAN or high latency connection it may matter. Do how do we get rid of it?

There is no commandline argument or config setting that disables the splash so I figured that my only option would be to compile the DOSBox source and leave out the splash screen.

So I downloaded the source files from the sourceforge project page and launched Visual Studio 2010.

The Splashscreen is in sdlmain.cpp but I noticed this comment:

 “&gt;

This presented me with a dilemma: I really think the creators deserve their credit but at the same time I want to get rid of the splash.

So I decided to change the code in a way that the Splash screen is shown when run from the Console but not when run from an RDP or ICA session. This change was very easy, I surrounded the Splash screen code with a conditional statement:

“&gt;

In order to compile the code with Visual Studio I followed the [Building DOSBox with Visual C 2008 Express](http://www.dosbox.com/wiki/Building_DOSBox_with_Visual_C_2008_Express) article from the Wiki.

Below you will find two downloads:

- A binary package, containing my compiled DOSBox.exe and it’s dependancies.
- A source code package, containing the modified source and the SDL Development Libraries. The modified source is licensed under the [GNU GPL license](http://www.gnu.org/copyleft/gpl.html).

If you are going to use DOSBox I highly encourage you to make a donation to [Support the DOSBox project](http://sourceforge.net/donate/index.php?group_id=52551).

A few other causes for the “NTVM encountered a hard error” message may be:

- <font color="#35383d">The %TEMP% or %TMP% variable point to a directory that is not in a short (8.3) format. See also [CTX110996](http://support.citrix.com/article/CTX110996).</font>
- Error message when you run a 16-bit program in Windows Server 2003: "NTVDM has encountered a hard error<font color="#35383d">” ([Hotfix KB937932](http://support.microsoft.com/kb/937932)).</font>
- <font color="#35383d">Check that HKLM\\System\\CurrentControlSet\\Control\\WOW\\DisallowedPolicyDefault is set to 0</font>
- <font color="#35383d">Check that HKLM\\SYSTEM\\CurrentControlSet\\Control\\VirtualDeviceDrivers\\VDD doesn’t contain non existing files.</font>


[ DOSBox 0.74 Source Code (2166 downloads ) ](http://192.168.40.25:8081/download/dosbox-0-74-source-code/?tmstv=1726048919 "Version 0.74")