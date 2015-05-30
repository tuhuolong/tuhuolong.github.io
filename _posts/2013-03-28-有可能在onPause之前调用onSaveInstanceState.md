---
layout: post
title: 有可能在onPause之前调用onSaveInstanceState
date: 2013-03-28 20:07:00
categories: [Android]
tags: []
---
In extreme cases, the system might simply kill your app process without calling the activity's final `[onDestroy()](http://developer.android.com/reference/android/app/Activity.html#onDestroy())` callback,
 so it's important you use `[onStop()](http://developer.android.com/reference/android/app/Activity.html#onStop())` to
 release resources that might leak memory.
系统会杀掉进程而不调用onDestroy,因此最好在onStop释放资源

