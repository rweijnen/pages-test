---
id: 461
title: 'Unattended Install of the Citrix Xenapp WebInterface 5.2'
date: '2009-11-17T08:55:44+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=461'
permalink: /2009/11/17/unattended-install-of-the-citrix-xenapp-webinterface-5-2/
views:
    - '4050'
short-url:
    - 'http://bit.ly/hHICjW'
categories:
    - Altiris
    - Citrix
---

I noticed that XenApp 5 Feature Pack comes with a new version of the Web Interface (5.2) (it is also available as standalone download). The parameters to install it in silent mode have changed but there’s no documentation at all on the Citrix Site:

[![WIDocs](http://192.168.40.25:8081/wp-content/uploads/2009/11/widocs-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/widocs.png)

Fortunately /? still works, so you can (from a command prompt) enter

> WebInterface /?

[![WICommands](http://192.168.40.25:8081/wp-content/uploads/2009/11/wicommands-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/wicommands.png)

I used this commandline to install it:

> WebInterface.exe -p *“%CTX\_WI\_PATH%”* -q -v *“%FULLPATH%\\WebInterface-%COMPUTERNAME%.log”* -l EN -c *“WIDest=%CTX\_WI\_DEST%,WIDefaultSite=%CTX\_WI\_DefaultSite%,  
> XMLService=CXRPA01;CXRPA02;CXRPA03;CXRPA04;CXRPA05,  
> XMLSPort=80,XMLSProtocol=HTTP,AuthenticationPoint=  
> WebInterface,FarmName=%CTX\_WI\_FARMNAME%”*

The variables like %CTX\_WI\_PATH% are User Tokens in my Deployment Server (I use Altiris) so this makes it easy to re-use jobs in other Environments.

[![UserTokens](http://192.168.40.25:8081/wp-content/uploads/2009/11/usertokens-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/usertokens.png)