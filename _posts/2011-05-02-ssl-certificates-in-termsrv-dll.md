---
id: 1726
title: 'SSL Certificates in termsrv.dll'
date: '2011-05-02T14:54:39+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/05/02/ssl-certificates-in-termsrv-dll/'
permalink: /2011/05/02/ssl-certificates-in-termsrv-dll/
views:
    - '3397'
short-url:
    - 'http://bit.ly/iiUe9w'
categories:
    - PowerShell
    - 'Terminal Server'
    - 'Windows 2008'
---

I was digging around in termsrv.dll yesterday when I noticed that there are some (well 372 to be exact) SSL certificates inside the Terminal Server binary (termsrv.dll):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image7.png)

Two of them seem to actually contain the private keys as well, but I am not 100% sure it may be just a certificate in another format.

I wrote a small PowerShell script to import these certificates into the personal certificate store so I could inspect them easily:

 “&gt;

The certificates appear to be public certificates of root CA’s, I have no idea of their intended purpose (why they need to be inside the termsrv.dll):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image8.png)

The keys that were marked as private are these two:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image9.png)