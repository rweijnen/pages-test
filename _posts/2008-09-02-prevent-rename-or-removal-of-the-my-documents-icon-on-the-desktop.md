---
id: 116
title: 'Prevent rename or removal of the My Documents icon on the desktop'
date: '2008-09-02T09:27:23+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/09/02/prevent-rename-or-removal-of-the-my-documents-icon-on-the-desktop/'
permalink: /2008/09/02/prevent-rename-or-removal-of-the-my-documents-icon-on-the-desktop/
views:
    - '4769'
short-url:
    - 'http://bit.ly/eCLaVq'
categories:
    - Uncategorized
---

<span style="color: #ff0000;">**EDIT:**</span> **Please read the** [**Desktop Icons, hide, show, prevent rename or delete article**](http://192.168.40.25:8081/2010/12/19/desktop-icons-hide-show-prevent-rename-or-delete/)**, it may be a better solution!**

One of my customers recently asked if it was possible to preven the user from renaming or deleting the My Documents icon on the desktop.

If you know that deleting the icon from the desktop doesn’t really delete the My Documents folder from disk but just hides the icon then it’s obvious that it must be some kind of registry setting.

So I fired up Process Monitor from Sysinternals and deleted the icon. This showed that after deleting the icon changed registry keys at the following location:

HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\HideDesktopIcons\\ClassicStartMenu\\

If you delete an Icon from the desktop a DWORD value is created and set to 1. The name of the value corresponds to the GUID of the icon.

For instance, My Documents has the GUID {450D8FBA-AD25-11D0-98A8-0800361B1103}.

The same key is also set under HKEY\_CURRENT\_USER\\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\HideDesktopIcons\\NewStartPanel to also remove the icon if you use the new start menu.

If you rename the My Documents icon the Defaul value of the following registry key is set to the new name: HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\CLSID\\{450D8FBA-AD25-11D0-98A8-0800361B1103} (again the GUID of My Documents).

So knowing that I decided to set a deny DACL on the registry keys and this made it impossible to rename or delete the icon (without Explorer showing an error message).

By adding a short VB Script to the loginscript the Deny is set for every user that logs in. You can download the script below. Please note that the scripts uses Helge Klein’s excellent [SetACL utility](http://setacl.sourceforge.net/) (the ActiveX version).

[ MyDocuments.vbs (1433 downloads ) ](http://192.168.40.25:8081/download/mydocuments-vbs/?tmstv=1726048918 "Version 1.0")