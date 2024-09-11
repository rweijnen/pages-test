---
id: 1175
title: 'Self Signing Word Macro&rsquo;s'
date: '2011-01-12T15:45:51+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1175'
permalink: /2011/01/12/self-signing-word-macros/
views:
    - '7877'
short-url:
    - 'http://bit.ly/dEtpUt'
categories:
    - Citrix
    - General
    - 'Terminal Server'
---

Today I noticed that a recently added Application to the Citrix Test environment added a Macro to the Office Startup directory.

When a user launches Word he will get a popup because the Template (.dot file) was not signed:

[![OfficeMacro](http://192.168.40.25:8081/wp-content/uploads/2011/01/OfficeMacro_thumb.png "OfficeMacro")](http://192.168.40.25:8081/wp-content/uploads/2011/01/OfficeMacro.png)

It would have been a lot easier if Application Vendors sign their stuff because in that case I could have just added the certificate using Group Policy ([yesterday’s post](/blog/2011/01/11/java-webapplication-certificates-and-citrix/) describes how to do this).

Application Vendors usually tell you that you should lower the Macro security in Office (or Word in this case) to Low to get rid of this message. But I think there’s a better solution: we will sign the .dot file ourselves!

If you have a CA server or a publicly trusted certificate that you can use, the procedure is very simple. But today I will describe a solution using a Self Signed certificate.

First we will create the certificate: go to the Office Tools folder in your Start Menu and choose Digital Certificate for VBA Projects.

Give the certificate a name such as the name of your company and press OK:

[![CreateCertificate](http://192.168.40.25:8081/wp-content/uploads/2011/01/CreateCertificate_thumb.png "CreateCertificate")](http://192.168.40.25:8081/wp-content/uploads/2011/01/CreateCertificate.png)

Now Open the .dot file in Word and go to the Visual Basic Editor (Alt-F11), locate the .dot file in the Project Explorer and select it.

Select Digital Signature from the Tools menu and click Choose:

[![DigitalSignature](http://192.168.40.25:8081/wp-content/uploads/2011/01/DigitalSignature_thumb.png "DigitalSignature")](http://192.168.40.25:8081/wp-content/uploads/2011/01/DigitalSignature.png)

Now select the Certificate you created earlier:

[![SelectCertificate](http://192.168.40.25:8081/wp-content/uploads/2011/01/SelectCertificate_thumb.png "SelectCertificate")](http://192.168.40.25:8081/wp-content/uploads/2011/01/SelectCertificate.png)

Press OK on the Next Dialog:

[![DigitalSignature2](http://192.168.40.25:8081/wp-content/uploads/2011/01/DigitalSignature2_thumb.png "DigitalSignature2")](http://192.168.40.25:8081/wp-content/uploads/2011/01/DigitalSignature2.png)

And of course don’t forget to SAVE the .dot file.

Now we are going to Export the certificate to a file using the mmc certificates plugin. Start mmc.exe and add the Certificates plugin for the Current User.

[![Certificates](http://192.168.40.25:8081/wp-content/uploads/2011/01/Certificates_thumb.png "Certificates")](http://192.168.40.25:8081/wp-content/uploads/2011/01/Certificates.png)

Right Click the Certificate and choose All Tasks | Export and save the Certificate to a file.

The last step is to deploy the certificate using a Group Policy to both the Trusted Root Certification Authorities and the Trusted Publishers Store:

[![GPOCertificate](http://192.168.40.25:8081/wp-content/uploads/2011/01/GPOCertificate_thumb.png "GPOCertificate")](http://192.168.40.25:8081/wp-content/uploads/2011/01/GPOCertificate.png)

And now the user is no longer bugged with a popup while you can still maintain the Medium or even High Macro Security Level in Office. So I think this is a better solution, even if you don’t have you own CA server or (expensive) Public Certificate.

For a more detailed explanation of deploying Certificates using a Group Policy read [yesterday’s post](/blog/2011/01/11/java-webapplication-certificates-and-citrix/).