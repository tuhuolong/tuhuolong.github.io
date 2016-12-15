---
layout: post
title: "VirtualAlloc和VirtualCopy----VirtualFree"
date: 2011-03-11 16:16:00 
comments: true
categories: [wince]
tags: [wince]
description: "VirtualAlloc和VirtualCopy----VirtualFree"
keywords: wince
---


 
  
   VirtualAlloc 和VirtualCopy
  
  
  VirtualAlloc 首先会从我们的虚拟地址空间中申请(或者说预留)一块虚拟空间，准备接下来要用它。注意此时，可用的物理内存并没有减少，只是虚拟地址少了一块可用的区域。
  
  真正把这块之前reserved的虚拟空间映射到物理的内存区域就是由VirtualCopy来干的，此时，MMU的页表就会增加一个entry，来表示物理--虚拟的映射关系。
  
 
 
  
   VirtualFree
  
 
 
  VirtualFree 释放内存(映射->;分配/分配->;空闲/映射->;空闲)
  
  
  
   再说动态虚拟内存映射和静态虚拟内存映射
  
  
  OEMaddressTable只是建立了一个一级的
  
   静态
  
  虚拟--物理的映射关系，一般给kernel（NK.EXE）通过直接访问的形式来用(OALPAtoVA)；
  
  MmMapIOSpace（VirtualAlloc +VirtualCopy）是一种动态虚拟映射的手段，一般给驱动根据当前需要（对硬件操作）动态申请并建立映射。
 
 
 
 
 
 
  那就是说VirtualCopy 可以直接把物理内存映射到虚拟内存中
  
  而OEMaddressTable所建立的静态虚拟--物理的映射不是必须的
  
  
  也就是说比如对寄存器的映射：
  
  可以通过VirtualAlloc申请（预留）一块虚拟内存给寄存器（如：v_pIOPregs）
  
  然后用
  
  VirtualCopy直接把寄存器的物理地址映射到刚才申请（预留）的虚拟地址空间
  
  
  而在OEMaddressTable中所做的寄存器物理地址与虚拟地址的映射这步可以省略
  
  
  不是省略不省略的问题，是这两种方法是在不同的场合下使用的，如2楼说的，内核部分就要用这个OEMaddressTable来转换，wince5.0下 用户态的驱动（wince6.0驱动貌似是在内核态）就要用这个VirtualAlloc+VirtualCopy来进行地址映射。
  
  
  OEMaddressTable的定义，你可以查看 bsp/src/inc/oemaddrtab_cfg.inc ，那个g_oalAddressTable便是。
 


