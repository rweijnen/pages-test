---
id: 1651
title: 'Tips for using SysPrep with Altiris'
date: '2011-03-22T12:29:32+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/22/tips-for-using-sysprep-with-altiris/'
permalink: /2011/03/22/tips-for-using-sysprep-with-altiris/
views:
    - '8514'
short-url:
    - 'http://bit.ly/gUeuPr'
categories:
    - Altiris
tags:
    - Altiris
    - Sysprep
---

Altiris has built in support for [Sysprep](http://en.wikipedia.org/wiki/Sysprep) when creating or distributing images.

The documentation doesn’t mention some things that are worth knowing so I will try to address them in this post.

Sysprep support can be added to Altiris during the install where it will ask you for the Sysprep install files (deploy.cab) per selected OS.

If you didn’t add Sysprep during install you can copy deploy.cab to one of subfolders in the Sysprep folder. Eg for 32 bit Windows 2003 deploy.cab goes to *Sysprep\\DotNet\\x86*:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb46.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image46.png)

As I wrote earlier in [this post](http://192.168.40.25:8081/2010/11/03/cannot-complete-customization-when-cloning-from-template/) it’s very important to use the correct Sysprep version as each OS has it’s own version.

As a general rule you should grab the deploy.cab from the install media (in the Support folder).

**<u>Creating Images:</u>**

When creating Images you can activate Sysprep support by setting the “Prepare using Sysprep” checkbox:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb47.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image47.png)

If you do not need to perform special configuration for you mass storage (eg add drivers) you can select “*Disable mass storage device support*” under Advanced Settings.

This will make the initial Sysprep run a little faster:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb48.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image48.png)

Optionally you can select an Automation environment, I like the LinuxPE option since it’s boots fast like the DOS option but still as fast imaging like Windows PE.

A few earlier post regarding PXE Boot that may be useful: - [STOP: 0x0000005D when booting Windows PE](http://192.168.40.25:8081/2011/03/15/stop-0x0000005d-when-booting-windows-pe/)
- [Compiling Drivers for Altiris Linux PXE Image Part 1](http://192.168.40.25:8081/2011/03/17/compiling-drivers-for-altiris-linux-pxe-image-part-1/)
- [Compiling Drivers for Altiris Linux PXE Image Part 2](<http://Compiling Drivers for Altiris Linux PXE Image Part 2>)

When you schedule a Create Image task with Sysprep activated, Altiris will copy the files in the deploy.cab to C:\\Sysprep on the target computer and run Sysprep.

It will also copy a default sysprep.inf file that is used to restore the target computer after imaging. But where does this sysprep.inf file come from?

The Sysprep folder is populated with some default answer files, named standard&lt;os version&gt;.inf:

[![SNAGHTML34e5fbd](http://192.168.40.25:8081/wp-content/uploads/2011/03/SNAGHTML34e5fbd_thumb.png "SNAGHTML34e5fbd")](http://192.168.40.25:8081/wp-content/uploads/2011/03/SNAGHTML34e5fbd.png)

So I assumed it would select the answer file based on the selected OS version. I selected Windows 2003 in my Job:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb49.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image49.png)

So I assumed it would copy *standard2003.inf* but instead it always seems to copy ***standardxp.inf**.*

If you cannot use this file, eg because you have differing files per OS you can copy a sysprep anwer file after the create image job.

If you want to do this using Linux a sample command would be:

 “&gt;

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb50.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image50.png)

**<u></u>**

**<u></u>**

**<u>Deploying Images</u>**

In the Deploy Images task you can enable Sysprep support in the same manner:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb51.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image51.png)

But this time the Advanced Settings buttons allows us to select a custom Sysprep Answer file:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb52.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image52.png)

In the Answer file we can use both Default and Custom tokens, Altiris will replace them to the actual values before injecting it.

An example of using the Tokens is using the name in the Altiris Console as the new Computer Name:

“&gt;

Another place where this feature comes in handy is the *AdminPassword* key in the GuiUnattended section:

“&gt;

%ADMIN\_PASSWORD% is not a default token so you must enter the password in the user\_tokens table:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb53.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image53.png)

The password is stored securely because a regular user shouldn’t be able to access the eXpress database directly.

Note that I also tried to use an encrypted password in the answer file (setupmgr.exe allows you to do so) but this results in an error message during Sysprep which will halt the process until you click OK.

This behavior is documented [here](http://support.microsoft.com/kb/312394/en-us).

Note: if you use the HP branded version of Altiris a simple User Tokens Editor is included in the Tools | HP Tools menu.

**<u>Multiple Disks</u>**

By default Altiris will only create or deploy an image of the first disk. If you want to create or deploy images for multiple disks you can create a second task and use the -d&lt;number&gt; parameter.

The “*Do not boot to Production*” option will ensure that the second image is created directly after the first disk image:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb54.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image54.png)

When restoring multiple disks, the easiest way is to restore the extra disk(s) <u>first</u> by using the -d&lt;number&gt; parameter again.

Deselect “*Automatically perform configuration task after completing this imaging task*“:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb55.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image55.png)

The system disk image must be the last one restored and there you enable both configuration options:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb56.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image56.png)

(Don’t forget to select the Sysprep Answer file using the Advanced Settings option)