---
layout: post
title: "mac编译android-6.0.0"
date: 2015-10-06 18:47:00 
comments: true
categories: []
tags: []
description: "mac编译android-6.0.0"
keywords: 
---


 (0)repo init -u https://android.googlesource.com/platform/manifest -b 

 (1)hdiutil create -type SPARSE -fs 'Case-sensitive Journaled HFS+' -size 100g ~/workspace/android.dmg
 (2)function mountAndroid { hdiutil attach ~/workspace/android.dmg.sparseimage -mountpoint ~/workspace/AndroidDisk; }
 (3)build/core/combo/HOST_darwin-x86.mk
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
 (4) external/iptables/include/linux/netfilter/xt_dscp.h ->; external/iptables/include/linux/netfilter/xt_DSCP.h
 (待定)unset NDK_ROOT
 (5). build/envsetup.sh
 (6)lunch
 (7)Binaries for Nexus Device (uncompress + root dir + run .sh)
 (8)git clone https://github.com/jamesonwilliams/vendor_google_gapps.git vendor/google/gapps
 (9)vendor/moto/shamu/BoardConfigVendor.mk

-include vendor/google/gapps/BoardConfigPartial.mk


 (10)vendor/moto/shamu/device-vendor.mk

$(call inherit-product-if-exists, vendor/google/gapps/device-partial.mk)


 (11)make clobber
 (12)make -j8
 (13)adb reboot bootloader
 (14)fastboot flashall -w
 单刷
fastboot flash boot boot.img
fastboot flash system system.img
fastboot flash userdata userdata.img

fastboot reboot


