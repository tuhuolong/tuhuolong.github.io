---
layout: post
title: "java线程池(ThreadPool)"
date: 2013-02-20 10:14:00 
comments: true
categories: []
tags: []
description: "java线程池(ThreadPool)"
keywords: 
---


 
  (1)自定义
 
 
  
   
    
     ThreadPoolExecutor(int corePoolSize,
    
   
  
  
   
    
     int maximumPoolSize,
    
   
  
  
   
    
     long keepAliveTime,
    
   
  
  
   
    
     TimeUnit unit,
    
   
  
  
   
    
     BlockingQueue<Runnable>; workQueue,
    
   
  
  
   
    
     RejectedExecutionHandler handler)
    
   
  
  
   
    
     (2)工厂类
    
   
  
  
   
    
     
      Executors.newCachedThreadPool()
     
     
     
    
   
  
  
   
    
     
      
       Executors.newFixedThreadPool(int)
      
      
      
     
    
   
  
  
   
    
     
      
       
        Executors.newSingleThreadExecutor()
       
       
       
      
     
    
   
  
 


