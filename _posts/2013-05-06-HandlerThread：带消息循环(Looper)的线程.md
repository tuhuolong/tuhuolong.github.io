---
layout: post
title: "HandlerThread：带消息循环(Looper)的线程"
date: 2013-05-06 12:34:00 
comments: true
categories: []
tags: []
description: "HandlerThread：带消息循环(Looper)的线程"
keywords: 
---


 
  
   （1）Looper.prepare() 创建Looper，保存在线程局部变量(ThreadLocal)
  
 
 
  
   （2）onLooperPrepared() 回调消息循环前的准备方法 (可用于注册Handler)
  
 
 
  
   （3）Looper.loop() 启动消息循环
  
 


