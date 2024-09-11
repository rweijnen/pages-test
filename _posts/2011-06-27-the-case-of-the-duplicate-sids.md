---
id: 1932
title: 'The case of the duplicate SID&rsquo;s'
date: '2011-06-27T10:04:01+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1932'
permalink: /2011/06/27/the-case-of-the-duplicate-sids/
short-url:
    - 'http://bit.ly/if7v6e'
views:
    - '6858'
categories:
    - 'Active Directory'
    - Exchange
tags:
    - 'Active Directory'
    - Exchange
    - NTDSUtil
---

[![SNAGHTML1ca684c](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png?9d7bd4 "SNAGHTML1ca684c")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png?9d7bd4)

I encountered another interesting error during Exchange 2010 installation today. During the Organization Preparation I got the following error:

[![The requested object has a non-unique identifier and cannot be retrieved.Active directory response: 0000219D: SvcErr: DSID-031A0FC0, problem 5003 (WILL_NOT_PERFORM), data 0](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb25.png "Exchange Server 2010 Setup")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image25.png)

The setup.log doesn’t give us much more detailed info:  
“&gt;  
I remembered from a [Tweet by Helge Klein](http://twitter.com/#!/HelgeKlein/status/79641965238562817) recently that the Active Directory schema has no mechanism for enforcing uniqueness of an attribute.

My first assumption was a duplicate [sAMAccountName](http://msdn.microsoft.com/en-us/library/ms679635(v=vs.85).aspx) but I couldn’t find any evidence for that.

Then I checked for duplicate [SID](http://en.wikipedia.org/wiki/Security_Identifier)‘s, you can do this via NTDSUtil:

[![check duplicate sid](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb26.png "NTSDUtil")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image26.png)

NTDSUtil creates a log file in the current directory (dupsid.log) and indeed I was having duplicate SID’s:  
“&gt;  
What made this very interesting is that the names listed here like *Public Folder Management* and *Recipient Management* are all created during Exchange 2010 installation.

This behaviour is described in [Prepare Active Directory and Domains](http://technet.microsoft.com/en-us/library/bb125224.aspx) in a little note:  
“&gt;  
So what happened?

I did a P2V of 2 existing domain controller to test an AD upgrade in a Sandbox. After the AD upgrade I kept the sandbox and decided to test Exchange installation as well.

I first created a P2V of DC002, the least important domain controller, and put in online in the sandbox to test the P2V process.

Because the DC002 had the **RID Master FSMO role** it was possible to allocate a block (the same block) of relative ID’s in both the sandbox and the production environment.

A couple of days later I did the P2V of the other domain controller and placed it online as well. And this is where the duplicate block became a problem.

Because I am in a sandbox I could take the easy solution which is use the **cleanup duplicate sid** command in NTDSUtil:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb27.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image27.png)

<span style="color: #ff0000;">**Warning: the cleanup duplicate sid command will delete BOTH objects having the same sid!**</span>

**Sidenote**: It seems that the problem with the [otherWellKnownObjects attribute I described earlier](http://192.168.40.25:8081/2011/06/24/exchange-2010-well-known-object-entry-install-error/) was actually caused by the Exchange Setup as well!

**My recommendations for P2V of Domain Controllers**:

- P2V all Domain Controllers at the same time.
- If you cannot p2v all Domain Controllers at the same time, do not bring a copy online before you finished all P2V’s.
- Make sure the RID Master is brought online LAST