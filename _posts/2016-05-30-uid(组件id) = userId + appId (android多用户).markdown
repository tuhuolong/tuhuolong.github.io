---
layout: post
title: "uid(组件id) = userId + appId (android多用户)"
date: 2016-05-30 22:52:00 
comments: true
categories: []
tags: []
description: "uid(组件id) = userId + appId (android多用户)"
keywords: 
---


 
  public static final int getUid(int userId, int appId) {
        if (MU_ENABLED) {
            return userId * PER_USER_RANGE + (appId % PER_USER_RANGE);
        } else {
            return appId;
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
 


