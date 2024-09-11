---
id: 1985
title: 'Set Exchange Product Key with PowerShell'
date: '2011-08-15T09:17:14+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/08/15/set-exchange-product-key-with-powershell/'
permalink: /2011/08/15/set-exchange-product-key-with-powershell/
views:
    - '4737'
short-url:
    - 'http://bit.ly/pMDLdF'
categories:
    - Exchange
    - PowerShell
tags:
    - Exchange
    - Exchange2010
    - PowerShell
---

[![SNAGHTML1ca684c](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png?9d7bd4 "SNAGHTML1ca684c")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png?9d7bd4)

By default Exchange 2007 and 2010 are installed in Trial mode so before going into production you need to enter the Product Key.

The Exchange Management Console will warn you if one or more servers are still in trial mode:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/08/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/08/image.png)

You can Enter the Product Key in the Server Configuration Section but you cannot enter it for all server at once:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/08/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/08/image1.png)

Fortunately you can do this very easily using PowerShell:

 “&gt;

Note that I am filtering for Exchange 2010 (or higher) using the *IsE14OrLater* property since I still have some Exchange 2003 servers. For Exchange 2007 you can combine this with the *IsExchange2007OrLater* property or use the *ExchangeVersion* property.