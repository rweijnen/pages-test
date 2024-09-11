---
id: 3586
title: 'Update AMD Display Driver under BootCamp'
date: '2015-09-21T22:57:15+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3586'
permalink: /2015/09/21/update-amd-display-driver-under-bootcamp/
short-url:
    - 'http://bit.ly/1iJJFNS'
views:
    - '93558'
categories:
    - Apple
    - BootCamp
tags:
    - AMD
    - Apple
    - BootCamp
    - Catalyst
    - Crimsom
    - Driver
    - MacBook
    - Radeon
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image.png)I’m using Windows 10 with [BootCamp](https://www.apple.com/support/bootcamp/) on my [MacBook Pro (Retina, 15-inch, Mid 2015](https://support.apple.com/kb/SP719?locale=en_US)). Overall I’m pretty happy with the hardware but Apple seems to limit functionality when running under Windows.

A good example is the trackpad which simply doesn’t operate as smoothly as under Mac OSX. This isn’t because Windows is a less good Operating System, it’s simply Apple supplying drivers and support software this is less good.

For the trackpad I found a good solution with [Trackpad++](http://trackpad.powerplan7.com/) which enables 2, 3- and 4-finger gestures and improves scrolling.

[![Display driver stopped responding and has recovered](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb1.png "Display driver AMD driver stopped responding and has successfully recovered.")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image1.png)The AMD Display Drivers as supplied with BootCamp are instable leading to “Display driver stopped responding” messages.

Some of the Advanced features in the supplies AMD Catalyst Control center such as Power Management are simply unavailable.

An annoyance is that AMD Catalyst Control Center reports that updates are available but when trying to install it errors with this message:

[![We are unable to find a driver for your system. No supported AMD hardware was detected.](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb2.png "AMD Catalyst Install Manager")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image2.png)

At this point [Andrew Morgan](https://twitter.com/andyjmorgan) and [Barry Schiffer](https://twitter.com/barryschiffer) are likely tweeting that I should stop using Windows on a Mac but I will ignore that.

So let’s go ahead and update those AMD drivers, shall we?

In my case the Catalyst Control Center stored the drivers in C:\\AMD\\AMD-Catalyst-15.7.1-Win10-64bit so I will use this path and this particular version and bitness (64 bit).

Note that modified files for the installation can be download from the bottom of the page. But please read/understand the steps below before downloading.

**<u>Step 1 – Edit InstallManager.cfg  
</u>**Add the line EnableFalcon=true to Config\\InstallManager.cfg:

[![EnableFalcon=true](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb3.png "InstallManager.cfg - Notepad")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image3.png)

<u>**Step 2 – Find your hardware id** </u>Open Device Manager, expand Display Adapter, right-click your Display Adapter and select Properties. Go to the Details Tab, select Hardware Ids and Copy the VENdor and DEVice id string:

[![Display Adapters | AMD Radeon R9 M370X | Details | Hardware IDs](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb4.png "Device Manager")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image4.png)

**<u>Step 3 – Edit driver inf file</u>**

Open Packages\\Drivers\\Display\\WT6A\_INF\\C0187674.inf with Notepad and search for PCI\\VEN\_1002&amp;DEV\_6821 (take the closes match, in my case REV\_83):

[![PCI\VEN_1002&DEV_6821&DEV_6821_REV_83](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb5.png "c0187674.inf - Notepad")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image5.png)

Change this line to match the Hardware Id in Device Manager:

[![PCI\VEN_1002&DEV_6821&DEV_6821&SUBSYS_0149106B&REV_83](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb6.png "c0187674.inf - Notepad")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image6.png)

Now take the section name of the same line, in my case **ati2mtag\_R577** and search for that section:

[![ExcludeID=PCI\VEN_1002&DEV_6821&SUBSYS_0149106B](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb7.png "c0187674.inf - Notepad")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image7.png)

**Remove** the line ExcludeID=&lt;Hardware Id&gt; and save the file.

**<u>Step 4 – Recreate the driver CAT file</u>**  
Because the AMD driver is digitally signed we needed to recreate the driver’s .cat file after changing the .inf file.

To do this we need the [inf2cat](https://msdn.microsoft.com/en-us/library/windows/hardware/ff553618(v=vs.85).aspx) tool, I used the version from the [Windows 10 Driver Kit](https://msdn.microsoft.com/en-us/windows/hardware/dn913721.aspx) (DDK).

Use the following command to create a new cat file (note that you should not specify the path to the inf file but to the directory where the inf file resides):  
“&gt;  
**<u>Step 5 – Digitally sign the driver CAT file</u>**  
Final step is to resign the CAT file, note that you need a code signing certificate that is valid for signing Kernel Mode drivers. I use a certificate from [DigiCert](https://www.digicert.com/code-signing/kernel-mode-certificates.htm),a company that I can highly recommend!  
“&gt;  
**<u>Step 6 – Install away!</u>**  
Now we can finally install by launch the Setup.exe, I recommend choosing for Custom installation and uncheck the Gaming Evolved App:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image8.png)

Much Better:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image9.png)

Also notice that PowerPlay options are now available:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/09/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/09/image10.png)

But most importantly – I have had no more crashes of the Display Driver!

As most people probably do not have a (kernel mode) code signing certificate, please find the modified .cat and .inf files below

**EDIT 25-11-2015:** Added AMD Crimsom 15.11 Driver to the download package

**EDIT 01-12-2015:** Added Radeon-Software-Crimson-Edition-15.11.1-Beta-64Bit-Win10-Win8.1-Win7-Nov30

**EDIT 18-12-2015:** Added Radeon-Crimson-15.12-Win10-64Bit

**EDIT 08-01-2016:** Added WHQL-Radeon-Software-Crimson-Edition-16.1-64Bit-Win10-Win8.1-Win7-Jan7

**EDIT 02-02-2016:** Added Non-WHQL-64Bit-Radeon-Software-Crimson-16.1.1-Win10-Win8.1-Win7-Jan30 **includes brightness control fix**

**EDIT 05-02-2016:** Added Non-WHQL-64Bit-Radeon-Software-Crimson-16.1.1-Win10-Win8.1-Win7-Feb3.zip (also includes brightness control fix)

**EDIT 25-02-2016:** Added Non-WHQL-64Bit-Radeon-Software-Crimson-16.2-Win10-Win8.1-Win7-Feb23 (also includes brightness control fix)

**EDIT 03-03-2016:** Added Non-WHQL-64Bit-Radeon-Software-Crimson-16.2.1-Win10-Win8.1-Win7-Feb27

**EDIT 18-03-2016:** Added Non-WHQL-64Bit-Radeon-Software-Crimson-16.3.1-Win10-Win8.1-Win7-March16.zip (also includes brightness control fix)

**EDIT 29-03-2016:** Added Radeon-Crimson-16.3.2-Win10-64Bit (also includes brightness control fix)

**EDIT 13-04-2016:** Added Non-WHQL-64Bit-Radeon-Software-Crimson-16.4.1-Win10-Win8.1-Win7-Apr4.zip (also includes brightness control fix

**EDIT 17-06-2016:** Added Non-WHQL-64Bit-Radeon-Software-Crimson-16.6.1-Win10-Win8.1-Win7-June2.zip (also includes brightness control fix

**EDIT 21-09-2016:** Added Non-WHQL-Win10-64Bit-Radeon-Software-Crimson-16.9.1-Sep7 (also includes brightness control fix

**EDIT 17-10-2016:** Added WHQL-Win10-64Bit-Radeon-Software-Crimson-16.10.1-Oct13 (also includes brightness control fix

**EDIT 16-01-2017:** Added Win10-64Bit-Radeon-Software-Crimson-ReLive-16.12.2-Jan3 (also includes brightness control fix) -&gt; note that this is a seperate download and as test I have included the **complete** installer. Download [here](https://remkoweijnen.sharefile.eu/d-sc5ffa5462ac4e56b "here")

**EDIT 18-01-2017:** Added Non-WHQL-Win10-64Bit-Radeon-Software-Crimson-ReLive-17.1.1-Jan16 (also includes brightness control fix) -&gt; note that this is a seperate download and as test I have included the **complete** installer. Download [here](https://remkoweijnen.sharefile.eu/d-sc6e502305814b37b "here")

**EDIT 10-06-2017:** Added Non-WHQL-Win10-64Bit-Radeon-Software-Crimson-ReLive-17.6.1-June6. note that this is a seperate download including **complete** installer. Download [here](https://remkoweijnen.sharefile.eu/d-sfdfbb4f715f4463a "here")

**EDIT 12-07-2017:** Added Non-WHQL-Win10-64Bit-Radeon-Software-Crimson-ReLive-17.7.1-July10. note that this is a seperate download including **complete** installer. Download [here](https://remkoweijnen.sharefile.eu/d-s2b195bbae6449d19 "here")

**EDIT 19-01-2018:** Added Win10-64Bit-Radeon-Software-Adrenalin-Edition-18.1.1-Jan18.zip. Note that this is a seperate download including **complete** installer. Download [here](https://remkoweijnen.sharefile.eu/d-s73888b8fdef4113b "here")

[![Download modified inf and cat file](http:///192.168.40.25:8081/wp-content/uploads/2014/07/downloadbutton.gif?resize=120%2C36 "Download")](https://remkoweijnen.sharefile.eu/d-s9d78113402d4f048)