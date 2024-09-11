---
id: 748
title: 'The case of the failing Exchange 2007 Install'
date: '2010-11-08T20:56:24+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=748'
permalink: /2010/11/08/the-case-of-the-failing-exchange-2007-install/
views:
    - '1725'
short-url:
    - 'http://bit.ly/fcbJ4w'
categories:
    - 'Active Directory'
    - Exchange
---

I was creating an unattended Exchange 2007 install job today and while testing it, it failed with the following error:

> Active Directory operation failed on nl-dc001.MYDOMAIN.LAN. The object ‘CN=Default Global Address List,CN=All Global Address Lists,CN=Address Lists Container,CN=My Organisation,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=MYDOMAIN,DC=LOCAL’ already exists

I opened the Exchange System Manager and expanded the Tree (Recipient | All Address Lists | All Global Address Lists) and I found 2 Global Address Lists but not the Default Global Address List:

![GAL2](http://192.168.40.25:8081/wp-content/uploads/2010/11/gal2.png)

I then opened Adsi Edit to verify permissions and found the culprit:

[![GAL3](http://192.168.40.25:8081/wp-content/uploads/2010/11/gal3-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/gal3.png)

There were Deny permissions set for Domain Users on the Default Global Address List. This was probably an attempt to hide this Address List for users and show the newly created Address Lists. It would of course have sufficied to remove the Read permissions…

Once I had removed the Deny permissions the install proceeded.