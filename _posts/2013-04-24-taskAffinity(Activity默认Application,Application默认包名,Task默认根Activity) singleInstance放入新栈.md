---
layout: post
title: "taskAffinity(Activity默认Application,Application默认包名,Task默认根Activity) singleInstance放入新栈"
date: 2013-04-24 15:19:00 
comments: true
categories: []
tags: []
description: "taskAffinity(Activity默认Application,Application默认包名,Task默认根Activity) singleInstance放入新栈"
keywords: 
---


 
  
   
    Application
   
   的taskAffinity
   
    默认
   
   值为
   
    包名
   
  
 
 
  
   
    Activity
   
   的taskAffinity
   
    默认
   
   值为
   
    application
   
   的taskAffinity值
  
 
 
  
   
    Task
   
   的taskAffinity
   
    默认
   
   值为
   
    根
   
   Activity的taskAffinity值
  
 
 
  
  
 
 
  
   singleInstance肯定会放入新栈(task)中
   
   
  
 
 
  
   FLAG_ACTIVITY_NEW_TASK将Activity放到taskAffinity指定的栈(task)中
   
   
  
 
 
  
   
   
  
 
 
  
   
    当一个应用程序加载一个singleInstance模式的Activity时，如果该Activity没有被实例化，那么就重新创建一个Task，并入栈，如果已经被实例化，那么就调用该Activity的onNewIntent
   
   
   
  
 
 
  
   
    
    
   
  
 
 
  
   
    
     singleInstance的Activity所在的Task不允许存在其他Activity，任何从该Activity加载的其它Actiivty（假设为Activity2）都会被放入其它的Task中，如果存在与Activity2相同affinity的Task，则在该Task内创建Activity2。如果不存在，则重新生成新的Task并入栈
    
    
    
   
  
 
 
  
   
    
     
     
    
   
  
 
 
  
   
    
     
      allowTaskReparenting设置为true，它进入后台，当一个和它有相同affinity的Task进入前台时，它会重新宿主，进入到该前台的task中
     
    
    
    
   
  
 


