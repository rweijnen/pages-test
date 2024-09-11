---
id: 4103
title: 'Where does the Citrix Linux VDA store settings?'
date: '2017-03-23T17:02:44+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4103'
permalink: /2017/03/23/where-does-the-citrix-linux-vda-store-settings/
views:
    - '787'
categories:
    - Citrix
    - Uncategorized
tags:
    - Citrix
    - Linux
    - Postgres
    - VDA
    - XenApp
    - XenDesktop
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-21.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-21.png)I recently (well today really) started playing with the Citrix Linux VDA. I took Ubuntu to test because I happen to like Ubuntu.

I didn’t get it to work correctly right away though and during troubleshooting I wanted to know where the VDA is storing it’s settings.

I found the following file `/etc/xdl/ctx-vda.conf`with the following contents:

  
“&gt;  
I didn’t check but I am assuming that the password is random and unique per VDA…

This clearly indicates that the settings are stored in a postgres database so let’s have a little look:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-22.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-22.png)

No access with the `psql` command so let’s check the postgres configuration:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-23.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-23.png)

The user with access to all databases is postgres (the default user) so we can become this user with `su – postgres` and query the list of databases with `\\l+` and list the schema’s with `\\dn+`.

The actual data is in the schema `reg`so I also set the search path:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-24.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-24.png)

Let’s see what tables are in the database:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-25.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-25.png)

And finally let’s see what’s inside the tables with `TABLE reg.”Key”`:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-26.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-26.png)

Wow that’s registry keys, at least we now understand why the schema is named reg!

Table Properties doesn’t really contain anything interesting:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-27.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-27.png)

Table `Value` holds the interesting data:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-28.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-28.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-29.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-29.png)

So for instance `ListOfDDCs` is stored in `Values`:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-30.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-30.png)

And here an example to change the DDCs:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-31.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-31.png)

**Note: making changes is likely unsupported by Citrix.**