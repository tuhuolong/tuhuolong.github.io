---
layout: post
title: "minSdkVersion <= targetSdkVersion <= compileSdkVersion"
date: 2016-01-08 18:39:00 
comments: true
categories: []
tags: []
description: "minSdkVersion <= targetSdkVersion <= compileSdkVersion"
keywords: 
---


 
  minSdkVersion <= targetSdkVersion <= compileSdkVersion
  
   
    
   
  
  
   理想情况
  
  minSdkVersion (lowest possible) <= targetSdkVersion == compileSdkVersion (latest SDK)
  
   
    
   
  
  targetSdkVersion:不同SdkVersion系统行为可能不一致
  
   
    
     
      https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd?linkId=20147489#.flvnjxtww
     
    
   
  
 
 
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
 


