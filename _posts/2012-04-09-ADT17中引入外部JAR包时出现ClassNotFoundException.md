---
layout: post
title: "ADT17中引入外部JAR包时出现ClassNotFoundException"
date: 2012-04-09 20:22:00 
comments: true
categories: [jar]
tags: [jar]
description: "ADT17中引入外部JAR包时出现ClassNotFoundException"
keywords: jar
---


 
  在ADT17之前，如果要在Android项目中引入外部JAR包，只需在项目属性的Java Build Path中选择Add External JARs，然后选择相应JAR包即可。但是，在升级到ADT17之后，Android项目下多出了一个Android Dependencies目录，之前通过Add External JARs加入的jar包依然出现在Referenced Libraries目录之下，但是似乎已经被弃用，虽然没有提示任何错误，但是在运行时会抛出ClassNotFoundException。
 
 
  解决方法是将JAR包加入到Android Dependencies下，其中一个默认引入目录是libs/，因此只要将JAR包复制或者symbolic link到libs/目录下即可。
 


