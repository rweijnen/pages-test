---
id: 290
title: 'TSAdminEx Features Part 2'
date: '2009-02-24T10:17:07+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/02/24/tsadminex-features-part-2/'
permalink: /2009/02/24/tsadminex-features-part-2/
views:
    - '2164'
short-url:
    - 'http://bit.ly/dGn5bi'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
    - Vista
    - 'Windows 2008'
tags:
    - TSAdminEx
---

[Part1](http://192.168.40.25:8081/2009/02/23/tsadminex-features-part-1)

Now that a [TSAdminEx beta is ready](http://192.168.40.25:8081/2009/02/20/tsadminex-beta-release/) I will be showing you some features. In this part I will show the Sessions View.

Let’s start again with a compare of TSAdmin and TSAdminEx:

[![TSAdminSessionView](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminsessionview-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminsessionview.png)

[![TSAdminExSessionView](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsessionview-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsessionview.png)

As you can see TSAdminEx shows more details, it shows the following extra columns:

- Remote Address: This is the actual IP address that the client uses to connect to the Terminal Server (as opposed to the Client Address that Terminal Server API exposes which is merely the internal IP address of the client). Nice detail is that when you move the mouse over the Remote Address you can also see the port number.

[![TSAdminExRemoteIP](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexremoteip-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexremoteip.png)

- Incoming Bytes: The number of incoming bytes for a particular session. A high number indicates high traffic from the client to the server.
- Outgoing Bytes: The number of outgoing bytes for a particular session. A high number indicates high traffic from the server to the client.
- Compression Ratio: Shows the effect of protocol compression.

By sorting on Incoming or Outgoing bytes you can rapidly see which user is consuming the most bandwidth:

[![TSAdminExBandWidth](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexbandwidth-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexbandwidth.png)

Also note that both the User and Session view are automatically updated when the state of a session changes. For instance when a new user logs on or an existing session is disconnect. If you watch this when a new user logs an you can actually see the process of session creation.

In the first step, when the user has not yet logged in the status is connected:

[![TSAdminExUpdate1](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate1.png)

When we are logged in the state changes to Active (actually the session has some other states in between but they go to fast for me to take screenshots):

[![TSAdminExUpdate2](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate2.png)

When we disconnect the status is updates immediately:

[![TSAdminExUpdate3](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate3-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate3.png)

If we reconnect we can see that first a new session is created and then we are reconnected to our old session:

[![TSAdminExUpdate4](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate4-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexupdate4.png)

When you move the mouse over the State column you get a hint that shows you the actual Shadow (Remote Control) permissions:

[![TSAdminExShadowHint](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexshadowhint-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexshadowhint-1.png)

Using the Toolbar buttons or the Context Menu (right-click) you can perform several operations on Sessions:

[![TSAdminExSessionMenu](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsessionmenu-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsessionmenu.png) ![TSAdminExToolbarButtons](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminextoolbarbuttons.png)

This is the Send Message dialog:

[![TSAdminExSendMessage](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsendmessage-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexsendmessage.png)

And the Shadow (Remote Control) Dialog:

[![TSAdminExShadow](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexshadow-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/tsadminexshadow.png)

Well that’s it for today, tomorrow I will discuss the Process View.

If you would like to participate in the beta test please view [here](http://192.168.40.25:8081/2009/02/20/tsadminex-beta-release/).