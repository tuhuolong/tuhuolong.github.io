---
layout: post
title: "批量读取productFlavors"
date: 2015-10-07 14:58:00 
comments: true
categories: []
tags: []
description: "批量读取productFlavors"
keywords: 
---


 
  productFlavors {
        def path="./channel.txt"
        file(path).eachLine { line->;
            def words = line.split(':')
            def key = words[0]
            def channel = words[1]
            if (key == '') {
                key = channel
            }
            def name = 's'+channel
            "$name" {
                manifestPlaceholders=[APP_KEY_PLACEHOLDER:key, APP_PID_PLACEHOLDER:name]
            }
        }
    }
 
 
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
 


