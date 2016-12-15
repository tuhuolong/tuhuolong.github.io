---
layout: post
title: "android图片编解码architecture"
date: 2016-06-03 14:02:00 
comments: true
categories: []
tags: []
description: "android图片编解码architecture"
keywords: 
---


 
  
   指定图片和屏幕密度
  
  public int inDensity;
public int inTargetDensity;
public int inScreenDensity;
  
   
  
  
   
    libandroid_runtime
   
  
  
   
    libskia
   
  
  
   
    libjpeg
   
   
    libpng
   
   
    libgif
   
   
    libwebp-decode/libwebp-encode
   
  
 
 
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
 


