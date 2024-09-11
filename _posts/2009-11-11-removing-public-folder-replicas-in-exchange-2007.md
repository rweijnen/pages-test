---
id: 442
title: 'Removing Public Folder Replicas in Exchange 2007'
date: '2009-11-11T15:47:35+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/11/11/removing-public-folder-replicas-in-exchange-2007/'
permalink: /2009/11/11/removing-public-folder-replicas-in-exchange-2007/
views:
    - '6725'
short-url:
    - 'http://bit.ly/ek9kze'
categories:
    - Exchange
    - PowerShell
---

When you uninstall Exchange 2007 you need to remove all Mailbox- and Public Folder databases. If you try to remove the Public Folder Database this will fail because there are replicas of the system folders in the Public Folder database. I didn’t find a way to remove these replicas from the Exchange Management GUI but it can be done with the following Powershell Code:

> Get-PublicFolder -Server &lt;exchange server&gt; “\\” -Recurse -ResultSize:Unlimited | Remove-PublicFolder -Server &lt;exchange server&gt; -Recurse -ErrorAction:SilentlyContinue  
> Get-PublicFolder -Server &lt;exchange server&gt; “\\Non\_Ipm\_Subtree” -Recurse -ResultSize:Unlimited | Remove-PublicFolder -Server &lt;exchange server&gt; -Recurse -ErrorAction:SilentlyContinue  
> Get-PublicFolder -Server &lt;exchange server&gt; “\\Non\_Ipm\_Subtree” -Recurse -ResultSize:Unlimited | Remove-PublicFolder -Server &lt;exchange server&gt; -Recurse -ErrorAction:SilentlyContinue

(change &lt;exchange server&gt; to the name of your Exchange Server)