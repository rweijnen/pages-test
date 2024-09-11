---
id: 784
title: 'Altiris Job Builder'
date: '2010-11-23T10:26:34+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=784'
permalink: /2010/11/23/altiris-job-builder/
views:
    - '3260'
short-url:
    - 'http://bit.ly/fALlO3'
categories:
    - Altiris
    - Delphi
    - Programming
    - 'SQL Server'
---

I have created a little tool for myself that I have call Altiris Job Builder, it retreives the Jobs from the Altiris database and shows them in a Treeview.

Then I can assemble a Master Build Job by dragging the needed Jobs to another Treeview on the right. Since it’s just for me it doesn’t have a fancy gui:

[![JobBuilder2](http://192.168.40.25:8081/wp-content/uploads/2010/11/jobbuilder2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/jobbuilder2.png)

So why did I write it? Well I have divided my Jobs into Prerequisites and Packages, for instance IIS and Terminal Server and Java are prereqisuites for Citrix. But many prereqisuites are required for one or more other packages, eg Java is also used for certain applications.I don’t want to make copies of Jobs since that means I have to change it in multiple places. For instance say we upgrade to a new Java version we would need to update multiple jobs instead of one (which is very likely to wrong ).

So the idea behind this master build is to create a Master Build Job that fires off all the needed jobs for a certain package. I use AxSched in an integrated script job for that. So my tool generates something like this:  
“&gt;  
But lazy as I am I don’t want to manually copy/paste this into a job but rather insert it directly into the database. If we look at the eXpress database scheme we can see that there is an event table (a Job is really an Event in Altiris):

![EventTable](http://192.168.40.25:8081/wp-content/uploads/2010/11/eventtable.png)

Only the event\_id field is mandatory and it’s also the Primary Key of the table. However the event\_id is not auto generated so if we insert a new record how do we now the right value for event\_id?

One approach could be to read the highest value and increment that by 1 but that seems dangerous for various reasons. So I figured there was probably a Stored Procedure for adding a new Job/Event.

In the SQL Server Management Studio we can find Stored Procedures under the Programmability Node of the Database:

![StoredProcedures](http://192.168.40.25:8081/wp-content/uploads/2010/11/storedprocedures.png)

If you look into the available Stored Procedures you will see that there are del\_xxx and ins\_xxx events. We need the ins\_event Stored Procedure:

![InsEvent](http://192.168.40.25:8081/wp-content/uploads/2010/11/insevent.png)

In Delphi we can use the TADOCommand object to call a Stored Procedure:

“&gt;  
The ins\_event Stored Procedure returns the new JobId in the @RETURN\_VALUE parameter. This is very convenient because we need the JobId to add tasks to the new Job, in this case a script task.

But enough talk for today, I hope you find the subject interesting and I would be very interested to hear about other solutions or concepts for a Master Build Job.