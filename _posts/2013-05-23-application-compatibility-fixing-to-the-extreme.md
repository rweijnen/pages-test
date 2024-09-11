---
id: 3256
title: 'Application Compatibility Fixing to the Extreme?'
date: '2013-05-23T16:48:36+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3256'
permalink: /2013/05/23/application-compatibility-fixing-to-the-extreme/
short-url:
    - 'http://bit.ly/14CntZC'
views:
    - '1065'
categories:
    - Citrix
    - Packaging
---

Today’s blog is about an application that was migrated to Citrix XenApp. During testing the users reported that several application menu’s were missing.

An example is the settings menu where the System tab is missing:

| **Fat Client:** | **XenApp:** |
|---|---|
| [![clip_image002[5]](http://192.168.40.25:8081/wp-content/uploads/2013/05/clip_image0025_thumb.jpg "clip_image002[5]")](http://192.168.40.25:8081/wp-content/uploads/2013/05/clip_image0025.jpg) | [![clip_image002](http://192.168.40.25:8081/wp-content/uploads/2013/05/clip_image002_thumb.jpg "clip_image002")](http://192.168.40.25:8081/wp-content/uploads/2013/05/clip_image002.jpg) |

I suspected a permissions issue so I added the account to the Local Administrator group to verify that. And indeed the System tab was visible.

**<u>Process Monitor</u>**   
I removed the account from the Administrators group and fired up [Process Monitor](http://technet.microsoft.com/en-us/sysinternals/bb896645.aspx). I set a filter on the process name (ra60.exe) and on Result (ACCESS DENIED):

[![SNAGHTML1b3aa033](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML1b3aa033_thumb.png "SNAGHTML1b3aa033")](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML1b3aa033.png)

The Process Monitor trace shows that the application tries to register an ActiveX component at launch:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image.png)

I don’t know why so many developers seem to think that’s a good idea, components should be registered at install time and only at install time!

Even though the failure to register the component is not blocking, it’s a good idea to fix this issue by either changing this registry keys permissions or by adding this key to HKCU\\Software\\Classes (HKCR is a merged view on HKLM and HKCU Classes key).

Applications tend to check permissions in two ways:

1. <font color="#35383d">Try to write in the filesystem (eg Program Files) or registry (eg HKLM) to see if that succeeds.</font>
2. <font color="#35383d">Do a hardcoded "IsAdministrator" check</font>

The Process Monitor trace does not show any trace of writing to Program Files or HKLM (other than the ActiveX component) so a hardcoded check is most likely the case here.

**<u>Ida Pro</u>**   
A hardcoded check usually works by calling the [GetTokenInformation](http://msdn.microsoft.com/en-us/library/windows/desktop/aa446671%28v=vs.85%29.aspx) API to inspect the current process token.

I loaded ra60.exe in [Ida Pro](https://www.hex-rays.com/products/ida/index.shtml) and waited for the autoanalysis to finish. Then I went to the Imports tab and searched for the GetTokenInformation API then pressed Ctrl-X so search for code that calls into this API:

[![SNAGHTML1edc8891](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML1edc8891_thumb.png "SNAGHTML1edc8891")](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML1edc8891.png)

The first occurrence is a direct hit, let’s have a look at the pseudo code:

 “&gt;

Let’s work on that code to make a little better readable:

“&gt;

The call to AllocateAndInitializeSid constructs the SID of the Administrators group. Then the CheckTokenMembership API is called to check if the current thread’s token contains the Administrators group SID. If it does the function returns 1 (TRUE), if not it returns 0 (FALSE).

**<u>Bug bug bug</u>**

Besides the fact that the developer who thought it was a good idea to include an admin check should be shot, this code is broken! On Vista and higher versions of Windows because the Admin token is filtered out by User Account Control (unless we are elevated).

**<u>Patch</u>**

I decided to patch this function and make it always return TRUE (1), this is actually quite easy.

Go to the Disassembly view (IDA View-A) and position the cursor on the first line of code in the function:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image1.png)

Go to the Edit Menu and choose the Assemble option from the Patch Program menu and enter the following code:

“&gt;

The first line xor’s eax with itself, the result is that the eax register contains the value 0.

The second line, inc eax, increments the eax register value by one. This makes eax contain the value 1 (TRUE).

The last line, ret, returns from the function.

This is the end result:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image2.png)

Finally use the Apply patches to input file option from the Edit | Patch Program menu to save our changes back to disk (don’t forget to select the Create backup option:

[![SNAGHTML1f3afcc9](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML1f3afcc9_thumb.png "SNAGHTML1f3afcc9")](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML1f3afcc9.png)

I started the patched executable and guess what? The missing menu is back!

**<u>Discussion</u>**

Question is though, should we go this far to get an application to run? Obviously it would be better if the vendor would fix this issue and in all honesty I didn’t ask…

It’s also possible that my customer is using an older version of the application (for whatever reason).

So let’s hear it, I am very much interested in your opinion! Is this solution acceptable for you? Or would you rather make those users Administrator?