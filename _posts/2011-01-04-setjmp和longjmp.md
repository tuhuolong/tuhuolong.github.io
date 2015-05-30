---
layout: post
title: setjmp和longjmp
date: 2011-01-04 13:58:00
categories: [C++]
tags: [语言, c]
---
**编译性语言(C++):分支计算返回---**可能内存泄露或死锁
CPU结构体:用来保存当前运行环境,EAX,EBX...与CPU对应
 
ret = setjmp()  //设置一个返回点:现场(寄存器)保存到CPU结构体----------第一次返回0
if(ret == 0) {
//多核并行计算,提供服务
    CPU1
    ...(服务)...
    longjmp(buf,1);//得到结果,恢复(必须释放掉所有资源)
 
    CPU2
    ...(服务)...
    longjmp(buf,2);//得到结果,恢复(必须释放掉所有资源)
 
    CPU3
    ...(服务)...
    longjmp(buf,3);//得到结果,恢复(必须释放掉所有资源)
 
} else {
    switch(ret){
        case 1:
        case 2:
        ......
    }
}
 
**解释性语言(Java/JS/Python):分支计算返回**
**** 
 
//多核并行计算,提供服务
foo1:
    CPU1
    ...(服务)...
    return;(不需考虑资源释放)
 
foo2: 
    CPU2
    ...(服务)...
    return;(不需考虑资源释放) 
 
foo3:
    CPU3
    ...(服务)...
    return;(不需考虑资源释放) 
 
 
 
 
 
 
