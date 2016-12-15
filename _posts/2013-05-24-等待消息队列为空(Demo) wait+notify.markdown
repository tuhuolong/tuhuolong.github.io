---
layout: post
title: "等待消息队列为空(Demo) wait+notify"
date: 2013-05-24 10:39:00 
comments: true
categories: []
tags: []
description: "等待消息队列为空(Demo) wait+notify"
keywords: 
---


 
  void waitForIdle() {
  
   synchronized (LoaderTask.this) {
   
    
     mHandler.postIdle(new Runnable() {
     
      public void run() {
      
       synchronized (LoaderTask.this) {
       
        LoaderTask.this.notify();
        
         }
         
          }
          
           });
           
            
             
             
             try {
             
              this.wait();
              
               
               
               } catch (InterruptedException ex) {
               
                
                
                }
                
                 
                  }
                  
                   }
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


