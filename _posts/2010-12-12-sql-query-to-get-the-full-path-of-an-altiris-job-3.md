---
id: 845
title: 'SQL Query to get the Full Path of an Altiris Job #3'
date: '2010-12-12T19:50:32+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=845'
permalink: /2010/12/12/sql-query-to-get-the-full-path-of-an-altiris-job-3/
views:
    - '2969'
short-url:
    - 'http://bit.ly/fQ8J8M'
categories:
    - Altiris
    - 'SQL Server'
---

Last time I [showed](http://192.168.40.25:8081/2010/12/09/sql-query-to-get-the-full-path-of-an-altiris-job-2/ "SQL Query to get the Full Path of an Altiris Job #2") a User Defined Function to the Full Path of an Altiris Job given it’s id (event\_id). Note that Altiris calls a Job an Event so the terms Event and Jobs are interchangeable here.

To complete it we first need to prepend the server and share name to the path.

I looked into the Altiris database tables to find the best place to get the servername and it seems that the hostname column of the mmsettings table is a good way.

In my database there was only one row in the table but I restrict the results by adding top 1:  
“&gt;  
Then I looked into the available tokens for one that returns a job id but we can only return a job name or a computer id. Since a job name is not unique I decided to use the computername and find the active job for this computer.

When a Job is scheduled an entry is added to the event\_schedule table. If you look into this table you will notice a column status\_code which is NULL initially and when the job start it will get a value of -1 which indicates the job is active.

When the job has finished the status will always be 0 or higher.So we can determine the active job by querying the event\_schedule table for the computer\_id and status\_code = -1.  
“&gt;  
But I found out that during the phase where the Custom Tokens are evaluated the status\_code column is still NULL.

However if we query for status\_code is NULL we may get back more than one row if there a several jobs scheduled for this computer. So again I used the TOP 1 statement to limit the results:  
“&gt;  
This is the final UDF:  
“&gt;  
And we use it like this in the Job:  
“&gt;  
I didn’t find the share name in any of the tables so I hardcoded it as eXpress. If there’s a better way please let me know!

I would be very interested in your comments, do you find this approach usefull or do you have suggestions? Please leave a comment!