---
id: 947
title: 'Master Return codes in Altiris'
date: '2010-12-24T15:48:17+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=947'
permalink: /2010/12/24/master-return-codes-in-altiris/
views:
    - '6212'
short-url:
    - 'http://bit.ly/eaOSLl'
categories:
    - Altiris
---

Altiris offers a Master Return code, a very usefull feature that is not used often I think. In this article I will explain why I find them usefull and we will implement a Master Return code for Installations that require a reboot.

A Master Return code can be consired as a Global Action handler, whenever a job returns a specific error/return/result code this Global handler will be called (unless you override it in the job with another action).

A perfect usage for a Global Action handler is installing MSI files that need I reboot. If you do nothing the system will be rebooted after the successfull installation and your job will return as failed with error 3010.

Microsoft [documents](http://msdn.microsoft.com/en-us/library/aa368542(v=vs.85).aspx "Error Codes (Windows)") that 3010 means ERROR\_SUCCESS\_REBOOT\_REQUIRED: A restart is required to complete the install. This message is indicative of a success.

To prevent the reboot we can pass REBOOT=ReallySurpress to the MSI but we still need to indicate to Altirs that 3010 actually means Success and that we need to reboot.

So let’s create our Master Return code. First we a folder where we will place our Master Return code jobs, I named this folder Ret.

In the Ret Folder add a job named 3010 and add a Power Control action to this Job and select Restart. The result will be something like this:

[![Ret3010](http://192.168.40.25:8081/wp-content/uploads/2010/12/ret3010-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/ret3010.png)

Now go to your Install job and add REBOOT=ReallySurpress to the cmdline and goto the Return codes screen of the Job (press Next twice) and click Master Return Codes:

[![ReturnCodes](http://192.168.40.25:8081/wp-content/uploads/2010/12/returncodes-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/returncodes.png)

In the next screen click Add and fill in 3010 for Code and Success for Result and in the Response field choose “Select a job…”:

![AddReturnCode](http://192.168.40.25:8081/wp-content/uploads/2010/12/addreturncode1.png)

Now select the Job we created earlier and press OK:

[![SelectJob](http://192.168.40.25:8081/wp-content/uploads/2010/12/selectjob-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/selectjob.png)

In the Status field you can enter a description (I used the error description text):

![Status](http://192.168.40.25:8081/wp-content/uploads/2010/12/status.png)

Finally when you execute the Install Job it will return with Success:

[![ResultSuccess](http://192.168.40.25:8081/wp-content/uploads/2010/12/resultsuccess-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/resultsuccess.png)