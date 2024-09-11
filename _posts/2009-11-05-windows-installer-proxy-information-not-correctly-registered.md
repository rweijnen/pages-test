---
id: 425
title: 'Windows Installer proxy information not correctly registered'
date: '2009-11-05T22:11:06+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/11/05/windows-installer-proxy-information-not-correctly-registered/'
permalink: /2009/11/05/windows-installer-proxy-information-not-correctly-registered/
views:
    - '9696'
short-url:
    - 'http://bit.ly/gknUZW'
categories:
    - Altiris
    - Citrix
    - Programming
    - 'Windows 2008'
---

I was deploying an unattended installation of Citrix XenApp 5.0 with Altiris Deployment Server. The installation consists of several prerequisites, the installation of XenApp and finally the Citrix Management Consoles.

The installation is performed with a special account and not the Local System account because the install packages are located on the network.

When testing the deployment on a Windows Server 2008 I noticed that sometimes MSI based installations would fail with error code 1603 or 1601.

MSI Errors are defined in the Product SDK, specifically in WinError.h, so I looked up the error codes:

> //  
> // MessageId: ERROR\_INSTALL\_SERVICE\_FAILURE  
> //  
> // MessageText:  
> //  
> // The Windows Installer Service could not be accessed. This can occur if the Windows Installer is not correctly installed. Contact your support personnel for assistance.  
> //  
> \#define ERROR\_INSTALL\_SERVICE\_FAILURE 1601L  
> …  
> //  
> // MessageId: ERROR\_INSTALL\_FAILURE  
> //  
> // MessageText:  
> //  
> // Fatal error during installation.  
> //  
> \#define ERROR\_INSTALL\_FAILURE 1603L

Then I looked in the logfiles and that showed the following errors:

MSI (c) (5C:8C) \[19:34:33:864\]: Windows Installer proxy information not correctly registered

MSI (c) (5C:8C) \[19:34:33:864\]: Failed to connect to server. Error: 0x80004005

Searching on these errors didn’t give me any good solutions, most results suggested to reregister msiexec but since most MSI’s were installed without errors this was not the solution.

A trace with Process Monitor revealed this:

[![ProcessMonitor](http://192.168.40.25:8081/wp-content/uploads/2009/11/processmonitor-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/processmonitor.png)

I looked up this error code (0xC0000425) and it is defined in ntstatus.h:

> //  
> // MessageId: STATUS\_HIVE\_UNLOADED  
> //  
> // MessageText:  
> //  
> // Illegal operation attempted on a registry key which has already been unloaded.  
> //  
> \#define STATUS\_HIVE\_UNLOADED ((NTSTATUS)0xC0000425L)

That gave the clue that the profile of the installation account was forcefully unloaded by the User Profile Service. The Event Log confirmed this:

Windows detected your registry file is still in use by other applications or services. The file will be unloaded now. The applications or services that hold your registry file may not function properly afterwards.

> DETAIL –  
> 1 user registry handles leaked from \\Registry\\User\\S-1-5-21-1556653526-4280407051-3911480998-1615\_Classes:  
> Process 1512 (\\Device\\HarddiskVolume1\\Windows\\System32\\msiexec.exe) has opened key \\REGISTRY\\USER\\S-1-5-21-1556653526-4280407051-3911480998-1615\_CLASSES

I am not sure if this happens because of bug in the User Profile Service, an Altiris bug of simply because the installation does many logons and logoffs in a short time period. Anyway I decided that I would need to prevent the User Profile Service from unloading the profile. My first guess was there would be an exclusion list somewhere in the registry so I disassembled the User Profile Service with Ida Pro but didn’t find any evidence that such a key exists. I did find 2 interesting functions though: CUserProfile::IncrementRefCount and CUserProfile::DecrementRefCount. Not surprisingly IncrementRefCount is called when a profile is loaded (CUserProfile::Load) and DecrementRefCount is called when a profile is unloaded (CUserProfile::UnLoad).

This RefCount is stored in the registry under HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Windows NT\\Current Version\\ProfileList\\&lt;User’s SID&gt; as a REG\_DWORD value with the name RefCount:

[![RefCountRegistry](http://192.168.40.25:8081/wp-content/uploads/2009/11/refcountregistry-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/refcountregistry.png)

I figured that this RefCount is used to determine if open handles should be forcefully closed. This made me believe we could prevent this by writing a program that manually loads the User Profile with the [LoadUserProfile](http://msdn.microsoft.com/en-us/library/bb762281%28VS.85%29.aspx) function and simply never unloads it (with the [UnloadUserProfile](http://msdn.microsoft.com/en-us/library/bb762282%28VS.85%29.aspx) function).

The program proved I was right and all works fine now and since it might be useful for other people I decided to write this article and publish the tool. The Delphi source code is included if you are interested in it.

If you are going to use it (with or without Altiris) it’s usage is very simple, just run it under the account that you use to install. The output will look similar to this screenshot:

[![LoadProfileScreenShot](http://192.168.40.25:8081/wp-content/uploads/2009/11/loadprofilescreenshot-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/loadprofilescreenshot.png)

Download: [ LoadProfile.zip (2129 downloads ) ](http://192.168.40.25:8081/download/loadprofile-zip/?tmstv=1726048918 "Version 1.0")

If this tool or it’s source is useful please leave a comment, donations are although appreciated not required.