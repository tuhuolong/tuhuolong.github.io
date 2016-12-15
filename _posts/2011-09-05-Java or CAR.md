---
layout: post
title: "Java&CAR"
date: 2011-09-05 10:55:00 
comments: true
categories: [java]
tags: [java]
description: "Java&CAR"
keywords: java
---


 
  (1)设计Java类时,有需要快速访问的属性,则可以设计成复合类/复合对象(Java对象+CPP对象),将正常属性和共享属性放入Java对象(通过Java/JNI方法)访问,将快速属性放入C++对象(通过C++函数访问)。
 
 
  (2)实际上C++部分的具体实现放入了构件(通过复合类的Annotation中的构件名和C++类名指定)
 
 
  (3)加载一个Java类时,有xxx的Annotation,则这是一个复合类,创建ClassObject对象,再根据构件名加载构件,根据C++类名反射出C++类接口指针(放入ClassObject)
 
 
  (4)创建一个复合类的复合对象时,创建Java对象,调用C++类接口创建C++对象,(对象指针)放在Java对象的后面
 


