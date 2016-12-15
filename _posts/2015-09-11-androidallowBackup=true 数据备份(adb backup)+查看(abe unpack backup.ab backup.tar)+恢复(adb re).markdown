---
layout: post
title: "android:allowBackup=true 数据备份(adb backup)+查看(abe unpack backup.ab backup.tar)+恢复(adb re)"
date: 2015-09-11 12:26:00 
comments: true
categories: []
tags: []
description: "android:allowBackup=true 数据备份(adb backup)+查看(abe unpack backup.ab backup.tar)+恢复(adb re)"
keywords: 
---


 
  备份
adb backup -nosystem -apk com.xiaomi.smarthome
  查看
abe unpack backup.ab backup.tar
  恢复
adb restore
 
 
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
 


