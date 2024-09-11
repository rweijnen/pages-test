---
id: 3609
title: 'Deploy VCSA 6 to vCloud Director or vCloud Air'
date: '2015-10-05T20:11:47+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3609'
permalink: /2015/10/05/deploy-vcsa-6-to-vcloud-director-or-vcloud-air/
short-url:
    - 'http://bit.ly/1KWFGGN'
views:
    - '1482'
categories:
    - PowerShell
    - VMWare
tags:
    - PowerShell
    - 'vCloud Air'
    - 'vCloud Directory'
    - VCSA
    - VMWare
---

In versions prior to 6.0 VMware supplied the VCSA (vCenter Server Appliance) as an OVF template that could be imported directly.

Starting with version 6.0 the installation process has changed and now consist of an .iso file containing a custom, HTML based, installer. Vladan Seget has a nice [blog post](http://www.vladan.fr/install-vmware-vcsa-6-0/) where he describes the installation.

This installation process is annoying, it needs a separate client (Windows) machine to run the installer on, requires the Client Integration Plugin (which doesn’t appear to run well on chrome now that support for npapi/dpapi has been removed):[   
![Please install the Client Integration Plugin 6.0 provided in the vCenter Server Appliance ISO image (requires quitting the browser)](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb.png "Wheb prompted, allow access to the Client Integration Plugin")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image.png)   
But even worse is that we cannot import VCSA 6.0 in vCloud Director. Even converting the OVF inside the iso file doesn’t help because vCloud directory lacks support for Deployment Options.

William Lam wrote a nice blogpost describing [How to deploy VCSA 6 on vCloud Director and vCloud Air](http://www.virtuallyghetto.com/2015/04/how-to-deploy-vsphere-6-0-vcsa-esxi-on-vcloud-director-and-vcloud-air.html).

I followed the 5 steps William describes to get a working OVF, however when I tried to import the OVF into vCloud Director I received the following message:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image1.png)

This happens because the VirtualHardwareSection still contains references to all the configurations (that we removed in step 3):

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image2.png)

So for example if you’ve chosen “Tiny” you need to remove small, medium, large etc for all items which is quite a lot of work and error prone. And with the next version of VCSA you’ll need to do it again.

Therefore I’ve automated the process with PowerShell, let’s have a look at how the script works.

You need to extract the .iso file you downloaded from VMware and then run the PowerShell script.

**Step 1** – The script will show a file dialog which you need to point to the vmware-vcsa file:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image3.png)

**Step 2** – The script will call ‘ovftool’ to convert the OVA file to an OVF (the OVF will be written to an ovf subfolder):

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image4.png)

**Step 3** – The script will parse the OVF and ask you which type of appliance you’d like to deploy:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image5.png)

**Step 4** – The script will ask which configuration you’d like:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image6.png)

The script will save the new OVF file as vmware-vcsa-new.ovf in the OVF subfolder. And that’s it, you can now import/upload vmware-vcds-new.ovf

[![Download signed PowerShell script to convert VMware VCSA iso into OVF that can be deployed to vCloud Director or vCloud Air](http:///192.168.40.25:8081/wp-content/uploads/2014/07/downloadbutton.gif?resize=120%2C36 "Download")](https://remkoweijnen.sharefile.eu/d-s5e916b0d7134406b)