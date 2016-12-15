---
layout: post
title: "WebKit Event"
date: 2012-06-04 20:22:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "WebKit Event"
keywords: webkit
---


 
  
 
 
 
 
  
   EventTarget直接依赖于EventListener，EventListener是一个抽象类,然后具体的监听器在下面派生,注意，JSEventListener，以及JSLazeEventListener是与具体的js解释引擎有关系的。那么事件监听器的注册的流程是怎么样的了？下面以body的onload为例进行说明 。
  
 
 
  
   
    
    
   
  
 
 
 


