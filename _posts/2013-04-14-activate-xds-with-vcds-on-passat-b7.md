---
id: 3210
title: 'Activate XDS with VCDS on Passat B7'
date: '2013-04-14T14:47:02+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3210'
permalink: /2013/04/14/activate-xds-with-vcds-on-passat-b7/
views:
    - '12909'
short-url:
    - 'http://bit.ly/YUKcPH'
categories:
    - Automotive
tags:
    - Passat
    - VCDS
    - XDS
---

![XDS](http://www.volkswagen.co.uk/assets/content/technology/hero_xds.jpg)XDS is an electronic differential lock, which was developed for the Golf GTI. But later on it was introduced as an option for other cars.

XDS is an advanced electronic differential lock, linked to the ESP system.

In moments of fast cornering XDS gives out exactly the right amount of power, providing pressure on the inside wheel to prevent wheel spinning. The result is better traction and a reduction of any tendency to under steer.

**<u>Golf mk6   
</u>**On the Golf mk 6 it’s possible to activate XDS with VCDS by going to control module 03, ABS Brakes, then 10, Adaptation.

Channel 36 controls XDS which can be set to:

0 – default   
1 – weak   
2 – strong

**<u>Skoda Octavia II</u>**   
It’s also possible on other cars, for instance the Skoda Octavia II where it can be coded in module 03, ABS Brakes, 07 Long Coding, Byte 17, Bit 3.

**<u>Passat B7</u>**   
I wanted to know how to activate this on a Passat B7, but searching with google returned nothing.

I found a copy of VAS PC, the software that VW uses, and browsing through it’s files I found an interesting file named "sys45\_\_\_\_\_\_\_\_1\_1010\_11\_zuweisung\_xds".

Inside the "Zuweisung XDS" file was the following interesting piece:

 “&gt;

*Zuweisung* means "to grant" in English.

RDW stands for *Rest der Welt, "*Rest of the World" in English.

The rest is pretty obvious, for the Passat we can enable XDS by setting Adaptation channel 67 value to 1.

The file mentions the following VW ABS Module numbers: 3AA614109H, 3AA614109J, 3AA614109K, 3AA614109L, 3AA614109AB, 3AA614109AC, 3AA614109AD,3AA614109AE.

I don’t know to which vehicles those part numbers match but since the Passat B7 shares it’s control modules with the Passat CC I presume it will work on the CC as well.

It’s also possible that the Passat B6 is included but I can’t confirm this.

**<u>Tiguan</u>**

The Tiguan also appears in this file as *5N\_Tiguan\_2008\_RDW* and *5N\_Tiguan\_2008\_Usa\_Cdn* where the adaptation channel 67 is set to the value 2. The default (for other models) is channel 36 which corresponds to the known adaptation channel for the Golf mk 6.

**<u>Touran</u>**

The Touran is also mentioned as 1T\_Touran\_2003\_RDW and 1T\_Touran\_2011\_RDW but it’s not clear (to me) what the correct value should be because the script seems to popup a dialog. Based on the input in the popup the value is set to 0 or 1.

I passed all of this information to [Ross-Tech](http://www.ross-tech.com/index.html) a while ago and it seems like it was processed in the latest [VCDS Beta](http://www.ross-tech.com/vcds/download/beta/current.html), version 12.10.4. Thumbs up to Ross-Tech!

So let’s test this on a Passat B7!

Go to module 03-ABS Brakes:

[![SNAGHTMLb16e9ac](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb16e9ac_thumb.png "SNAGHTMLb16e9ac")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb16e9ac.png)

Go to Adaptation – 10:

[![SNAGHTMLb17f936](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb17f936_thumb.png "SNAGHTMLb17f936")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb17f936.png)

Select Electronic Differential Lock (XDS) and set it to 1:

[![SNAGHTMLb1893d0](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb1893d0_thumb.png "SNAGHTMLb1893d0")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb1893d0.png)

Or if you use an earlier version of VCDS, enter 67 in the Channel box and click Read:

[![SNAGHTMLb1ae52a](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb1ae52a_thumb.png "SNAGHTMLb1ae52a")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb1ae52a.png)

Set the New value to 1 and click Test followed by Save:

[![SNAGHTMLb1cec8a](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb1cec8a_thumb.png "SNAGHTMLb1cec8a")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb1cec8a.png)