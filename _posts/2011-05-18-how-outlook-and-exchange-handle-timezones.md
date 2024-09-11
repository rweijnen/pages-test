---
id: 1761
title: 'How Outlook and Exchange handle Timezones'
date: '2011-05-18T19:53:27+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1761'
permalink: /2011/05/18/how-outlook-and-exchange-handle-timezones/
short-url:
    - 'http://bit.ly/mvwQH5'
views:
    - '2471'
categories:
    - Delphi
    - Exchange
    - Programming
---

Again an old war story, this time about timezone handling in Outlook/Exchange.

I am not sure which year it was but I had just started to work for a new company and inherited an Exchange 5.5 Server.

The mail had been migrated from an earlier version and calendar data was migrated from [Schedule+](http://en.wikipedia.org/wiki/Microsoft_Schedule_Plus).

On the first change to Daylight Savings (DTS) all recurring appointments where shown one hour later (or earlier can’t remember) in Outlook. A manual change was not an option: there were over 2000 mailboxes each with a lot of appointments.

We first tried a workaround by disabling DTS on the the workstations and then manually change the time when changing from and to DTS.

But this influenced the timestamps on externals mails and of course appointments with external parties.

After a lot of (and I really mean a lot) of researching I found that Outlook stores all times in an appointment as relative (UTC) time.

Upon display it uses an undocumented TimeZone descriptor field to convert to Local Time.

This Timezone information is in a Named Property of the Appointment that can only be read using Extended MAPI.

I found the property (Named Property 0x8233) with [OutlookSpy](http://www.dimastr.com/outspy/) (absolutely brilliant tool btw) and it’s of type PT\_BINARY (array of Byte).

In my code I placed the following note to describe the format (with the correct data for the Dutch TimeZone):

 [![SNAGHTML53925c3](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML53925c3_thumb.png "SNAGHTML53925c3")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML53925c3.png)

When I was writing this article today, I looked at this value again with Outlook Spy (using Outlook 2010 with Exchange 2010).

I was pretty surprised that OutlookSpy now knows this property:

[![SNAGHTML51cd5fe](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML51cd5fe_thumb.png "SNAGHTML51cd5fe")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML51cd5fe.png)

And I even found that Microsoft has actually documented this property now as [PidLidTimeZoneStruct](http://msdn.microsoft.com/en-us/library/cc815376.aspx) which looks like this:

 “&gt;

I also found it in the Outlook 2007 docs as [dispidTimeZoneStruct](http://msdn.microsoft.com/en-us/library/cc160680(v=office.12).aspx) but the wStandardYear and wDaylighYear are noted as Reserved there:

“&gt;

But on with the story: I wrote a tool in Delphi 6 that enumerated the Global Address List and opened each mailbox on the server.

From each mailbox I opened the calendar folder and from that folder enumerated all Appointments.

For each Appointment I needed to check if it was recurring (only recurrring items have this property) and if so I updated the property with the proper value (that I found by creating a new recurring appointment).

I found back a part of the code because I posted it to the [MAPI Developers group in 2006](http://www.techienuggets.com/commentList.jsp?tx=103&tx=103&d-49653-p=5). Sadly I don’t have the complete code anymore but this is the interesting part:

“&gt;

The tool was started on a Friday evening and ran till Sunday night but was (luckily) finished on time before the next working day.

The Monday was a little exciting but all appointments were processed 100% good and all was ok!