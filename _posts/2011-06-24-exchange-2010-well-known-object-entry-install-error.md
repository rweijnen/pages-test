---
id: 1921
title: 'Exchange 2010 well-known object entry install error'
date: '2011-06-24T12:52:30+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/06/24/exchange-2010-well-known-object-entry-install-error/'
permalink: /2011/06/24/exchange-2010-well-known-object-entry-install-error/
views:
    - '15753'
short-url:
    - 'http://bit.ly/lGXjSR'
categories:
    - 'Active Directory'
    - Exchange
    - PowerShell
tags:
    - 'Active Directory'
    - Exchange
    - PowerShell
---

[![SNAGHTML1ca684c](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png "SNAGHTML1ca684c")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png)Today I was testing the installation of Exchange 2010 in a VMWare sandbox environment. We created the sandbox to test migration from a 2003 AD and Exchange environment to 2008 R2 with Exchange 2010.

We used a P2V to get real copies of the Active Directory and the AD upgrade to 2008 R2 was already tested.

But during the Exchange installation in the sandbox I got the following error:

[![The well-known object entry on the otherWellKnownObjects attribute in the container object CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=zorg,DC=local points to an invalid DN or a deleted object.  Remove the entry, and then rerun the task.](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb16.png "Exchange Server 2010 Setup Error")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image16.png)

The setup log (located in C:\\ExchangeSetupLogs) shows a little more detail:  
“&gt;  
The strange thing is that it’s referring to a deleted object (since it’s in the deleted objects container). So what’s going on?

I used the ldp.exe tool to connect to the deleted objects container and inspect the Organization Management object but I couldn’t find any invalid data in it. So I was looking at the wrong place

But if you break down the error message then it’s actually very clear where you need to look:

The attribute [otherWellKnownObjects](http://msdn.microsoft.com/en-us/library/ms679095(v=vs.85).aspx) of the object *CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=zorg,DC=local* (which is a multivalued object) has a value that refers to a deleted item (*B:32:C262A929D691B74A9E068728F8F842EA:CN=Organization Management\\0ADEL:c1b94668-b67b-4231-8e5a-b11ecf5b7838,CN=Deleted Objects,DC=zorg,DC=local*).

So I opened ADSI Edit and navigated to the Microsoft Exchange container:

[![CN=Microsoft Exchange, CN=Configuration](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb17.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image17.png)

Then I looked at the properties of CN=Microsoft Exchange we can see the otherWellKnownObjects attribute:

[![otherWellKnownObjects Value](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb18.png "CN=Microsoft Exchange Properties")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image18.png)

But unfortunately ADSI Edit cannot view or modify this attribute:

[![There is no editor registered to handle this attribute type](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb19.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image19.png)

My next attempt was [ADExplorer](http://technet.microsoft.com/en-us/sysinternals/bb963907) from SysInternals (version 1.42). Once again I navigated to the Microsoft Exchange container:

[![Sysinternals Active Directory Explorer](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb20.png "AD Explorer")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image20.png)

AD Explorer has no problems showing the values:

[![otherWellKnownObjects Properties](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb21.png "Attribute Properties")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image21.png)

I thought I was almost there: I right clicked the wellKnownObjects Attribute then Modify and after selecting the Deleted value I clicked Remove followed by OK:

[![Modify Attribute](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb22.png "AD Explorer")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image22.png)

And this made AD Explorer hang itsself:

[![AD Explorer Hangs](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb23.png "AD Explorer")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image23.png)

Followed by Crash:

[![AD Explorer Crashes](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb24.png "AD Explorer")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image24.png)

So I had to solve it myself with the help of a PowerShell script.

First I read the the otherWellKnownObjects attribute with PowerShell (I wrote about that [earlier](http://192.168.40.25:8081/2011/06/24/reading-the-otherwellknownobjects-attribute-with-powershell/)).

This returns a Collection that I walk backwards with a for loop, this is important when removing items in a collection during a loop (don’t shoot yourself in the foot).

For each item in the Collection I get the distinguishedName from the DNString property and if it contains “0ADEL” then I assume the object it refers to has been deleted so I remove this item from the Collection.

Finally I check if we have deleted at least one item and if so I call SetInfo() to commit the changes to Active Directory.

<span style="color: #ff0000;">**If you want to test the script, be sure to comment the SetInfo() call to prevent the actual deletion in your Active Directory!**</span>  
“&gt;