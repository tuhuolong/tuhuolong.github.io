---
layout: post
title: "JavaScript instanceof的实现"
date: 2013-11-24 18:47:00 
comments: true
categories: []
tags: []
description: "JavaScript instanceof的实现"
keywords: 
---


 
 
 
  JavaScript instanceof 运算符代码
 
 
   function instance_of(L, R) {//L 表示左表达式，R 表示右表达式
  var O = R.prototype;// 取 R 的显示原型
  L = L.__proto__;// 取 L 的隐式原型
  while (true) { 
    if (L === null) 
      return false; 
    if (O === L)// 这里重点：当 O 严格等于 L 时，返回 true 
      return true; 
    L = L.__proto__; 
  } 
 }
 
 
  
  
  
   
   
  
  
   
   
  
 


