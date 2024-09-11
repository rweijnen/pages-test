---
id: 1719
title: 'Extremely slow Virtual Machines on HP Smart Array P410'
date: '2011-05-02T14:15:50+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/05/02/extremely-slow-virtual-machines-on-hp-smart-array-p410/'
permalink: /2011/05/02/extremely-slow-virtual-machines-on-hp-smart-array-p410/
views:
    - '20069'
short-url:
    - 'http://bit.ly/jTH62r'
categories:
    - Citrix
    - VMWare
tags:
    - Citrix
    - HP
    - VMWare
---

I was deploying virtualized Citrix XenApp Servers on HP BL460c G6 servers and somehow the storage (direct attached) responded very slowly.

I had expected reduced performance (see [my earlier post](http://192.168.40.25:8081/2010/10/16/slow-power-on-and-storage-operations-with-hp-smart-array-p410i-controller-on-vmware-vsphere-4-0/)) since I didn’t have the Battery Backed Write Cache module installed.   
I did order them but had to start deployment before they arrived.

I did not however expect such an extreme bad performance. Deployment took ages or sometimes failed completely and when logging in to a VM it responded very sluggish.

**<u>Disk Latency</u>**

I looked in the vSphere console what the Disk Latency was. Latency under 10ms is usually considered good while a latency between 10 and 20ms is a potential performance problem.

I was shocked to notice that the Disk Latency was much higher with peaks toward 2.000 ms (2 seconds!):

[![DiskLatency](http://192.168.40.25:8081/wp-content/uploads/2011/05/DiskLatency_thumb.png "DiskLatency")](http://192.168.40.25:8081/wp-content/uploads/2011/05/DiskLatency.png)

At the same time the transfer rates were very low:

[![DiskKbps](http://192.168.40.25:8081/wp-content/uploads/2011/05/DiskKbps_thumb.png "DiskKbps")](http://192.168.40.25:8081/wp-content/uploads/2011/05/DiskKbps.png)

**<u>CPU Ready time</u>**

A look at the CPU Ready and CPU Wait counters was the final confirmation that storage was the bottleneck.

To view these counters, select the VM in question and go to the Performance Tab and click Advanced:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image.png)

Then add the counters:

[![SNAGHTML15276d9f](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15276d9f_thumb.png "SNAGHTML15276d9f")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML15276d9f.png)

These counters indicate that the CPU is waiting for something (the storage in this case).   
You will want these numbers to be as low as possible and they are very high here:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image1.png)

**<u>   
Console Errors</u>**

On the VMWare console I noticed the following errors:

- scsi\_cmd\_alloc returned NULL!
- WARNING: NMP: nmp\_DeviceRetryCommand
- WARNING: NMP: nmp\_DeviceAttemptFailover

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image2.png)

I went back from deploying multiple machines at once to just one but this didn’t improve the performance at all.

Using the error messages above I found this knowledge base article from Hewlett Packard: [To Obtain Optimal Disk Subsystem Performance on ProLiant G6 Servers Configured with Smart Array P410i/P410/P411/P412/P212/P712m Controllers Running VMware](http://h20000.www2.hp.com/bizsupport/TechSupport/Document.jsp?objectID=c01832427&lang=en&cc=us&taskId=101&prodSeriesId=3884082&prodTypeId=15351).

The article states: *When the Battery Backed Write Cache is not included in the configuration, even moderate disk I/O can negatively impact server performance.*

In my opinion the article actually says: performance without the battery backed cache module is is absolutely horrible and there is no way you can successfully run any Virtual Machine on the hardware without it!

And then, to my great relief, the upgrade hardware (including the BBWC) was delivered. So I immediately built in the BBWC because I wanted to include the results in this post:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image3.png)

After placing the BBWC module it will not be activated until the battery has been sufficiently charged:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image4.png)

**<u>   
Results</u>**

And then the results are amazing:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image5.png)

And finally a successful deployment:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image6.png)

Especially the time needed for restoring the base image went down a lot (this includes not only the actual restore but also sysprep, network config and several reboots).

**<u></u>**

Related links

- [To Obtain Optimal Disk Subsystem Performance on ProLiant G6 Servers Configured with Smart Array P410i/P410/P411/P412/P212/P712m Controllers Running VMware](http://h20000.www2.hp.com/bizsupport/TechSupport/Document.jsp?objectID=c01832427&lang=en&cc=us&taskId=101&prodSeriesId=3884082&prodTypeId=15351)
- [Slow virtual machine storage when using HP Smart Array P410 with ESX 3.5](<http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1036961&sliceId=1&docTypeID=DT_KB_1_1&dialogID=178739822&stateId=0 0 178741131>) (also on 4.1).
- [Slow power on and storage operations with the HP Smart Array P410i controller](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1018794)
- [HP 512MB P-Series Battery Backed Write Cache Upgrade](http://h10010.www1.hp.com/wwpc/uk/en/sm/WF06c/A1-329290-332469-3965891-3965891-434812-3890508.html)