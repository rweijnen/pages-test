---
id: 1266
title: 'The case of the UPS discovery not working'
date: '2011-01-24T15:22:03+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1266'
permalink: /2011/01/24/the-case-of-the-ups-discovery-not-working/
views:
    - '2093'
short-url:
    - 'http://bit.ly/hCNH7w'
categories:
    - Citrix
    - General
    - VMWare
tags:
    - EATON
    - SNMP
    - UPS
---

I am doing a project involving a Citrix Xenapp environment running on VMWare vSphere.

The physical machines are powered by two Eaton Uninterruptable Power Supplies that both a network card.

I received some [documentation](http://download.mgeops.com/install/linux/ipp/IPP_how_to_vmware_esx_en_1_9.pdf) that describes how to implement automatic shutdown in a VMWare vSphere environment.

This documentation describes that a [vSphere Management Assistant](http://www.vmware.com/support/developer/vima/) (vMA) must be deployed in which we need to install some software from Eaton.

I followed the documentation that even described the needed iptables rules needed for their software.

In the last step a discovery is done and the UPS is supposed to be found. And you have probably guessed by now: it didn’t!

At first I figured that maybe the iptables configuration was still too tight so I stopped the iptables service but that didn’t help.

After some digging around I found a file debug.js:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image7.png)

In that file I set the line ‘enabled’ to True

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image8.png)

Then I restarted the server (*sudo service Eaton-IPP restart*) and retried. Now a logfile was created named *run\_yyyymmdd\_hhmmss.log*.

In that log file I could see that it found the UPS but it resolved the name to the name of pc that at some in point time has had this address:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image9.png)

So I created a static IP for the UPS but if you look again in the screenshot above there are also some HTTP 503 errors which means service unavailable.

I then opened the Network settings on the UPS and changed the Read-Only Security Level from Auth No Priv:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image10.png)

to No Auth No Priv:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image11.png)

Bingo, now it worked!

After adding the UPS I could enter the username/password (default is admin/admin):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image12.png)