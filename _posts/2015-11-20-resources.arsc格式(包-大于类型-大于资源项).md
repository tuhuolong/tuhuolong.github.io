---
layout: post
title: "resources.arsc格式(包->类型->资源项)"
date: 2015-11-20 16:45:00 
comments: true
categories: []
tags: []
description: "resources.arsc格式(包->类型->资源项)"
keywords: 
---


 
  
   
  
  
   
    资源项属于不同的包，不同的类型，不同的配置
   
  
  
   
  
  RES_TABLE_TYPE_SPEC_TYPE (type id)
RES_TABLE_TYPE_SPEC_TYPE (type id)
RES_TABLE_TYPE_SPEC_TYPE (type id)
RES_TABLE_TYPE_SPEC_TYPE (type id)
RES_TABLE_TYPE_SPEC_TYPE (type id)

RES_TABLE_TYPE_TYPE (type id + config)
RES_TABLE_TYPE_TYPE (type id + config)
RES_TABLE_TYPE_TYPE (type id + config)
RES_TABLE_TYPE_TYPE (type id + config)
RES_TABLE_TYPE_TYPE (type id + config)
RES_TABLE_TYPE_TYPE (type id + config)
 
 
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
 


