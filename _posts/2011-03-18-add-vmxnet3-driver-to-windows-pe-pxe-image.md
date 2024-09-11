---
id: 1626
title: 'Add VMXNET3 driver to Windows PE PXE Image'
date: '2011-03-18T14:20:08+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/18/add-vmxnet3-driver-to-windows-pe-pxe-image/'
permalink: /2011/03/18/add-vmxnet3-driver-to-windows-pe-pxe-image/
views:
    - '10208'
short-url:
    - 'http://bit.ly/gWKQCe'
categories:
    - Altiris
    - VMWare
tags:
    - Altiris
    - PXE
    - VMWare
    - 'Windows PE'
---

After [compiling the VMWare VMXNET3 Driver](http://192.168.40.25:8081/2011/03/17/compiling-drivers-for-altiris-linux-pxe-image-part-2/) for Linux I needed a driver for the Windows PE Image as well.

Compared to what I needed to do for Linux this was a breeze!

First we need the VMWare tools again so I grabbed windows.iso from /vmimages/tools-isomages.

The driver files are in a cab file, VMXNET3.cab, extract this cab file somewhere and open the Altiris PXE Configuration tool.

Select the Windows PE Entry and click Edit:[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb39.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image39.png)

Then click Edit Boot Image:   
[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb40.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image40.png)

In the Boot Disk Creator right click and select Edit Configuration:[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb41.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image41.png)

Click Next:   
[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb42.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image42.png)

Click Have Disk:   
[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb43.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image43.png)

Browse to the folder where you extract the cab file, I choose the [NDIS5](http://msdn.microsoft.com/en-us/windows/hardware/gg463234) driver since it’s probably the most compatible version:   
[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb44.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image44.png)

Select the appropriate platform and click OK:   
[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb45.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image45.png)

Finish the Wizard and upload the modified image to your PXE Server and that’s really all!