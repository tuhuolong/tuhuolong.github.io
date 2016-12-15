---
layout: post
title: "arm-eabi-gcc: error trying to exec 'cc1': execvp: No such file or directory"
date: 2011-05-11 10:58:00 
comments: true
categories: [file]
tags: [file]
description: "arm-eabi-gcc: error trying to exec 'cc1': execvp: No such file or directory"
keywords: file
---


 
  arm-eabi-gcc: error trying to exec 'cc1': execvp: No such file or directory
 
 
  1、确认CROSS－COMPILE的目录在PATH里面
 
 
  2、确认cc1是存在的
 
 
  3、确认交叉编译工具的权限
 
 
  find prebuild -name cc1
 


