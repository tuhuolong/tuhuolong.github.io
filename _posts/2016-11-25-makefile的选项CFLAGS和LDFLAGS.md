---
layout: post
title: "makefile的选项CFLAGS和LDFLAGS"
date: 2016-11-25 11:01:00 
comments: true
categories: [makefile]
tags: [makefile]
description: "makefile的选项CFLAGS和LDFLAGS"
keywords: makefile
---


 
  
   CFLAGS 表示用于 C 编译器的选项，
   
    CXXFLAGS 表示用于 C++ 编译器的选项。
    
     这两个变量实际上涵盖了编译和汇编两个步骤。
    
   
  
 
 
  
   先来看几个相关的环境变量：PATH、LDFLAGS、CFLAGS
   
    
     PATH：大家应该都很熟悉。安装一个包后可能会在安装目录建立一个bin目录，里面都是些可执行程序，为了让系统能找到这些程序，就需要我们把该路径加入到PATH环境变量中。下面会讲到如何加入。
     
      
       LDFLAGS：gcc 等编译器会用到的一些优化参数，也可以在里面指定库文件的位置。用法：LDFLAGS=“-L/usr/lib -L/path/to/your/lib”。每安装一个包都几乎一定的会在安装目录里建立一个lib目录。如果你明明安装了某个包，而安装另一个包时，它愣是说找不到，那就把那个包的lib路径加入的LDFALGS中试一下吧。
       
        
         CFLAGS：和LDFLAGS类似，只不过要向里加如的是头文件（.h文件）的路径，如：CFLAGS=“-I/usr/include -I/path/to/your/include”。同样地，安装一个包时会在安装路径下建立一个include目录，当安装过程中出现问题时，试着把以前安装的包的include目录加入到该变量中来。
         
          
           那如何在那些变量中加入这些路径呢？以PATH变量为例。
           
            一种方法是：直接在命令行下：
            
             $ PATH="$PATH:/path/to/your/bin:/another/path/to/bin"
             
              $ export PATH
              
               这种方法的好处是方便，立即生效，缺点是把当前会话或shell关了之后就失效了，下次还得运行一遍。
               
                
                 另一种方法是：把下面的两行内容加入到~/.bashrc文件中，如果想让其影响到整个系统而不仅仅是当前用户，就把下面的内容加入到/etc/bash.bashrc（记得有的系统是/etc/bashrc这个文件）
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
  
   PATH="$PATH:/path/to/your/bin:/another/path/to/bin"
   
    export PATH
   
  
 
 
  
   然后，重新启动shell即可。
   
    
     需要注意的是，PATH变量的分隔符是：号，其他的是空格，
     
      所以LDFLAGS应该是这样：
      
       LDFLAGS=“$LDFLAGS -L/path/to/lib -L/path/to/lib”
       
        CFLAGS应该是这样：
        
         CFLAGS=“$CFLAGS -I/path/to/iclude -I/path/to/include”
        
       
      
     
    
   
  
 


