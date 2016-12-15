---
layout: post
title: "Rendering: repaint, reflow/relayout, restyle的学习笔记"
date: 2010-09-12 15:05:00 
comments: true
categories: [浏览器]
tags: [浏览器]
description: "Rendering: repaint, reflow/relayout, restyle的学习笔记"
keywords: 浏览器
---


 
  
   http://www.zhuoqun.net/html/y2009/1430.html
  
 
 
 
 
  
   
    Rendering tree是DOM tree的可见部分
   
  
 
 
 
 
 
 
  
   
   
  
 
 
  
   
   
  
 
 
  页面渲染的过程:
 
 
  
 
 
  1. 解析HTML代码并生成一个 DOM 树。
 
 
  2. 解析CSS文件，顺序为：浏览器默认样式->;自定义样式->;页面内 的样式</p>; <p>;3. </p>;
 
 
  3. 生成一个渲染树（render tree）。这个渲染树和DOM树的不同之处在于，它是受样式影响的。它不包括那些不可见的节点。
 
 
  4. 当渲染树生成之后，浏览器就会在屏幕上“画”出所有渲染树中的节点。
  
  
  那什么是 reflow 呢？reflow 是指渲染树中的一些节点被重新验证或者大小被重新计算。一个页面至少会在初始化的时候 reflow 一次。而 repaint 是指屏幕中的一些显示需要更新，比如更改了某个元素的背景颜色。
 
 
  reflow 和 repaint 都是需要计算的，会消耗资源，影响用户体验。所有对用来生成渲染树的信息的改动都会触发 reflow 或者 repaint 。比如：添加或删除一个DOM节点、把一个节点的display设为none 等。由于reflow 和 repaint 会消耗资源，所以浏览器一般都会对此进行优化。有些浏览器会建立一个队列，把发生的 reflow 和 repaint 放进去，然后进行批处理。但是当你获取某些节点的样式信息的时候，会破坏浏览器的这种默认优化，强迫浏览器执行当前所有的 reflow 和 repaint。比如当你获取某个节点的 offsetTop 的时候，浏览器必须提供给你最新的数值，所以就需要把之前的 reflow 和 repaint 都执行。
 
 
  为了节省资源提高用户体验，我们在编写代码的时候就必须要注意不能让浏览器 reflow 和 repaint 太频繁，尽量避免这样的代码：
 
 
  
   
    
     
      1
2
3

     
     
      for(i=0;i<n;i++){
el.style.left = el.offsetLeft + 10 + "px";
}
     
    
   
  
 
 
  下面提供一些减少 reflow 和 repaint 的思路：
 
 
  1. 不要一个一个地去直接修改元素的样式，最好去修改样式表，然后修改元素的className。
 
 
  
   
    
     
      1
2
3
4
5
6
7
8
9

     
     
      var left = 10,
    top = 10;
el.style.left = left + "px";
el.style.top  = top  + "px";
 
// 这样不好
 
el.className += " theclassname";
// 这样很好
     
    
   
  
 
 
  2.批量处理DOM操作，最好不要在当前的DOM树中直接操作。比如：使用documentFragment 来暂存一些操作；先将要进行操作的DOM节点clone，更改完毕之后再去替换当前的节点；先将要操作的DOM节点的display设置为hidden，执行操作之后再更改回来。
 
 
  3. 避免一直计算元素样式。比如在循环中，你可以先用本地变量将计算的样式保存。
 
 
  
   
    
     
      1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16

     
     
      // 不要这么做
for(big; loop; here) {
    el.style.left = el.offsetLeft + 10 + "px";
    el.style.top  = el.offsetTop  + 10 + "px";
}
 
// 最好先存储到变量里。
var left = el.offsetLeft,
    top  = el.offsetTop
    esty = el.style;
for(big; loop; here) {
    left += 10;
    top  += 10;
    esty.left = left + "px";
    esty.top  = top  + "px";
}
     
    
   
  
 


