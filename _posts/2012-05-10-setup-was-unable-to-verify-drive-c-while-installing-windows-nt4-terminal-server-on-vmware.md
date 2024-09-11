---
id: 2581
title: 'Setup was unable to verify drive C while installing Windows NT4 Terminal server on VMWare'
date: '2012-05-10T13:56:29+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/05/10/setup-was-unable-to-verify-drive-c-while-installing-windows-nt4-terminal-server-on-vmware/'
permalink: /2012/05/10/setup-was-unable-to-verify-drive-c-while-installing-windows-nt4-terminal-server-on-vmware/
views:
    - '1476'
short-url:
    - 'http://bit.ly/K4Z6bz'
categories:
    - VMWare
tags:
    - VMWare
    - 'Windows NT'
---

For a research project I tried to install Windows NT 4 Terminal Server on VMWare Workstation (version 8).

The setup would always fail however with the following error:

[![Setup was unable to verify drive C:\ | Your computer may lack sufficient memory to carry out the verification, or your Windows Terminal Server CD-ROM may contain some corrupt files. | Press ENTER to continue](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb.png "Windows Terminal Server Setup")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image.png)

Obviously the installation doesn’t really fail because of too little memory and neither is the installation disc (an iso file) corrupt, it’s a bug.

Fortunately this particular bug was fixed in SP3 (build number 4.00.1381.32772). So make sure you integrate SP3 or higher in your install media and the it installs perfectly:

[![This portion of Setup had completed successfully](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb1.png "Windows Terminal Server Setup")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image1.png)