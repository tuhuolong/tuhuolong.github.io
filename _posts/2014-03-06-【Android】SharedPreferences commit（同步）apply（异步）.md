---
layout: post
title: 【Android】SharedPreferences commit（同步）apply（异步）
date: 2014-03-06 18:35:00
categories: [Android]
tags: []
---
Unlike `[commit()](http://developer.android.com/reference/android/content/SharedPreferences.Editor.html#commit())`,
 which writes its preferences out to persistent storage synchronously, `[apply()](http://developer.android.com/reference/android/content/SharedPreferences.Editor.html#apply())` commits
 its changes to the in-memory `[SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html)`immediately
 but starts an asynchronous commit to disk and you won't be notified of any failures. If another editor on this `[SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html)` does
 a regular`[commit()](http://developer.android.com/reference/android/content/SharedPreferences.Editor.html#commit())` while
 a `[apply()](http://developer.android.com/reference/android/content/SharedPreferences.Editor.html#apply())` is
 still outstanding, the `[commit()](http://developer.android.com/reference/android/content/SharedPreferences.Editor.html#commit())` will
 block until all async commits are completed as well as the commit itself.