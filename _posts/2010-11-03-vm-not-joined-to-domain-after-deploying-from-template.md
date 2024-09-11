---
id: 718
title: 'VM not joined to Domain after Deploying from Template'
date: '2010-11-03T16:50:01+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=718'
permalink: /2010/11/03/vm-not-joined-to-domain-after-deploying-from-template/
views:
    - '6476'
short-url:
    - 'http://bit.ly/eiaCjV'
categories:
    - VMWare
---

As I wrote earlier today I am provisioning Virtual Machines with PowerCLI. I don’t know if this is intentional behaviour but after Deploying (Cloning) a Virtual Machine from a template the Network Adapter is not automatically connected at power on.

This made the Join Domain operation which is party of the Customization Specification to fail. I noticed this in the log file which can be found in %WINDIR%\\Temp\\vmware-imc\\guestcust.log:

> Joining domain mydomain using account mydomain\\myaccount and password ‘\*\*\*\*\*’  
> The specified domain either does not exist or could not be contacted.

So I wrote a small fix script in PowerCLI:

“&gt;  
And of course I changed the Provisioning script to do this as well 😀