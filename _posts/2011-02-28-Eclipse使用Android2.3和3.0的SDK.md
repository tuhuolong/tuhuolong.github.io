---
layout: post
title: "Eclipse使用Android2.3和3.0的SDK"
date: 2011-02-28 15:43:00 
comments: true
categories: [eclipse]
tags: [eclipse]
description: "Eclipse使用Android2.3和3.0的SDK"
keywords: eclipse
---


 
  Android2.3(Gingerbread)和3.0(Honeycomb)
 
 
  1、Android2.3/3.0在android-sdk下创建一个platform-tools文件夹，其中包含了应该在tools文
件夹下的东西，我们只需要将platform-tools文件夹下的所有文件复制到tools目录下。
 
 
  2、然后在platforms文件夹下的android-9文件夹下创建一个tools文件夹（如果已经存在就不用新建了），然后同样将
platform-tools文件夹中的文件复制到android-9/tools文件夹下。
 
 
  3、重启Eclipse，更新ADT即可。
 


