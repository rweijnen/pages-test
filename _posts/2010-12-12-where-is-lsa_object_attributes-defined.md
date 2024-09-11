---
id: 853
title: 'Where is LSA_OBJECT_ATTRIBUTES defined?'
date: '2010-12-12T23:06:20+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/12/12/where-is-lsa_object_attributes-defined/'
permalink: /2010/12/12/where-is-lsa_object_attributes-defined/
views:
    - '1271'
short-url:
    - 'http://bit.ly/eemdam'
categories:
    - Programming
---

If you read MSDN documentation for [LSA\_OBJECT\_ATTRIBUTES](http://msdn.microsoft.com/en-us/library/ms721829(v=vs.85).aspx "LSA_OBJECT_ATTRIBUTES Structure") you will think it’s defined in LsaLookup.h:

[![MSDN](http://192.168.40.25:8081/wp-content/uploads/2010/12/msdn-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/msdn.png)

And that it’s supported since Windows 2000 but I couldn’t find it in this header file.

Instead I found it in NTSecAPI.h, so I decided to check the different SDK versions and starting from SDK v7 LsaLookup.h exists but in earlier SDK’s (v5.0, v6.0a and v6.1) there is no LsaLookup.h.