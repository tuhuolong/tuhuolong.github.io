---
layout: post
title: C++中class与struct的区别
date: 2011-04-26 16:55:00
categories: [C++]
tags: [struct, class, c++]
---
**关于使用大括号初始化
**class和struct如果定义了构造函数的话，都不能用大括号进行初始化
如果没有定义构造函数，struct可以用大括号初始化。
如果没有定义构造函数，且所有成员变量全是public的话，可以用大括号初始化。**关于默认访问权限**
class中默认的成员访问权限是private的，而struct中则是public的。

**关于继承方式**
class继承默认是private继承，而struct继承默认是public继承

