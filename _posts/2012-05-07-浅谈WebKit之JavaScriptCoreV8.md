---
layout: post
title: "浅谈WebKit之JavaScriptCore/V8"
date: 2012-05-07 15:08:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "浅谈WebKit之JavaScriptCore/V8"
keywords: webkit
---


 
  WebKit作为一个浏览器引擎，其中Javascript实现包括JavaScriptCore和V8，为了能更全面的了解WebKit，我们需要深入的了解Javascript实现的基本原理、其在WebKit中的作用以及与其他部分之间的交互，同时与Gecko中的Javacript实现作初步的对比。让我们开始了解WebKit之Javascript实现JavaScriptCore、V8之旅吧。
  
   
    
     一、Javascript实现的作用
    
    
     正与浅谈Gecko关键部分之六认识javascript实现及应用部分对什么是javascript的描述那样，在WebKit中其Javascript实现，同样相当于一个符合ECMAScript标准的动态库，其往往依附于浏览器引擎，由浏览器引擎来提供运行环境，并控制或发起javascript实现进行编译、解析执行脚本、垃圾回收等，同样需提供对浏览器引擎扩展的支持如Dom Binding等；
     
      
       由于Web2.0的提出，动态网页的交互如运行ajax更加的频繁，Javascript脚本运行的总体效率以及安全往往成为浏览器内核的关键，而其Javascript实现就担负着如此重任。
       
        
         
          二、JavaScriptCore实现特点
         
         
          相对于其他的Javascript实现，JavaScriptCore提出了虚拟机的概念，在编译脚本时生成高效的bytecode，bytecode统一在一个虚拟机的环境中执行。而其高效的虚拟机实现常称为SquirrelFish，通过Announcing SquirrelFish、Introducing SquirrelFish Extreme可更进一步了解关于SquirrelFish的相关内容。
          
           
            
             三、V8实现特点
            
            
             
              Fast Property Access
             
             
              To reduce the time required to access JavaScript properties, V8 does not use dynamic lookup to access properties. Instead, V8 dynamically creates
              
               hidden classes
              
              behind the scenes. This basic idea is not new - the prototype-based programming language
 Self used maps to do something similar. (See for example, An Efficient Implementation of Self, a Dynamically-Typed Object-Oriented Language Based on Prototypes). In V8, an object changes its hidden class when a new property is added.
              
               
                
                 Dynamic Machine Code Generation
                
                
                 V8 compiles JavaScript source code directly into machine code when it is first executed. There are no intermediate byte codes, no interpreter. Property access is handled by inline cache code that may be patched with other machine instructions as V8 executes.
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
 
 
  During initial execution of the code for accessing a property of a given object, V8 determines the object's current hidden class. V8 optimizes property access by predicting that this class will also be used for all future objects accessed in the same section
 of code and uses the information in the class to patch the inline cache code to use the hidden class. If V8 has predicted correctly the property's value is assigned (or fetched) in a single operation. If the prediction is incorrect, V8 patches the code to
 remove the optimisation.
 
 
  
   Efficient Garbage Collection
  
  
   V8 reclaims memory used by objects that are no longer required in a process known as garbage collection. To ensure fast object allocation, short garbage collection pauses, and no memory fragmentation V8 employs a stop-the-world, generational, accurate, garbage
 collector. This means that V8:
  
 
 
  
   stops program execution when performing a garbage collection cycle.
  
  
   processes only part of the object heap in most garbage collection cycles. This minimizes the impact of stopping the application.
  
  
   always knows exactly where all objects and pointers are in memory. This avoids falsely identifying objects as pointers which can result in memory leaks.
  
 
 
  In V8, the object heap is segmented into two parts: new space where objects are created, and old space to which objects surviving a garbage collection cycle are promoted. If an object is moved in a garbage collection cycle, V8 updates all pointers to the object.
 
 
  
   
    四、JavaScriptCore、V8如何与WebCore交互
   
  
  
   
    在WebCore::Frame的数据结构中包含数据成员KJSProxy* m_jscript;而在Chrome的代码中调整为JSBridge* m_jscript;而针对不同实现JavaScriptCore、V8，分别有：
   
   
    
     class KJSBridge : public JSBridge {
    
    
     
      public:
     
     
      
       KJSBridge(Frame* frame) : m_proxy(new KJSProxy(frame)) { }
      
      
       
        virtual ~KJSBridge() { delete m_proxy; }
       
       
        
         ........................
        
        
         
          private:
         
         
          
           KJSProxy* m_proxy;
          
          
           
            };
           
           
            
             
              class V8Bridge : public JSBridge {
             
             
              
               public:
              
              
               
                explicit V8Bridge(Frame* frame);
               
               
                
                 virtual ~V8Bridge();
                
                
                 
                  .......................
                 
                 
                  
                   private:
                  
                  
                   
                    V8Proxy* m_proxy;
                   
                   
                    
                     };
                    
                    
                     
                      V8Bridge::V8Bridge(Frame* frame) {
                     
                     
                      
                       m_proxy = new V8Proxy(frame);
                      
                      
                       
                        }
                       
                       
                        
                         V8Bridge::~V8Bridge() {
                        
                        
                         
                          delete m_proxy;
                         
                         
                          
                           }
                          
                          
                           
                            
                             而不同的KJSProxy与V8Proxy分别对应不同的Javascript实现，它们分别实现了与WebCore之间的共同接口，其主要数据结构分别如下:
                            
                            
                             
                              class KJSProxy {
                             
                             
                              
                               Frame* m_frame;
                              
                              
                               
                                KJS::ProtectedPtr< KJS::JSDOMWindow>; m_globalObject;
                               
                               
                                
                                 int m_handlerLineno;
                                
                                
                                 
                                  .........................................
                                 
                                 
                                  
                                   };
                                  
                                  
                                   
                                    
                                     class V8Proxy {
                                    
                                    
                                     
                                      Frame* m_frame;
                                     
                                     
                                      
                                       v8::Persistent<v8::context>; m_context;
                                      
                                      
                                       
                                        v8::Persistent<v8::object>; m_global;
                                       
                                       
                                        
                                         // Special handling of document wrapper;
                                        
                                        
                                         
                                          v8::Persistent m_document;
                                         
                                         
                                          
                                           int m_handlerLineno;
                                          
                                          
                                           
                                            ...........................
                                           
                                           
                                            
                                             };
                                            
                                            
                                             
                                              具体不同Javascript实现如何实现与WebCore的接口，需了解不同Javascript实现逻辑；
                                             
                                             
                                              
                                               
                                                如对Javascript实现逻辑及基本原理感兴趣，可具体参考其提供的api及sample等等；
                                               
                                               
                                                
                                                 
                                                  至于Dom Binding的实现，JavaScriptCore与V8通过通过同样的方式来实现，可参考
                                                 
                                                 
                                                  浅谈WebKit之WebCore篇
                                                 
                                                 
                                                  中所描述的Javascript实现如何与WebCore集成等；
                                                 
                                                 
                                                  
                                                   
                                                    具体Dom Binding的实现可参考generate-bindings.pl生成的代码，其中的内容一定会让你受益非浅，同时为将Javascript实现嵌入到其他应用中去提供非常有益的参考。如对window的实现，特别是open方法的实现，很值得研究研究。。。
                                                   
                                                   
                                                    
                                                     
                                                      
                                                       五、初步对比JavaScriptCore、V8、SpiderMonkey等
                                                      
                                                     
                                                     
                                                      
                                                       具体JavaScriptCore、V8、SpiderMonkey、TracMonkey执行效率对比如何，不同的测试方法会有不同的测试结果，在这里不再阐述。
                                                      
                                                      
                                                       
                                                        
                                                         就个人了解而言，觉得JavaScriptCore关于对象的方法、属性的安全访问控制方面略有欠缺；
                                                        
                                                        
                                                         
                                                          
                                                           SpiderMonkey作为最早一批实现Javascript的引擎，其代码使用C语言来实现，稍现复杂，没有象后来的实现如JavaScriptCore、V8等借鉴了最新的虚拟机技术如JVM等；
                                                          
                                                          
                                                           
                                                            
                                                             V8作为新近推出的Javascript实现，正与其特点所描述，拥有很多优势，同时基于C++实现，充分利用了C++ template，代码相对简洁，便于学习使用；
                                                            
                                                            
                                                             
                                                              
                                                               关于TracMonkey请参考Firefox to get massive JavaScript performance boost
                                                              
                                                              
                                                               
                                                                
                                                                 
                                                                  六、参考资源
                                                                 
                                                                
                                                                
                                                                 
                                                                  Wiki Javascript
                                                                 
                                                                 
                                                                  
                                                                   V8
                                                                  
                                                                  
                                                                   
                                                                    Announcing SquirrelFish
                                                                   
                                                                   
                                                                    
                                                                     Introducing SquirrelFish Extreme
                                                                    
                                                                    
                                                                     
                                                                      SpiderMonkey Internals
                                                                     
                                                                     
                                                                      
                                                                       Tamarin
                                                                      
                                                                     
                                                                    
                                                                   
                                                                  
                                                                 
                                                                
                                                               
                                                              
                                                             
                                                            
                                                           
                                                          
                                                         
                                                        
                                                       
                                                      
                                                     
                                                    
                                                   
                                                  
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


