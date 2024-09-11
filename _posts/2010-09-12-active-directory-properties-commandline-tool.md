---
id: 629
title: 'Active Directory Properties Commandline Tool'
date: '2010-09-12T21:56:01+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=629'
permalink: /2010/09/12/active-directory-properties-commandline-tool/
views:
    - '5198'
short-url:
    - 'http://bit.ly/iayMdJ'
categories:
    - 'Active Directory'
    - Vista
    - 'Windows 2008'
    - 'Windows 7'
---

I have written a small commandline tool that shows the Active Directory Property Sheet for a given account.

The Property sheet is what you get when you doubleclick an object in Active Directory &amp; Computers. Basically this tool is meant to make it easy to quickly view or change properties without needing to start a GUI tool and looking up the account in the AD Tree.  
It takes only one parameter in the form DOMAIN\\Account where Account is the sAMAccountName and Domain is required.

[![AdProps1](http://192.168.40.25:8081/wp-content/uploads/2010/09/adprops1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/09/adprops1.png)

The result in this sample is:

[![AdProps2](http://192.168.40.25:8081/wp-content/uploads/2010/09/adprops2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/09/adprops2.png)

I hope you will find it usefull!

If you are interested in a customized version or the source code please use the Contact Form.

[ AdProps (2150 downloads ) ](http://192.168.40.25:8081/download/adprops/?tmstv=1726048919 "Version 1.0")