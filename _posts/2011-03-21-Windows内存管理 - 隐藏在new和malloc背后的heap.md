---
layout: post
title: "Windows内存管理 - 隐藏在new和malloc背后的heap"
date: 2011-03-21 14:08:00 
comments: true
categories: [windows]
tags: [windows]
description: "Windows内存管理 - 隐藏在new和malloc背后的heap"
keywords: windows
---


 
  先来说，heap是什么，heap就是堆，在不知道具体细节的时候，我们只知道，通过new和malloc，我们可以动态获得一个内存区域，用来存放自己的对象和变量，而这些内存区域都是在heap上的。heap应该就是一个内存区域吧。应该会有很多人这么认为过。
  
   后来，学习了Windows的内存模型和内存管理机制，知道了地址空间，明白了Windows（32bit）给每个进程一个假象，就好像是进程拥有一个大到4G的内存空间。这个就是虚拟的地址空间，程序在这个地址空间上访问数据，而这个数据的在物理内存中的真实位置是不确定的，可能在物理内存的某个页中，也可能因为换页，存放在页面文件中。Windows的核心管理着虚拟地址空间和物理内存的对应关系，而作为程序员，我们所见到，只有虚拟地址而已。
   
    操作系统的内存管理机制的绝妙之处在于，一个进程可以使用的地址空间很大，但是除非这些地址被保留后提交，否则就不会占用真实的物理内存。翻翻MSDN，发现以下这些API：
    
     VirtualAlloc
     
      VirtualAllocEx
      
       VirtualFree
       
        VirtualFreeEx
        
         VirtualLock
         
          VirtualProtect
          
           VirtualProtectEx
           
            VirtualQuery
            
             VirtualQueryEx
             
              VirtualUnlock
             
            
           
          
         
        
       
      
     
    
   
  
 
 
  
   VirtualAlloc和VirtualAllocEx用来在地址空间上保留和提交一个区域。
  
 
 
  VirtualFree和VirtualFreeEx用来释放或者反提交一个区域。
 
 
  VirtualLock用于将一个地址空间上的区域锁在物理内存上，也就是防止因为换页而导致的这个区域的数据进入页面文件。 VirtualUnlock的作用正相反，是将锁到物理内存上的地址区域解锁，使之允许被换页出物理内存。
 
 
  VirtualProtect和VirtualProtectEx用于设置一个地址区域的保护属性。
 
 
  VirtualQuery和VirtualQueryEx用于获取一个地址区域的额外信息。
 
 
  
   这些函数是Windows API中，我们能够接触到的，内存分配的最核心的API了，我们可以对内存分配的过程的许多细节做精细的控制，要考虑的内容也很多，比如页的大小，内存分配粒度问题。Windows上所有的内存分配都要用到这些函数。
   
    既然内存分配的API都有了，new和malloc有是怎么回事。
    
     首先，C和C++都是内核非常精简的语言，关键字很少，很多功能都是通过库函数进行扩充的，而C和C++的运行时库是这些库中最不可或缺的一个库，它负责在C和C++编写的程序开始执行之前，初始化一个可供程序运行的环境（这个环境包括栈、堆、输入输出和环境变量等等），而malloc是运行时库中负责内存分配的函数。
     
      C和C++编程语言要做到跨平台，当然就需要有针对不同平台的运行时库的实现了。malloc函数，要在Windows上面实现，那最终还是需要用到Windows上面的内存分配函数，要在Linux上面实现，那肯定也需要使用Linux的内存分配函数。
      
       new和malloc的区别在于，malloc是运行时库函数，而new是语言的关键字，关键字是在语言的核心进行支持的，目的是支持类的创建和构造。new在生产一个对象实例的时候，必须判断类的构造函数，并准确的调用，而光用mallloc分配内存是不够的。这个是new的特殊之处，但是new的实现中一定也使用了malloc函数。
       
        看似malloc可以直接调用虚拟内存分配的API，申请内存，可是Windows并没有这么做，而是在地址空间上又加了一个层次，这个就是heap，通过heap来管理一个区域的地址空间，这样就可以有多个heap，就像把地址空间分成了若干个小的内存池，彼此之间互不影响。
        
         这样有很多好处，第一，在申请或释放内存的时候，操作系统要查找未分配或已分配的内存区域的链表，这种小型化的内存池相比整个地址空间作为一个完整内存池的方案来说，效率是要高很多的；第二，在多线程运行的环境下，如果线程使用不是同一个heap，那么也就不需要考虑线程同步的问题了，可以提高多线程的效率。其他的好处，恕我愚钝，还没想到。
         
          使用heap的好处，我们平常没有认识到，因为我们太习惯于new和malloc了。有些时候new和malloc不是使用内存的好方案，为什么呢？因为效率。
         
        
       
      
     
    
   
  
 
 
  
   C和C++的运行时库在初始化的时候，会创建一个heap，叫做运行时库heap。是的，没错，在这之后就运行时库不会再创建其他heap了，也就是说，如果你用malloc申请内存，始终是在这个运行时库heap上分配内存，无论是一个字节还是一个大数组，这样一来，这个heap就会很零散。
   
    最要命的是，运行时库heap是一个序列化的heap，所谓序列化，就是说在这个heap上面的的所有操作必须有先后顺序，比如线程A正在用malloc申请内存，这个时候线程B马上也调用了malloc，但是A的调用还没结束，线程B只能等待。在单CPU单核的机器上，性能的差异不明显，在多核多CPU的机器上，这样的等待如果多了，就会明显的影响多线程的效率了。
   
  
 
 
  继续说一些heap的信息。Windows下面，每一个进程都会创建一个默认的heap，初始大小是1MB，Windows的许多函数都需要一些临时的内存块，这些都是从进程的默认heap中分配的。如果是一个C或C++程序，就得加上运行时库heap，这样最起码得有2个heap，但是我试着创建一个控制台程序，用API统计一下heap的数目，竟然有4个heap，不知道其他的heap是什么时候创建的。
  
   对于系统默认的heap的访问必须有线程保护，也就是顺序操作（序列化的heap），这个会浪费一些时间（上面已经说明了）。如果要想使得你的线程以最快的速度访问heap，你需要为线程创建一个只被自己访问的heap，并取消heap的序列化选项。但是默认的heap必定是序列化的。
  
 
 
  
   Windows下面与heap相关API
   
    GetProcessHeap
    
     GetProcessHeaps
     
      HeapAlloc
      
       HeapCompact
       
        HeapCreate
        
         HeapDestroy
         
          HeapFree
          
           HeapLock
           
            HeapQueryInformation
            
             HeapReAlloc
             
              HeapSetInformation
              
               HeapSize
               
                HeapUnlock
                
                 HeapValidate
                 
                  HeapWalk
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
 
 
  
   这里只罗列一下，怎么使用就参考MSDN吧。
  
 
 
 
 
  
   http://blog.csdn.net/zencher/archive/2010/05/13/5588748.aspx
  
 


