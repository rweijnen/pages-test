---
id: 711
title: 'The case of the VMware vSphere Client'
date: '2010-10-27T21:59:32+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/10/27/the-case-of-the-vmware-vsphere-client/'
permalink: /2010/10/27/the-case-of-the-vmware-vsphere-client/
views:
    - '7036'
short-url:
    - 'http://bit.ly/gVLIxC'
categories:
    - Delphi
    - VMWare
---

Today I connected with my laptop to VMWare Virtual Center using vSphere client. Because I had an older version of the client I needed to update and the installer failed with this message:

[![The Microsoft Visual J# 2.0 Second Edition installer returned the error code '4113'](http://192.168.40.25:8081/wp-content/uploads/2010/10/visualjinstall-small5.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/visualjinstall.png)

I remembered this error from the last install of this client (about a year ago), it happens because Microsoft Visual J# was already installed (in my case it was previously installed by Embarcadero’s Rad Studio).

Last year I “fixed” it by modifying the msi file but I remembered that Assarbad posted an easier solution on his [Blog](http://blog.assarbad.net/20100808/vmware-vsphere-client-4-1-installation-woes/ "VMware vSphere client 4.1 installation woes") a while ago.

His solution was to set a public property in the MSI (USING\_VIM\_INSTALLER) but it means we need to unpack the installer exe first to obtain the MSI file.  
I figured there might be a way to pass this property directly to the installer. Launching the installer with /? as argument which shows us the possible Command line parameters:

[![InstallShieldArgs](http://192.168.40.25:8081/wp-content/uploads/2010/10/installshieldargs-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/installshieldargs.png)

Seems like /V is what we need and after trying several variations the correct one is:

> VMware-viclient.exe /V”USING\_VIM\_INSTALLER=1″

Note there must **not** be a space between /V and “USING\_VIM\_INSTALLER=1”.

And then finally it works!

[![InstallationCompleted](http://192.168.40.25:8081/wp-content/uploads/2010/10/installationcompleted-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/installationcompleted.png)