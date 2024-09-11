---
id: 1147
title: 'Java Webapplication, certificates and Citrix'
date: '2011-01-11T15:23:47+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1147'
permalink: /2011/01/11/java-webapplication-certificates-and-citrix/
short-url:
    - 'http://bit.ly/gT1JdB'
views:
    - '10461'
categories:
    - Citrix
    - Java
    - Packaging
    - 'Terminal Server'
    - 'Unattended Installation'
---

Yesterday I created an Unattended Installation of a webapplication. Of course it was “just a web link” and the application vendor usually says: you don’t need to install it just go the URL and that’s it.

The reality is usually that you go to the URL and need to install several (ActiveX) components and maybe other dependencies such as Java.

While a user may have the permissions for this on his own pc, on a Citrix or Terminal Server environment this is highly unlikely.

So we need to package and pre-install this for the users.

Nothing special so far but this particular application had some special things that were interesting enough to blog about.

So let’s start with what happened, I visited the URL of an application called Centric Key 2 Financien.

First I got a few popups with Certificates that needed to be accepted:

[![Cert1](http://192.168.40.25:8081/wp-content/uploads/2011/01/Cert1_thumb.png "Cert1")](http://192.168.40.25:8081/wp-content/uploads/2011/01/Cert1.png)

The application’s instructions say that the user must accept this and set the “Always trust content from this publisher” checkbox.

If you do that a file called *trusted.certs* is generated in *%appdata%\\Sun\\Java\\Deployment\\Security*.

So I think it’s better to pre-deploy these certificates so we don’t have to bother the user with accepting this (and relieve the helpdesk because the user will probably call them).

The easiest resolution would probably be to copy this trusted.certs file to a network share and deploy it to all users (eg with the Logon Script).

But what will happen if the user already had a trusted.certs file or if we have another application that wants to place certificates there?

As you have probably guessed by now I went for the “difficult” option and found another solution!

Java comes with a tool called keytool that is Java’s bin directory (C:\\Program Files\\Java\\Jre6\\Bin in my case).

We can use keytool to list the certificates in the trusted.certs file:  
“&gt;  
When keytool asks for the Store Password just press Enter. To be able to export the certificates we need the Alias name so we can filter that out:  
“&gt;  
In my case it showed the Aliases for 2 certificates:  
“&gt;  
Now that we know the Alias we can use the -export switch to export the certificate to a .cer file:  
“&gt;  
Now we can deploy the Certificates using Group Policies, in my case I am adding the Certificates to the Citrix GPO.

Open the Policy, go to Computer Configuration | Windows Settings | Security Settings | Public Key Policies.

Now Right Click on Trusted Publishers and choose Import and import both Certificate Files:

[![GPO](http://192.168.40.25:8081/wp-content/uploads/2011/01/GPO_thumb.png "GPO")](http://192.168.40.25:8081/wp-content/uploads/2011/01/GPO.png)

You also need check if the Certificate can be resolved, in my case I needed to add the [Thawte Code Signing CA](https://search.thawte.com/support/ssl-digital-certificates/index?page=content&actp=CROSSLINK&id=AR1406) to the Intermediate Certification Authorities store.

You can check the deployment by opening the MMC Certificates Addin (for Current User) as a user that has the policy applied:

[![CertAsUser](http://192.168.40.25:8081/wp-content/uploads/2011/01/CertAsUser_thumb.png "CertAsUser")](http://192.168.40.25:8081/wp-content/uploads/2011/01/CertAsUser.png)

Double Click the Certificate and check if the Certificate status is OK and the Certification Path resolves:

[![CertAsUserCheck](http://192.168.40.25:8081/wp-content/uploads/2011/01/CertAsUserCheck_thumb.png "CertAsUserCheck")](http://192.168.40.25:8081/wp-content/uploads/2011/01/CertAsUserCheck.png)

The last step is to the application, close any open browsers, *delete the existing trusted.certs* file and op the URL again.

If all is OK, the application should open without any Certificate Popups:

[![Key2Financien](http://192.168.40.25:8081/wp-content/uploads/2011/01/Key2Financien_thumb.png "Key2Financien")](http://192.168.40.25:8081/wp-content/uploads/2011/01/Key2Financien.png)

Now close the Application and the Browser and confirm that the trusted.certs file was not created again.

I hope you enjoyed this article, please leave some comments if you found it useful.

References:

- [Deploy Certificates by Using Group Policy](http://technet.microsoft.com/en-us/library/cc770315(WS.10).aspx)