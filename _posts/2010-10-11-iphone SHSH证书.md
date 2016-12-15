---
layout: post
title: "iphone SHSH证书"
date: 2010-10-11 00:27:00 
comments: true
categories: [iphone]
tags: [iphone]
description: "iphone SHSH证书"
keywords: iphone
---


 
  
   扫盲：
  
  
   SHSH
  
  
   是简称，全称应该为
  
  
   ECID SHSH
  
  
   或者
  
  
   SHSH Blob
  
  
   或者
  
  
   ECID SHSH Blob
  
  
   。
  
  
  
 
 
  
  
  
   在说
  
  
   SHSH
  
  
   之前，我们先说什么是
  
  
   ECID
  
  
   ，
  
  
   ECID
  
  
   的英文全名是
  
  
   Exclusive Chip ID
  
  
   ，也就是每一台
  
  
   
    
     iphone
    
   
  
  
   、
  
  
   Ipod
  
  
   、
  
  
   Ipad
  
  
   都根据自己的芯片有一个唯一的识别码。这个可以通过在
  
  
   
    
     
      手机
     
    
   
  
  
   查看、查看注册表，用软件读取等多种手段得到。
  
 
 
  
  
  
   然后我们说为什么苹果要做
  
  
   SHSH
  
  
   ，以前的智能手机，大家刷了新版，不好用，就再刷回低版本。以前还无所谓，在
  
  
   Apple
  
  
   出了
  
  
   Iphone
  
  
   之后，这个问题就严重了。
  
  
   
    
     
      苹果
     
    
   
  
  
   的新版本一般都会屏蔽旧版本的越狱，苹果不想让大家越狱，当然苹果就不想让大家再刷回去，所以用了一种新的验证技术，这就是
  
  
   SHSH
  
  
   了。
  
  
   
    SHSH
   
  
  
   是根据每台机的
  
  
   
    ECID
   
  
  
   和当前最新的
   
    版本
   
   经过复杂运算而得出的一个签名文件，
  
  
   SHSH
  
  
   不是存在你的机子上的，是存在
  
  
   Apple
  
  
   的服务器上的。当你刷机的时候，
  
  
   Apple
  
  
   会连上服务器来验证当前你的刷机版本和
  
  
   ECID
  
  
   所产生的
  
  
   SHSH
  
  
   和服务器上的是否匹配，如果不匹配，则不能刷机。
  
 
 
  
  
  
   那么，明白了
  
  
   SHSH
  
  
   是怎么来的，你就会很容易理解以下的问题：
  
 
 
  
  
  
   为什么新版本的程序一出来，就不能备份旧版本的
  
  
   SHSH
  
  
   了，因为一旦
   
    出了新版本，所有的
   
  
  
   
    SHSH
   
  
  
   
    都已经更新了
   
   ，而更新的目的，就是防止你刷回低版本。
  
  
  
  
   如果你不想刷回旧版本，那么
  
  
   Shsh
  
  
   对你是没有任何用处的。
  
  
  
  
   无论你现在用的
  
  
   4.0
  
  
   还是
  
  
   4.0.1
  
  
   ，当你备份你的
  
  
   SHSH
  
  
   的时候，你都只能备份到当前最新的版本的
  
  
   SHSH
  
  
   文件。
  
 
 
  
  
  
   其实不管验证技术有多高明，我们明白了验证机制，我们就可以为所欲为了，自己用一台服务器来代替
  
  
   
    
     
      苹果
     
    
   
  
  
   （通过修改
  
  
   Hosts
  
  
   文件轻易实现），把自己以前备份的
  
  
   SHSH
  
  
   文件放在服务器上，这样就可以刷回以前的老版本了。（这一段是百度来的）
  
 
 
  
   
  
  
   步入正题
  
  
   
  
  
   请确定
  
  
   
    
     
      电脑
     
    
   
  
  
   已按照
  
  
   iTunes
  
  
   客户端，
  
  
   windows
  
  
   系统下。
  
  
   
  
  
   请确定你的
  
  
   iPhone4
  
  
   已经成功越狱。
  
  
   
  
  
   需要用到的软件，
  
  
   umbrella-4.01.03.exe
  
  
   ，百度搜索吧。
  
 
 
  
   1
  
  
   、越狱后进入
  
  
   cydia
  
  
   ，请先点击一下
  
  
   make my life easier,thanks!,
  
  
   其实这一步就已搞定，但是保险起见，建议持续以下操作。
  
  
   
   2
  
  
   、下载
  
  
   umbrella-4.01.03.exe
  
  
   解压到自己的任意文件夹，一般是下面两个文件
  
 
 
  
   
  
  
   
   
  
  
   
   
   
  
 
 
  
   3
  
  
   、打开你的
  
  
   iPhone4
  
  
   ，连接电脑，强烈建议使用原装数据线，然后打开
  
  
   umbrella-4.01.03.exe
  
  
   ，勾选
  
  
   advanced options
  
  
   ，
  
  
   device/version
  
  
   选择
  
  
   iPhone4 4.0.1
  
  
   ，
  
  
   request from
  
  
   ：
  
  
   cydia
  
 
 
  
   
  
  
   
   
  
  
   
   
   
  
 
 
  
   4
  
  
   、
  
  
   save my SHSH
  
  
   ，大概
  
  
   10
  
  
   秒钟即可完成，保存路径一般为
  
  
   C:DocumentsandSettingsAdministrator.shsh
  
  
   格式，备份大小为
  
  
   59.5K
  
 
 
  
   
  
  
   
   
  
 
 
  
   
  
  
   
   
  
  
   
   
   
  
 
 
  
   5
  
  
   、至于后续的降级方法，需要更进一步操作，相对复杂些，首先要建立自己的服务器，
  
  
   stat TSS server
  
  
   ，根据网速状况，可能要持续数分钟时间。具体教程留待以后发布吧，理论上备份过
  
  
   4.0.1SHSH
  
  
   的
  
  
   iPhone4
  
  
   都可以再降级至
  
  
   4.0.1
  
  
   的。
  
  
   
  
 
 
  
   
  
  
   
   
  
 
 
  
  
  
   欧若建议，再下一个越狱软件发布之前，不要贸然升级你的
  
  
   iPhone
  
  
   ，即使升级，请先备份好自己的
  
  
   SHSH
  
  
   。祝大家玩机快乐！
  
 
 
  
   
   
  
 
 
 


