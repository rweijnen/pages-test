---
id: 4289
title: 'signtool sign returns error 0x800700C1'
date: '2018-01-19T17:54:49+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4289'
permalink: /2018/01/19/signtool-sign-returns-error-0x800700c1/
views:
    - '1027'
categories:
    - UWP
tags:
    - SignTool
    - UWP
---

I was trying to sign an .appx package that I created with the Desktop App Converter. However signtool returned the following error: Sign returned error: `0x800700C1  
For more information, please see <http://aka.ms/badexeformat>`

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-13.png "Signtool: Sign returned error: 0x800700C1 | For more information, please see http://aka.ms/badexeformat")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-13.png)

Sadly signtool doesn’t return more detailed information, even when passing the debug switch:

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-14.png)

So what’s going on?

> Signing an .appx package requires that all executables (.exe and .dll) inside the package are either **unsigned** or have a **valid** signature.

In my case it turned out that one of the binaries that comes with Notepad++ has a corrupted signature.

I found this by checking all the files and then noticed that the only file that appeared to be unsigned was `uninstall.exe`.

After I deleted that file (after all we don’t need an uinstaller with UWP), signtool happily signed the .appx package.

However what if you have a file that is actually needed?

I opened the `uninstall.exe` file in CFF Explorer and noticed that the Security Directory had an invalid address:

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-15.png)

`Signtool.exe` has an option to remove signing information from a file so I tested that but unfortunately signtool doesn’t know how to handle a corrupted signature:

[![ignTool Error: CryptSIPRemoveSignedDataMsg returned error: 0x00000057](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-16.png "signtool")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-16.png)

However with CFF Explorer it’s an easy task to simply remove the Security Directory:

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-17.png)

Now we can sign the .appx package without removing files.

However it is still a tedious task to find out which file(s) are invalid so I wrote a PowerShell script that not only detects invalid or corrupted signatures but can also fix them.

I will document and publish this script in a followup blog.