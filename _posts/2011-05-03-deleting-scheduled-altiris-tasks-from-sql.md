---
id: 1731
title: 'Deleting scheduled Altiris tasks from SQL'
date: '2011-05-03T10:27:30+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1731'
permalink: /2011/05/03/deleting-scheduled-altiris-tasks-from-sql/
short-url:
    - 'http://bit.ly/j76AkA'
views:
    - '2527'
categories:
    - 'SQL Server'
    - VMWare
---

I needed to delete around 50 scheduled tasks from several machines in Altiris because something went wrong in on of the first jobs.

It would have better if the jobs were configured to fail on error and not continue but they weren’t.

Deleting the jobs from the Altiris console is very, very, slow. First the console asks for confirmation (after showing the hourglass for a long time):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image10.png)

Then the actual delete can take a few minutes and then the next server and so on.

I decided to delete the tasks directly from SQL.

I know this is not preferred but I think in the end it’s safe enough because I found a stored procedure called *del\_event\_schedule* which looks like this:

 “&gt;

So al it does is a (transacted) delete from the table.

First I created a query that selects the current scheduled tasks:

“&gt;

This selects the computer name and task name for all computers that match VCTXA070% that have not yet started (*status\_code* is null).

The *start\_time* check prevents deleting recurring tasks like this one:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image11.png)

   
Since this gave me the correct results I deleted them:

“&gt;  
If you want to delete completed tasks as well you can just leave out the *status\_code* check:

“&gt;