---
layout: post
title: 【Android】mac编译android-kernel-hammerhead
date: 2014-09-17 03:26:00
categories: [Android]
tags: []
---

	export PATH=$(ANDROID_ROOT)/prebuilts/gcc/darwin-x86/arm/arm-eabi-4.6/bin:$PATH
	sudo cp -a /Volumes/android/external/elfutils/libelf/elf.h /usr/include
	
	include/linux/netfilter/xt_tcpmss.h -> include/linux/netfilter/xt_TCPMSS.h
	net/netfilter/xt_tcpmss.c -> net/netfilter/xt_TCPMSS.c
	
	$ export ARCH=arm
	$ export SUBARCH=arm
	$ export CROSS_COMPILE=arm-eabi-$ make hammerhead_defconfig
	$ make


