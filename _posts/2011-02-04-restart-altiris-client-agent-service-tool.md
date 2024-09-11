---
id: 1391
title: 'Restart Altiris Client Agent Service Tool'
date: '2011-02-04T10:57:48+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/02/04/restart-altiris-client-agent-service-tool/'
permalink: /2011/02/04/restart-altiris-client-agent-service-tool/
views:
    - '3714'
short-url:
    - 'http://bit.ly/h3apcv'
categories:
    - Altiris
---

After a restart of the Altiris Services or the Altiris Server some machines refuse to reconnect.

They are shown in the Computers Tree with the Inactive state icon:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image.png)

The fastest way to resolve this is to restart the “Altiris Deployment Agent” service.

I wrote a little commandline tool to make this easy for myself, it’s called AClientFix.

If you don’t specify any parameters it will restart the services on the local machine. If you specify a Computername as parameter it will restart the services of a remote machine (admin rights needed of course).

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/02/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/02/image1.png)

 [ AClientFix (2753 downloads ) ](http://192.168.40.25:8081/download/aclientfix/?tmstv=1726048919 "Version 1.0")