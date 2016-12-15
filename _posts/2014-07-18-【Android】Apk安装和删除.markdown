---
layout: post
title: "【Android】Apk安装和删除"
date: 2014-07-18 20:03:00 
comments: true
categories: []
tags: []
description: "【Android】Apk安装和删除"
keywords: 
---


 
  
   (1)取基本信息
  
 
 
  packageName
 
 
  versionCode
 
 
  installLocation
 
 
  
   (2)拷文件
  
 
 
  /data/app            (apk)
 
 
  /data/app-lib       (so)
 
 
  
   (3)解析
  
  AndroidManifest.xml
 
 
  
   (4)设置
  
 
 
  /data/data/……
 
 
  /data/data/lib ->; /data/app-lib/……
 
 
  /data/dalvik-cache       (dex)
 
 
  
   (5)注册
  
 
 
  -------------------------------------------------------------
 
 
  
   (1)杀掉应用 killApp
  
 
 
  
   (2)注销
  
 
 
  
   (3)删data
  
 
 
  /data/data/......
 
 
  
   (4)删文件
  
 
 
  /data/app                   (apk)
 
 
  /data/app-lib             (so)
 
 
  /data/dalvik-cache  (dex)
 
 
 
 
 
 
 
 
  PackageInfoLite 基本信息
 
 
  adb install ->;
 
 
  exec app_process $base/bin com.android.commands.pm.Pm "$@"
  
  
 
 
 
 
  
  
 


