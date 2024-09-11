---
id: 1607
title: 'Compiling Drivers for Altiris Linux PXE Image Part 2'
date: '2011-03-17T15:39:12+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1607'
permalink: /2011/03/17/compiling-drivers-for-altiris-linux-pxe-image-part-2/
short-url:
    - 'http://bit.ly/gJg0x3'
views:
    - '4616'
categories:
    - Altiris
    - VMWare
---

In the [previous part](http://192.168.40.25:8081/2011/03/17/compiling-drivers-for-altiris-linux-pxe-image-part-1/) we have already setup the Ubuntu Virtual Machine and we did a build of the kernel image.

So now we can finally compile the driver, in my case I needed a driver for VMWare’s VMXNET3 Network Card.

[VMXNET3](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1001805) is VMWare’s paravirtualized network driver and offers better performance with less host processing power compared to the default e1000 driver.

First we need the source for the driver, we can obtain this from the VMWare Tools either from a running Linux VM or like I did by transferring the file linux.iso from /vmimages/tools-isomages from the vSphere server.

In the iso file is a single file, VMWARETO.TGZ and after unpacking we get a folder called *vmware-tools-distrib*.

In *vmware-tools-distrib/lib/modules/source* we find the vmxnet3.tar file that contains our sources. Copy the tar to the Virtual Machine and unpack it, then start a Terminal and cd to the directory where you unpacked the tar.

The first time I attempted a compile I received an error indicating that the file *autoconf.h* could not be found. After I found this [bug report](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/579813) I was able to fix this by creating a link:  
“&gt;  
We can compile the driver with the make command, referencing the kernel image we created earlier:

  
“&gt;  
[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb33.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image33.png)

After the compile we need to copy the .ko file, in my case vmxnet3.ko to the Altiris server, it needs to be placed in  
“&gt;  
[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb34.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image34.png)

And the final step is to rebuild the PXE Image, goto the PXE Configuration Manager and select Linux in the Regenerate Boot Images part and click Regenerate:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb35.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image35.png)

When the regeneration has been completed you must click Save and wait until all servers are updated:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb36.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image36.png)

In the Status tab you can check the Progress (this may take a while!):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb37.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image37.png)

And when it’s updated, boot the VM and check:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb38.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image38.png)

For your convenience the compiled binary can be downloaded below (but do know that if probably only works on the same kernel version).

[ VMWare VMXNET3 Linux Driver (1414 downloads ) ](http://192.168.40.25:8081/download/vmware-vmxnet3-linux-driver/?tmstv=1726048919 "Version 1")