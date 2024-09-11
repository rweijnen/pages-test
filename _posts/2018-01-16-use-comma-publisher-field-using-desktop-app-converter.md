---
id: 4243
title: 'How to use a comma in the Publisher field when using the Desktop App Converter'
date: '2018-01-16T14:55:51+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4243'
permalink: /2018/01/16/use-comma-publisher-field-using-desktop-app-converter/
views:
    - '153'
categories:
    - UWP
tags:
    - 'Code Signing'
    - 'Desktop App Converter'
    - PowerShell
    - UWP
---

![DAC Icon](https://docs.microsoft.com/en-us/windows/uwp/porting/images/desktop-to-uwp/dac.png)When using the Desktop App Converter there’s no need to sign the resulting .appx packages with your own code signing certificate when you submit them to the Store.

However if you want to test the package on a different machine or distribute it to test users you may want to sign the .appx with a certificate.

One option is to use the `-sign` parameter, in this case the Desktop App Converter generates a code signing certificate and signs the package with it. Although easy to use, it’s not very convenient if you want to distribute the .appx as you need to add the certificate to the `Trusted People` certificates store (for each user). See [Run the Packaged App](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#run-app) in the documentation.

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-1.png)If you want to sign the .appx package with a trusted certificate (e.g. issued by a trusted certificate authority such as [DigiCert](https://www.digicert.com/code-signing/)) you need to make sure that you pass the subject (the CN) from your code signing certificate to the Desktop App Converter (using the `-Publisher` parameter).

In my case the CN for the certificate is [`E=support@cloudhouse.com,CN=Cloudhouse](mailto:`E=support@cloudhouse.com,CN=Cloudhouse) Technologies Limited,O=Cloudhouse Technologies Limited,L=London,S=London,C=GB` as can be seen from the following screenshot:

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-2.png)

However if we pass this value on the Desktop App Converter we get the following error message: “*Cannot validate argument on parameter ‘Publisher’. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again*“.

[![Desktop App Converter](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-3.png "Cannot validate argument on parameter 'Publisher'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-3.png)

I saw a couple of people asking about this exact issue on stackoverflow ([here](https://stackoverflow.com/questions/39828885/how-can-we-add-a-comma-in-publisher-name-in-appxmanifest-while-using-desktop-app) for example) but the suggested answer didn’t work for me.

I noticed that when I view the certificate in PowerShell that the attributes in the CN have a space after the comma:

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-4.png)

So next attempt was to insert the spaces and rerun the command but unfortunately that failed with the same error.

I then tried to call the same command from PowerShell (as the Desktop App Converter is really a PowerShell script) and t

hat worked!

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-5.png)

I also tried the CN without spaces but this continues to fail, even when launched from PowerShell:

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb-6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image-6.png)

So the working recipe is launch from PowerShell and seperate the attributes in the CN with a space, followed by a comma!