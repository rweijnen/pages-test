---
id: 2046
title: 'Settings NTFS Permissions by SID in PowerShell'
date: '2011-09-02T17:21:42+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/09/02/settings-ntfs-permissions-by-sid-in-powershell/'
permalink: /2011/09/02/settings-ntfs-permissions-by-sid-in-powershell/
views:
    - '7516'
short-url:
    - 'http://bit.ly/n5r4VI'
categories:
    - 'Active Directory'
    - PowerShell
tags:
    - 'Active Directory'
    - ADSI
    - PowerShell
    - Security
---

![](http://t0.gstatic.com/images?q=tbn:ANd9GcTPzlU95MOmfR0YwGb55TQkoZENCxgxFUKqp6qqfMMaa9skPMT5gw)I am currently creating a PowerShell script that creates a user with all needed Active Directory attributes, Exchange mailbox, (TS) Home- and Profile directories and so on.

In such a script you can easily get failures because of Active Directory replication.

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image.png)Image that you create a new user account and later on you need set an additional attribute. What happens if the user was created while connected to Domain Controller A and you try to set an additional attribute while connected to Domain Controller B before replication has completed?

We can prevent this easily by performing all actions on the same domain controller. In my script I query for any Domain Controller that has the Global Catalog role:

 “&gt;

Insert the $DC variable in your ldap binding eg:

“&gt;

Next problem is when you perform non ADSI operations such as setting NTFS permissions on a fileserver (eg homedirectory).

This server may not yet be able to resolve the username to it’s SID and thus the operation may fail!

We can solve this easily by giving permissions to the SID directory instead to the username. Example:

“&gt;