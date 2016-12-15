---
layout: post
title: "【Android】Looper消息分发(msg.target.dispatchMessage), Handler消息处理(消息回调/外部回调/自身回调)"
date: 2014-05-03 17:16:00 
comments: true
categories: []
tags: []
description: "【Android】Looper消息分发(msg.target.dispatchMessage), Handler消息处理(消息回调/外部回调/自身回调)"
keywords: 
---


 
  
   
    分发消息
   
  
 
 
 
 
  
   for (;;) {
   
    Message msg = queue.next(); // might block
    
     
      msg.target.dispatchMessage(msg);
      
       
        msg.recycle();
        
         }
         
          
          
         
        
       
      
     
    
   
  
 
 
  
   处理消息
  
 
 
 
 
  
   public void dispatchMessage(Message msg) {
   
    if (msg.callback != null) {
    
     handleCallback(msg);
     
      } else {
      
       if (mCallback != null) {
       
        if (mCallback.handleMessage(msg)) {
        
         return;
         
          }
          
           }
           
            handleMessage(msg);
            
             }
             
              }
             
            
           
          
         
        
       
      
     
    
   
  
 


