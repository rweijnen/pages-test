---
id: 3007
title: 'The case of the missing audio'
date: '2013-01-25T16:07:06+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3007'
permalink: /2013/01/25/the-case-of-the-missing-audio/
short-url:
    - 'http://bit.ly/W4tVXc'
views:
    - '902'
categories:
    - Citrix
    - RES
    - 'Windows XP'
---

Yesterday I was asked to investigate a problem with a presentation pc. Even though the volume was set maximal there was not audio output.

The machine was used to connect to a Citrix XenApp desktop and RES Workspace Extender was used to integrate local applications in the XenApp desktop.

The local sound volume control was published as a subscribed application so I launched that and verified that the volume was set to Maximum:

[![Volumeregeling](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb16.png "Volumeregeling")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image16.png)

I decided to launch the local explorer shell and noticed that there were two volume control icons in the Traybar:

[![Volume Controls](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb17.png "Traybar")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image17.png)

I clicked the second icon and a control named “Philips Audio Control” popped up:

[![Volume Muted](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb18.png "Philips Audio Control")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image18.png)

I raised the volume in the Philips Audio Control and immediately the audio worked as expected.

I was curious where this “Philips Audio Control” came from and found out that it belongs to the Philips [G2 Speech](http://www.g2speech.com) software.

Apparently the G2 Speech driver was put in the pc image to accommodate medical staff that use dictation and speech recognition software (so they can use it on any pc).

When the G2 Speech hardware is plugged in the volume controls are synched so if you adjust one of the two volumes the other follows.

However when the G2 Speech hardware is not present, the Philips Audio Control volume is set to mute by default and setting the master volume to maximum still gives no audio.

This was a potential issue on all pc’s so I needed to find a way to set the volume programmatically.

I investigated the Philips Audio Control binary (PspContr.exe) with my favourite tool, Ida Pro. PspContr.exe loads an audio driver:

 “&gt;

I loaded pspaudrv.dll in Ida Pro calls and it calls into a function named PSPSBEXTwodMessage from pspsbext.dll with different integers (messages?). I thought that this might be of interest because wodMessage could mean [Waveform Output Driver](http://msdn.microsoft.com/en-us/library/ms923287.aspx) Message.

“&gt;

If you look at the marked lines you can see that message 17 is setvolume and message 16 is getvolume!

From the code in PspContr.exe we can see a sample for calling getvolume:

“&gt;

That made it quite easy to guess the function parameters:

“&gt;

I made a commandline tool to get and set the volume so it could be deployed with the customer’s deployment tool (RES Automation Manager).

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image19.png)

[ Philips G2 Speech Volume Commandline Tool (1595 downloads ) ](http://192.168.40.25:8081/download/philips-g2-speech-volume-commandline-tool/?tmstv=1726048920 "Version 1.0")