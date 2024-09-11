---
id: 1286
title: 'Adding a hidden Exchange mailbox to Outlook'
date: '2011-01-25T13:11:42+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1286'
permalink: /2011/01/25/adding-a-hidden-exchange-mailbox-to-outlook/
short-url:
    - 'http://bit.ly/hASWDO'
views:
    - '48466'
categories:
    - 'Active Directory'
    - Exchange
    - PowerShell
tags:
    - Address
    - Check
    - Exchange
    - Global
    - Hidden
    - List
    - Names
    - Outlook
    - Resolve
---

In Exchange it’s possible to hide a Mailbox from the (Global) Address List. You can do that in the Exchange System Manager:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image13.png)

But after you have hidden a Mailbox you cannot create an Outlook profile for it (or add it as an extra mailbox).

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image14.png)

When you click Check Name in the wizard you’ll get an error:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image15.png)

The common workaround is to remove the “*Hide from Exchange address lists*” setting, create the profile (or add the Mailbox) and afterwards set it again.

Once the profile is created it all keeps working.

There is an easier solution though!

  
If you add the mailbox using the [legacyExchangeDN](http://msdn.microsoft.com/en-us/library/aa144763(EXCHG.65).aspx "LegacyDN Property") attribute then Outlook happily accepts.

This works because Outlook really uses this value and not the user’s accountname to connect to a Mailbox.

 The Error above occurs because Outlook cannot resolve the username to the legacyExchangeDN name using the Address List.

So how do we get the user’s legacyExchangeDN?

It can be read using ADSI Edit:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image16.png)

But I prefer to read it with a script since it means less clicking.

Using PowerShell it’s also a very simple script:  
“&gt;  
Now we take the result, in my case:  
“&gt;  
Now we copy &amp; paste this into the User Name Edit:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image17.png)

And Outlook will resolve it to the username ![Open-mouthed smile](http://192.168.40.25:8081/wp-content/uploads/2011/01/wlEmoticon-openmouthedsmile.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb18.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image18.png)

The same procedure can be used if you want to Add a Hidden Mailbox as Additional Mailbox:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image19.png)