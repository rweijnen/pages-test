---
id: 4204
title: 'Sign APPX packages with PowerShell'
date: '2017-12-20T15:00:33+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4204'
permalink: /2017/12/20/sign-appx-packages-powershell/
views:
    - '503'
categories:
    - PowerShell
tags:
    - APPX
    - PowerShell
    - SignTool
    - UWP
---

I have been working with Microsoft’s Desktop App Converter a lot recently. Even though there’s an option to autosign the resulting package with the `-Sign` switch I prefer to sign APPX packages myself using signtool.

The reason is that I can send UWP packages to testers for sideloading without requiring them to import the auto generated certificate (which is different on each (re)build).

However I always forget the exact path to signtool.exe (this comes with the [Windows SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk)).

The Windows 10 SDK is installed by default in `C:\\Program Files (x86)\\Windows Kits\\10`.

Signtool.exe will be in the folder`&lt;sdkpath&gt;\\bin\\&lt;version&gt;\\&lt;platform&gt;\\signtool.exe`.

As there are multiple version of Windows 10 there are multiple version of the SDK and you can install those concurrently.

But then I found the PowerShell cmdlet [Resolve-Path](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/resolve-path?view=powershell-3.0) which “Resolves the wildcard characters in a path, and displays the path contents”.

This does exactly what I need:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/12/image_thumb-5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/12/image-5.png)

Wow, Resolve-Path is a perfect example of the many hidden gems in PowerShell!

So I decided to wrap signtool.exe in a PowerShell cmdlet as PowerShell also makes it easy to locate the correct code signing certificate from the certificate store.

The certificate can either be provided with a parameter but if this is omitted the script will search for code signing certificates in the Personal certificates store.

If multiple code signing certificates are found, the script will ask which one you’d like to use:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/12/image_thumb-6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/12/image-6.png)  
“&gt;  
The script can be downloaded directly from my [Github repo.](https://github.com/rweijnen/Sign-File)