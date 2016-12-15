---
layout: post
title: "【android】进程优先级(Framework设置优先级,LowMemoryKiller查杀)"
date: 2014-09-29 15:39:00 
comments: true
categories: []
tags: []
description: "【android】进程优先级(Framework设置优先级,LowMemoryKiller查杀)"
keywords: 
---


 
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    NATIVE_ADJ
   
  
  = -17;
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    SYSTEM_ADJ
   
  
  = -16;   //  系统进程
 (init + system-server)
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    PERSISTENT_PROC_ADJ
   
  
  = -12;  //核心进程
 
 
 
 
  
   
    static
   
  
  
  
  
   
    final
   
  
  
  
  
   
    int
   
  
  
   
    FOREGROUND_APP_ADJ
   
  
  =
 0;    //前台进程
 
 
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    VISIBLE_APP_ADJ
   
  
  = 1;     //可见进程
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    PERCEPTIBLE_APP_ADJ
   
  
  = 2;   //可感知进程
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    BACKUP_APP_ADJ
   
  
  = 3;
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    HEAVY_WEIGHT_APP_ADJ
   
  
  = 4;  //重量进程
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    SERVICE_ADJ
   
  
  = 5;   //服务进程
 
 
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    HOME_APP_ADJ
   
  
  = 6;   //Home进程
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    PREVIOUS_APP_ADJ
   
  
  = 7;  //前个进程
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    SERVICE_B_ADJ
   
  
  = 8;
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    CACHED_APP_MIN_ADJ
   
  
  = 9;
 
 
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    CACHED_APP_MAX_ADJ
   
  
  = 15;
 
 
  
   
    static
   
   
    final
   
   
    int
   
  
  
   
    UNKNOWN_ADJ
   
  
  = 16;
 
 
  
  
 
 
  
  
 
 
 
 
  优先级            所剩内存
 
 
  0    ---------   3 *512,
  
   /* 6MB */
  
 
 
  1    ---------   2 *1024,
  
   /* 8MB */
  
 
 
  6    ---------   4 *1024,
  
   /* 16MB */
  
 
 
  12  ---------  16 *1024,
  
   /* 64MB */
  
 
 
  
   
   
  
  
   
    (可杀进程中)优先级最低+占有内存最多
   
   
   
  
  
   
   
  
  
  
  
   
   
  
  
   
   
  
 


