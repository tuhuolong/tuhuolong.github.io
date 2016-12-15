---
layout: post
title: "Widget创建过程(将RemoteViews发给WidgetHost)"
date: 2013-05-27 18:38:00 
comments: true
categories: []
tags: []
description: "Widget创建过程(将RemoteViews发给WidgetHost)"
keywords: 
---


 
  
   
    创建Widget过程
   
  
 
 
  
   
    
     
     
    
   
  
 
 
  
   
    （1）
    
     分配id
    
    （用HostId请求分配WidgetId：WidgetHost.allocateAppWidgetId：由WidgetService分配一个WidgetID）
   
  
 
 
  
   
    WidgetService端：Host列表+WidgetId列表
   
  
 
 
  
   
    （2）
    
     绑定id
    
    （将WidgetId绑定到Provider）
   
  
 
 
  
   WidgetManager.bindAppWidgetId：将WidgetID绑定到WidgetProvider
  
 
 
  
   将WidgetID放入相应WidgetProvider的ID列表
  
 
 
  
   
    发送
   
   Enable(第一次)+Update
   
    广播
   
   （
   
    Service
   
   
    发送广播
   
   
    请求RemoteViews）
   
  
 
 
  
   
    
    
    WidgetService.sendEnableIntent 发送Enable广播
   
  
 
 
  
   
    
    
    WidgetService.sendUpdateIntent 发送Update广播(WidgetIDs)
   
  
 
 
  
   
    （3）
    
     Provider发送RemoteViews
    
    （接受广播，创建RemoteViews，再发给Service）
   
  
 
 
  
   
    WidgetProvider接受广播，回调onEnable，onUpdate
   
  
 
 
  
   
    创建RemoteViews（每一个WidgetID创建一个）
   
  
 
 
  
   
    WidgetManager.updateAppWidget 将RemoteViews发给WidgetService
   
  
 
 
  
   
    （4）
    
     Service
    
    通知Host
   
  
 
 
  
   
    
     WidgetService通过IBinder回调WidgetHost的Callbacks（updateAppWidget，providerChanged，viewDataChanged）
    
   
  
 
 
  
   
    
     （5）
     
      添加到桌面
     
     （Host创建View，刷新）
    
   
  
 
 
  
   
    
     WidgetHost主线程调用updateAppWidgetView
    
   
  
 
 
  
   
    
     WidgetHost根据WidgetID取到WidgetHostView,再根据RemoteViews创建View添加到WidgetHostView
    
   
  
 
 
  
  
 


