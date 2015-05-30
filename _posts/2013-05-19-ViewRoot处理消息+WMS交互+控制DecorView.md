---
layout: post
title: ViewRoot:处理消息+WMS交互+控制DecorView
date: 2013-05-19 12:52:00
categories: [Android]
tags: []
---
ViewRoot本质上是一个Handler，并且实现了ViewParent接口。ViewRoot的主要功能是：
1.      负责分发消息事件，如Key、Motion事件等；
2.      负责和WMS的交互，分发WMS的交互命令；
3.      作为DecorView的parent，对DecorView进行draw、measure、layout等操作；
