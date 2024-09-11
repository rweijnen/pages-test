---
id: 716
title: 'Cannot complete customization when cloning from Template'
date: '2010-11-03T11:24:32+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=716'
permalink: /2010/11/03/cannot-complete-customization-when-cloning-from-template/
views:
    - '8583'
short-url:
    - 'http://bit.ly/gj9LfO'
categories:
    - VMWare
---

I am currently provisioning a lot of Virtual Machines in VMWare vSphere 4.1. Because I had already sized the Virtual Machines I am doing this from [PowerCLI](http://communities.vmware.com/community/vmtn/vsphere/automationtools/powercli "VMWare vSphere PowerCLI") based on my Excel Sheet.

I will probably blog later about the details of how I am doing this in PowerCLI (would you be interested in that?) but after successfully deploying some Windows 2008 VM’s I got this error in PowerShell:

> New-VM : 3-11-2010 10:00:50 New-VM The operation for the entity VirtualMachine-vm-150 failed with the following message: “Cannot complete customization.”
> 
> At C:\\Users\\Administrator\\Documents\\NewVm.ps1:64 char:14
> 
> \+ $VM = New-VM &lt;&lt;&lt;&lt; -Name $Name -VMHost $VMHost -Template $Template -OSCustomizationSpec $Spec -DiskStorageFormat $DiskFormat -Datastore $LargestDataStore
> 
> \+ CategoryInfo : NotSpecified: (:) \[New-VM\], CustomizationFault
> 
> \+ FullyQualifiedErrorId : Client20\_TaskServiceImpl\_CheckServerSideTaskUpdates\_OperationFailed,VMware.VimAutomation.ViCore.Cmdlets.Commands.NewVM

In the vCenter console the following error was logged:

> Cannot deploy template: Cannot complete customization.

I decided to try a Deployment from the vCenter GUI so I would get some more detailed error information and indeed the error was more meaningfull:

[![Windows customization resources were not found on the server](http://192.168.40.25:8081/wp-content/uploads/2010/11/deploytemplateerror-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/deploytemplateerror.png)

So this meant there was probably something wrong with the SysPrep files which are located under “%ALLUSERSPROFILE%\\VMware\\VMware VirtualCenter\\sysprep\\&lt;OS VERSION&gt;”.

I decided to replace the sysprep version that I downloaded from Microsoft with the one from the OS Install Media (SUPPORT\\TOOLS\\Deploy.cab). You can do this by simply extracting the files from the Deploy.cab and placing them in the proper subdir (in this case svr2003).

After that I could Deploy successfully.

BTW: This [kb article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1003582 "Virtual machine customization fails after an upgrade of VirtualCenter") from VMWare is also helpfull.