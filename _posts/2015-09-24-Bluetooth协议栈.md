---
layout: post
title: "Bluetooth协议栈"
date: 2015-09-24 15:56:00 
comments: true
categories: []
tags: []
description: "Bluetooth协议栈"
keywords: 
---


 
  
   
    
     
      
       
        
         
          
           
            
             
              
               
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
  BLE设备必须实现GAP和GATT
  BCS设备必须实现SPP
  
   
  
  GAP  GATT 
SMP  ATT
  
   
    
     https://www.safaribooksonline.com/library/view/getting-started-with/9781491900550/ch01.html
    
   
  
 
 
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
 


