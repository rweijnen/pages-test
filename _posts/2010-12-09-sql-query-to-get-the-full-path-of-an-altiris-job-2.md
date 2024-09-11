---
id: 840
title: 'SQL Query to get the Full Path of an Altiris Job #2'
date: '2010-12-09T10:00:43+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=840'
permalink: /2010/12/09/sql-query-to-get-the-full-path-of-an-altiris-job-2/
views:
    - '1710'
short-url:
    - 'http://bit.ly/fFFawY'
categories:
    - Altiris
    - 'SQL Server'
---

Earlier I described a [SQL Query to get the Full Path of an Altiris Job](http://192.168.40.25:8081/2010/12/07/sql-query-to-get-the-full-path-of-an-altiris-job/), today I will describe how we can make a [User Defined Function](http://msdn.microsoft.com/en-us/library/ms189593.aspx "User Defined Functions") (UDF) in SQL so we can call it easier.

I am using an UDF because it allows us to specify parameters, in this case a single parameter (the EventId (or job id).

This is the SQL that creates the UDF:  
“&gt;  
After you have executed this, you can find the UDF in the Programmability Section of SQL:

![SqlProgSection](http://192.168.40.25:8081/wp-content/uploads/2010/12/sqlprogsection.png)

To call this UDF you need to use the two part name (dbo.GetFullPath) as described [here](http://msdn.microsoft.com/en-us/library/ms175562.aspx "Executing User-Defined Functions").

Example:

![UdfCallExample](http://192.168.40.25:8081/wp-content/uploads/2010/12/udfcallexample.png)

Next step will be to prepend the [\\\\SERVER\\Share](file://\\SERVER\Share) part which we can do in the UDF as well.