---
layout: post
title: "Baidu_Location_SDK"
date: 2013-04-26 14:44:00 
comments: true
categories: []
tags: []
description: "Baidu_Location_SDK"
keywords: 
---


 
  
   (1)启动注册
   
    GPS+WiFi+基站
   
   服务
  
 
 
  
  
 
 
  
   GPS:
  
 
 
  
   WiFi:SCAN_RESULTS_AVAILABLE_ACTION
  
 
 
  
   基站:LISTEN_SIGNAL_STRENGTHS+LISTEN_CELL_LOCATION
  
 
 
  
   (2)请求定位获取
   
    GPS+WiFi+基站
   
   信息
  
 
 
  
   GPS:转换成BDLocation直接返回
  
 
 
  
   网络定位:将所有定位信息上传到服务端处理，转换成BDLocation返回
  
 
 
  
   
   
  
 


