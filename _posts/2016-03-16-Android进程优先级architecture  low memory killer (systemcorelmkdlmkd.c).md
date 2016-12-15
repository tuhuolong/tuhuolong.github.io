---
layout: post
title: "Android进程优先级architecture : low memory killer (/system/core/lmkd/lmkd.c)"
date: 2016-03-16 16:08:00 
comments: true
categories: []
tags: []
description: "Android进程优先级architecture : low memory killer (/system/core/lmkd/lmkd.c)"
keywords: 
---


 
  更新
ActivityManagerService.updateOomAdjLocked
  保存
/proc/pid/oom_adj
/proc/pid/oom_score_adj
/proc/pid/oom_score
  /system/bin/lmkd
find_and_kill_process
  
   
    
     
      
     
    
   
  
  
   
    Android low memory killer 机制
   
  
  
   
    Android Low Memory Killer
   
  
 
 
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
 


