---
layout: post
title: android查看kernel log
date: 2015-04-19 20:39:00
categories: [Android]
tags: []
---
	root@:/ # cat /proc/kmsg
	root@:/ # dmesg
	root@:/ # cat /proc/sys/kernel/printk
	7       4       1       7
	(7=Current level, 4=Default level, 1=Minimum level, 7=Boot up level)



[android-logging-the-linux-kernel-log](http://bamboopuppy.com/android-logging-the-linux-kernel-log/)
