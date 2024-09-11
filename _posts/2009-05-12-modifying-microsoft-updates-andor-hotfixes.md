---
id: 345
title: 'Modifying Microsoft Updates and/or hotfixes'
date: '2009-05-12T13:53:34+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/05/12/modifying-microsoft-updates-andor-hotfixes/'
permalink: /2009/05/12/modifying-microsoft-updates-andor-hotfixes/
views:
    - '3226'
short-url:
    - 'http://bit.ly/goAeAc'
categories:
    - Citrix
    - General
    - 'Terminal Server'
---

As you might know Microsoft distributes updates and hotfixes with in installer, update.exe. When you run update.exe it looks into the supplied .inf files to see what it has to install. It’s not possible to make changes to the inf files however because that will invalidate it’s signature (and update.exe checks the signature that is stored in an accompanying .cat file).

In my case I wanted to deploy the [MUI pack for Internet Explorer 7](http://www.microsoft.com/Downloads/details.aspx?familyid=F29D348A-78F9-47AD-92EB-632F9621BC84&displaylang=en) to be able to support multiple languages. By default this pack installs 35 (!) languages and I wanted to install only Dutch language on top of existing English.

I first left out the directories with the languages I didn’t want but update.exe refuses to install, you can see lines like this in the install log:

> 1.453: Missing file trk\\wininet.dll.mui

Next I tried to edit the inf file, in my case (for Server 2003) update\_srv03.inf. I removed all lines pointing to other languages (the file extension has a 3 letter abbreviation, for example:

> \[ProductInstall.CopyFilesAlways\]  
> CopyFiles=CopyAlways.Help.Files.ara  
> CopyFiles=CopyAlways.Inf.IEM.MUI.Files.ara  
> CopyFiles=CopyAlways.InternetExplorer.Mui.Files.ara  
> CopyFiles=CopyAlways.System32.Mui.Files.ara  
> CopyFiles=CopyAlways.Help.Files.chs  
> CopyFiles=CopyAlways.Inf.IEM.MUI.Files.chs  
> CopyFiles=CopyAlways.InternetExplorer.Mui.Files.chs  
> CopyFiles=CopyAlways.System32.Mui.Files.chs  
> …
> 
> \[SourceDisksFiles\]  
> **enu\\srv03sp1\\ieframe.dll=1  
> enu\\ieframe.dll.mui=1** ara\\ieakmmc.chm=1  
> ara\\ieeula.chm=1  
> ara\\iesupp.chm=1  
> ara\\iexplore.chm=1  
> ara\\inetcorp.iem=1

Don’t remove lines with enu…

Then I ran the update again and it failed because of the signature I mentioned, once again the log file is clear about that:

> 1.250: IsInfFileTrusted: ValidateSingleFileSignature Failed: STR\_FAILED\_INF\_INTEGRITY

How convenient that the name of the function is mentioned 😉  
So I decided to patch update.exe. I opened it in Ida and luckily Microsoft’s public symbols contain this function, so the patch was really easy:

It’s signature is: \_\_stdcall IsInfFileTrusted(x) @.0104A33F

We can simply make it return true by changing the first bytes of the function:

> .text:0104A33F ; \_\_stdcall IsInfFileTrusted(x)  
> .text:0104A33F [\_IsInfFileTrusted@4](mailto:_IsInfFileTrusted@4):  
> .text:0104A33F 53 push ebx  
> .text:0104A340 55 push ebp  
> .text:0104A341 56 push esi  
> .text:0104A342 FF 35 48 10 09 01 push \_g\_hInf

to:

> .text:0104A33F ; \_\_stdcall IsInfFileTrusted(x)  
> .text:0104A33F [\_IsInfFileTrusted@4](mailto:_IsInfFileTrusted@4):  
> .text:0104A33F ; InventoryThread(x)+4CFp  
> .text:0104A33F B8 01 00 00 00 mov eax, 1  
> .text:0104A344 C2 04 00 retn 4

And now the update runs fine, you can use the /passive switch to make it unattended and deploy it with your favorite deployment tool.

You can download the generic patch for update.exe here: [ Windows Service Pack Setup Patcher (1087 downloads ) ](http://192.168.40.25:8081/download/windows-service-pack-setup-patcher/?tmstv=1726048918 "Version 1.0"). It is based on update.exe 6.2.29.0 dd 27-06-2007 but it uses a search/replace patch method so you might be able to use it with other versions too.