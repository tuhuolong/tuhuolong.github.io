---
layout: post
title: "ubuntu adb 调试手机"
date: 2012-10-15 19:00:00 
comments: true
categories: [ubuntu]
tags: [ubuntu]
description: "ubuntu adb 调试手机"
keywords: ubuntu
---


 
  
   
   
  
 
 
  
   
    . build/envsetup.sh
   
  
 
 
  
   
    lunch
   
   (将adb路径加入PATH)
  
 
 
  
   
    
    
   
  
 
 
  
   
    lsusb
   
  
 
 
  
   
    gedit
    
     /etc/udev/rules.d/
    
    
     51-android.rules
    
   
  
 
 
  
   # fastboot protocol on grouper (YiPingTai)
  
 
 
  
   SUBSYSTEM=="usb", ATTR{idVendor}=="
   
    413c
   
   ", ATTR{idProduct}=="
   
    
     b108
    
   
   ",
 MODE="
   
    0666
   
   ", OWNER="<username>;"
  
 
 
  
   
   
  
 


