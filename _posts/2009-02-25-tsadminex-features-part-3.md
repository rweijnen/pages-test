---
id: 320
title: 'TSAdminEx Features Part 3'
date: '2009-02-25T13:08:31+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/02/25/tsadminex-features-part-3/'
permalink: /2009/02/25/tsadminex-features-part-3/
views:
    - '3051'
short-url:
    - 'http://bit.ly/gMze5d'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
    - Vista
    - 'Windows 2008'
tags:
    - TSAdminEx
---

| [Beta Release](http://192.168.40.25:8081/2009/02/20/tsadminex-beta-release/) | [Part 1](http://192.168.40.25:8081/2009/02/23/tsadminex-features-part-1/) | [Part 2](http://192.168.40.25:8081/2009/02/24/tsadminex-features-part-2/) |  |  |
|---|---|---|---|---|

This is part 3 of the TSAdminEx Features series. Today I will discuss the Process View. As usual we will start by comparing TSAdmin to TSAdminEx again. So let’s look at TSAdmin Process View:

[![TSAdminProcess](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminprocess-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminprocess.png)

And the one from TSAdminEx:

[![TSAdminExProcessView](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexprocessview-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexprocessview.png)

As you might notice TSAdminEx shows many more details but also more processes. I ran both tools on the same server with the same account (just a normal user account). TSAdmin is very restricted under non admin accounts, let’s look closer:

[![TSAdminProcessCloser](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminprocesscloser-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminprocesscloser.png)

TSAdmin shows all processes in my Session and shows for all other sessions the csrss.exe process and the winlogon process. It doesn’t seem to know what user is tied to those processes.

Let’s look at what Task Manager shows us:

[![TaskManager](http://192.168.40.25:8081/wp-content/uploads/2009/02/taskmanager-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/taskmanager.png)

Task Manager shows only the processes in my Session but again does not show the user names for wnlogon.exe and csrss.exe.

If we look at TSAdminEx we see that it has no problem showing all user processes and the user who owns the process:

[![TSAdminExProcessCloser](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexprocesscloser-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexprocesscloser.png)

But even TSAdminEx has some limitations under Non Administrative accounts, if we sort on Process Id we can see that we do not see the System Process and the System Idle Process:

[![TSAdminExSortOnPid](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsortonpid-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsortonpid.png)

So let’s run it as an Administrator now:

[![TSAdminExSortOnPid2](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsortonpid2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsortonpid2.png)

Please note that the System and System Idle Process are important because they are needed to calculate CPU Time. Which brings us to that feature of TSAdminEx.

If we select the Auto Update checkbox on the Process View TSAdminEx will automatically update the process information every second (presently this is a fixed interval but this might change to selectable interval).

After an update TSAdminEx calculates the relative CPU time for each process and displays this in the CPU Column:

[![TSAdminExCPU](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexcpu-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexcpu.png)

We can also do this for remote and even multiple (remote) servers at a time:

[![TSAdminExCPU2](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexcpu2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexcpu2.png)

So this gives us a view we can instantly see relative CPU time on all servers and by sorting on the CPU column we can keep an eye on processes that consume a lot of CPU time.

For even more insight we can look at the CPU Time column which displays the total CPU time that has been allocated to the process since it’s creation in HH:MM:SS. The Process Age column is also helpfull since it’s displays how long ago the process was created in d+HH:MM:SS (d=Days).

[![TSAdminExCPU3](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexcpu3-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexcpu3.png)

By looking at the process with the highest Process Age (sort on this column) we can easily determine the uptime of the server:

[![TSAdminExUpTime1](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexuptime1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexuptime1.png)

[![TSAdminExUpTime2](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexuptime2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexuptime2.png)

It is no coincidence that the oldest process is SMSS.exe as it is one of the first processes launched at boot.

Besides CPU allocation TSAdminEx also presents detailed information on Process Memory Allocation:

[![TSAdminExMemoryStats](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexmemorystats-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexmemorystats.png)

With a right-click on a process (or the corresponding Toolbar button) we can end a process:

![TSAdminExEndProcess](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexendprocess.png)

As you might have noticed TSAdminEx tries to show the process Icon where possible. This only works if this process has an icon and the process can be found on the machine that TSAdminEx runs ons (querying icon remotely over the network would decrease performance too much).

I want to end this post with a remark I forgot to make yesterday: TSAdmin always sorts columns Case Sensitive, this has always annoyed me because it makes searching for a particular user or process difficult. Ofcourse TSAdminEx fixes this and sorts Case Insensitive.

TSAdmin:

![TSAdminSort](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminsort.png)

TSAdminEx:

![TSAdminExSort](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsort.png)

Don’t forget, participating in the [Beta test](http://192.168.40.25:8081/2009/02/20/tsadminex-beta-release/) is still possible…