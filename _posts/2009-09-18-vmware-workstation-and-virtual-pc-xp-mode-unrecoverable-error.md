---
id: 412
title: 'VMWare Workstation and Virtual PC XP Mode: unrecoverable error'
date: '2009-09-18T11:33:46+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/09/18/vmware-workstation-and-virtual-pc-xp-mode-unrecoverable-error/'
permalink: /2009/09/18/vmware-workstation-and-virtual-pc-xp-mode-unrecoverable-error/
views:
    - '6319'
short-url:
    - 'http://bit.ly/ef0PVh'
categories:
    - VMWare
    - 'Windows 7'
---

I just installed my laptop with Windows 7 (x64) and I was curious how the new Windows XP mode worked (more on that topic later). After installing it I could no longer start any Virtual Machines in VMWare Workstation. The VM fired up but immediately halted with the following error: “VMware Workstation unrecoverable error: (vcpu-0)” “VCPU 0 RunVM failed: -2”.

![VMWareError](http://192.168.40.25:8081/wp-content/uploads/2009/09/vmwareerror-2.png)

This happens because Virtual PC (which the XP mode uses) leaves a process running even after the XM mode is exited. Apparently this process has exclusive access to the Virtualisation Technology (Intel VT) on my processor. Killing this process fixed the issue.

[![vpc](http://192.168.40.25:8081/wp-content/uploads/2009/09/vpc-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/09/vpc.png)

I was curious what would happen the other way round so I started a VM in VMWare Workstation and then launched XP Mode. The results was disastrous: the VMWare VM halted and was shut down. Virtual PC started but without Integration Features.

After closing XP Mode and (once again) killing the vpc.exe process I could start the VMWare VM again. As you can see it dit not shut down correctly:

[![WindowsErrorRecovery](http://192.168.40.25:8081/wp-content/uploads/2009/09/windowserrorrecovery-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/09/windowserrorrecovery.png)

So the message is: be carefull about launching XP Mode if you have VMWare running!