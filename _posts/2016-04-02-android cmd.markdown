---
layout: post
title: "android cmd"
date: 2016-04-02 04:03:00 
comments: true
categories: []
tags: []
description: "android cmd"
keywords: 
---


 
  
   
    keytool -list -printcert -jarfile
   
   查看证书
  
  
   
    select * from instanceof android.app.Activity
   
   内存泄露
  
  
   
    adb logcat -v time | grep ActivityManager
   
   页面启动时间
  
  
   
    adb logcat -v time | grep AndroidRuntime
   
   crash log
  
  
   
    adb logcat -v time | grep "D\/Dalvik"
   
  
  
   
    adb logcat -v time | grep "I\/art"
   
  
  
   
    adb shell "kill -s SIGUSR1 12345"
   
   手动gc
  
  
   
    Log.getStackTraceString(new Throwable())
   
  
  
   
    NDK_ROOT/toolchains/arm-linux-androideabi-4.9/.../arm-linux-androideabi-readelf -Ws xxx.so
   
  
  
   
    NDK_ROOT/toolchains/arm-linux-androideabi-4.9/.../arm-linux-androideabi-nm -gC xxx.so
   
  
  
   
  
  
   
    android list sdk --extended --proxy-host test.com --proxy-port 1234
   
  
  
   
    android update sdk --no-ui -t 1,2,3 --proxy-host test.com --proxy-port 1234
   
  
  
   
    gradle -Dhttp.proxyHost=test.com -Dhttp.proxyPort=1234 -Dhttps.proxyHost=test.com -Dhttps.proxyPort=1234
   
   
    
   
  
  
   备用
  
  
   
    android list sdk --all --extended --proxy-host test.com --proxy-port 1234
   
  
  
   
    android update sdk --no-ui --all --filter build-tools-23.0.3 --proxy-host test.com --proxy-port 1234
   
  
  
   
    adb shell monkey -p your.app.package.name -c android.intent.category.LAUNCHER 1
   
  
  
   
  
  
   
    setprop ctl.start [cmd]
   
   setprop ctl.start bootanim/bugreport
  
  
   
    adb shell bugreport >; ~/tmp/bugreport.txt
   
  
  "FATAL EXCEPTION:"
"Process: com.xiaomi.smarthome, PID:"
  miui:*#*#284#*#*     (bugreport->;dumpstate->;dumpsys)
  
   
    gradle -Dorg.gradle.jvmargs="-Xmx8192m -XX:MaxPermSize=6144m"
   
  
  
   
    hprof-conv xxx.hprof xxx.std.hprof
   
  
  
   
    adb remount 或者 mount -o rw,remount yaffs2 /system
   
  
  
   
    aapt s -i xxx.9.png -o xxx.png
   
   编译点9图
  
  
   
    getprop net.dns1
   
  
  
   
    echo <password>; | sudo -S networksetup -setautoproxystate Wi-Fi on
   
  
  
   
    echo <password>; | sudo -S networksetup -setautoproxyurl Wi-Fi http://xxxx.pac
   
  
  
   
    echo <password>; | sudo -S networksetup -setwebproxystate 'Wi-Fi' on
   
  
  
   
    echo <password>; | sudo -S networksetup -setsecurewebproxystate 'Wi-Fi' off
   
  
  
   
  
  
   
    gradle assembleRelease
   
   生成未签名包
  
  
   
  
  
   
    repo init -u https://android.googlesource.com/platform/manifest -b
   
  
 
 
  $(function () {
                $('pre.prettyprint code').each(function () {
                    var lines = $(this).text().split('\n').length;
                    var $numbering = $('').addClass('pre-numbering').hide();
                    $(this).addClass('has-numbering').parent().append($numbering);
                    for (i = 1; i ').text(i));
                    };
                    $numbering.fadeIn(1700);
                });
            });
 


