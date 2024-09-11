---
id: 2526
title: 'Edit Document requires a Windows SharePoint Services-compatible application'
date: '2012-03-09T14:23:11+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/09/edit-document-requires-a-windows-sharepoint-services-compatible-application/'
permalink: /2012/03/09/edit-document-requires-a-windows-sharepoint-services-compatible-application/
views:
    - '5787'
short-url:
    - 'http://bit.ly/x2CPb9'
categories:
    - Citrix
    - PowerShell
    - 'Windows 2003'
tags:
    - PowerShell
    - SharePoint
---

Today I was troubleshooting a message that appeared when a user tries to edit a document from SharePoint on a Citrix XenApp server.

The user browsed to a word document on Sharepoint and selected “Edit in Microsoft Office Word” from the Combobox:

[![Edit in Microsoft Office Word](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb8.png "Sharepoint Document Context Menu")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image8.png)

This would present the following error message to the user:

[!['Edit Document' requires a Windows SharePoint Services-compatible application and Microsoft Internet Explorer 6.0 or greater.](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb9.png "Windows Internet Explorer")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image9.png)

A Google on this error message leads to [Microsoft kb833714](http://support.microsoft.com/kb/833714) which tells us to register OWSSUPP.DL (using RegSvr32). If you try that it will always fail with error 0x80070716 which is defined as ERROR\_RESOURCE\_NAME\_NOT\_FOUND in winerror.h.

On a machine where opening documents from SharePoint worked as expected I looked into the Internet Explorer Add-Ons an noticed “SharePointOpenDocuments” which refers to OWSSUPP.DLL:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image10.png)

The referenced GUID, {9F9C4924-C3F3-4459-A396-9E9E0D8B83D1} is a Class Id so I looked that up in the registry:

[![HKEY_CLASSES_ROOT\CLSID\{9F9C4924-C3F3-4459-A396-9E9E0D8B83D1} | C:\Program Files\Microsoft Office\OFFICE11\OWSSUPP.DLL](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb11.png "Registry Editor")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image11.png)

I verified that the path was correct, next step was to check the other parameters. I wrote a PowerShell script to check a bunch of machines:

 “&gt;

Let’s check the output:

“&gt;

A server where it didn’t work had the TreatAs key additionally, let’s check where that Guid is going:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image12.png)

It leads to ProgID “SharePoint OpenDocuments”, however ProgID “SharePoint.OpenDocuments” cannot be resolved:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image13.png)

I removed the TreatAs key and after that it worked perfectly!