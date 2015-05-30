---
layout: post
title: android系统镜像:boot.img kernel.img ramdisk.img system.img userdata.img cache.img recovery.img
date: 2015-03-27 19:29:00
categories: [Android]
tags: []
---
![这里写图片描述](http://img.blog.csdn.net/20150327224027694 "")
- boot.img(kernel.img+ramdisk.img) 
- ramdisk.img(/)
- system.img(/system) 
- userdata.img(/data)
- cache.img(/cache)

- recovery.img(kernel.img+ramdisk-recovery.img)

- ramdisk-recovery.img(/)

***
`split_bootimg.pl boot.img`
`gunzip boot.gz`
`cpio -i -F boot`
