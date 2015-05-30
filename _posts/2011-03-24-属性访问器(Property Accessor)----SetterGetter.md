---
layout: post
title: 属性访问器(Property Accessor)----Setter/Getter
date: 2011-03-24 11:06:00
categories: [Tuhuolong]
tags: [accessor, setter, getter, function, interface, 扩展]
---
Setter/Getter:属性/成员变量的封装  本质上是实例方法，但是在类的外部作为属性来访问，它允许创建只读和只写属性。使用：getter方法：必须有返回类型，且和要访问的私有属性类型一致。必须有返回语句，返回要访问的私有属性。setter方法：必须要有参数，且参数类型要和访问的私有属性类型一致，返回类型为void，要有赋值语句。***
好处:①独特的**访问控制**(public/private...重新封装)能力。访问getter和setter创建的方法可以像访问属性一样方便。而且还可以做其他的事情，比如例子中每次访问hello时，都会使gettimes增加1：②**隐藏**了**类**的**属性**的**实现**细节。根据封装思想，将**属性直接暴露**在外部是不好的做法，尽量通过getter和setter(**属性别名**)方法来访问属性。③在getter和setter方法中可以插入额外的代码逻辑，易于日后修改和扩展，如上例中的gettimes和settimes。④用来**绑定**数据**访问**等。比如设置个变量为public类型用来在外部调用，如果想更改这个变量的值并进行相关的一系列处理就会比较麻烦，比如凡是调用这个变量的地方全部都要重新改，采用EnterFrame侦听来检查数据是否发生了变化显然不好。如果使用getter和setter只要更改这两个方法就可以了。⑤访问类的属性而不需要将这个类公开，比如有个container内部有个rect_mc，如果想在container外部获取rect_mc的高度，总不能将container设为public，通过public function get rect_mc.Width():Number
{
  return rect_mc.width;
}就可以访问了。⑥给类添加只读和只写属性。方便修改类的私有成员属性。⑦Interface的属性只能使用方法，所以必须使用getter/setter设定。***
理解：可以理解成是一种属性的函数(方法)应用，即按属性方式 函数(方法)处理和包装，方便捕获事件和获取想要的结果。另外，setter方法不一定要和该类中的某个成员变量联系一起，比如public function set doOtherThing(...){...}，可以在函数体中处理其他的事情。