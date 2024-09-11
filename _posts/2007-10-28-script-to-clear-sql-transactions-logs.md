---
id: 39
title: 'Script to clear SQL Transactions logs'
date: '2007-10-28T13:11:15+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/28/script-to-clear-sql-transactions-logs/'
permalink: /2007/10/28/script-to-clear-sql-transactions-logs/
views:
    - '8197'
short-url:
    - 'http://bit.ly/esfLfx'
categories:
    - script
    - 'SQL Server'
---

Script to clear SQL Transactions Logs

Did you know that when you backup a SQL database with Backup Exec (with the SQL Agent) the transaction log is not cleared? This means that if you use the full recovery model your transaction log keeps growing and growing. I tested this with Backup Exec v11D and you can only create a seperate scheduled job to backup the transactions logs but not one to just clear it or truncate it after successfull backup.

Therefore I made a VBS script that clears SQL transactions logs, an option would be to schedule this as a post backup job.

This is the script:

  
“&gt;  
[ ClrSQLLogs.zip (1690 downloads ) ](http://192.168.40.25:8081/download/clrsqllogs-zip/?tmstv=1726048918 "Version 1.0")