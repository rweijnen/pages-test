---
id: 1695
title: 'Apply IP Configuration from a Database'
date: '2011-04-29T10:27:27+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/04/29/apply-ip-configuration-from-a-database/'
permalink: /2011/04/29/apply-ip-configuration-from-a-database/
views:
    - '2021'
short-url:
    - 'http://bit.ly/j3u62s'
categories:
    - Altiris
    - script
    - 'SQL Server'
---

I am currently deploying 64 Citrix XenApp servers with Altiris. The deployment consists of an OS Image, OS Configuration and finally Citrix XenApp and Applications.

In the OS Configuration part the IP configuration needs to be applied and I decided to do this with a database.

The database consists of 2 tables; one table with the per host settings and one table with the global settings (such as DNS).

In the Altiris job both tables are read from an embedded VBScript and assigned to the NIC.

<span style="text-decoration: underline;">**Database configuration**</span>

I created a database (SQL Server) called IPManagement with 2 tables:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image7.png)

The globalconfig table has a name and a data field:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image8.png)

And contains only the DNS settings:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image9.png)

The hostconfig table has fields for hostname, ip address, netmask and default gateway:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image10.png)

Some sample data:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image11.png)

**<span style="text-decoration: underline;"> </span>**

**<span style="text-decoration: underline;">Altiris Configuration</span>**

In Altiris I have added the database as a custom data source, this can be done via Tools | Options | Custom Data Sources:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image12.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image13.png)

**<span style="text-decoration: underline;"> </span>**

**<span style="text-decoration: underline;">The Script:</span>**

The final step is the VBScript, which use the [Custom Token feature](http://www.myitforum.com/articles/5/view.asp?id=5439) to retreive the data from the database:  
“&gt;  
Note that I had to do a little trick using the Trim statement and a space to get a correct assignment to the variables. Without the space Altiris doesn’t properly recognize the custom token.

The array is necessary beasue the EnableStatic and SetGateways Methods require array parameters even when there’s just a single entry.