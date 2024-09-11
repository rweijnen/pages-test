---
id: 799
title: 'Why is my scheduled job not executed in Altiris?'
date: '2010-11-23T20:34:15+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=799'
permalink: /2010/11/23/why-is-my-scheduled-job-not-executed-in-altiris/
views:
    - '2131'
short-url:
    - 'http://bit.ly/en3nKi'
categories:
    - Altiris
    - 'SQL Server'
---

Earlier today I wrote about my [Altiris Job Builder](http://192.168.40.25:8081/2010/11/23/altiris-job-builder/) tool but when I tested the actual produced build job I noticed something weird: the job was scheduled but not executed.

I then tried to manually push a job to this server and that one executed fine.

When I clicked the Job I could see that it was scheduled:

[![Job1](http://192.168.40.25:8081/wp-content/uploads/2010/11/job1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/job1.png)

But when I clicked the Server it wasn’t there:

[![Job2](http://192.168.40.25:8081/wp-content/uploads/2010/11/job2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/job2.png)

Then I fired up SQL Server Management Studio and made a query on the computer table:

[![ComputerTable](http://192.168.40.25:8081/wp-content/uploads/2010/11/computertable-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/computertable.png)

Interesting results no? At least it’s clear what happens, AxSched schedules the job for *the first computer that matches the given computername*.

And in this case the computer is longer present in the console (it’s group\_id is NULL). I assume this happened because I redeployed the Virtual Machine.

So we can fix the error by issueing a SQL Statement, let’s check first:

![SQLCheck](http://192.168.40.25:8081/wp-content/uploads/2010/11/sqlcheck.png)

Yep, that query is ok, so we can delete it.  
**CAUTION: BEFORE YOU CONTINUE MAKE A BACKUP OF THE DATABASE**

> DELETE FROM computer WHERE name = ‘cxnrd02’ AND group\_id IS NULL

or to clean all:

> DELETE FROM computer WHERE group\_id IS NULL

My assumption was right, this fixed the problem!