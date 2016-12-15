---
layout: post
title: "TSD(Thread Specific Data)线程专有数据"
date: 2011-02-21 15:06:00 
comments: true
categories: [thread]
tags: [thread]
description: "TSD(Thread Specific Data)线程专有数据"
keywords: thread
---


 
  
   
    
     
      
       (1)全局变量
      
     
    
   
  
 
 
  
   
    
     
      
       (2)局部变量
      
     
    
   
  
 
 
  
   
    
     
      
       (3)TSD(Thread-Specific Data 线程专有数据)
       
       
      
     
    
   
  
 
 
  1.http://upczap.itpub.net/
 
 
  
   
    
     
      在单线程的程序里，有两种基本的数据：全局变量和局部变量。但在多线程程序里，还有第三种数据类型：线程数据（TSD: Thread-Specific Data）。它和全局变量很象，在线程内部，各个函数可以象使用全局变量一样调用它，但它对线程外部的其它线程是不可见的。这种数据的必要性是显而易见 的。例如我们常见的变量errno，它返回标准的出错信息。它显然不能是一个局部变量，几乎每个函数都应该可以调用它；但它又不能是一个全局变量，否则在 A线程里输出的很可能是B线程的出错信息。要实现诸如此类的变量，我们就必须使用线程数据。我们为每个线程数据创建一个键，它和这个键相关联，在各个线程 里，都使用这个键来指代线程数据，但在不同的线程里，这个键代表的数据是不同的，在同一个线程里，它代表同样的数据内容。
     
    
   
  
  
  
  
  
  2.http://bbs.chinaunix.net/viewthread.php?tid=941730
  
  
  linux 多线程编程中引入了Thread-Specific Data(线程相关的数据)的概念
  
  为什么需要"线程相关的数据"呢?怎样使用"线程相关的数据"呢?
  
  
  1. 为什么需要Thread-Specific Data "线程相关的数据"
  
  
  这里只介绍我个人认为的一个原因, 当然它还有许多其他用途,欢迎大家讨论
  
  
  例子:实现同时运行两个线程,对于每个线程,在该线程调用的每个函数中打印线程的名字,以及它正在调用的函数的名字.
  
  
  (下面的例子与实现只是为了说明问题,有些地方可能不妥)
  
  
  不使用"线程相关的数据"的两种实现方法:
  
  
  实现方法1. 不使用全局变量
 
 
  
   #include <string.h>;
   
   #include <pthread.h>;
   
   #define MAXLENGTH 20
   
   
   void another_func (const char * threadName)
   
   {
   
   printf ("%s is running in another_func/n", threadName);
   
   }
   
   
   void * thread_func (void * args)
   
   {
   
   char threadName[MAXLENGTH];
   
   strncpy (threadName, (char *)args, MAXLENGTH-1);
   
   
   printf ("%s is running in thread_func/n", threadName);
   
   another_func (threadName);
   
   
   }
   
   
   int main (int argc, char * argv[])
   
   {
   
   
   pthread_t pa, pb;
   
   pthread_create ( &pa, NULL, thread_func, "Thread A");
   
   pthread_create ( &pb, NULL, thread_func, "Thread B");
   
   
   pthread_join (pa, NULL);
   
   pthread_join (pb, NULL);
   
   }
  
 
 
  
  输出结果为:
  
  Thread A is running in thread_func
  
  Thread A is running in another_func
  
  Thread B is running in thread_func
  
  Thread B is running in another_func
  
  
  该方法的缺点是:由于要记录是哪一个线程在调用函数,每个函数需要一个额外的参数来
  
  记录线程的名字,例如another_func函数需要一个threadName参数
  
  如果调用的函数多了,则每一个都需要一个这样的参数
  
  
  实现方法2. 使用全局变量
 
 
  
   #include <string.h>;
   
   #include <pthread.h>;
   
   #define MAXLENGTH 20
   
   
   char threadName[MAXLENGTH];
   
   pthread_mutex_t sharedMutex=PTHREAD_MUTEX_INITIALIZER;
   
   
   void another_func ()
   
   {
   
   printf ("%s is running in another_func/n", threadName);
   
   }
   
   
   void * thread_func (void * args)
   
   {
   
   pthread_mutex_lock(&sharedMutex);
   
   strncpy (threadName, (char *)args, MAXLENGTH-1);
   
   printf ("%s is running in thread_func/n", threadName);
   
   another_func ();
   
   pthread_mutex_unlock(&sharedMutex);
   
   
   }
   
   
   int main (int argc, char * argv[])
   
   {
   
   
   pthread_t pa, pb;
   
   pthread_create ( &pa, NULL, thread_func, "Thread A");
   
   pthread_create ( &pb, NULL, thread_func, "Thread B");
   
   
   pthread_join (pa, NULL);
   
   pthread_join (pb, NULL);
   
   }
  
 
 
  
  该方法的缺点是:由于多个线程需要读写全局变量threadName,就需要使用互斥机制
  
  
  分析以上两种实现方法,Thread-Specific Data "线程相关的数据"的一个好处就体现出来了:
  
  (1)"线程相关的数据"可以是一个全局变量,并且
  
  (2)每个线程存取的"线程相关的数据"是相互独立的.
  
  
  
  2. 怎样使用"线程相关的数据"
  
  
  这是利用"线程相关的数据"的实现方式:
 
 
  
   #include <string.h>;
   
   #include <pthread.h>;
   
   
   pthread_key_t p_key;
   
   
   void another_func ()
   
   {
   
   printf ("%s is running in another_func/n", (char *)pthread_getspecific(p_key));
   
   }
   
   
   void * thread_func (void * args)
   
   {
   
   pthread_setspecific(p_key, args);
   
   printf ("%s is running in thread_func/n", (char *)pthread_getspecific(p_key));
   
   another_func ();
   
   
   }
   
   
   int main (int argc, char * argv[])
   
   {
   
   
   pthread_t pa, pb;
   
   
   pthread_key_create(&p_key, NULL);
   
   
   pthread_create ( &pa, NULL, thread_func, "Thread A");
   
   pthread_create ( &pb, NULL, thread_func, "Thread B");
   
   
   pthread_join (pa, NULL);
   
   pthread_join (pb, NULL);
   
   }
  
 
 
  
  说明:
  
  (1)
  
  线程A, B共用了p_key,
  
  通过p_key,就可以存取只跟当前线程相关的一个值(这个值由编译器管理)
  
  线程A----->;p_key----->;线程A相关的值(由编译器管理)
  
  线程B----->;p_key----->;线程B相关的值(由编译器管理)
  
  
  设置"线程相关的数据",使用
  
  int pthread_setspecific(pthread_key_t key, const void *pointer);
  
  读取"线程相关的数据",使用
  
  void * pthread_getspecific(pthread_key_t key);
  
  
  注意到,这两个函数分别有一个void类型的指针,我们的线程就是通过这两个指针分别与
  
  "线程相关的数据"的数据进行交互的
  
  
  (2)
  
  由于p_key是一个全局变量,
  
  函数another_func不需要额外的参数就可以访问它;
  
  又因为它是"线程相关的数据", 线程A, B通过p_key存取的数据是相互独立的,
  
  这样就不需要额外的互斥机制来保证数据访问的正确性了.
 


