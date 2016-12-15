---
layout: post
title: "【JS】angular和require的区别"
date: 2014-02-28 01:45:00 
comments: true
categories: []
tags: []
description: "【JS】angular和require的区别"
keywords: 
---


 
  
   Require and DI are similar concepts. Because they are similar developers often get confused and they think they can substituted one for the other, but in reality they are complementary.
  
 
 
  
   require.js is concerned with loading the right code into the VM in the right order once before the application can run.
   
    DI is concerned by instantiating the right classes in the right order many times during the application runtime.
   
  
 
 
  
   The key differences are loading vs instantiating, classes vs instances, as well as once before application start vs many times during application runtime.
  
 
 
  
   One needs to load the code before the code can be instantiated, hence the complementary nature of the two.
  
 
 
  
   There is also a difference of scope and granularity. Require.js works on file level, and while you can essentially link different files at lead-time, once the files are linked, the relationships are fixed.
  
 
 
  
   DI works on class level. It answers a question such as Class A needs Interface B, where do I get B from. While require.js can substituted different B code at load time, DI can chose among different instance of same
 B class or different subclass based of the context of where A is instantiated.
  
 


