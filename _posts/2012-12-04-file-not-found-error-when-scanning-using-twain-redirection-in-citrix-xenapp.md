---
id: 2795
title: 'File not found error when scanning using Twain Redirection in Citrix XenApp'
date: '2012-12-04T15:07:10+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2795'
permalink: /2012/12/04/file-not-found-error-when-scanning-using-twain-redirection-in-citrix-xenapp/
views:
    - '2583'
short-url:
    - 'http://bit.ly/Yvfcan'
categories:
    - 'Application Compatibility'
    - Citrix
    - 'Terminal Server'
tags:
    - Citrix
    - 'Terminal Server'
    - Twain
---

[![Twain Logo](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb.png "Twain")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image.png)Scanners attached to client machines can be used from within a Citrix XenApp session via a mechanism called Twain Redirection.

For this mechanism to work correctly the file twain\_32.dll must be present in the Windows directory.

<span style="color: #35383d;">On Windows 2008 this dll should be copied from winsxs (side by side) to the windows directory as described in [CTX123981](http://support.citrix.com/article/CTX123981).</span>

On Windows 2003 the dll is already in the correct directory, however applications that are not [Terminal Server Aware](http://msdn.microsoft.com/en-us/library/01cfys9z%28v=vs.80%29.aspx) cannot find this dll because the Windows directory is redirected to the user profile. Citrix recommends copying twain\_32.dll to each user’s profile directory but this will take up unnecessary space.

So what alternatives do we have?

**<span style="text-decoration: underline;">Method 1:</span> Adjust DllCharacteristics field in the executable**  
The afore mentioned link on MSDN lists a procedure to set the TSAWARE flag when compiling (linking) an executable. But when can also set this property on binary (.exe) files.

There are various tools that can do this such as Microsoft’s [editbin](http://msdn.microsoft.com/en-us/library/xd3shwhf%28v=vs.80%29.aspx) that comes with Visual Studio. In this example I will use NTCore’s [CFF Explorer](http://www.ntcore.com/exsuite.php).

Start CFF Explorer and load the target executable, select [Optional Header](http://msdn.microsoft.com/en-us/library/windows/desktop/ms680339%28v=vs.85%29.aspx) in the Treeview and change DllCharacteristics:

[![Change DllCharacteristics filed in the Optional Header](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4cd5855_thumb.png "CFF Explorer")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4cd5855.png)

Check the option “Terminal Server Aware” and click OK:
[![Set Terminal Server Aware Flag](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4cfc8e24_thumb.png "DllCharachteristics")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML4cfc8e24.png)

Now save and test the executable.

**<span style="text-decoration: underline;">Method 2:</span> Set Terminal Server Application Compatibility Flag:** This method has the advantage that it doesn’t need a modification to the executable. It does have to be set per server but you could easily do this with a deployment tool or script.

Open regedit and create a key with the executable name (without extension) under “**HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Terminal Server\\Compatibility\\Applications**“.

In this key we need to set a Flags value for “Do not substitute user Windows directory” and “Windows 32-bit application”. The values are documented in Microsoft [kb186499](http://support.microsoft.com/default.aspx?scid=kb;en-us;186499) as 0x00000400 and 0x00000008 which we need to “OR”.

So we create a DWORD named Flags and give the HEX value 408.

Or use the following reg file (don’t forget to change the executable name!):  
“&gt;  
With both methods the twain\_32.dll will correctly load from the Windows directory!