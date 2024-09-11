---
id: 831
title: 'SQL Query to get the Full Path of an Altiris Job'
date: '2010-12-07T22:22:56+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=831'
permalink: /2010/12/07/sql-query-to-get-the-full-path-of-an-altiris-job/
views:
    - '3141'
short-url:
    - 'http://bit.ly/eS2363'
categories:
    - Altiris
    - 'SQL Server'
---

I wanted to query the full path name of an Altiris Job, this sounds easier that it is though.

Consider the following hierarchy:

![Tree](http://192.168.40.25:8081/wp-content/uploads/2010/12/tree1.png)

Now I want to assemble the full Path, in this case: RPA\\Getronics\\PKG\_p007.Citrix\_Components.

This is important because I want to automatically create the corresponding directory structure which would be: \\\\SERVER\\eXpress\\RPA\\Getronics\\PKG\_p007.Citrix\_Components.

Altiris stores the Job (04.DeliveryServicesConsole) in the events table and in this table is a column folder\_id. We can then lookup this folder\_id in the event\_folder table where each folder\_id has a parent\_id (which can be null in case of a root folder).

So we need to traverse the tree from the bottom up until we find the root folder. SQL Server has a mechanism for recursive queries called [Common Table Expressions](http://msdn.microsoft.com/en-us/library/ms186243.aspx).

Using this mechanism I made the following query:   
“&gt;  
The output is: RPA\\Getronics\\PKG\_\\p007.Citrix\_Components

I added the Level column to be able to sort on it (descending) to get the proper path order, else it would have been p007.Citrix\_Components\\PKG\_\\Getronics\\RPA.

Next steps are to prepend the \\\\SERVER\\eXpress part and make the query parameterised so I can call it from within a job using the %JOBID% token.

To be continued…