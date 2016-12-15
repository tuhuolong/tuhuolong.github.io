---
layout: post
title: "gcc 的visibility"
date: 2012-05-15 10:39:00 
comments: true
categories: [gcc]
tags: [gcc]
description: "gcc 的visibility"
keywords: gcc
---


 
  
   -fvisibility=default|internal|hidden|protected
   
    gcc的visibility是说，如果编译的时候用了这个属性，那么动态库的符号都是hidden的，除非强制声明。
    
     
      
       1.创建一个c源文件，内容简单
       
        #include<stdio.h>;
        
         #include<stdlib.h>;
         
          
           __attribute ((visibility("default"))) void not_hidden ()
           
            {
            
             printf("exported symbol\n");
             
              }
              
               
                void is_hidden ()
                
                 {
                 
                  printf("hidden one\n");
                  
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    }
   
  
 
 
  
   
    
    
   
  
 
 
  
   想要做的是，第一个函数符号可以被导出，第二个被隐藏。
   
    先编译成一个动态库，使用到属性-fvisibility
    
     gcc -shared -o libvis.so -fvisibility=hidden vis.c
     
      
       现在查看
       
        # readelf -s libvis.so |grep hidden
        
         7: 0000040c    20 FUNC    GLOBAL DEFAULT   11 not_hidden
         
          48: 00000420    20 FUNC    LOCAL  HIDDEN   11 is_hidden
          
           51: 0000040c    20 FUNC    GLOBAL DEFAULT   11 not_hidden
           
            可以看到，属性确实有作用了。
            
             
              
               现在试图link
               
                vi main.c
                
                 int main()
                 
                  {
                  
                   not_hidden();
                   
                    is_hidden();
                    
                     return 0;
                     
                      }
                      
                       
                        
                         试图编译成一个可执行文件，链接到刚才生成的动态库，
                         
                          gcc -o exe main.c -L ./ -lvis
                          
                           结果提示：
                           
                            /tmp/cckYTHcl.o: In function `main':
                            
                             main.c:(.text+0x17): undefined reference to `is_hidden'
                             
                              
                               说明了hidden确实起到作用了。
                               
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


