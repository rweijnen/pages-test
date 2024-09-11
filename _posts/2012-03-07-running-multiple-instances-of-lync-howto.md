---
id: 2513
title: 'Running multiple instances of Lync (howto)'
date: '2012-03-07T22:55:37+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/07/running-multiple-instances-of-lync-howto/'
permalink: /2012/03/07/running-multiple-instances-of-lync-howto/
views:
    - '29900'
short-url:
    - 'http://bit.ly/yATioL'
categories:
    - Lync
    - ThinApp
tags:
    - Hack
    - Lync
    - ThinApp
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image4.png)Yesterday I showed a [video](http://192.168.40.25:8081/2012/03/06/run-multiple-instances-of-lync-2010poc/) demonstrating it’s possible to run multiple instances of the Microsoft Lync 2010 client simultaneously.

A little warning before we go on: the Lync Client was not designed to run with multiple instances. Or better said: it was designed specifically to prevent this, let’s see how it does this:

On startup Lync calls an internal function called COcAppNoUI::InitializeMainInstance. In this function it creates a [Mutex](http://msdn.microsoft.com/en-us/library/windows/desktop/ms684266(v=vs.85).aspx) named “Office Communicator\_” in the Global [namespace](http://192.168.40.25:8081/2009/01/27/accessing-kernel-objects-in-other-sessions/). It also creates an [Event](http://msdn.microsoft.com/en-us/library/windows/desktop/ms682655(v=vs.85).aspx) in the Global namespace called “COMMUNICATOR-“.

When a second instance of Lync is launched it checks if the Global Mutex exists and if it does it fires the Global Event. The Main instance has a thread that waits for this event using the [WaitForMultipleObjects](http://msdn.microsoft.com/en-us/library/windows/desktop/ms687025(v=vs.85).aspx) API.

Let’s check this using Process Explorer, select the Lync process (communicator.exe) and display the process handles in the lower pane view:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image5.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image6.png)

If we close the handles to the Mutex and the Event (which is NOT recommended) we are able to launch a second instance of Lync.

But to get a fully working Lync Client we there a few more things we need to take care of:

**Shared Memory Section**   
Lync creates a shared memory section named “MicrosoftOfficeCommunicatorSharedMemoryAccess”. Since we do not know what kind of data is exchanged in this shared section we must ensure that each instance has it’s own sections.

**Additional Mutexes**   
Lync creates several additional Mutexes that need to be unique as well. Unfortunately the Mutex names are composed in a special way;

The first Mutex is named “&lt;guid&gt; – SID” eg: “eed3bd3a-a1ad-4e99-987b-d7cb3fcfa7f0 – S-1-5-21-1792247254-158287795-3068212004-1000”. The guid is obtained by calling the [GetUserNameEx](http://msdn.microsoft.com/en-us/library/windows/desktop/ms724435(v=vs.85).aspx) API with the [NameUniqueId](http://msdn.microsoft.com/en-us/library/windows/desktop/ms724268(v=vs.85).aspx) parameter. The SID is of course the SID of the logged-on user.

The second Mutex is named “Communicator.&lt;#&gt;\_&lt;SID&gt;” eg: “Communicator.0\_S-1-5-21-1792247254-158287795-3068212004-1000”

**Registry** Lync stores it’s user settings in HKCU\\Software\\Microsoft\\Communicator. Every instance will need it’s own settings or it will user (and overwrite) the settings of other users. This includes the sign-in address and the (encrypted) password.

Although I was able to run two instances of the Lync Client locally by closing the handles I decided the easiest solution was to use Application Virtualization. In this how-to I will use ThinApp.

**ThinApp** To get the Lync Client to work with ThinApp we need to enable external out-of-process COM objects to run by adding the following line to the package.ini:

 “&gt;

Next we need to make sure that we isolate the Mutexes and Events by adding the following line to the package.ini:

“&gt;

As you can see I used wildcards to match both Local and Global objects and to match the Guid and Sid.

**ThinApp fail** ThinApp will now add the Sandbox Path to the object name, however for some reason it fails to do this for the “COMMUNICATOR-” event:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image7.png)

I tried several variation with wildcard but I didn’t get this thing to work. I presume this happens because the event name is not hardcoded but assembled from a dynamic string.

**Delphi to the rescue**

To solve this final step I wrote a small commandline tool in Delphi that searches for the Communicator.exe process and closes the handles to the “COMMUNICATOR-” event. I will go into details about the Delphi code below as it’s out of scope for this blog. If you have questions about the code please let me know.

“&gt;

I then added a vb script to the ThinApp that calls the commandline tool:

“&gt;

It’s not ideal but it works

**Registry** ThinApp will virtualize the Registry in the Sandbox by default so we don’t need to do anything there.

We can now run one instance of Lync natively and another instance ThinApped.

From my (limited) testing all functionalities seem to work including Instant Messaging and Lync Call but please do keep in mind that this method IS NOT SUPPORTED IN ANY WAY!

**Download**

I have added a download below for the package.ini I used, the KillMutex.vbs script and the CloseMutex commandline tool (including source).

[ DualLync (4043 downloads ) ](http://192.168.40.25:8081/download/duallync/?tmstv=1726048919 "Version v1.0")