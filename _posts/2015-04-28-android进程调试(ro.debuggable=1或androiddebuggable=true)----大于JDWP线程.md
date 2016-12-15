---
layout: post
title: "android进程调试(ro.debuggable=1或android:debuggable="true")---->JDWP线程"
date: 2015-04-28 11:53:00 
comments: true
categories: []
tags: []
description: "android进程调试(ro.debuggable=1或android:debuggable="true")---->JDWP线程"
keywords: 
---


 
  
   ro.debuggable=1或android:debuggable=”true”
  
  
   VM process
  
  
   JDWP thread  —– adbd(JDWP service) ———— adb server —–adb client
  
  
   
  
  
   VM process启动创建jdwp thread->;adbd注册
  
 
 
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
 


