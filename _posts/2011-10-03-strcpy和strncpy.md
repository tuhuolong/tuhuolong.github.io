---
layout: post
title: "strcpy和strncpy"
date: 2011-10-03 17:46:00 
comments: true
categories: [dst]
tags: [dst]
description: "strcpy和strncpy"
keywords: dst
---


 
 
 
  char * __cdecl strcpy(char * dst, const char * src)
 
 
  {
 
 
  char * cp = dst;
 
 
  while( *cp++ = *src++ );
 
 
  return( dst );
 
 
  }
 
 
  
  
 
 
 
 
 
 
  strcpy
 
 
 
 
 
 
  
   源字串
   
    全部拷贝
   
   到目标字串中,
   
    包括'\0'
   
   ，(但是程序员必须保证目标串长度足够，且不与源串重叠)
  
 
 
 
 
 
 
  strncpy
 
 
  
   只拷贝
   
    指定长,
   
   源不足补\0;
  
 
 
  
   
    如果目标长>;=指定长>;源长，则将源串全部拷贝到目标串，连同'\0'
   
  
  
   
    如果指定长<源长，则将截取源串中按指定长度拷贝到目标字符串，不包括'\0'
   
  
  
   
    如果指定长>;目标长，错误!
   
  
 
 
  
   
   
  
  
   
   
  
  
   //源 <= 目标
  
  
   char*p="123456789";
  
 
 
  
  
  
   char name1[12]="aaaaaaaaaaa",
  
  
   char name2[12]="bbbbbbbbbbb";
  
  
   strcpy(name1,p);
  
  
   //遇空字符结束，结果追加1个空字符：123456789\0
  
  
   strncpy(name2,p,5);
  
  
   //若n>;=源，则不足的部分填充\0，结果：123456789\0\0\0
   
    //若n<源例如=5，则只拷贝5个字符，结果：12345bbbbbb\0
   
  
  
  
 
 
 
 
 
 
 
 
 


