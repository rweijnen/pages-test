---
id: 669
title: 'Error while creating  large Datastore on VMWare vSphere 4.1'
date: '2010-10-18T16:12:43+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=669'
permalink: /2010/10/18/error-while-creating-large-datastore-on-vmware-vsphere-4-1/
views:
    - '7957'
short-url:
    - 'http://bit.ly/fZoEse'
categories:
    - VMWare
---

I was trying to create a (very) large Datastore on VMWare vSphere 4.1 but although VMWare correctly identifies the LUN on my SAN it refuses to create the Datastore and gives me this error:

[![Call HostDatastoreSystem.QueryVmfsDatastoreCreateOptions for object ha-datastoresystem on ESX failed.](http://192.168.40.25:8081/wp-content/uploads/2010/10/createdatastoreerror1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/createdatastoreerror1.png)  
The complete error text is

> Call “HostDatastoreSystem.QueryVmfsDatastoreCreateOptions” for object “ha-datastoresystem” on ESX “&lt;IP of ESX&gt;” failed.

The strange thing is that VMWare detects the partition and identifies the correct size (2,18 TB in this case):

[![VMWare vSphere 4.1 Current Disk Layout](http://192.168.40.25:8081/wp-content/uploads/2010/10/createdatastoreerror2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/createdatastoreerror2.png)

It seems though that VMWare is simply unable to handle such a large partition as the error is not present on smaller partitions.