---
id: 2106
title: 'Exchange Move Mailbox Experiences Part 2'
date: '2011-09-27T15:07:15+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/09/27/exchange-move-mailbox-experiences-part-2/'
permalink: /2011/09/27/exchange-move-mailbox-experiences-part-2/
views:
    - '3639'
short-url:
    - 'http://bit.ly/pJlEdh'
categories:
    - Exchange
tags:
    - Exchange
    - Exchange2010
    - OutlookSpy
---

In [part 1](http://192.168.40.25:8081/2011/09/21/exchange-move-mailbox-experiences-part-1/) we saw that a corrupted rule made the Mailbox Move fail.

I wanted to know if I had really a corrupted mailbox or maybe even corruption in the store or another problem.

So in this part I will describe how to break down the Mailbox Move Log.

First go to the Failed Move Request and select Properties:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image12.png)

Then click *Log -&gt; View* and scroll down to Error context which is like a stack, the item on top happened last:

 “&gt;

The lines below indicate in which folder it happened (*Backup Mail John Doe/Postvak IN*). This folder seems to be a backup of another mailbox that was placed in this folder:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image13.png)

That’s why this folder also has an Inbox (Postvak IN in Dutch) which may have Rules and Out Of Office objects.

Now we open this Folder in Outlook Spy by selecting it and pressing the IMAPIFolder button. Then select the PR\_RULES\_TABLE tab:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image14.png)

In my case there was only one row and the PR\_RULE\_PROVIDER property indicates that it’s an Out Of Office rule (value MSFT:TDX OOF Rules)

Before we go it’s important to understand some details of the internal implementation of Out Of Office Messages.

So let’s first look at a Mailbox with a working and correct Out Of Office notice.

I created a test mailbox and set an OOF message (not that we just need to set the text we don’t need to set the account as currently Out of Office):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image15.png)

Then I opened this OOF rule with OutlookSpy via Inbox -&gt; IMAPIFolder -&gt; PR\_RULES\_TABLE tab.

Doubleclick PR\_RULE\_ACTIONS, copy the value of lpAction.lpEntryId to the clipboard and click Close:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image16.png)

Now select the OpenEntry tab and click Enter Entry ID Manually:

Paste the value of lpAction.lpEntryId from the clipboard and press Ok:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image17.png)

Press OK on the next screen:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb18.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image18.png)

Now we see the actual Out Of Office Text:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image19.png)

So the Out Of Office Text is actually stored inside a hidden message. Now that we know this let’s proceed with the erroneous Out Of Office rule.

Go back to mailbox with the error and open the folder that contains the corrupted OOF rule (*Backup Mail John Doe/Postvak IN).*

Goto the PR\_RULES\_TAB (via IMAPIFolder), select the rule and inspect PR\_RULE\_ACTIONS.

Copy again the value of lpAction.lpEntryID to the clipboard and click Close:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image20.png)

As before, click the OpenEntry Tab followed by Enter Entry ID Manually. Now paste the value of lpAction.lpEntryID and press Ok:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb21.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image21.png)

Click OK on the next Dialog:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb22.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image22.png)

In my case I got the following error:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb23.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image23.png)

<font color="#ff0000">**And this is the explanation of the Move Mailbox Error: The Out Of Office Message (the one that contains the actual text) no longer exists**</font>.

And the Entry ID that we pasted is equal to the one from the Move Mailbox log file (after data=):

“&gt;