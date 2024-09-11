---
id: 4302
title: 'DesktopAppConverter : error &#8216;E_MANIFEST_USE_DEFAULT_VALUE_FAILED'
date: '2018-03-22T11:54:54+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4302'
permalink: /2018/03/22/desktopappconverter-error-e_manifest_use_default_value_failed/
views:
    - '301'
categories:
    - Uncategorized
tags:
    - 'Desktop App Converter'
    - UWP
---

I had a strange error today using the Desktop App Converter with the parameters given from the Store for Business.

The conversion would fail with the following error:

[![DesktopAppConverter : error 'E_MANIFEST_USE_DEFAULT_VALUE_FAILED': Property 'Package.Applications.Application.Id' in AppxManifest.xml could not be set to the default value](http://192.168.40.25:8081/wp-content/uploads/2018/03/image_thumb.png "DesktopAppConverter")](http://192.168.40.25:8081/wp-content/uploads/2018/03/image.png)

I’m not sure why this fails as the PackageName is provided by the store and should be valid. An answer on stackoverflow suggested to use a different value for the PackageName parameter and then edit the manifest.

I don’t like this method as manual modifications of the manifest often leads to errors when submitting the application to the store.

So let’s have a look and see why we’re getting this error.

I searched for `E\_MANIFEST\_USE\_DEFAULT\_VALUE\_FAILED` in the DesktopAppConverter folder and found 1 occurence in `ManifestOps.ps1`.

From a look at the code it wasn’t immediately clear where the validation failed so I decided to debug it.

I launched the DesktopAppConverter from the PowerShell ISE and set a breakpoint just before the  
“&gt;

Then I stepped into `$this.Validate()` which throws the actual error and it had the following text:

> Error found in XMLThe ‘Name’ attribute is invalid – The value ‘Company.ApplicationIdentifierXXXXXXXXXX’ is invalid according to its datatype ‘[http://schemas.microsoft.com/appx/manifest/types:ST\_PackageName’](http://schemas.microsoft.com/appx/manifest/types:ST_PackageName') – The actual length is greater than the MaxLength value.

Ah so there is a constraint on the PackageName length!

So let’s have a look at the XSD file and see what the constraints are for ST\_PackageName. I search for `ST\_PackageName` in all \*.xsd files in the DesktopAppConverter and found it in `AppxManifestTypes.xsd` :  
“&gt;

So the issue in here is that the PackageName I used was longer that 50 characters!

Now there’s 2 solutions from here and that’s either reduce the PackageName length or specify an alternative, shorter, name by adding the `-AppId` switch. E.g. `-AppId “MyApplication”`.