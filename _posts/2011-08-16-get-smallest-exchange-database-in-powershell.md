---
id: 1986
title: 'Get smallest Exchange Database in PowerShell'
date: '2011-08-16T14:16:33+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/08/16/get-smallest-exchange-database-in-powershell/'
permalink: /2011/08/16/get-smallest-exchange-database-in-powershell/
views:
    - '3082'
short-url:
    - 'http://bit.ly/olqtRk'
categories:
    - Exchange
    - PowerShell
tags:
    - Exchange
    - Exchange2010
    - PowerShell
---

[![SNAGHTML1ca684c](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png?9d7bd4 "SNAGHTML1ca684c")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png?9d7bd4)

I needed to adapt some scripts that create a user with mailbox for Exchange 2010. The existing scripts had a hardcoded database for new mailboxes.

I wanted the mailbox to be created in the smallest database, but how do we determine this?

For Exchange 2010 this is fairly easy using PowerShell:

 “&gt;