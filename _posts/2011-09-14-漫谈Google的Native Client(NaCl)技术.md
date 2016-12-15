---
layout: post
title: "漫谈Google的Native Client(NaCl)技术"
date: 2011-09-14 16:13:00 
comments: true
categories: [google]
tags: [google]
description: "漫谈Google的Native Client(NaCl)技术"
keywords: google
---


 
 
 
  Native Client简介
 
 
  Native Client是Google在浏览器领域推出的一个开源技术，它允许在浏览器内编译Web应用程序，并执行原生的编译好的代码。Native Client有以下几个优势（参考Google官方英文介绍）：
 
 
  
   
    为Web提供更多的图形，音频以及其他功能
   
   ：可以直接在web上执行了原生的2D，3D图形渲染程序（对Web游戏很有用），播放音视频，响应鼠标键盘事件，多线程执行代码等等，而这一切，不需要浏览器安装任何插件。
  
  
   
    良好的可移植性
   
   ：一个Web程序，只需要开发一份代码，即可以在所有平台（包括Windows，linux，Mac等）运行。
  
  
   
    高安全性
   
   ：安装不被信任的桌面程序一级浏览器插件，可能带来很高的安全风险（如程序携带木马，病毒）。而Native client使用了双层沙盒（sandbox）设计来保护用户的本地资源。Native
 Client的架构可以保证web要应用的安全性，并且取得和原声代码相同或相近的性能。
  
  
   
    方便从桌面迁移
   
   ：很多开发厂商之前花了大力气开发桌面程序，随着云计算的到来，越来越多的程序会被移植到互联网上，由于NaCl支持直接执行C/C++/Java等代码，Native Client技术可以简化移植过程，减少移植成本。
  
  
   
    高性能
   
   ：Native Client可以让web应用已接近桌面程序的性能运行，这就为在浏览器内运行性能苛刻的程序提供了基础，如大型3D游戏。
  
 
 
  Native Client技术概要
 
 
  有人说，Native Client技术是抄袭ActiveX的，个人不以为然，ActiveX主要是基于COM的，是操作系统提供一套可重用的接口给web应用，而NaCl则是独立于操作系统的。说实话，感觉这技术更像是抄袭Adobe的Alchemy技术，Achemy技术主要目的是解决Flash的低性能，这和Native Client是接近的，并且，这两种技术都采用了LLVM技术，具体可以看这篇文章
  
   http://yjrl.iteye.com/blog/320665
  
  。
 
 
  
   LLVM
  
  是 Low Level Virtual Machine
 的简称，这个库提供了与编译器相关的支持，能够进行程序语言的编译期优化、链接优化、在线编译优化、代码生成。简而言之，可以作为多种语言编译器的后端来使用。而基于LLVM ，开源社区开发出了Clang，一个C++ 编写、基于 LLVM、发布于 LLVM BSD 许可证下的 C/C++/Objective C/Objective C++ 编译器，其目标（之一）就是超越 GCC。
 
 
  Google 的Native Client的关键技术就是PNaCl(Portable Native Client Executables),而PNaCl实现的一个关键就是LLVM。下面是整个PNaCl结构示意图：
  
   
  
 
 
  
   在PNaCl中，开发者通过一些前端编译器将C/C++/Fortan源代码用对应的编译器前端编译成LLVM的中间字节码，并且进行优化以及链接（这一整套流程可以在Google提供的SDK中完成）。之后，服务器将链接好的字节码进行分发，在客户端，通过LLVM的后端将字节码翻译成本地的二进制对象文件，并且和本地的相关库链接，最终执行完成（这些功能应该是集成在浏览器中的）。
  
 
 
  另外，为了保证安全性，NaCl采用了沙盒技术，具体可以看这篇论文：《
  
   Native
 Client: A Sandbox for Portable, Untrusted x86 Native Code
  
  》。
 
 
  参考：
 
 
  http://www.lingcc.com/2010/06/02/10955/
 
 
  http://nativeclient.googlecode.com/svn/data/site/pnacl.pdf
 


