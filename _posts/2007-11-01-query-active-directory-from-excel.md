---
id: 49
title: 'Query Active Directory from Excel'
date: '2007-11-01T18:23:55+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/11/01/query-active-directory-from-excel/'
permalink: /2007/11/01/query-active-directory-from-excel/
views:
    - '177635'
short-url:
    - 'http://bit.ly/hnyvmc'
categories:
    - 'Active Directory'
    - script
---

I had to lookup some users in Active Directory today which I received by mail. Offcourse I got full users name while I needed either samAccountName or full adsPath. Usually I write a small VBS script to do the lookup and paste this in Excel for further processing. But today I decided that an Excel function to do the lookup would be nice. So I wrote it.

The function is called GetAdsProp and allows you to search on a specific AD field in the whole AD tree and return the value of another field.

So how does it work? In this example I have full name in Cell A2, in B2 I want to lookup the Accountname and in C2 the E-Mail address.

In Cell B2 use the formula: =GetAdsprop(“cn”; A2; “samAccountName”)

In Cell B3 use the formula: =GetAdsprop(“cn”; A2; “mail”)

![GetAdsProp Screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/11/getadspropscreenshot.png)

This is the code:  
“&gt;  
That’s nice, but want if we want to perform some action on the looked up results? In my case I needed to move the users to another OU. So I made a function for that too. It’s called MoveADObject.  
“&gt;  
To move an object (eg a user) in Active Directory you need it’s full path which is something like (“CN=User,OU=MyOU,DC=MyDomain,DC=local”). Every object in Active Directory has this stored in a propery adsPath. So we use the GetAdsProp function to do a lookup. Put this formula in Cell B4: =GetAdsProp(“cn”;A2;”AdsPath’)

![GetAdsProp Screenshot #2](http://192.168.40.25:8081/wp-content/uploads/2007/11/getadspropscreenshot2.png)

Now use the following formula in Cell D2: =MoveADObject(C2;”OU=Admin Users,OU=Users,OU=Netherlands,DC=europe,dc=unity”)

The function asks for your confirmation first:

![MoveADObject Screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/11/moveadobjectscreenshot.png)

And the the object is moved. After the moving the value of the Cell will be the *new adsPath*.

I added both functions to an Excel XLA Addin which you can add in your Addins directory, then it will be available in all you Excel sheets. (The Addins directory is usuall in %userprofile%\\Application Data\\Microsoft\\Addins)

[ RWADAddin.zip (19781 downloads ) ](http://192.168.40.25:8081/download/rwadaddin-zip/?tmstv=1726048918 "Version 1.0")