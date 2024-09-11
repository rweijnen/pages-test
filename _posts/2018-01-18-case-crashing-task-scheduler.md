---
id: 4265
title: 'Case of the Crashing Task Scheduler'
date: '2018-01-18T14:56:23+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4265'
permalink: /2018/01/18/case-crashing-task-scheduler/
views:
    - '2928'
categories:
    - 'Windows 10'
tags:
    - 'Task Scheduler'
    - 'Windows 10'
---

Recently I installed a new Windows 10 machine (version 1709 aka Fall Creators Update).

After a while I noticed a problem with the Task Scheduler: when I wanted to open the “Schedule Tasks” option from settings I received the following error message:

[![The remote computer was not found.](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-7.png "Task Scheduler")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-7.png)

The Task Scheduler MMC snapin was empty:

[![Task Scheduler (Local) | Empty](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-8.png "Task Scheduler")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-8.png)

I opened the Services snapin and noticed that the Task Scheduler was not running. Attempting to start it would either result in immediate crash with Error 1067 or crash shortly after starting:

[![Windows could not start the Task Scheduler service on Local Computer | Error 1067: The process terminated unexpectedly](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-9.png "Services")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-9.png)

The System eventlog didn’t provide any helpful details other than notification that the service terminated unexpectedly:

[![The Task Scheduler service terminated unexpectedly. It has done this 1 time(s). The following corrective action will be taken in 0 milliseconds: Restart the service in a separate process.](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-10.png "Event 7031, Service Control Manager")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-10.png)

Next step was to check the Event Log for the Task Scheduler which is located under *Application and Services Logs* | *Microsoft* | *Windows* | *TaskScheduler* | *Operational*. Note that this log needs to be enabled:

[![Microsoft | Windows | TaskScheduler | Operational](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-11.png "Enable Log")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-11.png)

I then attempted start of the Task Scheduler service again (several times) and the last line in the log would always be: `Task Scheduler launched action “” in instance “{f9903615-3d9f-4cb5-9838-70a38327eec8}” of task “\\Microsoft\\Windows\\.NET Framework\\.NET Framework NGEN v4.0.30319 64 Critical”.`

I checked the corresponding xml file in `C:\\Windows\\System32\\Tasks\\Microsoft\\Windows\\.NET Framework\\.NET Framework NGEN v4.0.30319 64 Critical` and compared it to a working system and there were no differences.

Next attempt was to delete the TaskCache registry key which is located under `HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Schedule\\TaskCache`.

**Before you do this make a backup or simply rename the key!**

You need to take ownership of this registry key by opening Permissions by right clicking on the key and taking ownership by clicking Advanced and then Change next to Owner (System by default). Set the Owner to be either Administrators, click OK and tick “Full Control” for Administators.

The Task Scheduler service now started and didn’t crash however the view in the Task Scheduler snapin was still empty (no tasks).

I used the Repair Tasks tool from [this](https://repairtasks.codeplex.com/) link and clicked Scan followed by Repair:

[![Scan | Repair | Recycle existing tasks | Take tasks from backup](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-12.png "Repair Tasks")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-12.png)

Finally that fixed it!