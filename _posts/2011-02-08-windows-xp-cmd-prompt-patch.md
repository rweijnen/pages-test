---
id: 1428
title: 'Windows XP Cmd Prompt Patch'
date: '2011-02-08T22:28:31+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1428'
permalink: /2011/02/08/windows-xp-cmd-prompt-patch/
views:
    - '4572'
short-url:
    - 'http://bit.ly/g0UEDQ'
categories:
    - 'Windows XP'
---

A few days ago I needed to test a few things on a Windows XP Workstation running under a regular user account.

I wanted to verify if some files and registry keys existed but Group Policies were in place that denied me access to the command prompt and regedit.

While this may be a good thought to secure the pc it is not very convenient if you need to verify some settings.

For that purpose I created patched versions of the Windows Server 2003 command prompt and regedit utilities.

They are patched to ignore the Group Policy settings and I usually place them in some share, secured by NTFS permissions.

You can read about it in my post: [Registry editing has been disabled by your administrator (not anymore!)](http://192.168.40.25:8081/2008/08/12/registry-editing-has-been-disabled-by-your-administrator/).

However due to kernel differences you cannot use the Windows 2003 cmd.exe on Windows XP (you can do it the other way round btw). So I decided to create a patched version of the XP version as well.

I thought it might be interesting to show you how it’s done so here we go:

First we open up cmd.exe in Ida Pro:

On the first screen you can accept the defaults:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image3.png)

Wait until the Auto Analyses has finished, this may take a little while:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image4.png)

Now go to the Functions Window and sort on Function name and look for any functions that may have something to do with the policy check.

Or use the Search function (ALT-T) like I did:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image5.png)

The first and only hit is a function Called GetCmdPolicy which sounds like what we need. DoubleClick on the function name to go to the Disassembly:

We can quickly see that this code is opening the Policies key:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image6.png)

And in the next block we can see that the DisableCMD value is checked:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image7.png)

Now we will patch this function to return 0 which will make it always start, regardless of the value of the DisableCMD value.

Press the Space Bar to go to Ida’s Flat View and note down the Address of this function:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image8.png)

Now we are going to use a Hex Editor to create the Patch.

I use [Hiew](http://www.hiew.ru/) in this example because it can Disassemble and Assemble so it’s a very convenient tool for this purpose.

Open cmd.exe in Hiew and press F4 to change the Mode to Decode:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image9.png)

Press F5 (Goto) and enter a dot (.) and the Address from Ida:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image10.png)

Now we see the same Disassembly as in Ida:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image11.png)

Now Press F3 to go to Edit Mode, followed by F2 to Select Assembly mode.

In the window that appears change mov edi,edi to mov eax,0 and press Enter:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image12.png)

Then change push ecx to ret 4 and press Enter:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image13.png)

Press Escape to Exit the Edit mode, we have now made the following changes:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image14.png)

Finally press F9 to save the changes and F10 to quit and we’re done!

And for the Lazy People the patched cmd.exe can be downloaded, usage is at your own risk:

[ Patched XP Command Prompt (1641 downloads ) ](http://192.168.40.25:8081/download/patched-xp-command-prompt/?tmstv=1726048919 "Version 1.0")