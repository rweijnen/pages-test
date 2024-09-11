---
id: 1809
title: 'Installing Dell Wireless 5530 HSPA Mini PCI'
date: '2011-05-27T17:40:14+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/05/27/installing-dell-wireless-5530-hspa-mini-pci/'
permalink: /2011/05/27/installing-dell-wireless-5530-hspa-mini-pci/
views:
    - '48563'
short-url:
    - 'http://bit.ly/kl4HEC'
categories:
    - Uncategorized
tags:
    - Dell
    - Hack
    - MSI
    - Orca
    - Wireless
---

**EDIT:** See my [followup article](http://192.168.40.25:8081/2011/11/13/installing-dell-wireless-5530-hspa-mini-pc-2/) to learn how to reverse driver\_auth.exe, decrypt and encrypt dell\_wwan\_sysID.dat.

I bought a Dell Wireless 5530 HSPA Mini PCI card for my Dell Precision M4500 laptop.

This is a small expansion card that works together with the built in SIM card slot that is present in most Dell (Business) laptops.

[![-) 016](http://192.168.40.25:8081/wp-content/uploads/2011/05/016_thumb.jpg "-) 016")](http://192.168.40.25:8081/wp-content/uploads/2011/05/016.jpg)

This SIM card slot is usually located near the battery compartment:

[![SimCardSlot](http://192.168.40.25:8081/wp-content/uploads/2011/05/SimCardSlot_thumb.png "SimCardSlot")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SimCardSlot.png)

The card was installed in a few minutes since the antenna cables were present already and on my laptop I only needed to remove the backcover with just one screw.

Then I wanted to install the required software but this card is not officially supported in the M4500 (I bought this card because it was much cheaper on ebay).

So I took the driver from the M4400/Latitude E range, labeled [R251153](http://ftp.dell.com/comm/R251153.exe "R251153.exe") but I got this error message when installing:

[![Internal error 23000. Authentification failed. The Dell Wireless 5540 HSPA Mobile Broadband Mini-Card cannot be installed on this computer](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb22.png "Internal error 23000")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image22.png)

The problem is that the installer checks if the combination of the Mini PCI card and the laptop is authorized by Dell. For this purpose the installer extracts two custom executables to the temp folder. In my case the folder was *%temp%{85A2C545-B193-4053-8F3E-BB1527A73676}*.

The files are: *driver\_auth.exe* and *devcon.exe*. driver\_auth.exe decrypts a file that is delivered with the installation package named *dell\_wwan\_sysID.dat*.

Although it wouldn’t be very hard to patch driver\_auth.exe to our likings we would need to repack it in the original msi, that in turn is packed in an Installshield .cab file.

So I figured it would be easier to patch the msi and remove the check. I used Microsoft’s Orca tool for this.

The MSI was extracted to *%temp%\\{A9664DEE-892B-4461-9E86-4EBAF86F69C2}\\Dell Wireless HSPA Mini-Card Drivers.msi*.

Here comes the step by step process:

Start the Dell installer until you get the error message above, *do not click OK* but leave the message on the screen.

Open this MSI file in Orca and remove the following sections:

CustomAction | Authenticate:

[![Orca Screenshot: CustomAction | Authenticate](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15daea8f_thumb.png "Orca Screenshot: CustomAction | Authenticate")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15daea8f.png)

InstallExecuteSequence | Authenticate

[![Orca Screenshot: InstallExecuteSequence | Authenticate](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15dbdb05_thumb.png "Orca Screenshot: InstallExecuteSequence | Authenticate")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15dbdb05.png)

InstallUISequence | Authenticate:

[![Orca Screenshot: InstallUISequence | Authenticate](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15e0c0e1_thumb.png "Orca Screenshot: InstallUISequence | Authenticate")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15e0c0e1.png)

Now Save the MSI, *DO NOT USE Save As but overwrite the original MSI*, and Install it.

Then click OK on the Error Message from the Dell Installer and finish the install.

Final step is to reinstall the software completely by running the Dell Installer, it will notice that it’s already been installed so it will do a full install now without complaining.

That’s all folks!

[![Dell Mobile Broadband Manager](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15e00df7_thumb.png "Dell Mobile Broadband Manager")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15e00df7.png)

**EDIT:** If you have a Lenovo machine with a Bios WhiteList you may find a solution [here](http://www.sbbala.com/DellWWAN/Whitelist.htm)  
**EDIT 2:** See my [followup article](http://192.168.40.25:8081/2011/11/13/installing-dell-wireless-5530-hspa-mini-pc-2/) to learn how to reverse driver\_auth.exe, decrypt and encrypt dell\_wwan\_sysID.dat.