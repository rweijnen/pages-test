---
id: 601
title: 'Obtain Fully Qualified Domain Name from Netbios Domain Name'
date: '2010-06-17T14:34:42+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=601'
permalink: /2010/06/17/obtain-fully-qualified-domain-name-from-netbios-domain-name/
views:
    - '4241'
short-url:
    - 'http://bit.ly/ib3zOG'
categories:
    - 'Active Directory'
    - Delphi
    - Programming
---

I needed to obtain the Fully Qualified Domain Name (FQDN) for a given NetBios domain name. Eg from MYDOMAIN to dc=mydomain,dc=local.

I did some tests with the [TranslateName](http://msdn.microsoft.com/en-us/library/ms725484%28VS.85%29.aspx) API and if you append a \\ to the domain name it returns the FQDN.

Here is a short example:

  
“&gt;