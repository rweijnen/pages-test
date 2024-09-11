---
id: 1211
title: 'Recursive group Membership in Powershell'
date: '2011-01-18T12:54:17+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1211'
permalink: /2011/01/18/recursive-group-membership-in-powershell/
short-url:
    - 'http://bit.ly/hghoSb'
views:
    - '5336'
categories:
    - 'Active Directory'
    - PowerShell
---

In this post I will show an easy way to get the recursive group membership for the current user.

I use this in a logon script to handle certain tasks based on group membership.

Most scripts I see for this task do a manual recursive enumeration but in a large environment this could be very slow.

A better way would be to use the [tokenGroups](http://msdn.microsoft.com/en-us/library/ms680275(v=vs.85).aspx) attribute of the Active Directory user object.

The tokenGroups attribute is an array of SIDs computed by Active Directory and is used to verify user access.

We need to translate these SIDs to their sAMAccountNames to get the actual group names.

In unmanaged code this could be accomplished by calling the [DsCrackNames](http://msdn.microsoft.com/en-us/library/ms675970(VS.85).aspx) API or the [IADsNameTranslate](http://msdn.microsoft.com/en-us/library/aa706046(v=VS.85).aspx) interface.

![](http://192.168.40.25:8081/wp-content/uploads/2011/01/trans.gif "More...")In Powershell the easiest way is to use the [UserPrincipal](http://msdn.microsoft.com/en-us/library/system.directoryservices.accountmanagement.userprincipal.aspx) class (requires .NET Framework 3.5 or higher) which exposes the [GetAuthorizationGroups](http://msdn.microsoft.com/en-us/library/system.directoryservices.accountmanagement.userprincipal.getauthorizationgroups(v=VS.90).aspx) method.

This makes it a very easy task. In the sample below I also use the where object to filter the results and the select object to return only the SamAccountName property.  
“&gt;