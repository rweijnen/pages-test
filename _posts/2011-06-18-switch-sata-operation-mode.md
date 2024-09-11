---
id: 1883
title: 'Switch SATA Operation Mode'
date: '2011-06-18T21:38:19+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1883'
permalink: /2011/06/18/switch-sata-operation-mode/
short-url:
    - 'http://bit.ly/kmNbUW'
views:
    - '51210'
categories:
    - 'Windows 7'
tags:
    - AHCI
    - Dell
    - IIRT
    - Intel
---

Modern systems usually offer different SATA Operation Modes such as ATA, AHCI or IRRT.

The AHCI mode offers extra features such as hot swapping and [native command queuing](http://en.wikipedia.org/wiki/Native_Command_Queuing "Wikipedia Native Command Queueing").

Many vendors set the SATA Operation Mode to ATA by default because it’s the most compatible mode but there are a few reasons why you might want to change it:

- AHCI has a higher performance than ATA.
- AHCI is a requirement for the [TRIM command](http://www.bit-tech.net/hardware/storage/2010/02/04/windows-7-ssd-performance-and-trim/ "Why You Need TRIM For Your SSD").
- AHCI is required for self encrypting hard drives

<span style="color: #4c4c4c;">Please note that the IRRT (integrated raid) mode is supposed to support all functionality of AHCI but in my experience it doesn’t. </span>

<span style="color: #4c4c4c;">So the question is: how do we switch the SATA Operation Mode from ATA or IRRT to AHCI?</span>

<span style="color: #4c4c4c;">The switch itself is very easy, just go to your bios and change the setting. On my Dell laptop, the setting is System Configuration | SATA Operation:</span>

[![Dell Precision M4500 BIOS SATA Operation Mode AHCI](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb12.png "Dell Precision M4500 BIOS SATA Operation Mode AHCI")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image12.png)

<span style="color: #4c4c4c;">But if we change this setting after we have installed the Operating System it will crash on boot with a STOP 0x0000007B INACCESSABLE\_BOOT\_DEVICE error:</span>

![STOP 0x0000007B INACCESSIBLE BOOT DEVICE BSOD](http://forums.citrix.com/servlet/JiveServlet/download/170-252702-1408148-27794/bsod_07b.JPG "STOP 0x0000007B INACCESSIBLE BOOT DEVICE BSOD")

There is a [Microsoft KB Article](http://support.microsoft.com/kb/922976) that describes the steps required to fix this problem. However for Intel SATA Controllers (the most common) we need to perform a few additional steps.

After some experiments I was able to create a recipe for it:

1. Set the Microsoft MSAHCI driver to Autostart
2. Add the Hardware (PnP) Id of your SATA controller to the Critical Device Database in the registry
3. Add Intel’s iaStor driver to both registry and drivers folder
4. Add the Device Instance Path of your SATA controller to the iaStor registry key.

<span style="color: #4c4c4c;">On a Dell Precision M4500 the reg file to accomplish these steps looks like this:</span>

[![Registry File for Dell Precision M4500 Switch AHCI mode](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb13.png "Registry File for Dell Precision M4500 Switch AHCI mode")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image13.png)

Unfortunately both the Hardware Id and Device Instance Path differ for each SATA controller. So how do we obtain the correct values?

If you have another machine with the same SATA controller which is already in AHCI mode then you can simply look up the values in Device Manager.

Just find your AHCI controller under IDE ATA/ATAPI controllers:

[![Device Manager](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML5ed560a_thumb.png "Device Manager")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML5ed560a.png)

Right Click, select properties and on the Details tab select Hardware Ids. The PNP id is the bottom value:

[![SATA AHCI Controller Properties](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML6f44c8e_thumb.png "SATA AHCI Controller Properties")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML6f44c8e.png)

Then select the Device Instance Path Property:

[![SATA AHCI Controller Device Instance Path](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTMLd60a7f_thumb.png "SATA AHCI Controller Device Instance Path")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTMLd60a7f.png)

But how do we obtain the proper values if you are not fortunate enough to have another system?

I figured the easiest way would be to write a tool that can lookup these values programmatically and run that under Windows PE (from CD/DVD or USB).

So how does it work? Assuming you already have a Windows PE bootdisk, the steps are:

Switch the SATA Operation Mode to AHCI, boot Windows PE and start my tool (sci.exe). It will automatically search for and display all SATA storage controllers:

[![SATA Controller Identifier Screenshot](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTMLea846f_thumb.png "SATA Controller Identifier Screenshot")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTMLea846f.png)

Select the controller for which you would like to generate a reg file and click the Save button which allows you to pick the name and place where you want to save the reg file:

[![SNAGHTMLeb55b2](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTMLeb55b2_thumb.png "SNAGHTMLeb55b2")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTMLeb55b2.png)

**<span style="color: #ff0000;">Update 21-06-2011:</span>** The Save File Dialog didn’t work properly under Windows PE, I have uploaded a new version of the tool (1.1) that contains a fix.

Here is an example file for a Dell Latitude E6500:  
“&gt;  
Now we need to import the generated reg file in your Windows installation. To be able to do this, you have to first switch back the SATA Operation Mode to the previous setting to prevent the BSOD.

After importing the generated reg file, copy the iaStor file to the Drivers folder (usually C:\\Windows\\System32\\Drivers) and reboot.

Final step is to switch the SATA Operation Mode to AHCI for the last time and boot into the OS.

Although not strictly required it’s a good idea to install the full intel drivers (Intel Rapid Storage Technology). At the time of writing the latest version was 10.1.0.1008 which can be found [here](http://downloadcenter.intel.com/Detail_Desc.aspx?DwnldID=19607).

It would nice if we could compile a list of popular SATA controllers so we can help other people so please consider running the tool on your system and adding the results as a comment here.

[ SATA Controller Identifier (4177 downloads ) ](http://192.168.40.25:8081/download/sata-controller-identifier/?tmstv=1726048919 "Version 1.0")