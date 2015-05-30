---
layout: post
title: WebKit Event
date: 2012-06-04 20:22:00
categories: []
tags: [webkit, 引擎]
---
![](http://my.csdn.net/uploads/201206/04/1338812489_3294.jpg)

EventTarget直接依赖于EventListener，EventListener是一个抽象类,然后具体的监听器在下面派生,注意，JSEventListener，以及JSLazeEventListener是与具体的js解释引擎有关系的。那么事件监听器的注册的流程是怎么样的了？下面以body的onload为例进行说明 。
![](http://my.csdn.net/uploads/201206/04/1338812743_3823.jpg)


