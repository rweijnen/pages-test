---
id: 2081
title: 'Exchange Move Mailbox Experiences Part 1'
date: '2011-09-21T16:38:23+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/09/21/exchange-move-mailbox-experiences-part-1/'
permalink: /2011/09/21/exchange-move-mailbox-experiences-part-1/
views:
    - '7390'
short-url:
    - 'http://bit.ly/r4ugA2'
categories:
    - Exchange
tags:
    - Exchange
    - Migration
    - MoveMailbox
    - Outlook
---

[![Exchange Logo](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png?9d7bd4 "Exchange Logo")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png?9d7bd4)

I am currently working on a migration project from Exchange 2003 to Exchange 2010.

Most Exchange migration projects use Mailbox Moves to move the mailbox data to the new Exchange environment.

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image1.png)But there are some things I observed during Mailbox Moves (from Exchange 2003 to 2010) that are worth mentioning.

<span style="text-decoration: underline;">**Users can access their mailbox during moves**</span>

The users can continue to access their mailbox during a Mailbox Move but when the move has finished Outlook will request the user to restart Outlook:

[![The Microsoft Exchange administrator has made a change that requires you to quit and restart Outlook](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb2.png "Microsoft Outlook")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image2.png)

<span style="text-decoration: underline;">**Very Large Mailboxes**</span>

First of all, be prepared that move large mailboxes can take quite a large amount of time. For example a 20GB mailbox took almost 4 hours on my system:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image3.png)

But more importantly: **Mailboxes with a total size of *16 Gigabytes and higher* cannot be accesses during the move**. If you try to open them during a move you will probably get this Error Message:

[![Cannot open your default e-mail folders. Microsoft Exchange is not available. Either there are network problems or the Exchange server is down for maintenance](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb4.png "Microsoft Outlook")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image4.png)

Note that the exact message differs per Outlook version and language, for example on Dutch Outlook 2003:

[![Kan Microsoft Office Outlook niet starten. Kan het Outlook-venster niet openen. Kan de set mappen niet openen](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb5.png "Microsoft Outlook")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image5.png)

On the source Exchange server an MSExchangeIS event with *Event ID 9660* will be logged in the Application log:

[![User <userName> (<legacyExchangeDn>) failed to log on because their mailbox is in the process of being moved. ” width=”286″ height=”317″ border=”0″ /></a></p>
<p> </p>
<p><span style=](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb6.png "Event Properties")**Device Synchronization**](http://192.168.40.25:8081/wp-content/uploads/2011/09/image6.png)

![](http://www.agatsolutions.com/AgatSite/images/Filters/ActiveSync-Big.png)Users that synchronize their devices (smartphones, tablets etc) with Exchange using ActiveSync/Push Mail may receive an alert.

If such an alert is shown and the exact message depends on the device. This screenshot comes from an HTC Windows Mobile phone:

[![Alert | There has been a change made on your server that requires you to re-synchronise all items on your device. All changes made since your last successfull sync will be lost. Do you wish to continue?](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb7.png "Warning Message")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image7.png)

It seems that Windows Mobile first *deletes* the current ActiveSync profile (and thus all mail, contacts, tasks and appointment), then *creates a new* ActiveSync profile and finally *resynchronizes* this with the Exchange Server.

The sentence “All changes made since your last successful sync will be lost” is a little alerting. So you may wish to properly inform your users before you move their mailbox if they are not synchronizing continuously.

<span style="text-decoration: underline;">**Mailbox Corruptions**</span>

Mailbox corruptions can cause a Mailbox Move to fail. I’ve seen mostly messages in a corrupted state. If acceptable to your organization you can use the *BadItemLimit* parameter of the New-MoveRequest cmdlet to skip these message. I recommend a small number, eg 10.

The other corruption cases I’ve seen were corruption in Out of Office message or the Outlook Rules.

In the log file the Move operation fails on the IDestinationFolder.SetRules operation:  
“&gt;  
Both Outlook Rules and Out Of Office Settings are stored in the Associated Contents Folder of the Inbox and can be fixed with a MAPI tool, MFCMapi or OutlookSpy.

I used OutlookSpy, opened the mailbox and clicked the Inbox and then IMAPIFolder:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image8.png)

Select the Associated Contents Tab and inspect the Rows. If the PR\_MESSAGE\_CLASS property has the value “IPM.Note.Rules.OofTemplate.Microsoft” then you have found the Out of Office Object:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image9.png)

Outlook Rules have a PR\_MESSAGE\_CLASS of “IPM.Rule.Message”:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image10.png)

Both can be deleted via the Delete button, but before you do that you may want to backup the Outlook rules:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image11.png)

That’s it for now, I will continue in [Part 2](http://192.168.40.25:8081/2011/09/27/exchange-move-mailbox-experiences-part-2/) with more info.