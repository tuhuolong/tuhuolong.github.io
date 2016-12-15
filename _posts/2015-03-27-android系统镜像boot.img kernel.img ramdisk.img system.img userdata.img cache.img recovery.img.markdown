---
layout: post
title: "android系统镜像:boot.img kernel.img ramdisk.img system.img userdata.img cache.img recovery.img"
date: 2015-03-27 19:29:00 
comments: true
categories: []
tags: []
description: "android系统镜像:boot.img kernel.img ramdisk.img system.img userdata.img cache.img recovery.img"
keywords: 
---


 
  
   
  
  
   
    boot.img(kernel.img+ramdisk.img)
   
   
    ramdisk.img(/)
   
   
    system.img(/system)
   
   
    userdata.img(/data)
   
   
    
     cache.img(/cache)
    
   
   
    
     recovery.img(kernel.img+ramdisk-recovery.img)
    
   
   
    ramdisk-recovery.img(/)
   
  
  
   
    
     split_bootimg.pl boot.img
    
    
     
      gunzip boot.gz
     
     
      
       cpio -i -F boot
      
     
    
   
  
 
 
  $(function () {
                $('pre.prettyprint code').each(function () {
                    var lines = $(this).text().split('\n').length;
                    var $numbering = $('').addClass('pre-numbering').hide();
                    $(this).addClass('has-numbering').parent().append($numbering);
                    for (i = 1; i ').text(i));
                    };
                    $numbering.fadeIn(1700);
                });
            });
 


