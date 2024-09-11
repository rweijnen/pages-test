---
id: 2761
title: 'Query Administrator Account Name with PowerShell'
date: '2012-10-25T15:22:47+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2761'
permalink: /2012/10/25/query-administrator-account-name-with-powershell/
short-url:
    - 'http://bit.ly/QHXxVR'
views:
    - '2520'
categories:
    - PowerShell
tags:
    - PowerShell
---

A customer had partially implemented a (written) policy in the past where the the Local Administrator account was renamed according to a special convention.

This policy stated that the Administrator account needed to be renamed to admin with the computername as a prefix.

However they didn’t know exactly on which machines this policy had been applied to in the past. I was asked to write a script that would check a list of machine names, query the Administrator account name and write this in a new list.

The Administrator account has a [Well Known SID](http://support.microsoft.com/kb/243330) of S-1-5-21-xxxxxxx-500 where xxxxxxx is the SID of the computer.

This makes this an easy task for PowerShell: we use the Win32\_UserAccount WMI Class and filter this on ‘S-1-5-%-500’:

Here is the script:  
“&gt;