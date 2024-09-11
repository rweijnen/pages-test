---
id: 3136
title: 'Java has discovered application components that could indicate a security concern'
date: '2013-03-19T13:13:04+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3136'
permalink: /2013/03/19/java-has-discovered-application-components-that-could-indicate-a-security-concern/
short-url:
    - 'http://bit.ly/YDOndS'
views:
    - '2231'
categories:
    - Java
tags:
    - Java
    - Security
---

When starting a particular web based application Java popped up the following dialog:

[![Java has discovered application components that could indicate a security concern](http://192.168.40.25:8081/wp-content/uploads/2013/03/image12_thumb.png "Warning - Security")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image1211.png)

The dialog asks us if we want to Block potentially unsafe components so to continue we should click no. However users tend to not really read such messages and click Yes which leads to this error:

[![Error connecting to Central Configuration](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb26.png "Connection Error")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image26.png)

Java is not to blame here, it’s perfectly valid to popup this message since the vendor has signed a part of it’s code but failed to sign other parts (in which case not signing at all would have been a better choice, but at least the vendor is trying).

So how do we make this message go away?

**<span style="text-decoration: underline;">Method 1: Java Control Panel</span>**  
Open the Java Control Panel and go to the Advanced Tab. Expand the Security node and set the Mixed code option to Disable verification:

[![Advanced | Security | Mixed code | Disable verification](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb27.png "Java Control Panel")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image27.png)

The disadvantage for this method is obviously that it’s a lot of work to set this manually for all users. So on to method 2…

**<span style="text-decoration: underline;">Method 2: edit deployment.properties file  
</span>**Java stores it’s configuration in the deployment.properties file which is usually located in %appdata%\\sun\\java\\deployment:

[![edit deployment.properties file](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb28.png "deployment - notepad")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image28.png)

Using a deployment tool (or even the login script) we can easily copy a modified deployment.properties file. The risk in this approach is that we overwrite the complete configuration.

A better approach is to just add the line we need with a script which could be as simple as this:  
“&gt;  
But if there already was a line for mixcode we will end up with two lines and which line will prefer?

In both approaches the file might actually be located in a different path for example in Windows 7 it’s in AppData\\LocalLow…

Let’s see if method 3 will do a better job…

**<span style="text-decoration: underline;">Method 3: change it programmatically</span>**

I figured that the best way to change Java configuration would be to use Java for it. I googled for a way to change the deployment.properties file programmatically but failed to find a good answer. I got a quick answer on [stackoverflow](http://stackoverflow.com/questions/15479368/change-deployment-properties-file-programmatically) though.

Java exploses a [Config](http://javasourcecode.org/html/open-source/jdk/jdk-6u23/com/sun/deploy/config/Config.java.html) class that does what we need so using that I created a small tool to change the Mixed code setting.

When used without arguments (or /?) the possible commandline arguments and current setting are displayed:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb29.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image29.png)

Use the Disable argument (Java SetMixedCode Disable) to disable verification:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb30.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image30.png)

Note that you should not execute the tool with the .class extension (Java SetMixedCode.class) because it will throw an exception:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb31.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image31.png)

The tool can be download at the bottom of this page and includes source code.

If you are interested in the code, here it goes but please note that I didn’t write code in Java before so suggestion to improve are welcome!  
“&gt;  
[ SetMixedCode.zip (3344 downloads ) ](http://192.168.40.25:8081/download/setmixedcode-zip/?tmstv=1726048920 "Version 1.0")