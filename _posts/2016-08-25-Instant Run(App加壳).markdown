---
layout: post
title: "Instant Run(App加壳)"
date: 2016-08-25 14:31:00 
comments: true
categories: []
tags: []
description: "Instant Run(App加壳)"
keywords: 
---


 
                      BootClassLoader
                           |
                  IncrementalClassLoader
                           |
                     PathClassLoader
                     /     |      \
                    /      |       \
      DexClassLoader DexClassLoader DexClassLoader
       (Patch)
  
   
  
  
   
    从Instant-Run出发，谈谈Android上的热修复
   
  
 
 
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
 


