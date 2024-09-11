---
id: 1088
title: 'Another Oracle Error'
date: '2011-01-07T13:26:20+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/01/07/another-oracle-error/'
permalink: /2011/01/07/another-oracle-error/
views:
    - '1853'
short-url:
    - 'http://bit.ly/geDIvi'
categories:
    - 'Application Compatibility'
    - Oracle
    - Packaging
    - 'Unattended Installation'
---

My collegue that is working with me on the current project was packaging another application that uses Oracle.

The interesting thing about it is that he ran into the same error I did: the Oracle client tries to create (and access) an Object in the Global namespace.

But he got an entirely different error message:

![OCIEnvCreate failed with return code -1 but error message text was not available](http://192.168.40.25:8081/wp-content/uploads/2011/01/oracleerror.png)

To confirm it was the same issue I gave a test user the Create Global Objects privilege and after logging on again the problem went away.

So I applied the same fix that I wrote about yesterday in [Redirect Global Object to Local Objects (aka fix that bad app!)](http://192.168.40.25:8081/2011/01/06/redirect-global-object-to-local-objects-aka-fix-that-bad-app/ "Redirect Global Object to Local Objects (aka fix that bad app!)") to this application and it worked nicely!