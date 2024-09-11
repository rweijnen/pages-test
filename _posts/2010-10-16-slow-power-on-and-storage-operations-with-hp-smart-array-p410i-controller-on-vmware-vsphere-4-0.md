---
id: 663
title: 'Slow power on and storage operations with HP Smart Array P410i controller on VMWare vSphere 4.0'
date: '2010-10-16T11:00:00+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/10/16/slow-power-on-and-storage-operations-with-hp-smart-array-p410i-controller-on-vmware-vsphere-4-0/'
permalink: /2010/10/16/slow-power-on-and-storage-operations-with-hp-smart-array-p410i-controller-on-vmware-vsphere-4-0/
views:
    - '4755'
short-url:
    - 'http://bit.ly/gNjO2d'
categories:
    - VMWare
---

As you may have read I am currently implementing VMWare vSphere 4 on several HP Proliant DL380 G7 machines.

I ran across an interesting [knowledge base article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1018794) from VMWare that describes a possible issue that is summarized as “*Power on and storage operations are slow with the HP Smart Array P410i controller*“.

Essentially this just means a performance issue and the resolution is to install an extra (256 MB) cache module on the RAID controller.

The Cache Module has HP article # [462968-B21](http://h30094.www3.hp.com/product.asp?mfg_partno=462968-B21) (not to be confused with 462969-B21 which is the Battery Kit).

Google search on [462968-B21](http://www.google.nl/search?q=462968-B21).