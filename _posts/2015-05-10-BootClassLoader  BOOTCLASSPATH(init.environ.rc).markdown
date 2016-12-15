---
layout: post
title: "BootClassLoader  BOOTCLASSPATH(init.environ.rc)"
date: 2015-05-10 22:24:00 
comments: true
categories: []
tags: []
description: "BootClassLoader  BOOTCLASSPATH(init.environ.rc)"
keywords: 
---


 
  
   /system/framework/core.jar:
   
    /system/framework/conscrypt.jar:
    
     /system/framework/okhttp.jar:
     
      /system/framework/core-junit.jar:
      
       /system/framework/bouncycastle.jar:
       
        /system/framework/ext.jar:
        
         /system/framework/framework.jar:
         
          /system/framework/framework2.jar:
          
           /system/framework/telephony-common.jar:
           
            /system/framework/voip-common.jar:
            
             /system/framework/mms-common.jar:
             
              /system/framework/android.policy.jar:
              
               /system/framework/services.jar:
               
                /system/framework/apache-xml.jar:
                
                 /system/framework/webviewchromium.jar
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
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
 


