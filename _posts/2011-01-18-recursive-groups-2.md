---
id: 1234
title: 'Recursive Groups #2'
date: '2011-01-18T19:51:15+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1234'
permalink: /2011/01/18/recursive-groups-2/
views:
    - '2058'
short-url:
    - 'http://bit.ly/hBw7LV'
categories:
    - 'Active Directory'
    - PowerShell
---

In my [previous post](http://192.168.40.25:8081/2011/01/18/recursive-group-membership-in-powershell/) I explained how to get the recursive group membership with a very simple Powershell Script.

Commenter Michel thought that the script only tested one level deep but it doesn’t.

But let’s prove that!

Create 3 Global Groups in your Active Directory and name them Level1, 2 and 3:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image2.png)

Make Level3 a Member of Level 2 and make Level a member of Level 1 and finally add an account to the Level 3 group:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image3.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image4.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image5.png)

Now run the script and the output should include all 3 groups:  
“&gt;  
And the Output:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image6.png)