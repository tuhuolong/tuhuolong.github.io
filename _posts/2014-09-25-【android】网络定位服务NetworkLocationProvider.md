---
layout: post
title: "【android】网络定位服务NetworkLocationProvider"
date: 2014-09-25 17:49:00 
comments: true
categories: []
tags: []
description: "【android】网络定位服务NetworkLocationProvider"
keywords: 
---


 
  framework-res.apk res\values\strings.xml
  
   <string name="config_networkLocationProviderPackageName">;com.baidu.map.location</string>;
   
   
  
 
 
  
  
 
 
  frameworks/base/core/res/res/values/config.xml
  
  
 
 
  
  
 
 
  目前的服务提供商
 
 
  com.google.android.location.network.NetworkLocationService
 
 
  com.baidu.map.location.BaiduNetworkLocationService
 
 
  com.amap.android.location.NetworkLocationService
 
 
  com.qualcomm.location.nlp.NlpProxyService
 
 
  
  
 
 
  
  
 
 
  
  
 
 
  Service Action
 
 
  <action android:name="com.google.android.location.NetworkLocationProvider" />;
 
 
  <action android:name="com.android.location.service.NetworkLocationProvider" />;
 
 
  <action android:name="com.qualcomm.services.location.xtwifi.XTWiFiLocationProvider" />;
 
 
  <action android:name="com.baidu.bms.location.BaiduNetworkLocationProvider" />;
 
 
  
   <action
  
  
   android
  
  
   :
  
  
   name
  
  
   =
  
  "com.android.location.service.v2.NetworkLocationProvider"
  
   />;
  
 
 
  
   <action
  
  
   android
  
  
   :
  
  
   name
  
  
   =
  
  "com.android.location.service.v3.NetworkLocationProvider"
  
   />;
  
 
 
  
   <action
  
  
   android
  
  
   :
  
  
   name
  
  
   =
  
  "com.qualcomm.location.service.v2.NetworkLocationProvider"
  
   />;
  
 
 
  
  
 
 
  
  
 
 
  
  
 
 
  http://www.tuicool.com/articles/aMNbAb
  
  
 
 
  
  
 


