---
layout: post
title: v8学习笔记(二) Handle和HandleScope机制
date: 2010-09-03 15:48:00
categories: [JavaScript]
tags: [api, object]
---
**1.Handle****结构**
  (1)API Handle(v8::handle):含有一个T*对象指针，(实际上是相应内部对象指针的指针)
  (2)内部Handle(v8::internal::handle):T**内部对象指针的指针
**2.****相互转换:**
  (1)API Handle->内部Handle
         v8::internal::Handle<v8::internal::To> Utils::OpenHandle(const v8::From* that) 
         { 
               Return v8::internal::Handle<v8::internal::To>( 
                                                  reinterpret_cast<v8::internal::To**>(const_cast<v8::From*>(that))
                                                                                                                ); 
        }
         将API Handle中的API对象指针强制转化为内部对象指针的指针,创建内部Handle并返回
 
v8::internal::Handle<v8::internal::Object2> Utils::OpenHandle(* v8::Handle<v8::Object1>);   
   
  (2)内部Handle->API Handle
         Local<v8::To> Utils::ToLocal(v8::internal::Handle<v8::internal::From> obj) 
         { 
               ASSERT(obj.is_null() || !obj->IsTheHole());                             
               return Local<To>(reinterpret_cast<To*>(obj.location()));                
       }        
         将内部Handle中的内部对象指针的指针强制转化为API Handle中API对象指针,创建API Handle并返回
 
Local<v8::Object1> Utils::ToLocal(v8::internal::Handle<v8::internal::Object2>);  
 
**3.API Handle**
         (1)Local Handle(局部句柄)
         (2)Persistent Handle(持久句柄)
         Local Handle对应的内部对象指针会被放入HandleScope类中的Static HandleScopeData中,在HandleScope销毁的时候会恢复覆盖(清空)HandleScopeData，使得内部对象成为垃圾，被回收。而Persistent Handle对应的内部对象指针不放入Static HandleScopeData,内部对象指针的指针直接保存在Persistent Handle中,由用户自己销毁内部对象、释放内存。
**4.API Handle****的使用**
**  **函数调用: 
         (1)重载API Handle的->操作符,返回API对象指针(内部对象指针的指针)
                   inline T* operator->() const { return val_; } 
         (2)OpenHandle转换为内部句柄
         (3)重载内部Handle的->或者*操作符,返回内部对象指针
         (4)开始调用
 
 
**(****三****)v8 HandleScope****机制******
**1.****概述**
         HandleScope其实就是一个HandleScopeData的备份。
         HandleScopeData:内部对象指针的数组
**2.HandleScope****结构**
         (1)内部HandleScope
         static v8::ImplementationUtilities::HandleScopeData current_;当前HandleScopeData
         const v8::ImplementationUtilities::HandleScopeData previous_;备份
         (2)API HandleScope
                   HandleScopeData previous_;备份
 
         API HandleScope和内部HandleScope共用static HandleScopeData;
**3.****运行过程**
         创建API HandleScope时，备份当前static HandleScopeData到API HandleScope中的previous_。之后创建的对象的指针放入static HandleScopeData中,当销毁(析构)API HandleScope时,恢复static HandleScopeData为之前的备份。清空了期间创建的对象的指针,由垃圾回收器回收对象。
         内部HandleScope同理.
