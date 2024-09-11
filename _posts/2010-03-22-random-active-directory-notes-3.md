---
id: 537
title: 'Random Active Directory Notes #3'
date: '2010-03-22T22:57:30+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=537'
permalink: /2010/03/22/random-active-directory-notes-3/
views:
    - '2672'
short-url:
    - 'http://bit.ly/fNjpDq'
categories:
    - 'Active Directory'
    - Delphi
    - Programming
---

Last time I talked briefly about IDirectoryObject and IDirectorySearch, let’s go into a little more detail today.

IDirectoryObject is an Interface that we can use to query anything in Active Directory, users, groups, organizational units, containers and so on.

I thought the best explanation would be to build a very small sample project, so let’s do that!

First we need some units, so please add the following units to your uses clause:

- ComObj (for EOleException and it calls CoInitialize for us)
- JwaWindows for the proper Adsi declarations

Next declare the following types:  
“&gt;  
Now add a Button with the name StartButton and a Memo with the name Memo1 to your form. Place the code in the OnClick handler and declare some vars:  
“&gt;  
First we will retrieve an IADs interface to a special PathName called [rootDSE](http://msdn.microsoft.com/en-us/library/ms684291%28VS.85%29.aspx). rootDSE enables us to do a serverless binding to the default domain without knowing the domain name., this is perfect for our example code since you can all use it without changing the PathName.  
“&gt;  
In real life you will probably raise an exception instead of just exiting on failure…

Now we are going to read a property of rootDSE called defaultNamingContext which contains the DNS domain name (egDC=domain,DC=local):  
“&gt;  
Notice that IADs.Get is declared as SafeCall so we need to wrap this in a try..except block, IADs.Get will return an EOleException in case of failure

The last step before we can obtain the IDirectoryObject Interface is to compose the PathName. I am going to work with the Administrator account as this account will probably exist in most domains. Don’t worry if you don’t have Admin privileges in your domain, any domain account can read properties.  
“&gt;  
To obtain the IDirectoryObject Interface we can use AdsGetObject but this time we use IID\_IDirectoryObject:  
“&gt;  
We will now read out two attributes from the Administrator account, the username (sAMAccountName) and the description (description). Note that attribute names in Active Directory do not use CamelCase like in Delphi but Hungarian notation. Confusingly enough [Microsoft](http://msdn.microsoft.com/en-us/library/ms229043.aspx) refers to CamelCase as Pascal Casing and to Hungarian Notation as Camel Casing.

Although IDirectoryObject is not case sensitive I always try to keep the same case, so I use sAMAccountName and not SamAccountName when I refer to the attribute.

We are going to use the [GetObjectAttributes](http://msdn.microsoft.com/en-us/library/aa746358%28VS.85%29.aspx) function to read Attributes values, its first parameter is an array of AttributeNames:  
“&gt;  
The second parameter is the count of attributes, followed by a pointer to an array of ADS\_ATTR\_INFO records (PAdsAttrInfoArray) and the number of attributes returned:  
“&gt;  
It is important to note some things: the function can succeed but return less values than requested and the order of attributes returned is not necessarily the same as requested!

This means you should always check the pszAttrName member of the ADS\_ATTR\_INFO record and read the data from the pADsValues member:  
“&gt;  
When we are done we need to free the memory:  
“&gt;  
That’s it for today, you can find the sample project below. Next time I will discuss IDirectorySearch.

[ AdSample1 (1579 downloads ) ](http://192.168.40.25:8081/download/adsample1/?tmstv=1726048918 "Version 1.0")