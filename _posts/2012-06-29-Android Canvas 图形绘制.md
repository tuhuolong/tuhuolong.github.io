---
layout: post
title: "Android Canvas 图形绘制"
date: 2012-06-29 18:56:00 
comments: true
categories: [图形]
tags: [图形]
description: "Android Canvas 图形绘制"
keywords: 图形
---


 
  
   
    在View对象上绘图
   
  
 
 
  
   如果应用程序不需要大量的图形处理或很高的帧速率（如一个棋类游戏、Snake游戏或另外的慢动画类应用程序），那么就应该考虑创建一个定制的View组件，并且用该组件的View.onDraw()方法的Canvas参数来进行图形绘制。这么做最大的方便是，Android框架会提供一个预定义的Canvas对象，该对象用来放置绘制图形的调用。
  
 
 
  
   从继承View类（或其子类）开始，并定义onDraw()回调方法。系统会调用该方法来完成View对象自己的绘制请求。这也是通过Canvas对象来执行所有的图形绘制调用的地方，这个Canvas对象是由onDraw()回调方法传入的。
  
 
 
  
   Android框架只在必要的时候才会调用onDraw()方法，每次请求应用程序准备完成图形绘制任务时，必须通过调用invalidate()方法让该View对象失效。这表明可以在该View对象上进行图形绘制处理了，然后Android系统会调用该View对象的onDraw()方（尽管不保证该回调方法会立即被调用）。
  
 
 
  
   在定制的View组件的onDraw()方法内部，使用给定的Canvas对象来完成所有的图形绘制处理（如Canvas.draw…（）方法或把该Canvas对象作为参数传递给其他类的draw()方法）。一旦onDraw()方法被执行完成，Android框架就会使用这个Canvas对象来绘制一个有系统处理的Bitmap对象。
  
 
 
  
   注意：为了在一个线程中而不是主Activity的线程中发出一个失效请求，必须调用postInvalidate()。
  
 
 
  
   
   
  
 
 
  
   
    http://blog.csdn.net/fireofstar/article/details/7643892
   
   
   
  
 


