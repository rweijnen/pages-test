---
id: 2554
title: 'Create Active Directory Group with PowerShell'
date: '2012-03-15T15:51:23+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/15/create-active-directory-group-with-powershell/'
permalink: /2012/03/15/create-active-directory-group-with-powershell/
views:
    - '5453'
short-url:
    - 'http://bit.ly/zQ1aba'
categories:
    - 'Active Directory'
    - PowerShell
---

If you want to Create an Active Directory group with PowerShell there are a few things you need to be aware of:

First of all there is no direct way to create new objects in Active Directory. You always need to bind to the Domain or an Organizational Unit and call the Create method.

Example:

 “&gt;

However the group is not yet complete:

[![Group name (pre-Windows 2000)](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb22.png "TestGroup Properties")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image22.png)

So we need to set the sAMAccountName property:

“&gt;

however this will fail with the error message:

“&gt;

This happens because we first need to call CommitChanges() before setting additional properties:

“&gt;

Last step is to change the group type, which can be done using the groupType property:

“&gt;

And all the pieces together:

“&gt;