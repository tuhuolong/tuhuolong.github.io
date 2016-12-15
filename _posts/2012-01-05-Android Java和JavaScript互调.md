---
layout: post
title: "Android Java和JavaScript互调"
date: 2012-01-05 15:13:00 
comments: true
categories: [javascript]
tags: [javascript]
description: "Android Java和JavaScript互调"
keywords: javascript
---


 
  (1)JavaScript调Java
 
 
  addJavascriptInterface
 
 
  通过Android中 WebView.addJavascriptInterface(new JavaClass(),"JSName")注册Java对象 (注册死了-固定名字)
 
 
  JavaScript里windows.JSName调用
 
 
  (2)Java调JavaScript
 


