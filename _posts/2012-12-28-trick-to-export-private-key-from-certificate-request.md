---
id: 2937
title: 'Trick to Export Private Key from Certificate Request'
date: '2012-12-28T15:51:13+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2937'
permalink: /2012/12/28/trick-to-export-private-key-from-certificate-request/
views:
    - '32961'
short-url:
    - 'http://bit.ly/RlxIh6'
categories:
    - 'Windows 2003'
tags:
    - CA
    - Certificates
    - PKI
---

I noticed something interesting today: I needed to generate a Code Signing certificate from a Windows 2003 CA Server.

However the default Code Signing Template does not allow us to export the private key. I found a nice trick however that enables us to request a code signing certificate WITH private key.

To do this I first needed to enable the Code Signing template on the CA Server. This can be done using the Certification Authority MMC Snap-in: right click on the Certificate Templates node and select New | Certificate Template to Issue | Code Signing:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb29.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image29.png)

Now open Internet Explorer and navigate to <http://server/certsrv> (where server is the CA Server of course) and click Request a certificate:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb30.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image30.png)

On the next page click **advanced certificate request** followed by **Create and submit a request to this CA**.

Notice that the Mark keys as exportable option cannot be selected (greyed out):

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb31.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image31.png)

This matched with the template:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb32.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image32.png)

If we click OK (accepting the default options) a certificate will be generated:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb33.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image33.png)

Now click the Back button in Internet Explorer to go back to the previous page:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb34.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image34.png)

Let’s test if this really works, click "Mark keys as exportable", submit the request and click on Install this certificate:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb35.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image35.png)

Now open the Certificates MMC Snap-In and go to Personal | Certificates and export the new certificate.

As you can see we now have the option to export the private key:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb36.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image36.png)

**<u>Security Breach?</u>**   
So where does this leave us, is it a security breach?

I don’t think so because without this trick we already get a certificate with private key, the only difference is that we are not able to export it.

So as far as I am concerned this is just a trick that can be used to quickly get a certificate with private key in the pfx format so we can easily feed it to signtool.