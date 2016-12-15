---
layout: post
title: "【Android】刷Rom:adb reboot bootloader"
date: 2014-07-31 10:19:00 
comments: true
categories: []
tags: []
description: "【Android】刷Rom:adb reboot bootloader"
keywords: 
---


 
  进入刷机模式（fastboot模式）：
 
 $ adb reboot bootloader(使手机进入刷机状态)

 
  刷system，userdata, boot：
 
 $ fastboot flash system out/target/product/pisces/system.img
$ fastboot flash userdata out/target/product/pisces/userdata.img
$ fastboot flash boot out/target/product/pisces/boot.img

 
  重启手机
 
 $ fastboot reboot


