---
layout: post
title: "【Android】mac编译android-4.4.2"
date: 2014-08-19 00:13:00 
comments: true
categories: []
tags: []
description: "【Android】mac编译android-4.4.2"
keywords: 
---


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

 (5)unset NDK_ROOT
 (6). build/envsetup.sh
 (7)lunch
 (8)make clobber

 (9)make -j8
 (10)adb reboot bootloader
 (11)fastboot flashall -w


