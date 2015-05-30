---
layout: post
title: Android NDK带来什么
date: 2011-04-21 15:36:00
categories: [Android]
tags: [android, java, jni, 平台, google, c]
---
##1、前言
6月26日，Google Android发布了NDK，引起了很多发人员的兴趣。NDK全称：Native Development Kit。下载地址为：[http://developer.android.com/sdk/ndk/1.5_r1/index.html](http://developer.android.com/sdk/ndk/1.5_r1/index.html)。
##2、误解
新出生的事物，除了惊喜外，也会给我们带来一定的迷惑、误解。
#2.1、误解一：NDK发布之前，Android不支持进行C开发
在Google中搜索“NDK”，很多“Android终于可以使用C++开发”之类的标题，这是一种对Android平台编程方式的误解。其实，Android平台从诞生起，就已经支持C、C++开发。众所周知，Android的SDK基于Java实现，这意味着基于Android SDK进行开发的第三方应用都必须使用Java语言。但这并不等同于“第三方应用只能使用Java”。在Android SDK首次发布时，Google就宣称其虚拟机Dalvik支持JNI编程方式，也就是第三方应用完全可以通过JNI调用自己的C动态库，即在Android平台上，“Java+C”的编程方式是一直都可以实现的。
当然这种误解的产生是有根源的：在Android SDK文档里，找不到任何JNI方面的帮助。即使第三方应用开发者使用JNI完成了自己的C动态链接库（so）开发，但是so如何和应用程序一起打包成apk并发布？这里面也存在技术障碍。我曾经花了不少时间，安装交叉编译器创建so，并通过asset（资源）方式，实现捆绑so发布。但这种方式只能属于取巧的方式，并非官方支持。所以，在NDK出来之前，我们将“Java+C”的开发模式称之为灰色模式，即官方既不声明“支持这种方式”，也不声明“不支持这种方式”。
#2.2、误解二：有了NDK，我们可以使用纯C开发Android应用
Android SDK采用Java语言发布，把众多的C开发人员排除在第三方应用开发外（注意：我们所有讨论都是基于“第三方应用开发”，Android系统基于Linux，系统级别的开发肯定是支持C语言的。）。NDK的发布，许多人会误以为，类似于Symbian、WM，在Android平台上终于可以使用纯C、C++开发第三方应用了！其实不然，NDK文档明确说明：it is not a good way。因为NDK并没有提供各种系统事件处理支持，也没有提供应用程序生命周期维护。此外，在本次发布的NDK中，应用程序UI方面的API也没有提供。至少目前来说，使用纯C、C++开发一个完整应用的条件还不完备。


##3、NDK是什么
对NDK进行了粗略的研究后，我对“NDK是什么”的理解如下：
**1、NDK****是一系列工具的集合。**
- NDK提供了一系列的工具，帮助开发者快速开发C（或C++）的动态库，并能自动将so和java应用一起打包成apk。这些工具对开发者的帮助是巨大的。
- NDK集成了交叉编译器，并提供了相应的mk文件隔离CPU、平台、ABI等差异，开发人员只需要简单修改mk文件（指出“哪些文件需要编译”、“编译特性要求”等），就可以创建出so。
- NDK可以自动地将so和Java应用一起打包，极大地减轻了开发人员的打包工作。

**2、NDK****提供了一份稳定、功能有限的****API****头文件声明。**
Google明确声明该API是稳定的，在后续所有版本中都稳定支持当前发布的API。从该版本的NDK中看出，这些API支持的功能非常有限，包含有：C标准库（libc）、标准数学库（libm）、压缩库（libz）、Log库（liblog）。


 
##4、NDK带来什么
**1、NDK****的发布，使****“Java+C”****的开发方式终于转正，成为官方支持的开发方式。**
- 使用NDK，我们可以将要求高性能的应用逻辑使用C开发，从而提高应用程序的执行效率。
- 使用NDK，我们可以将需要保密的应用逻辑使用C开发。毕竟，Java包都是可以反编译的。
- NDK促使专业so组件商的出现。（乐观猜想，要视乎Android用户的数量）

**2、NDK****将是****Android****平台支持****C****开发的开端。**
NDK提供了的开发工具集合，使开发人员可以便捷地开发、发布C组件。同时，Google承诺在NDK后续版本中提高“可调式”能力，即提供远程的gdb工具，使我们可以便捷地调试C源码。在支持Android平台C开发，我们能感觉到Google花费了很大精力，我们有理由憧憬“C组件支持”只是Google Android平台上C开发的开端。毕竟，C程序员仍然是码农阵营中的绝对主力，将这部分人排除在Android应用开发之外，显然是不利于Android平台繁荣昌盛的。

