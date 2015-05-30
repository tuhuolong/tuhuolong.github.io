---
layout: post
title: CSS3  -webkit-transition(属性渐变)
date: 2012-10-16 20:06:00
categories: [WebKit, Web, Tuhuolong]
tags: [css, matrix, c]
---

transition(属性渐变):"CSS Transitions allow property changes in CSS values to occur smoothly over a specified duration"
-webkit-transition：CSS属性(none|all|属性)
  持续时间  时间函数  延迟时间
CSS属性(transition-property)：要变化的属性，比如元素变宽则是width，文字颜色要变色这是color；[W3C给出了一个可变换属性的列表](http://www.w3.org/TR/css3-transitions/)：除了以上属性外，还有css3中大放异彩的css3[变形](http://www.css88.com/archives/2168)，比如放大缩小，旋转斜切，渐变等。该取值还有个强大的“all”取值，表示上表所有属性**；**
持续时间(transition-duration)：动画执行的时间，以秒为单位，比如0.1秒可以写成”0.1s”或者”.1s”，注意后面有个“s”单位。
##
时间函数(transition-timing-function)**：**
****         ease：逐渐慢下来，函数等同于贝塞尔曲线(0.25, 0.1, 0.25, 1.0).
         linear：线性过度,函数等同于贝塞尔曲线(0.0, 0.0, 1.0, 1.0).
         ease-in：由慢到快,函数等同于贝塞尔曲线(0.42, 0, 1.0, 1.0).
         ease-out：由快到慢, 函数等同于贝塞尔曲线(0, 0, 0.58, 1.0).
         ease-in-out：由慢到快在到慢, 函数等同于贝塞尔曲线(0.42, 0, 0.58, 1.0)
         cubic-bezier：特定的cubic-bezier曲线。 (x1, y1, x2, y2)四个值特定于曲线上点P1和点P2。所有值需在[0, 1]区域内，否则无效。
##延迟时间(transition-delay)：在动作和变换开始之间等待多久，通常用秒来表示(比如,
 .1s)。如果你不想延迟，该值可省略。

    

定义CSS动画的位置:CSS伪类和JS事件
        :link          未访问的链接
        :visited      访问过的链接
        :hover       鼠标悬停
        :active       鼠标点击
        :focus        元素选中

参考阅读：
[http://www.qianduan.net/css-transitions-101.html](http://www.qianduan.net/css-transitions-101.html)
[http://www.zhangxinxu.com/wordpress/?p=498](http://www.zhangxinxu.com/wordpress/?p=498)
[http://fis.io/css-3-hover-animations.html](http://fis.io/css-3-hover-animations.html)
