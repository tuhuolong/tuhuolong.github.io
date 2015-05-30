---
layout: post
title: mac编译android-5.0.0
date: 2014-11-21 21:42:00
categories: [Android]
tags: []
---
	(0)repo init -u https://android.googlesource.com/platform/manifest -b 
	(1)hdiutil create -type SPARSE -fs 'Case-sensitive Journaled HFS+' -size 100g ~/workspace/android.dmg
	(2)function mountAndroid { hdiutil attach ~/workspace/android.dmg.sparseimage -mountpoint ~/workspace/AndroidDisk; }
	(3)Binaries for Nexus Device
	(4)build/core/combo/HOST_darwin-x86.mk
	# $(1): The file to check
	define get-file-size
	stat -f "%z" $(1)
	endef
	
	
	# $(1): The file to check
	define get-file-size
	GSTAT=$(which gstat) ; \
	if [ ! -z "$GSTAT" ]; then \
	gstat -c "%s" $(1) ; \
	else \
	stat -f "%z" $(1) ; \
	fi
	endef
	(5) 
external/iptables/include/linux/netfilter/xt_dscp.h -> external/iptables/include/linux/netfilter/xt_DSCP.h	(待定)unset NDK_ROOT
	(6). build/envsetup.sh
	(7)lunch
	(8)make -j8
	(9)adb reboot bootloader
	(10)fastboot flashall -w
	单刷
	fastboot flash boot boot.img
	fastboot flash system system.img
	fastboot flash userdata userdata.img
	fastboot reboot
