---
layout: post
title: "android查看kernel log"
date: 2015-04-19 20:39:00 
comments: true
categories: []
tags: []
description: "android查看kernel log"
keywords: 
---


 
  root@:/ # cat /proc/kmsg
  root@:/ # dmesg
  root@:/ # cat /proc/sys/kernel/printk
7       4       1       7
(7=Current level, 4=Default level, 1=Minimum level, 7=Boot up level)
  
   
    
     
    
   
  
  
   
    android-logging-the-linux-kernel-log
   
  
 
 
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
 


