---
layout: post
title: "java serialization/deserialization (序列化对象自描述)"
date: 2015-11-05 23:24:00 
comments: true
categories: []
tags: []
description: "java serialization/deserialization (序列化对象自描述)"
keywords: 
---


 
  serialization:对象的类信息+对象的成员变量
  deserialization:对象的类信息->;加载Class->;比较serialVersionUID->;创建实例->;读取成员变量的值(按照对象的类信息)赋值到相应实例
  反射
-keepnames class * implements java.io.Serializable
  serialization对象自描述
deserialization与成员变量顺序无关, 找到即可
 
 
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
 


