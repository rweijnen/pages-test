---
id: 249
title: 'TSAdminEx Beta release'
date: '2009-02-20T17:59:06+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/02/20/tsadminex-beta-release/'
permalink: /2009/02/20/tsadminex-beta-release/
views:
    - '8381'
short-url:
    - 'http://bit.ly/ejd7cs'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
    - Vista
    - 'Windows 2008'
tags:
    - TSAdminEx
---

Over the last months I have been working hard on TSAdminEx and now, finally, I can now present a first beta release.

If you don’t know what TSAdminEx is let me briefly introduce it. TSAdminEx is a tool that combines functionality of several existing tools: it has the power of task manager combined with the details of Process Explorer and the Terminal Server support of TSAdmin. On top of that it fully supports remote systems out of the box without installing any agents or services. It also has some unique features that neither of the mentioned tools can do!

Several new features have been implemented since the [last time I talked about TSAdminEx](http://192.168.40.25:8081/2008/01/27/test-2/) and I will show you the most exciting ones here:

In the processes tab, processes now show the icon (where possible) and more detailled memory info has been added (it now shows Working Set Size, Commit Size and Virtual Size). One thing I like very much is that there’s now also a columns that shows the relative CPU time just like Taskmanager. Ofcourse TSAdminEx can do this for remote system without installing an agent or service component on the remote system!

[![TSAdminExProcesses](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexprocesses-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexprocesses.png)

As you can see on the screenshot above, many details are show. You can use the new Select Columns Dialog to choose which columns you want to see (and ofcourse these settings are preserved in the registry):

[![TSAdminExSelectColumns](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns.png)

But there are also many changes “under the hood”:

- TSAdminEx fully supports elevation under Vista (just like Task Manager there is a Menu Option to restart with Admin Privileges).
- The thread engine is rewritten from scratch.
- Many bugs are fixed.

I will be showing more of TSAdminEx’ features in a series of posts, you can find part1 [here](http://192.168.40.25:8081/2009/02/23/tsadminex-features-part-1/).

Now I will concentrate on passing the Beta stage and for this I would like to request Beta testers. I am looking for people who can test TSAdminEx, preferably in large environments and different Operating Systems.

If you would like to be a Beta Tester please you can send me an email (r dot weijnen at gmail\_dot\_com). Please tell me something about your environment and how you could test my tool, thank you!

[![TSAdminExAbout](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexabout-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexabout.png)