---
id: 2545
title: 'Imprivata fails to logon with special characters in the password'
date: '2012-03-14T15:19:08+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/14/imprivate-fails-to-logon-with-special-characters-in-the-password/'
permalink: /2012/03/14/imprivata-fails-to-logon-with-special-characters-in-the-password/
views:
    - '20828'
short-url:
    - 'http://bit.ly/AvYhN5'
categories:
    - General
tags:
    - AutoIT
    - Imprivata
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image17.png)Interesting case today: customer uses Imprivate for two factor logon in combination with Citrix XenApp.

Users reported that logons failed after they had changed their password. After contacting the users we learned that this only happened with special characters in the password like ! and +.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image20.png)To do the actual logon to Citrix Imprivata uses an executable which is actually an AutoIT script compiled to an executable.

After authentication the executable get’s the password from the Imprivata Appliance.

I decompiled the executable to source and read the line that passes the password to XenApp:

“&gt;

I then checked the AutoIT documentation for the ControlSend function and learned there’s an extra parameter Flag with a default value of 0. This flags determines how keys are processed.

When Flag = 0 (default), special characters like + are used to indicate moving the cursor or indicate SHIFT. When Flag =1 the keys are send raw which is what we need for the passsword.

I changed the line to:

“&gt;

And now it works fine!