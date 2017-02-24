---
title: VS2013 kills IIS Express when stopping debugging
date: 2013-11-30
tags: visual studio
---
When developing web applications with Visual Studio 2013 I found it very annoying that the IIS Express gets killed as soon debugging is stopped.

The reason for this seems to be `Edit and Continue`, which is enabled by default.

![](/images/vs2013_editcontinue_PNG.png)

After disabling this feature IIS Express won't stop :)
