---
layout: post
title: "onSaveInstanceState在系统之后可能会杀掉Activity时调用,onStop之前"
date: 2013-03-28 21:35:00 
comments: true
categories: []
tags: []
description: "onSaveInstanceState在系统之后可能会杀掉Activity时调用,onStop之前"
keywords: 
---


 
  Note the "Killable" column in the above table -- for those methods that are marked as being killable, after that method
 returns the process hosting the activity may killed by the system
 
 
  at any time
 
 
  without
 another line of its code being executed
 


