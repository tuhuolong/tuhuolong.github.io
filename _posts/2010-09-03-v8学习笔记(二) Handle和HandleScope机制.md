---
layout: post
title: "v8学习笔记(二) Handle和HandleScope机制"
date: 2010-09-03 15:48:00 
comments: true
categories: [api]
tags: [api]
description: "v8学习笔记(二) Handle和HandleScope机制"
keywords: api
---


 
  
   
    
     
      1.Handle
     
    
   
   
    
     结构
    
   
  
 
 
  
   
    
     
     
     (1)API Handle(v8::handle):
    
   
   
    含有一个
   
   
    
     T*
    
   
   
    对象指针，
   
   
    
     (
    
   
   
    实际上是相应内部对象指针的指针
   
   
    
     )
    
   
  
 
 
  
   
    
     
     
     (2)
    
   
   
    内部
   
   
    
     Handle(v8::internal::handle):T**
    
   
   
    内部对象指针的指针
   
  
 
 
  
   
    
     
      2.
     
    
   
   
    
     相互转换
    
    
     
      :
     
    
   
  
 
 
  
   
    
     
     
     (1)API Handle->;
    
   
   
    内部
   
   
    
     Handle
    
   
  
 
 
  
   
    
    
    v8::
    
     internal
    
    ::Handle<v8::
    
     internal
    
    ::To>; Utils::OpenHandle(
    
     const
    
    v8::From* that)
   
  
 
 
  
   
    
    
    {
   
  
 
 
  
   
    
    
    
    
    
     Return
    
    v8::
    
     internal
    
    ::Handle<v8::
    
     internal
    
    ::To>;(
   
  
 
 
  
   
    
    
    
    
    
     reinterpret_cast
    
    <v8::
    
     internal
    
    ::To**>;(
    
     const_cast
    
    <v8::From*>;(that))
   
  
 
 
  
   
    
    
    );
   
  
 
 
  
   
    
    
    
    
    }
   
  
 
 
  
   
    
    
   
  
  
   将
  
  
   
    API Handle
   
  
  
   中的
  
  
   
    API
   
  
  
   对象指针强制转化为内部对象指针的指针
  
  
   
    ,
   
  
  
   创建内部
  
  
   
    Handle
   
  
  
   并返回
  
 
 
  
   
   
  
 
 
  
   
    
     v
     
      8::internal::Handle<v8::internal::Object2
     
     >;
    
    
     Utils::OpenHandle(* v8::Handle<v8::Object1>;);
     
     
    
   
  
 
 
  
   
    
     
     
    
   
  
 
 
  
   
    
     
     
     (2)
    
   
   
    内部
   
   
    
     Handle->;API Handle
    
   
  
 
 
  
   
    
    
    Local<v8::To>; Utils::ToLocal(v8::
    
     internal
    
    ::Handle<v8::
    
     internal
    
    ::From>; obj)
   
  
 
 
  
   
    
    
    {
   
  
 
 
  
   
    
    
    
    
    ASSERT(obj.is_null() || !obj->;IsTheHole());
    
    
   
  
 
 
  
   
    
    
    
    
    
     return
    
    Local<To>;(
    
     reinterpret_cast
    
    <To*>;(obj.location()));
    
    
   
  
 
 
  
   
    
    
    
    
    }
    
    
   
  
 
 
  
   
    
    
   
  
  
   将内部
  
  
   
    Handle
   
  
  
   中的内部对象指针的指针强制转化为
  
  
   
    API Handle
   
  
  
   中
  
  
   
    API
   
  
  
   对象指针
  
  
   
    ,
   
  
  
   创建
  
  
   
    API Handle
   
  
  
   并返回
  
 
 
  
   
   
  
 
 
  
   
    
     Local<v8::Object1>; Utils::ToLocal(v8::internal::Handle<v8::internal::Object2>;);
    
    
     
     
    
   
  
 
 
  
   
   
  
 
 
  
   
    
     
      3.API Handle
     
    
   
  
 
 
  
   
    
     
     
     (1)Local Handle(
    
   
   
    局部句柄
   
   
    
     )
    
   
  
 
 
  
   
    
     
     
     (2)Persistent Handle(
    
   
   
    持久句柄
   
   
    
     )
    
   
  
 
 
  
   
    
     
     
     Local Handle
    
   
   
    对应的内部对象指针会被放入
   
   
    
     HandleScope
    
   
   
    类中的
   
   
    
     Static HandleScopeData
    
   
   
    中
   
   
    
     ,
    
   
   
    在
   
   
    
     HandleScope
    
   
   
    销毁的时候会恢复覆盖
   
   
    
     (
    
   
   
    清空
   
   
    
     )HandleScopeData
    
   
   
    ，使得内部对象成为垃圾，被回收。而
   
   
    
     Persistent Handle
    
   
   
    对应的内部对象指针不放入
   
   
    
     Static HandleScopeData,
    
   
   
    内部对象指针的指针直接保存在
   
   
    
     Persistent Handle
    
   
   
    中
   
   
    
     ,
    
   
   
    由用户自己销毁内部对象、释放内存。
   
  
 
 
  
   
    
     
      4.API Handle
     
    
   
   
    
     的使用
    
   
  
 
 
  
   
    
     
      
      
      
      
     
    
   
   
    函数调用
   
   
    
     :
    
   
  
 
 
  
   
    
     
     
     (1)
    
   
   
    重载
   
   
    
     API Handle
    
   
   
    的
   
   
    
     ->;
    
   
   
    操作符
   
   
    
     ,
    
   
   
    返回
   
   
    
     API
    
   
   
    对象指针
   
   
    
     (
    
   
   
    内部对象指针的指针
   
   
    
     )
    
   
  
 
 
  
   
    
     
     
     inline T* operator->;() const { return val_; }
    
   
  
 
 
  
   
    
     
     
     (2)OpenHandle
    
   
   
    转换为内部句柄
   
  
 
 
  
   
    
     
     
     (3)
    
   
   
    重载内部
   
   
    
     Handle
    
   
   
    的
   
   
    
     ->;
    
   
   
    或者
   
   
    
     *
    
   
   
    操作符
   
   
    
     ,
    
   
   
    返回内部对象指针
   
  
 
 
  
   
    
     
     
     (4)
    
   
   
    开始调用
   
  
 
 
  
   
   
  
 
 
  
   
   
  
 
 
  
   
    
     (
    
   
  
  
   
    三
   
  
  
   
    
     )v8 HandleScope
    
   
  
  
   
    机制
   
  
  
  
 
 
  
   
    
     
      1.
     
    
   
   
    
     概述
    
   
  
 
 
  
   
    
     
     
     HandleScope
    
   
   
    其实就是一个
   
   
    
     HandleScopeData
    
   
   
    的备份。
   
  
 
 
  
   
    
     
     
     HandleScopeData:
    
   
   
    内部对象指针的数组
   
  
 
 
  
   
    
     
      2.HandleScope
     
    
   
   
    
     结构
    
   
  
 
 
  
   
    
     
     
     (1)
    
   
   
    内部
   
   
    
     HandleScope
    
   
  
 
 
  
   
   
   static
  
  
   v8::ImplementationUtilities::HandleScopeData current_;
  
  
   当前
   
    HandleScopeData
   
  
 
 
  
   
   
   
   
   
    const
   
   v8::ImplementationUtilities::HandleScopeData previous_;
  
  
   备份
  
  
   
    
    
   
  
 
 
  
   
    
     
     
     (2)API HandleScope
    
   
  
 
 
  
   
    
    
   
  
  
   HandleScopeData previous_;
  
  
   备份
  
 
 
  
   
   
  
 
 
  
   
    
     
     
     API HandleScope
    
   
   
    和内部
   
   
    
     HandleScope
    
   
   
    共用
   
   
    
     static HandleScopeData;
    
   
  
 
 
  
   
    
     
      3.
     
    
   
   
    
     运行过程
    
   
  
 
 
  
   
    
     
     
    
   
   
    创建
   
   
    
     API HandleScope
    
   
   
    时，备份当前
   
   
    
     static HandleScopeData
    
   
   
    到
   
   
    
     API HandleScope
    
   
   
    中的
   
   
    
     previous_
    
   
   
    。
   
   
   
   
    之后创建的对象的指针放入
   
   
    
     static HandleScopeData
    
   
   
    中
   
   
    
     ,
    
   
   
    当销毁
   
   
    
     (
    
   
   
    析构
   
   
    
     )API HandleScope
    
   
   
    时
   
   
    
     ,
    
   
   
    恢复
   
   
    
     static HandleScopeData
    
   
   
    为之前的备份。清空了期间创建的对象的指针
   
   
    
     ,
    
   
   
    由垃圾回收器回收对象。
   
  
 
 
  
   
   
  
  
   内部
  
  
   HandleScope
  
  
   同理
  
  
   .
  
 


