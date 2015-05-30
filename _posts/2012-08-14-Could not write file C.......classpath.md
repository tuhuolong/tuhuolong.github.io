---
layout: post
title: Could not write file: C:\......\.classpath
date: 2012-08-14 20:33:00
categories: [零碎]
tags: [file, c, windows, build, linux, path]
---

	'Setting build path' has encountered a problem.Could not write file:C:\XXXXXXX\.classpath

The cause was copying a workspace from Linux to Windows. The windows
 hidden attribute is set on the .classpath file. Removing this attribute fixes the error.


右键点击.classpath文件，取消“隐藏”








