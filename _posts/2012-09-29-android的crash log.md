---
layout: post
title: android的crash log
date: 2012-09-29 10:58:00
categories: [Android]
tags: [crash, android, c]
---
10-11 20:47:24.577: INFO/DEBUG(61):          #00  pc 00018f60  /system/lib/libc.so
10-11 20:47:24.577: INFO/DEBUG(61):          #01  pc 00019216  /system/lib/libc.so
10-11 20:47:24.577: INFO/DEBUG(61):          #02  pc000029fe  /system/lib/libxxxCamera.so
10-11 20:47:24.577: INFO/DEBUG(61):          #03  pc 00002bd4  /system/lib/libxxxCamera.so
10-11 20:47:24.577: INFO/DEBUG(61):          #04  pc 00002f72  /system/lib/libxxxCamera.so
10-11 20:47:24.577: INFO/DEBUG(61):          #05  pc 00002fd6  /system/lib/libxxxCamera.so
10-11 20:47:24.577: INFO/DEBUG(61):          #06  pc 00003018  /system/lib/libxxxCamera.so
10-11 20:47:24.577: INFO/DEBUG(61):          #07  pc 00017df4  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #08  pc 000495c4  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #09  pc 00042178  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #10  pc 0004ed9a  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #11  pc 0001cff4  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #12  pc 000220a4  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #13  pc 00020f9c  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #14  pc 0005fafc  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #15  pc 0005fd10  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #16  pc 000540e2  /system/lib/libdvm.so
10-11 20:47:24.577: INFO/DEBUG(61):          #17  pc 000118e4  /system/lib/libc.so
10-11 20:47:24.577: INFO/DEBUG(61):          #18  pc 000114b0  /system/lib/libc.so

/home/xufan/android/android-1.5/prebuilt/linux-x86/toolchain/arm-eabi-4.2.1/bin/arm-eabi-addr2line -e /home/xufan/android/android-1.5/out/target/product/generic/symbols/system/lib/libxxxCamera.so 000029fe

