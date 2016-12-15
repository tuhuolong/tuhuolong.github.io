---
layout: post
title: "ASCII+Unicode+UTF8(字符编码方式->字节)  Base64(字节编码方式->64字符)"
date: 2013-04-03 15:31:00 
comments: true
categories: []
tags: []
description: "ASCII+Unicode+UTF8(字符编码方式->字节)  Base64(字节编码方式->64字符)"
keywords: 
---


 
  Base64 编码是一种可以把二进制文件编码成文本的编码规则。在很多地方地方都有用到，比如我们可以把图像转成 Base64 编码，然后内联到 HTML 或者 CSS 中。
 
 
  Base64 编码用一些很常见的 ASCII 字符来表示 0-63 ，构成 6 个 bit 。用 A-Z 表示 0-25 ，a-z 表示 26-51 ， 0-9 表示 52-61 , + 表示 62， / 表示 63 。那怎么把一个 byte(8bit) 用这种只能表示 6 bit 的字符表示呢？转换规则如下：
 
 
  
   
  
 
 
  很简单吧，就是用4个 Base64 编码来表示 3 个 byte 的内容。
 
 
  如果到末尾，凑不齐 3 byte 怎么办呢？如果只有一个 byte ，就在后面补4个 0 bit ，构成 2 个 Base64 编码，如果余下 2 byte ,就补 2 个 0bit ，构成 3 个 Base64 编码。理论上，这样做就足够判断最后还剩下几个 byte ，但是有些系统需要明确指明最后剩下的 byte ，需要用 = 号来把 Base64 编码的个数补全到 4 的倍数。也就是如果最后余下了一个 byte , 就在编码后的字符补上 == ，如果余下了 2 byte ，就补上 = .
 


