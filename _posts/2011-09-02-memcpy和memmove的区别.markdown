---
layout: post
title: "memcpy和memmove的区别"
date: 2011-09-02 16:02:00 
comments: true
categories: [dst]
tags: [dst]
description: "memcpy和memmove的区别"
keywords: dst
---


 
 
 
  
  
  
   
    memcpy（）和memmove（）都是C语言中的库函数，在头文件string.h中，其原型分别如下：
   
   
    
     
     
    
   
   
    
     
      
       
        
         
          
           
            void
           
           
            *
           
           
            memcpy
           
           
            (
           
           
            void
           
           
            *
           
           dst
           
            ,
           
           
            const
           
           
            void
           
           
            *
           
           src
           
            ,
           
           
            size_t
           
           count
           
            )
           
           
            ;
           
           
            
             void
            
            
             *
            
            
             memmove
            
            
             (
            
            
             void
            
            
             *
            
            dst
            
             ,
            
            
             const
            
            
             void
            
            
             *
            
            src
            
             ,
            
            
             size_t
            
            count
            
             )
            
            
             ;
            
           
          
         
        
       
      
     
    
   
  
  
   
    
    
   
  
  
   
    它们都是从src所指向的内存中复制count个字节到dst所指内存中，并返回dst的值。当源内存区域和目标内存区域无交叉时，两者的结果是一样的，但如果有交叉呢？先看下图：
   
  
  
   
    
    
   
  
  
   
    
   
  
  
   
    图的上半部分为源内存区域在目标内存区域右边，下半部分为源内存区域在目标区域左边，源内存区域和目标内存区域都有交叉。
   
  
  
   
    memcpy（）是从src的起始部分开始复制，所以虽然第一种情况下没有问题，但如果遇到第二种情况，则会发生错误，如图所示，后两个字节在被复制前已经被覆盖掉了。而memmove（）则由于采用了不同的复制机制，所以可以正确处理第二种情况。
   
  
  
   
    VS.NET2003中所附源码如下（有删）：
   
  
  
   
    
     
      
       
        
         
          
           
            void
           
           
            *
           
           __cdecl
           
            memcpy
           
           
            (
           
           
            void
           
           
            *
           
           dst
           
            ,
           
           
            const
           
           
            void
           
           
            *
           
           src
           
            ,
           
           
            size_t
           
           
            count
           
           
            )
           
           
            
             {
            
            
             
              
               
                
                 
                  
                   
                    
                     
                      void
                     
                     
                      *
                     
                     ret
                     
                      =
                     
                     dst
                     
                      ;
                     
                     
                      
                       
                        
                         
                          
                           
                            
                             
                              
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
          
           
            
             
              
               
                
                 
                  
                   
                    
                     while
                    
                    
                     (
                    
                    
                     count
                    
                    
                     -
                    
                    
                     -
                    
                    
                     )
                    
                    
                     {
                    
                    
                     
                      
                       
                        
                         
                          
                           
                            
                             
                              
                               
                                
                                 
                                  
                                   
                                    
                                     
                                      *
                                     
                                     
                                      (
                                     
                                     
                                      char
                                     
                                     
                                      *
                                     
                                     
                                      )
                                     
                                     dst
                                     
                                      =
                                     
                                     
                                      *
                                     
                                     
                                      (
                                     
                                     
                                      char
                                     
                                     
                                      *
                                     
                                     
                                      )
                                     
                                     src
                                     
                                      ;
                                     
                                     
                                      
                                       
                                        
                                         
                                          
                                           
                                            
                                             
                                              
                                               
                                                
                                                 
                                                  
                                                   
                                                    
                                                     
                                                      dst
                                                      
                                                       =
                                                      
                                                      
                                                       (
                                                      
                                                      
                                                       char
                                                      
                                                      
                                                       *
                                                      
                                                      
                                                       )
                                                      
                                                      dst
                                                      
                                                       +
                                                      
                                                      1
                                                      
                                                       ;
                                                      
                                                      
                                                       
                                                        
                                                         
                                                          
                                                           
                                                            
                                                             
                                                              
                                                               
                                                                
                                                                 
                                                                  
                                                                   
                                                                    
                                                                     
                                                                      
                                                                       src
                                                                       
                                                                        =
                                                                       
                                                                       
                                                                        (
                                                                       
                                                                       
                                                                        char
                                                                       
                                                                       
                                                                        *
                                                                       
                                                                       
                                                                        )
                                                                       
                                                                       src
                                                                       
                                                                        +
                                                                       
                                                                       1
                                                                       
                                                                        ;
                                                                       
                                                                       
                                                                        
                                                                         
                                                                          
                                                                           
                                                                            
                                                                             
                                                                              
                                                                               
                                                                                
                                                                                 }
                                                                                
                                                                                
                                                                                 
                                                                                  
                                                                                   
                                                                                    
                                                                                     
                                                                                      
                                                                                       
                                                                                        
                                                                                         
                                                                                          
                                                                                           return
                                                                                          
                                                                                          
                                                                                           (
                                                                                          
                                                                                          ret
                                                                                          
                                                                                           )
                                                                                          
                                                                                          
                                                                                           ;
                                                                                          
                                                                                          
                                                                                           
                                                                                            }
                                                                                           
                                                                                          
                                                                                         
                                                                                        
                                                                                       
                                                                                      
                                                                                     
                                                                                    
                                                                                   
                                                                                  
                                                                                 
                                                                                
                                                                               
                                                                              
                                                                             
                                                                            
                                                                           
                                                                          
                                                                         
                                                                        
                                                                       
                                                                      
                                                                     
                                                                    
                                                                   
                                                                  
                                                                 
                                                                
                                                               
                                                              
                                                             
                                                            
                                                           
                                                          
                                                         
                                                        
                                                       
                                                      
                                                     
                                                    
                                                   
                                                  
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
   
    
     
      
       
        
         
          
           
            void
           
           
            *
           
           __cdecl
           
            memmove
           
           
            (
           
           
            void
           
           
            *
           
           dst
           
            ,
           
           
            const
           
           
            void
           
           
            *
           
           src
           
            ,
           
           
            size_t
           
           
            count
           
           
            )
           
           
            
             {
            
            
             
              
               
                
                 
                  
                   
                    
                     
                      void
                     
                     
                      *
                     
                     ret
                     
                      =
                     
                     dst
                     
                      ;
                     
                     
                      
                       
                        
                         
                          
                           
                            
                             
                              
                               
                                if
                               
                               
                                (
                               
                               dst
                               
                                <
                               
                               
                                =
                               
                               src
                               
                                |
                               
                               
                                |
                               
                               
                                (
                               
                               
                                char
                               
                               
                                *
                               
                               
                                )
                               
                               dst
                               
                                >;
                               
                               
                                =
                               
                               
                                (
                               
                               
                                (
                               
                               
                                char
                               
                               
                                *
                               
                               
                                )
                               
                               src
                               
                                +
                               
                               
                                count
                               
                               
                                )
                               
                               
                                )
                               
                               
                                {
                               
                               
                                
                                 
                                  
                                   
                                    
                                     
                                      
                                       
                                        
                                         
                                          
                                           
                                            
                                             
                                              
                                               
                                                //从地位向高位
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
          
           
            
             
              
               
                
                 
                  
                   
                    
                     
                      
                       
                        
                         
                          
                           
                            
                             while
                            
                            
                             (
                            
                            
                             count
                            
                            
                             -
                            
                            
                             -
                            
                            
                             )
                            
                            
                             {
                            
                            
                             
                              
                               
                                
                                 
                                  
                                   
                                    
                                     
                                      
                                       
                                        
                                         
                                          
                                           
                                            
                                             
                                              
                                               
                                                
                                                 
                                                  
                                                   
                                                    
                                                     
                                                      *
                                                     
                                                     
                                                      (
                                                     
                                                     
                                                      char
                                                     
                                                     
                                                      *
                                                     
                                                     
                                                      )
                                                     
                                                     dst
                                                     
                                                      =
                                                     
                                                     
                                                      *
                                                     
                                                     
                                                      (
                                                     
                                                     
                                                      char
                                                     
                                                     
                                                      *
                                                     
                                                     
                                                      )
                                                     
                                                     src
                                                     
                                                      ;
                                                     
                                                     
                                                      
                                                       
                                                        
                                                         
                                                          
                                                           
                                                            
                                                             
                                                              
                                                               
                                                                
                                                                 
                                                                  
                                                                   
                                                                    
                                                                     
                                                                      
                                                                       
                                                                        
                                                                         
                                                                          
                                                                           
                                                                            
                                                                             
                                                                              dst
                                                                              
                                                                               =
                                                                              
                                                                              
                                                                               (
                                                                              
                                                                              
                                                                               char
                                                                              
                                                                              
                                                                               *
                                                                              
                                                                              
                                                                               )
                                                                              
                                                                              dst
                                                                              
                                                                               +
                                                                              
                                                                              1
                                                                              
                                                                               ;
                                                                              
                                                                              
                                                                               
                                                                                
                                                                                 
                                                                                  
                                                                                   
                                                                                    
                                                                                     
                                                                                      
                                                                                       
                                                                                        
                                                                                         
                                                                                          
                                                                                           
                                                                                            
                                                                                             
                                                                                              
                                                                                               
                                                                                                
                                                                                                 
                                                                                                  
                                                                                                   
                                                                                                    
                                                                                                     
                                                                                                      
                                                                                                       src
                                                                                                       
                                                                                                        =
                                                                                                       
                                                                                                       
                                                                                                        (
                                                                                                       
                                                                                                       
                                                                                                        char
                                                                                                       
                                                                                                       
                                                                                                        *
                                                                                                       
                                                                                                       
                                                                                                        )
                                                                                                       
                                                                                                       src
                                                                                                       
                                                                                                        +
                                                                                                       
                                                                                                       1
                                                                                                       
                                                                                                        ;
                                                                                                       
                                                                                                       
                                                                                                        
                                                                                                         
                                                                                                          
                                                                                                           
                                                                                                            
                                                                                                             
                                                                                                              
                                                                                                               
                                                                                                                
                                                                                                                 
                                                                                                                  
                                                                                                                   
                                                                                                                    
                                                                                                                     
                                                                                                                      
                                                                                                                       
                                                                                                                        
                                                                                                                         }
                                                                                                                        
                                                                                                                        
                                                                                                                         
                                                                                                                          
                                                                                                                           
                                                                                                                            
                                                                                                                             
                                                                                                                              
                                                                                                                               
                                                                                                                                
                                                                                                                                 
                                                                                                                                  }
                                                                                                                                 
                                                                                                                                 
                                                                                                                                  
                                                                                                                                   
                                                                                                                                    
                                                                                                                                     
                                                                                                                                      
                                                                                                                                       
                                                                                                                                        
                                                                                                                                         
                                                                                                                                          
                                                                                                                                           else
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           {  //从高位向地位
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           
                                                                                                                                            
                                                                                                                                             
                                                                                                                                              
                                                                                                                                               
                                                                                                                                                
                                                                                                                                                 
                                                                                                                                                  
                                                                                                                                                   
                                                                                                                                                    
                                                                                                                                                     
                                                                                                                                                      
                                                                                                                                                       
                                                                                                                                                        
                                                                                                                                                         
                                                                                                                                                          
                                                                                                                                                          
                                                                                                                                                         
                                                                                                                                                        
                                                                                                                                                       
                                                                                                                                                      
                                                                                                                                                     
                                                                                                                                                    
                                                                                                                                                   
                                                                                                                                                  
                                                                                                                                                 
                                                                                                                                                
                                                                                                                                               
                                                                                                                                              
                                                                                                                                             
                                                                                                                                            
                                                                                                                                           
                                                                                                                                          
                                                                                                                                         
                                                                                                                                        
                                                                                                                                       
                                                                                                                                      
                                                                                                                                     
                                                                                                                                    
                                                                                                                                   
                                                                                                                                  
                                                                                                                                 
                                                                                                                                
                                                                                                                               
                                                                                                                              
                                                                                                                             
                                                                                                                            
                                                                                                                           
                                                                                                                          
                                                                                                                         
                                                                                                                        
                                                                                                                       
                                                                                                                      
                                                                                                                     
                                                                                                                    
                                                                                                                   
                                                                                                                  
                                                                                                                 
                                                                                                                
                                                                                                               
                                                                                                              
                                                                                                             
                                                                                                            
                                                                                                           
                                                                                                          
                                                                                                         
                                                                                                        
                                                                                                       
                                                                                                      
                                                                                                     
                                                                                                    
                                                                                                   
                                                                                                  
                                                                                                 
                                                                                                
                                                                                               
                                                                                              
                                                                                             
                                                                                            
                                                                                           
                                                                                          
                                                                                         
                                                                                        
                                                                                       
                                                                                      
                                                                                     
                                                                                    
                                                                                   
                                                                                  
                                                                                 
                                                                                
                                                                               
                                                                              
                                                                             
                                                                            
                                                                           
                                                                          
                                                                         
                                                                        
                                                                       
                                                                      
                                                                     
                                                                    
                                                                   
                                                                  
                                                                 
                                                                
                                                               
                                                              
                                                             
                                                            
                                                           
                                                          
                                                         
                                                        
                                                       
                                                      
                                                     
                                                    
                                                   
                                                  
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
          
           
            
             
              
               
                
                 
                  
                   
                    
                     
                      
                       
                        
                         
                          
                           
                            dst
                            
                             =
                            
                            
                             (
                            
                            
                             char
                            
                            
                             *
                            
                            
                             )
                            
                            dst
                            
                             +
                            
                            
                             count
                            
                            
                             -
                            
                            1
                            
                             ;
                            
                            
                             
                              
                               
                                
                                 
                                  
                                   
                                    
                                     
                                      
                                       
                                        
                                         
                                          
                                           
                                            
                                             src
                                             
                                              =
                                             
                                             
                                              (
                                             
                                             
                                              char
                                             
                                             
                                              *
                                             
                                             
                                              )
                                             
                                             src
                                             
                                              +
                                             
                                             
                                              count
                                             
                                             
                                              -
                                             
                                             1
                                             
                                              ;
                                             
                                             
                                              
                                               
                                                
                                                 
                                                  
                                                   
                                                    
                                                     
                                                      
                                                       
                                                        
                                                         
                                                          
                                                           
                                                            
                                                             
                                                              
                                                               
                                                                while
                                                               
                                                               
                                                                (
                                                               
                                                               
                                                                count
                                                               
                                                               
                                                                -
                                                               
                                                               
                                                                -
                                                               
                                                               
                                                                )
                                                               
                                                               
                                                                {
                                                               
                                                               
                                                                
                                                                 
                                                                  
                                                                   
                                                                    
                                                                     
                                                                      
                                                                       
                                                                        
                                                                         
                                                                          
                                                                           
                                                                            
                                                                             
                                                                              
                                                                               
                                                                                
                                                                                 
                                                                                  
                                                                                   
                                                                                    
                                                                                     
                                                                                      
                                                                                       
                                                                                        
                                                                                         *
                                                                                        
                                                                                        
                                                                                         (
                                                                                        
                                                                                        
                                                                                         char
                                                                                        
                                                                                        
                                                                                         *
                                                                                        
                                                                                        
                                                                                         )
                                                                                        
                                                                                        dst
                                                                                        
                                                                                         =
                                                                                        
                                                                                        
                                                                                         *
                                                                                        
                                                                                        
                                                                                         (
                                                                                        
                                                                                        
                                                                                         char
                                                                                        
                                                                                        
                                                                                         *
                                                                                        
                                                                                        
                                                                                         )
                                                                                        
                                                                                        src
                                                                                        
                                                                                         ;
                                                                                        
                                                                                        
                                                                                         
                                                                                          
                                                                                           
                                                                                            
                                                                                             
                                                                                              
                                                                                               
                                                                                                
                                                                                                 
                                                                                                  
                                                                                                   
                                                                                                    
                                                                                                     
                                                                                                      
                                                                                                       
                                                                                                        
                                                                                                         
                                                                                                          
                                                                                                           
                                                                                                            
                                                                                                             
                                                                                                              
                                                                                                               
                                                                                                                
                                                                                                                 dst
                                                                                                                 
                                                                                                                  =
                                                                                                                 
                                                                                                                 
                                                                                                                  (
                                                                                                                 
                                                                                                                 
                                                                                                                  char
                                                                                                                 
                                                                                                                 
                                                                                                                  *
                                                                                                                 
                                                                                                                 
                                                                                                                  )
                                                                                                                 
                                                                                                                 dst
                                                                                                                 
                                                                                                                  -
                                                                                                                 
                                                                                                                 1
                                                                                                                 
                                                                                                                  ;
                                                                                                                 
                                                                                                                 
                                                                                                                  
                                                                                                                   
                                                                                                                    
                                                                                                                     
                                                                                                                      
                                                                                                                       
                                                                                                                        
                                                                                                                         
                                                                                                                          
                                                                                                                           
                                                                                                                            
                                                                                                                             
                                                                                                                              
                                                                                                                               
                                                                                                                                
                                                                                                                                 
                                                                                                                                  
                                                                                                                                   
                                                                                                                                    
                                                                                                                                     
                                                                                                                                      
                                                                                                                                       
                                                                                                                                        
                                                                                                                                         
                                                                                                                                          src
                                                                                                                                          
                                                                                                                                           =
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           (
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           char
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           *
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           )
                                                                                                                                          
                                                                                                                                          src
                                                                                                                                          
                                                                                                                                           -
                                                                                                                                          
                                                                                                                                          1
                                                                                                                                          
                                                                                                                                           ;
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           
                                                                                                                                            
                                                                                                                                             
                                                                                                                                              
                                                                                                                                               
                                                                                                                                                
                                                                                                                                                 
                                                                                                                                                  
                                                                                                                                                   
                                                                                                                                                    
                                                                                                                                                     
                                                                                                                                                      
                                                                                                                                                       
                                                                                                                                                        
                                                                                                                                                         
                                                                                                                                                          
                                                                                                                                                           
                                                                                                                                                            }
                                                                                                                                                           
                                                                                                                                                           
                                                                                                                                                            
                                                                                                                                                             
                                                                                                                                                              
                                                                                                                                                               
                                                                                                                                                                
                                                                                                                                                                 
                                                                                                                                                                  
                                                                                                                                                                   
                                                                                                                                                                    
                                                                                                                                                                     }
                                                                                                                                                                    
                                                                                                                                                                    
                                                                                                                                                                     
                                                                                                                                                                      
                                                                                                                                                                       
                                                                                                                                                                        
                                                                                                                                                                         
                                                                                                                                                                          
                                                                                                                                                                           
                                                                                                                                                                            
                                                                                                                                                                             
                                                                                                                                                                              
                                                                                                                                                                               return
                                                                                                                                                                              
                                                                                                                                                                              
                                                                                                                                                                               (
                                                                                                                                                                              
                                                                                                                                                                              ret
                                                                                                                                                                              
                                                                                                                                                                               )
                                                                                                                                                                              
                                                                                                                                                                              
                                                                                                                                                                               ;
                                                                                                                                                                              
                                                                                                                                                                              
                                                                                                                                                                               
                                                                                                                                                                                }
                                                                                                                                                                               
                                                                                                                                                                              
                                                                                                                                                                             
                                                                                                                                                                            
                                                                                                                                                                           
                                                                                                                                                                          
                                                                                                                                                                         
                                                                                                                                                                        
                                                                                                                                                                       
                                                                                                                                                                      
                                                                                                                                                                     
                                                                                                                                                                    
                                                                                                                                                                   
                                                                                                                                                                  
                                                                                                                                                                 
                                                                                                                                                                
                                                                                                                                                               
                                                                                                                                                              
                                                                                                                                                             
                                                                                                                                                            
                                                                                                                                                           
                                                                                                                                                          
                                                                                                                                                         
                                                                                                                                                        
                                                                                                                                                       
                                                                                                                                                      
                                                                                                                                                     
                                                                                                                                                    
                                                                                                                                                   
                                                                                                                                                  
                                                                                                                                                 
                                                                                                                                                
                                                                                                                                               
                                                                                                                                              
                                                                                                                                             
                                                                                                                                            
                                                                                                                                           
                                                                                                                                          
                                                                                                                                         
                                                                                                                                        
                                                                                                                                       
                                                                                                                                      
                                                                                                                                     
                                                                                                                                    
                                                                                                                                   
                                                                                                                                  
                                                                                                                                 
                                                                                                                                
                                                                                                                               
                                                                                                                              
                                                                                                                             
                                                                                                                            
                                                                                                                           
                                                                                                                          
                                                                                                                         
                                                                                                                        
                                                                                                                       
                                                                                                                      
                                                                                                                     
                                                                                                                    
                                                                                                                   
                                                                                                                  
                                                                                                                 
                                                                                                                
                                                                                                               
                                                                                                              
                                                                                                             
                                                                                                            
                                                                                                           
                                                                                                          
                                                                                                         
                                                                                                        
                                                                                                       
                                                                                                      
                                                                                                     
                                                                                                    
                                                                                                   
                                                                                                  
                                                                                                 
                                                                                                
                                                                                               
                                                                                              
                                                                                             
                                                                                            
                                                                                           
                                                                                          
                                                                                         
                                                                                        
                                                                                       
                                                                                      
                                                                                     
                                                                                    
                                                                                   
                                                                                  
                                                                                 
                                                                                
                                                                               
                                                                              
                                                                             
                                                                            
                                                                           
                                                                          
                                                                         
                                                                        
                                                                       
                                                                      
                                                                     
                                                                    
                                                                   
                                                                  
                                                                 
                                                                
                                                               
                                                              
                                                             
                                                            
                                                           
                                                          
                                                         
                                                        
                                                       
                                                      
                                                     
                                                    
                                                   
                                                  
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
  
   
   
  
 


