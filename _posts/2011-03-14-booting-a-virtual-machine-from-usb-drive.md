---
id: 1545
title: 'Booting a Virtual Machine from USB Drive'
date: '2011-03-14T12:59:53+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/14/booting-a-virtual-machine-from-usb-drive/'
permalink: /2011/03/14/booting-a-virtual-machine-from-usb-drive/
views:
    - '2549'
short-url:
    - 'http://bit.ly/hHHCXM'
categories:
    - VMWare
tags:
    - Boot
    - USB
    - VMWare
---

I wanted to boot a Virtual Machine from an USB Stick but even though you can Connect USB devices to VMWare you cannot boot from it.

It can be done however using a boot manager that is able to perform a boot from USB media. I used [Plop Boot Manager](http://www.plop.at/en/bootmanager.html#download).

Download one of the stable releases (I used [5.0.11-2](http://download.plop.at/files/bootmngr/plpbt-5.0.11-2.zip)) and extract plpbt.img from the archive and mount this (don’t forget to select the Connect at power on option) and when booting press Esc for the Boot Menu.

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image6.png)

This would be a good time to Connect the USB device to the Virtual Machine, right click the USB device in the bottom bar:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image7.png)

And select the Connect option:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image8.png)

Click OK on the warning message:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image9.png)

Now press Enter in the VMWare Boot Menu and select USB in the Plop Boot Manager:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image10.png)

And that’s it, we boot from USB, in my case the USB stick also has a Boot menu:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image11.png)