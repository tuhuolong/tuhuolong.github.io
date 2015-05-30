---
layout: post
title: android进程调试(ro.debuggable=1或android:debuggable="true")---->JDWP线程
date: 2015-04-28 11:53:00
categories: [Android]
tags: []
---
ro.debuggable=1或android:debuggable=”true”
VM process
JDWP thread  —– adbd(JDWP service) ———— adb server —–adb client

VM process启动创建jdwp thread->adbd注册
