---
layout: post
title: "命令行更新android sdk + gradle设置代理"
date: 2015-11-09 13:10:00 
comments: true
categories: [android]
tags: [android]
description: "命令行更新android sdk + gradle设置代理"
keywords: android
---


 
  android list sdk --extended --proxy-host test.com --proxy-port 1234
  android update sdk --no-ui -t 1,2,3 --proxy-host test.com --proxy-port 1234
  gradle -Dhttp.proxyHost=test.com -Dhttp.proxyPort=1234 -Dhttps.proxyHost=test.com -Dhttps.proxyPort=1234
  
   
    
   
  
  
   备用
  
  android list sdk --all --extended --proxy-host test.com --proxy-port 1234
  android update sdk --no-ui --all --filter build-tools-23.0.3 --proxy-host test.com --proxy-port 1234
 
 
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
 


