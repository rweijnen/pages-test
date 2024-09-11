---
id: 2717
title: 'Error 1787 when logging in with domain account'
date: '2012-08-21T11:43:35+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2717'
permalink: /2012/08/21/error-1787-when-logging-in-with-domain-account/
short-url:
    - 'http://bit.ly/O0JsDl'
views:
    - '6814'
categories:
    - 'Active Directory'
    - PowerShell
---

After joining a new Windows 2008 R2 Server to the domain I could not login to the domain.

I would get the following error message:

[![The security database on the server does not have a computer acocunt for this workstation trust relationship.](http://192.168.40.25:8081/wp-content/uploads/2012/08/clip_image001_thumb.png "Error 1787")](http://192.168.40.25:8081/wp-content/uploads/2012/08/clip_image001.png)

Additionally the following error was logged in the Eventlog:

[![Event ID 3 | Error 1787 | Error Code 0x7 | KDC_ERR_S_PRINCIPAL_UNKNOWN | A Kerberos Message was received](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb5.png "Event Properties")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image5.png)

My guess is that the problem is related to the servicePrincipalName because in the Target Name the DNS suffix seems to be applied twice (or perhaps the UPN suffix):

[![Server Realm | Target Name: host/](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb6.png "Description")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image6.png)

I compared the value of the servicePrincipalName attribute with ADSI Edit to a working server but saw no differences:

[![servicePrincipalName](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb7.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image7.png)

I did notice that the displayName attribute was missing:

[![displayName missing](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb8.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image8.png)

So I set this value with ADSI Edit but this didn’t fix my problem.

I verified and even reset the secure channel:

[![Verify Secure Channel | Result Secure Channel | NLTest | Netdom | Windows 2008 R2](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image9.png)

But again no result so I removed the server from the domain and re-joined and even that had no result. I checked that the system clock was in sync with the domain (and it was).

In a last attempt I decided to rename the computer account (while still joined to the domain) after which I could logon the domain.

I renamed the computer account back to it’s original name and… the error was back!

By now I REALLY wanted to know what was going on so I wrote a PowerShell script to search Active Directory for this server’s SPN:

“&gt;

I ran the script with my SPN and I got back two results:

“&gt;

I got back two results which means there is a duplicate:

[![Duplicate Service Principal Names](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb10.png "PowerShell GridView")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image10.png)

I openend the account Svc-Pvs in ADSI Edit:

[![Duplicate servicePrincipalNames](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb11.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image11.png)

I have no clue why the PVS Service Account has the SPN’s of two (old) PVS servers (which names are re-used). If someone has an idea why, I would love to know!

I removed all four SPN’s and I was immediately able to logon to the Domain on my server!