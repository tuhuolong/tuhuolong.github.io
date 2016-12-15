---
layout: post
title: "sdch: chrome支持的新HTTP传输压缩算法"
date: 2012-05-17 19:52:00 
comments: true
categories: [chrome]
tags: [chrome]
description: "sdch: chrome支持的新HTTP传输压缩算法"
keywords: chrome
---


 
  概述
 
 
  我们知道，为了加快网络传输，一般都使用gzip对文本进行压缩。如果你现在用最新版的chrome去访问页面，然后打开network控制面板，查看http headers，细心的你会发现在Request Headers里的Accept-Encoding不再是gzip,deflate，而是多了个sdch，变成了gzip,deflate,sdch。如图：
 
 
  
 
 
  
  
 
 
  SDCH到底是什么
 
 
  sdch是Shared Dictionary Compression over HTTP的缩写，即通过字典压缩算法对各个页面中相同的内容进行压缩，减少相同的内容的传输。如：一个网站中一般都是共同的头部和尾部，甚至一些侧边栏也是共同的。之前的方式每个页面打开的时候这些共同的信息都要重新加载，但使用SDCH压缩方式的话，那些共同的内容只用传输一次就可以了。
 
 
  sdch主要分为3个部分：首次请求，下载字典，之后的请求。
 
 
  这种方式最开始的时候是Google工具栏里为IE准备的，目前Chrome已经完全支持了，不过暂时还没发现哪个网站在使用。
 
 
  SDCH与ajax+pushState
 
 
  SDCH压缩方式是为了减少相同内容的传输的，同时之前介绍的ajax+pushState也是减少相同内容的传输，他们想达到的效果是一样的。只是SDCH是Google出的，可能今后一段时间只有Chrome浏览器支持，但pushState是HTML5的一个标准，目前已经有Chrome和Firefox支持，之后会有越来越多的浏览器支持。
 
 
  个人觉得SDCH可能没有什么太大的发展，但可以作为一个新方向研究，并且在合适的时候添加到标准里，让网络传输越来越迅速。
 


