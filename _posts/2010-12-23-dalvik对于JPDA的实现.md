---
layout: post
title: dalvik对于JPDA的实现
date: 2010-12-23 09:31:00
categories: [Java]
tags: [socket, android, tcp]
---
                  -----发送JDWP请求----->
**Debugger**                                       **VM**
   (JDI)       <----发送JDWP响应-----    (JVMTI)
 
 
 
**DalvikVM**
(1)Dalvik启动时,创建一个线程(JDWPThread)与Debugger通信
    JDWP(应用层协议)可以使用两种传输层协议:TCP的Socket和Android的Adb
 
(2)JDWPThread线程收到JDWP请求,查映射表(gHandlerMap)、调用处理函数,最后返回JDWP响应(含调试结果)
 
 
**Debugger**
(1)Debugger按照JDI发送JDWP请求,再处理JDWP响应
