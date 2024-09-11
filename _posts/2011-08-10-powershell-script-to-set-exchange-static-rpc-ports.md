---
id: 1974
title: 'PowerShell script to set Exchange Static RPC Ports'
date: '2011-08-10T16:49:01+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1974'
permalink: /2011/08/10/powershell-script-to-set-exchange-static-rpc-ports/
short-url:
    - 'http://bit.ly/pKVDvR'
views:
    - '2989'
categories:
    - Exchange
    - PowerShell
tags:
    - Exchange
    - Exchange2010
    - PowerShell
---

[![SNAGHTML1ca684c](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png?9d7bd4 "SNAGHTML1ca684c")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png?9d7bd4)

I am currently working on an Exchange migration from 2003 to 2010. For the implementation of a load balancing solution for the CAS/HUB servers I needed to set Static RPC Ports for the *RPC Client Access Service* and the *Exchange Address Book Service*.

The procedure of changing these ports is described on the Technet Wiki: [Configure Static RPC Ports on an Exchange 2010 Client Access Server](http://social.technet.microsoft.com/wiki/contents/articles/configure-static-rpc-ports-on-an-exchange-2010-client-access-server.aspx)

![](http://t0.gstatic.com/images?q=tbn:ANd9GcTPzlU95MOmfR0YwGb55TQkoZENCxgxFUKqp6qqfMMaa9skPMT5gw)Since I am lazy I decided to do this with a PowerShell script that would automatically do this for all CAS/HUB servers in my 2010 environment.

First we need to load the Exchange Management SnapIns: “&gt;

   
Then I have defined the registry keys and port numbers (Microsoft recommends any port between 59531 and 60554, I am using 60200 and 60201):

**<font color="#ff0000">Update: The Address Book Service uses a REG\_SZ key and not a DWORD therefore I changed $ABPort to a string value.</font>**

“&gt;

   
Then I use the [Get-ClientAccessServer](http://technet.microsoft.com/en-us/library/bb124785.aspx) cmdlet to get all the CAS servers and set the values in a foreach loop:

“&gt;