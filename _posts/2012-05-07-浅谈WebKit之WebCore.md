---
layout: post
title: "浅谈WebKit之WebCore"
date: 2012-05-07 15:06:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "浅谈WebKit之WebCore"
keywords: webkit
---


 
  最近自从Google推出Chrome浏览器之后，浏览器受到人们更加广泛的关注，网上时而会出现这样那样的评价，作为一个浏览器内核爱好者，希望能乘着大家都关注的东风，能对浏览器内核有更深入的理解，进而能更好的进行Web开发及利用。
  
   
    Chrome浏览器的代码量其实是非常庞大的，要想对其有深入的理解，仅仅编译编译调试调试，是很难深入下去的。让我们还是从其主要部分如多进程管理通信、WebKit、V8、Skia、WinHttp、Sanbox等着手分析其主要流程及数据结构，或许能达到事半功倍的效果，而WebKit是其中非常重要的一部分，是Chrome的核心引擎部分，其他部分都是基于它来集成的，深入了解了WebKit，对Chrome的理解就会迎刃而解，再说WebKit作为一个相对独立的浏览器引擎在Safari、iPhone、Adobe AIR等中都有应用，非常值得大家深入的研究研究。
    
     
      就像前面的文章所说，WebKit主要包括三个部分WebCore、JavascriptCore及Ports部分，让我们先从WebCore部分出发吧。。。。
      
       
        
         一、WebCore所包含的主要内容
        
        
         
          1、从源代码目录结构来看
         
         
          WebCore目录主要包括如下目录：
          
           
            bindings
           
           包含将Dom Binding给JavascriptCore方面的代码，同时包含依据idl接口描述文件，自动生成对应于JavascriptCore的Binding实现的脚本等内容；
           
            
             
              bridge
             
             主要包含NPPlugin方面的接口访问等内容；
             
              
               
                css
               
               主要包括与css方面相关的内容如解析、不同css规则的定义与实现、css Binding给JS的接口定义等内容；
               
                
                 
                  dom
                 
                 主要包括dom方面相关的内容如不同dom元素的定义与实现、dom Binding给JS的接口定义等内容；
                 
                  
                   
                    html
                   
                   主要包括html方面相关的内容如不同html元素的定义与实现、HTMLTokenizer及HTMLParser等内容；
                   
                    
                     
                      loader
                     
                     主要包括装载资源如html页面、css、js及image等方面内容；
                     
                      
                       
                        page
                       
                       主要包括描述一个Web页面所涉及的内容如page、frame、frameview、frametree、setting、history、chrome、chromeclient等内容；
                       
                        
                         
                          rendering
                         
                         主要包括如何使用样式，组织布局、显示html元素等方面内容；
                         
                          
                           
                            plugins
                           
                           主要包括浏览端如何实现NPPlugin方面的内容；
                           
                            
                             
                              svg
                             
                             主要包括与svg方面相关的内容；
                             
                              
                               
                                xml
                               
                               主要包括与xml方面相关的内容如xml parser、XPath、XSLT等；
                               
                                
                                 
                                  platform
                                 
                                 主要包括与不同平台或外部库相关的内容如graphics(图形输出方面)、network(网络处理方面)、image-decoders(解析不同图片格式方面)等；
                                 
                                  
                                   
                                    2、主要数据结构
                                   
                                   
                                    为了更加简单有效的描述浏览网页的内容及过程，WebKit为了明显区分不同方面的内容，采取了不同的namespace如webcore、javascriptcore、webkit等，webcore方面的主要数据结构有：
                                    
                                     webcore::page、webcore::frame、webcore::FrameLoader、webcore::FrameView、Document、DOMWindow、KJSProxy、DocumentLoader、ResourceHandle、ResourceRequest、ResouceResponse、MainResourceLoader、RenderObject、RenderView等；主要数据结构描述如下：
                                     
                                      
                                       
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
  
   WebView及WebFrame与page、frame之间的关系
   
   
  
 
 
  
 
 
  图一
 
 
  
   
    FrameLoader、DocumentLoader、DocLoader类结构
   
  
 
 
  
  
  
   图二
  
  
   
    主要Document类结构
   
  
  
   
  
  
   图三
  
  
   
    FrameView类主要结构
   
  
  
   
  
  
   图四
  
  
   总的说来，WebCore包含了浏览器引擎的核心部分如处理html、dom、css、svg、获取资源、渲染页面过程控制、回调/通知外壳程序以及与Javascript实现的Binding等等；
  
  
   
    
     二、一个Http请求在WebCore中的主要流程
    
   
  
  
   1、当调用webkit_web_view_open(url)时会触发core(webView)->;mainFrame()->;loader()->;load(uri)(即调用FrameLoader.load)来发起一个Http页面请求；
  
  
   
    FrameLoader.load方法的主要处理过程如图：
   
  
  
   
  
  
   图五
   
    
    
   
  
  
   2、一旦发起ResourceHandle::start，就会由网络库向web服务器发起一个http请求；
  
  
   3、而MainResourceLoader作为一个ResouceHandleClient，提供了诸如didReceiveData()、didReceiveResponse()等回调接口以供网络库调用，一旦从web服务器获得相关数据后网络库部分则会调用相关接口如didReceiveData等；
  
  
   4、MainResouceLoader::didReceiveData的主要回调处理过程如下图：
  
  
   
    图六
   
  
  
   5、通过回调didReceiveData()方法，进而调用Node.attach()方法，这样就会解析生成document，同时会创建frameview、domwindow等；
  
  
   6、创建的frameview会触发layoutTimerFired时间Timer，进而调用layout()方法，从而触发RenderObject的创建、布局等，同时或许会invalidateRect，进而触发操作系统图形库的paint消息事件；
  
  
   7、由程序主消息处理循环接收paint消息事件，进而获取对应frame，获取或创建GraphicContext，然后调用frame->;view()->;paint(&ctx,...)，从而触发对应RenderObject树进行重画处理，这样一个完整的页面就会逐步的显示出来。
   
    
     
      
       三、网络库、图形库、Javascript实现与WebCore的集成
      
     
    
   
  
  
   为方便扩展及模块化，WebCore在处理浏览页面的过程中，往往使用了类似java或gecko中接口的概念，一般先定义一组公共接口或基类，然后由不同模块来实现。
  
  
   如网络处理部分由WebCore提供一个ResourceHandle类，而在不同的目录如cf、curl、qt、soup、win等中在不同网络库的支持下对ResourceHandle类提供不同的实现，待编译时择机选择对应目录下的实现，这种方式从架构的角度看比较简单，但往往不能让程序同时使用多个网络库，进而由程序动态切换使用不同网络库实现，而gecko在xpcom的基础上提供了对于这种扩展形式的支持；其中Chrome对ResouceHanle类的实现基于WinHttp网络库。
  
  
   同样WebCore对图形库的集成，也是采取这种方式来实现，如由WebCore提供一个GraphicsContext类，然后在不同的目录如cairo、cg、qt、win、wx中在不同的图形库支持下对GraphicsContext提供不同的实现。其中Chrome对GraphicsContext类的实现基于Skia图形库。
  
  
   WebCore中实现的dom、html、svg、css等，往往需要通过一定的方式输出给Javascript的实现如JavascriptCore、V8，以便JS Engineer能认识这些dom元素等，并且能调用其中的方法，这种方式叫做Binding，为了便于将WebCore中相对固定的dom、html、svg、css接口等极其方便的Binding出去，WebKit使用了极其高效及神奇的方式来实现。
  
  
   首先定义一组非标准的idl接口，然后通过运行一组perl脚本如generate-bindings.pl、CodeGenerator.pm、CodeGeneratorJS.pm等，就可根据idl接口定义，生成一组符合指定Javascript实现规则的脚本对象类。这样极大的减轻了开发人员的投入及编码错误的发生。
  
  
   这一点与gecko中将不同的xpcom接口Binding给Javascript实现有本质上的差别，在gecko中通过xpconnect及一组classinfo来维护原生元素与JS对象之间的关系，不同原生元素对应的JS对象的创建及属性方法的Binding完全依赖于xpconnect的实现及classinfo的定义，要添加删除修改Binding的属性与方法，只需修改classinfo；而WebKit中Binding，相对简单明了，不同原生元素对应的JS对象的属性与方法由idl接口文件来定义，而具体实现则交给威力强大的generate-bindings.pl来对应生成实现的代码，这样编译时就可以轻松实现Binding。。。
  
  
   
    四、参考资源
   
  
  
   
    The WebKit Open Source Project
   
  
 


