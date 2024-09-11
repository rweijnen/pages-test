---
id: 1509
title: 'Office 2010 cannot remove registry key'
date: '2011-03-10T14:12:22+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1509'
permalink: /2011/03/10/office-2010-cannot-remove-registry-key/
views:
    - '20075'
short-url:
    - 'http://bit.ly/f1QLf2'
categories:
    - General
tags:
    - 'Office 2010'
---

I have worked with Office 2010 x64 for a while now but because of compatibility issues I wanted to remove it and install the x86 version instead.

After uninstall Office left a key in the registry:

> HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Office\\Common\\SmartTag\\Actions\\{B7EFF951-E52F-45CC-9EF7-57124F2177CC}

I couldn’t remove it so I figured there was a specific process that had opened this key but couldn’t find anything (using Process Explorer).

Then I checked the permissions on the Office key but it was set to Full Control for Administrators.

Then I checked the {B7EFF951-E52F-45CC-9EF7-57124F2177CC} subkey and under the special permissions the Delete permission was missing:

[![Permissions](http://192.168.40.25:8081/wp-content/uploads/2011/03/Permissions_thumb.png "Permissions")](http://192.168.40.25:8081/wp-content/uploads/2011/03/Permissions.png)

So I reset the permissions and then I could remove the key:

[![ReplacePermissions](http://192.168.40.25:8081/wp-content/uploads/2011/03/ReplacePermissions_thumb.png "ReplacePermissions")](http://192.168.40.25:8081/wp-content/uploads/2011/03/ReplacePermissions.png)