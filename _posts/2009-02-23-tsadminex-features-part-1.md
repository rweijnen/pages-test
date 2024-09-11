---
id: 264
title: 'TSAdminEx Features Part 1'
date: '2009-02-23T16:18:28+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/02/23/tsadminex-features-part-1/'
permalink: /2009/02/23/tsadminex-features-part-1/
views:
    - '4724'
short-url:
    - 'http://bit.ly/f9cAFF'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
    - Vista
    - 'Windows 2008'
tags:
    - TSAdminEx
---

[Part 2](http://192.168.40.25:8081/2009/02/24/tsadminex-features-part-2/)

Now that a [TSAdminEx beta is ready](http://192.168.40.25:8081/2009/02/20/tsadminex-beta-release/) I will be showing you some features. In this part 1 I will be comparing the Users view to TSAdmin.

Let’s start TSAdmin, this tool is present by default on Windows 2003. If you use Windows XP or Windows Vista you can get it by installing the [Administration Pack](http://www.petri.co.il/download_windows_2003_r2_adminpak.htm). Please note that TSAdmin does not work on Vista RTM due to a [bug](http://192.168.40.25:8081/2007/12/19/why-tsadmin-crashes-on-windows-vista/) that was [corrected in Vista SP1](http://192.168.40.25:8081/2008/03/02/vista-sp1-changes-to-terminal-server-api/) (TSAdminEx works fine on both RTM as well as SP1)

[![TSAdmin1](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadmin1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadmin1.png)

Now let’s open TSAdminEx and start comparing…

[![TSAdminEx1](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminex1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminex1.png)

You will see that the Users view (which is default on both) looks similar, some minor flaws of TSAdmin were adresses here:

- TSAdminEx has an indicator to show the sorted column and sort direction.
- TSAdminEx saves settings like column width and has an optional autosize option in the View menu

[![TSAdminExAutoSize](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexautosize-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexautosize.png)

- TSAdminEx has a column selection option where you can choose which columns you’d like to see or hide:

[![TSAdminExSelectColumns2](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns2-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns2-1.png) [![TSAdminExSelectColumns3](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns3-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns3.png) [![TSAdminExSelectColumns](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns-2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexselectcolumns-2.png)

- In TSAdminEx you can simply select or row (for some reason you can in TSAdmin only select the server column):

[![TSAdminExRowSelect](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexrowselect-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexrowselect.png)

- In TSAdminEx you can quickly jump to a particular row by typing the first letters of it’s value (just like in Explorer). It takes the value of the active column (the sorted column).

Tomorrow I will discuss the Sessions View of TSAdminEx which has more details compared to TSAdmin, so stay tuned!

Take a look [here](http://192.168.40.25:8081/2009/02/20/tsadminex-beta-release/) if you would like to be a beta tester.

[TSAdminEx Features Part 2](http://192.168.40.25:8081/2009/02/24/tsadminex-features-part-2/)