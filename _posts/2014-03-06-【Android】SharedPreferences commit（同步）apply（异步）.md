---
layout: post
title: "【Android】SharedPreferences commit（同步）apply（异步）"
date: 2014-03-06 18:35:00 
comments: true
categories: []
tags: []
description: "【Android】SharedPreferences commit（同步）apply（异步）"
keywords: 
---


 
  
   Unlike
  
  
   
    commit()
   
  
  
   ,
 which writes its preferences out to persistent storage synchronously,
  
  
   
    apply()
   
  
  
   commits
 its changes to the in-memory
  
  
   
    SharedPreferences
   
  
  
   immediately
 but starts an asynchronous commit to disk and you won't be notified of any failures. If another editor on this
  
  
   
    SharedPreferences
   
  
  
   does
 a regular
  
  
   
    commit()
   
  
  
   while
 a
  
  
   
    apply()
   
  
  
   is
 still outstanding, the
  
  
   
    commit()
   
  
  
   will
 block until all async commits are completed as well as the commit itself.
  
 


