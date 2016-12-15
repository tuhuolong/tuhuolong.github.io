---
layout: post
title: "java proxy"
date: 2015-12-30 17:08:00 
comments: true
categories: []
tags: []
description: "java proxy"
keywords: 
---


 
  (被代理类ClassLoader, 被代理类接口）
Object proxyObj = Proxy.newProxyInstance(cls.getClassLoader(), cls.getInterfaces(), invocationHandler);

代理对象方法调用转到
static Object invoke(Proxy proxy, ArtMethod method, Object[] args) throws Throwable {
        InvocationHandler h = proxy.h;
        return h.invoke(proxy, new Method(method), args);
    }
  Interface i = (Interface)proxyObj;
 
 
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
 


