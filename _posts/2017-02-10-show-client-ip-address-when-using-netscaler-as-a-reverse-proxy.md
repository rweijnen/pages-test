---
id: 3963
title: 'Show Client IP Address when using NetScaler as a Reverse Proxy'
date: '2017-02-10T14:10:45+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=3963'
permalink: /2017/02/10/show-client-ip-address-when-using-netscaler-as-a-reverse-proxy/
views:
    - '4350'
categories:
    - Uncategorized
tags:
    - Citrix
    - 'Client IP'
    - NetScaler
    - 'Reverse Proxy'
    - Wordpress
    - X-Forwarded-For
---

[![Citrix NetScaler Logo](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb-3.png "Citrix NetScaler")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image-3.png)Recently I switched over my blog from a hoster to a self hosted VM. In my setup I am using Citrix NetScaler as a reverse proxy.

Simular to when you’re using a 3rd party reverse proxy such as [CloudFlare](https://www.cloudflare.com/) you will see the IP address from the reverse proxy instead of the actual Client IP Address on your webserver.

This means that your logging will all show the same, internal, IP address and that IP Based Access Rules will not work.

Fortunately this is easy to solve by having NetScaler add the Client IP Address in the headers and rewriting the address on your webserver.

On the NetScaler go to your Service or Service Group and activate the `Insert Client IP Address` under Settings and set a value in the Header box (`X-Forwarded-For`) seems to be a common one.

<div class="wp-caption alignnone" style="width: 250px">[![Insert Client IP Address | Header | X-Forwarded-For](http://192.168.40.25:8081/wp-content/uploads/2017/02/image_thumb-4.png "NetScaler Service (Group) Settings")](http://192.168.40.25:8081/wp-content/uploads/2017/02/image-4.png)NetScaler Service (Group) Settings

</div>On the webserver you’ll need to catch this header and instruct the webserver to use the IP Address provided in the `X-Forwarded-For` header.

There are a couple of WordPress plugins that can do this but it seemed more logical to handle this on the webserver itsself.

In my case I am using NGINX where you can set this in either your `nginx.conf` or your (default) site configuration under `sites-available`:  
“&gt;  
example:  
“&gt;