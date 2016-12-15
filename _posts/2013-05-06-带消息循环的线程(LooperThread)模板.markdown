---
layout: post
title: "带消息循环的线程(LooperThread)模板"
date: 2013-05-06 13:50:00 
comments: true
categories: []
tags: []
description: "带消息循环的线程(LooperThread)模板"
keywords: 
---


 
  class LooperThread extends Thread {
  
   public Handler mHandler;
   
    
     public void run() {
     
      Looper.prepare();
      
       
        mHandler = new Handler() {
        
         public void handleMessage(Message msg) {
         
          // process incoming messages here
          
           }
           
            };
            
             
              Looper.loop();
              
               }
               
                }
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


