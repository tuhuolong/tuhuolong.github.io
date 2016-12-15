---
layout: post
title: "ActiveServices"
date: 2016-07-01 10:56:00 
comments: true
categories: []
tags: []
description: "ActiveServices"
keywords: 
---


 
  
   
    mPendingServices 启动等待队列
   
  
  
   
    mRestartingServices 重启队列
   
  
  
   
  
  realStartServiceLocked 进程已启动
   app.thread.scheduleCreateService (onCreate)
   requestServiceBindingsLocked (onBind)
   sendServiceArgsLocked (onStartCommand)
  
   
  
  bringDownServiceLocked
   r.app.thread.scheduleUnbindService (onUnbind)
   r.app.thread.scheduleStopService (onDestroy)
  
   
    
   
  
  Service (bind基于intent)

public abstract IBinder onBind(Intent intent);

public boolean onUnbind(Intent intent);

public void onRebind(Intent intent);
 
 
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
 


