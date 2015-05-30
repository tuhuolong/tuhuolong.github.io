---
layout: post
title: Oolong and Gnoloo
date: 2010-10-20 16:11:00
categories: [Java]
tags: [汇编, class, java, jvm, 工具, 语言]
---
**Oolong:一种汇编语言, .j文件可以编译成.class文件**
Oo long乌狼**
**
**Oolong is closer to the JVM class file format than Java**
 
(1)directive(指示)
(2)instruction(Oolong指令)
 
 
 
**What is Oolong and Gnoloo?**
Oolong是一种为Java虚
拟机定制的汇编语言，其作者为Joshua Engel，Gnoloo则是将.class文件转成Onlong语言的一种反汇编工具。引用Joshua
 Engel书中的一段描述“the Oolong language is nearly 
equivalent to the class file format but easier to 
read and write.”
 
一般用Oolong写成的程序代码都保存在扩展名为.j的文件中。如
HelloOolong.j  -------
 经过Oolong转换 ------ 〉  HelloOolong.class
HelloOolong.class 
 ------- 经过Gnoloo转换 ------ 〉 
HelloOolong.j
 
我是在浏览《Programming for the Java™ Virtual Machine》这本书的时候遇到这些的，好奇驱使着我深入的了解一下Oolong这种汇编语言。在这里我不想说Oolong的指令与语法，我想Oolong和Gnoloo可以作为一种工具，来帮助Java学
习者了解Java程序的运行或者说看看JVM是
如何运行一个一个.class文件的。如果真的对之很是感兴趣的话或者说是一个super fans，你可以学习一下Oolong，并用Oolong写出一些可以运行在JVM上
的程序。
 
**下载Oolong****和Gnoloo**
Oolong和Gnoloo是由Joshua Engel编写的汇
编语言及工具，我们可以通过下面的链接下载到相关包：
[http://www.scifac.ru.ac.za/resourcekit/download/Engel.zip](http://www.scifac.ru.ac.za/resourcekit/download/Engel.zip)
 
解压后进入Engel目
录下，有三个重要的class文件Oolong.class、Gnoloo.class和DumpClass.class。
 
**编译和反编译Onlong****程序**
Now！
我们使用Oolong语法写一个“Hello Oolong!”程序（该程序改变自《Programming for the 
Java™ Virtual Machine》一书的2.1小节的那个“Hello World！”程序），代码如下：
 
//HelloOolong.j代码：
.class public HelloOolong
.super java/lang/Object
 
.method public static main([Ljava/lang/String;)V
.limit stack 2
.limit locals 1
   getstatic java/lang/System/out 
Ljava/io/PrintStream;
   ldc "Hello Oolong!"
   invokevirtual 
java/io/PrintStream/println
      (Ljava/lang/String;)V
   return
.end method
.end class
 
我们不关心具体的语法和指令。
打开控制台，进入该目录运行：
java Oolong HelloOolong.j 在当前目录下会产生HelloOolong.class文件
运行该class文
件我们会看到控制台输出：
Hello Oolong!
 
下面我们利用Gnoloo反编译class文件：（前提删除HelloOolong.j文件）
java Gnoloo HelloOolong.class  在当前目录下会产生HelloOolong.j文件
 
Oolong和Gnoloo为我们提供了一套查看JVM汇
编代码的解决方案，我们可以将由javac生成的class文件反汇编为Oolong汇编代码。
如：HelloOolong.java ----- javac----------> HelloOolong.class 
--------Gnoloo-----> HelloOolong.j
之后我们便可以查看汇编代码，然后了解我们的代码时如何在JVM上运行的了。
 
**DumpClass****工具**
在下载的包中还有一个很好用的工具，那就是DumpClass，它可以输出class文件的信息。使用
方法：java DumpClass
 HelloOolong.class， class文件的信息就会被输出到屏幕上。
 
**查看Java****字节码(****汇编码)****的另一种方法**
javap –c –verbose 
HelloOolong
javap是JDK自带的工具。

