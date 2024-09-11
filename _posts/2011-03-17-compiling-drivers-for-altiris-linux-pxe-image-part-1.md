---
id: 1594
title: 'Compiling Drivers for Altiris Linux PXE Image Part 1'
date: '2011-03-17T13:07:22+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/17/compiling-drivers-for-altiris-linux-pxe-image-part-1/'
permalink: /2011/03/17/compiling-drivers-for-altiris-linux-pxe-image-part-1/
views:
    - '3973'
short-url:
    - 'http://bit.ly/hhJDKt'
categories:
    - Altiris
    - VMWare
tags:
    - Altiris
    - Compile
    - Driver
    - Kernel
    - Linux
    - VMWare
---

First we need to setup a Linux Virtual Machine with a distro of choice (I recommend a 32 bit version). I will be using Ubuntu here and the first step is to [download](http://www.ubuntu.com/desktop/get-ubuntu/download) the iso.

At the time of writing Ubuntu 10.10 was the Latest version so I used that one.

Create a new Virtual Machine and use the iso as install media, I am using VMWare Workstation and it recognises Ubuntu and performs an “easy install”:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb23.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image23.png)

The install is unattended (when VMWare Tools are installed you need to perform a login) and took only 6 minutes on my laptop!

Now we need to install gcc (the compiler), open the Ubuntu Software Center:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb24.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image24.png)

Select Get Software | Developer Tools, search for GCC and select The GNU C Compiler from the list and click Install:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb25.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image25.png)

Authenticate with the root password:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb26.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image26.png)

Optionally install g++ as well:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb27.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image27.png)

Now we need to download the Kernel version that is used in the Altiris PXE environment.

Check the required version by starting the Boot Disk Creator and select Tools | Install Preboot Operating Systems (in my case it’s 2.6.27.7):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb28.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image28.png)

Open FireFox in the Virtual Machine and goto [http://www.kernel.org/pub/linux/kernel/v2.6/](http://www.kernel.org/pub/linux/kernel/v2.6/ "http://www.kernel.org/pub/linux/kernel/v2.6/") and download the file for your version (in my case [linux-2.6.27.7.tar.gz](http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.27.7.tar.gz)).

FireFox suggests to open it with the Archive Manager which is fine:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb29.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image29.png)

Extract the files using the Archive Manager to your home directory (I choose the Documents folder).

Now we need to obtain the config file, the easiest way is to manually boot into Linux Automation and copy the file to the eXpress share (*cp /proc/config.gz /mnt/ds*).

Copy this file to the directory where you extracted the kernel (in mycase /home/rweijnen/documents/linux-2.6.27.7).

Now start a Terminal via Accessories | Terminal:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb30.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image30.png)

cd to the directory where you extracted the kernel and extract the config.gz file (*gzip -d config.gz*) and then rename the config file to .config (*mv config .config*):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb31.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image31.png)

Now build the kernel (*make bzImage modules*):

I got a question about Removable Drive Bay, I choose N since it doesn’t seem necessary:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb32.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image32.png)

The build may take some time!

In [part 2](http://192.168.40.25:8081/2011/03/17/compiling-drivers-for-altiris-linux-pxe-image-part-2/) we will compile the driver and integrate it into the PXE Image.