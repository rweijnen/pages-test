---
id: 828
title: 'BSOD with STOP 0x0000008E when installing Window 7 Checked Build'
date: '2010-12-06T23:13:01+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/12/06/bsod-with-stop-0x0000008e-when-installing-window-7-checked-build/'
permalink: /2010/12/06/bsod-with-stop-0x0000008e-when-installing-window-7-checked-build/
views:
    - '2723'
short-url:
    - 'http://bit.ly/hvFXuO'
categories:
    - VMWare
    - 'Windows 7'
---

I was trying to install a checked build of Windows 7 under VMWare Workstation but after the first reboot during the install (the completing installation step) the system came up with a BSOD.

It can be fixed by adding a line to the VMX configuration file:

> piix4pm.smooth\_acpi\_timer = TRUE