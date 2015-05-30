---
layout: post
title: measure,layout,draw的相关方法
date: 2013-05-24 00:03:00
categories: [Android]
tags: []
---
（1）invalidate()：请求重新draw()，但只会绘制调用者本身
（2）setSelection() ：请求重新draw()，但只会绘制调用者本身
（3）setVisibility() ：INVISIBLE->VISIBLE：invalidate()，VISIBLE/INVISIBLE<->GONE：measure+layout+draw
（4）setEnabled() ： 请求重新draw()，但不会重新绘制任何视图包括该调用者本身
（5）requestLayout() ：会导致调用measure()过程 和 layout()过程
