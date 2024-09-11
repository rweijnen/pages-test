---
id: 1973
title: 'Exchange 2010 Install-ADTopologyService 0x800706D'
date: '2011-07-05T09:43:04+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/07/05/exchange-2010-install-adtopologyservice-0x800706d/'
permalink: /2011/07/05/exchange-2010-install-adtopologyservice-0x800706d/
views:
    - '3441'
short-url:
    - 'http://bit.ly/lWCXe3'
categories:
    - Uncategorized
tags:
    - Exchange
    - Exchange2010
---

[![Exchange 2010 Logo](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png?9d7bd4 "Exchange 2010")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png?9d7bd4)

I got the following error while installing Exchange 2010: “There are no more endpoints available from the endpoint mapper. (Exception from HRESULT: 0x800706D9)”

I looked up HRESULT 0x800706D9 which is defined in *winerror.h* as EPT\_S\_NOT\_REGISTERED with the same error text.

I looked at the ExchangeSetup.log in C:\\ExchangeSetupLogs and this indicates that the error occurs when the *install-ADTopologyService* cmdlet tries to add some rules to the firewall:

 “&gt;

   
![Windows Firewall Logo](http://t1.gstatic.com/images?q=tbn:ANd9GcQqdgbAi3Pmwlcph0Oc8Ll0HiEyP_kM-zHMXlnadxN-V0ReOCMSrw "Windows Firewall")The Windows Firewall service was disabled, so I set it to Automatic, started it and enabled it and then the setup ran without errors