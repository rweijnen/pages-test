---
id: 763
title: 'Removing Single Sign on Node from the Delivery Services Console'
date: '2010-11-12T15:51:53+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/11/12/removing-single-sign-on-node-from-the-delivery-services-console/'
permalink: /2010/11/12/removing-single-sign-on-node-from-the-delivery-services-console/
views:
    - '7274'
short-url:
    - 'http://bit.ly/gY1fcC'
categories:
    - Citrix
---

When installing the Citrix Delivery Services Console with an unattended install (using CtxInstall.exe) the Citrix Password Manager gets automatically installed as well.

You can see this in the Console where you will get an additional node called “Single Sign On”.

I couldn’t find any information on how to make CtxInstall exclude it so I was left with 2 options:

1. <div>Unregistering the .net assembly for the Single Sign On Node.</div>
2. <div>Unattended Uninstall of the Password Manager.</div>

I went for option 2 since 1 might mean it comes back after an update, the commandline is:

> msiexec /x {25F2D5CD-B428-4869-B32B-031A9D5CE0C1} /Qb

If it’s somehow possible to prevent installing it in the frist place, please let me know!