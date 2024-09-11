---
id: 2955
title: 'Change Tab Order in an Executable'
date: '2013-01-02T15:03:14+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2955'
permalink: /2013/01/02/change-tab-order-in-an-executable/
short-url:
    - 'http://bit.ly/133Utcb'
views:
    - '1090'
categories:
    - 'Application Compatibility'
tags:
    - Citrix
    - VB
    - 'Visual Basic'
    - XenApp
---

An application called Cardiology PACS was recently packaged for a Citrix XenApp environment. The functional tester reported a strange problem at the logon screen: after entering the username it was not possible to go to the password field with the TAB key.

This was a strange observation since I cannot imagine XenApp interfering with tab stops. So what was going on?

In the old situation the user was starting the application on his local pc. The application remembered the last username and pre-filled this, therefore the cursor was already in the Password field. The user simply entered his password and hit the Enter key:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image.png)

On XenApp the Username field is not pre-filled because the last username is kept globally per machine. Therefore the user has to enter both the username and the password:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image1.png)

I tested the Tab key behavior in both situations and as I expected it didn’t work in both situations. This happens because the Tab Order has been messed up by the programmer (if you press Tab 9 times you do end up in the Username field).

Because this is something that would annoy me if I were the user I decided to fix it.

<span style="text-decoration: underline;">**Identify Compiler**</span>

I first identified the compiler using a tool called [PEiD](http://www.softpedia.com/get/Programming/Packers-Crypters-Protectors/PEiD-updated.shtml) which identified “Microsoft Visual Basic”, my favorite Crapplication Creator:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image2.png)

<span style="text-decoration: underline;">**Modify the binary**</span>

[Modifying VB executables](http://192.168.40.25:8081/2012/08/04/modify-vb-executable-to-force-taskbar-button/) is fairly easy so let’s do it! We need the [VB Decompiler](http://www.vb-decompiler.org/) tool (free lite edition will do) and a HEX Editor (I use [010 Editor](http://sweetscape.com/)).

<span style="text-decoration: underline;">**Decompile**</span>

Start VB Decompiler and go to Tools | Options screen check Forms | Show Offsets and Advanced Features | Decompile only forms:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image3.png)

Then I decompiled the executable (PicomPowerClient.exe) and in the Forms node I found the form **frmServerLogon**. I opened the form and inspected it’s properties:  
“&gt;  
As you can see the Username field (Labeled **txtUsername2**) has TabIndex 21 but **txtPassword2** has TabIndex 0 (but should have 22).

Before we are going to change the TabIndex we need to verify that no other control has this TabIndex. I simply searched (Ctrl-F) for “TabIndex = 22” and the Label Label2 had it (That’s why we didn’t see it get focus):  
“&gt;  
We now have the Offsets of the controls we want to edit: **001430D7** for txtPassword2 and **001431B9** for Label2.

<span style="text-decoration: underline;">**Binary Template**</span>

To make editing a little easier I wrote a “[Binary Template](http://www.sweetscape.com/#templates)” in 010 Editor (Binary Templates is an awesome feature that makes working with Hex Data a lot easier).

This is my Template for a VB TextBox Control:  
“&gt;  
<span style="text-decoration: underline;">**Swap the Tab Indexes**</span>

Now I simply jump (Ctrl-G) to Label 2 Offset **001431B9** and Execute the Binary Template. This gives us a nice, structured view on the data:

[![SNAGHTMLb81835e](http://192.168.40.25:8081/wp-content/uploads/2013/01/SNAGHTMLb81835e_thumb.png "SNAGHTMLb81835e")](http://192.168.40.25:8081/wp-content/uploads/2013/01/SNAGHTMLb81835e.png)

Now we change the TabIndex from 22 to 0:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image4.png)

Save the changes (you did make a backup, right?), go to txtPassword2 Offset and change the TabIndex from 0 to 22:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image5.png)

<span style="text-decoration: underline;">**Fixed**</span>

Save and Launch the new executable. And now Tab nicely takes us to the Password Field:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image6.png)

Note that for similar changes to Applications written in C/C++ you can [Resource Hacker](http://www.angusj.com/resourcehacker/).