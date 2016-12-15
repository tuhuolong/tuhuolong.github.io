---
layout: post
title: "define 解析依赖，判断状态，初始化/触发加载 --------require 同步加载(直接返回)/异步加载(创建匿名模块，判断状态，初始化/触发加载)"
date: 2013-12-01 14:28:00 
comments: true
categories: []
tags: []
description: "define 解析依赖，判断状态，初始化/触发加载 --------require 同步加载(直接返回)/异步加载(创建匿名模块，判断状态，初始化/触发加载)"
keywords: 
---


 
  
   define
  
 
 
  
   （1）获取依赖 AMD/CMD
  
 
 
  
   （2）触发回调 onScriptLoad
  
 
 
  
   （3）解析依赖
  
 
 
  
   已加载：触发回调
  
 
 
  
   未加载：创建module，监听，触发加载
  
 
 
  
   检查module状态
  
 
 
  
   require 创建匿名模块，加载依赖
  
 


