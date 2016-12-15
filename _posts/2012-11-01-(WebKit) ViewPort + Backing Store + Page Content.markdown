---
layout: post
title: "(WebKit) ViewPort + Backing Store + Page Content"
date: 2012-11-01 20:02:00 
comments: true
categories: []
tags: []
description: "(WebKit) ViewPort + Backing Store + Page Content"
keywords: 
---


 
  
   
   
  
 
 
  
   
    
    
   
  
 
 
  
   
   
  
 
 
 
 
  
   
    
     
      视口Viewport
     
     表示用来显示网页内容的窗口大小。Qt中视口是一个QWidget对象。
     
      
      
     
    
   
  
  
   
    
     
      分片后备存储Tiled backing store
     
     可以认为一个off-screen离屏的pixmap缓冲区，为了让滚屏和缩放达到一个流畅的速度，这个缓冲区会覆盖一个比视口更大的区域。
     
      
      
     
    
   
  
  
   
    
     
      覆盖的页面内容 Covered content
     
     表示分片后备存储所覆盖的页面内容
     
      
      
     
    
   
  
  
   
    
     
      页面内容Page content
     
     整个页面的内容
    
   
  
 
 
  
  
 
 
 


