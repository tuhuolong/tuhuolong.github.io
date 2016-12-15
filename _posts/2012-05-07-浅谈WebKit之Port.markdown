---
layout: post
title: "浅谈WebKit之Port"
date: 2012-05-07 15:09:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "浅谈WebKit之Port"
keywords: webkit
---


 
  WebKit作为一个浏览器引擎，其相对于Gecko而言一个较大的特点就是便于移植，嵌入到其他程序中，目前大家已了解使用WebKit引擎的应用包括Safari、iPhone、Chrome、Android、Nokia S60 Browser及KDE QT4.4等，同时还有其他方面的移植如Gtk、wxWidget、3D等，可以说WebKit从架构上讲其Port移植方面的设计及应用，是非常优秀的。这一点相对于Gecko有相当大的优势，有时间可以参考一下浅谈Gecko关键部分之十二Embedding。为了更深入的了解WebKit，我们现在就从WebKit有关Ports方面入手，了解其有关Port方面的设计，从而了解究竟如何能移植WebKit到自己的应用中。
 
 
  
   
    
     一、有关Port方面的概述
    
   
   
    
     在通过了解
    
    
     浅谈WebKit之WebCore
    
    
     之后，应该说WebKitPort方面的内容是可以很广的，例如可将不同的图形库、网络库与WebCore集成，提供不同的Port接口供外部程序使用等，例如同样在windows平台上可以运行的Google
 Chrome和Safari就是针对WebKit的不同移植。
    
    
     
      
       我们想了解有关Port方面的主要内容在于提供不同的Port接口供外部程序使用以及如何与外部程序交互，因为WebKit中的其它两部分WebCore、Javascript实现，从逻辑上讲是不直接提供接口给外部程序使用的。同时为了完成浏览器的核心功能，WebKit也需要从外部程序中通过Port接口的方式获取一些支持。
      
      
       
        
         从这个角度讲WebKit作为一个相对独立的整体，它与外部程序之间的交互也就有一组相对固定的接口来定义及维护它们之间的关系，它们之间的关系与插件跟浏览器引擎之间的关系完全类似，接口相当一组协议，有的是由WebKit来实现，而供外部程序调用；有的的正好相反。
        
        
         
          
           通过前面的了解我们知道WebKit的主要功能集中在分析Html、渲染布局Web内容以及Javascript实现方面等，而这些Web内容显示在哪个窗口及消息处理的启动循环等都需要由外部程序来提供。
          
          
           
            
             
              二、初步分析已有WebKit Port移植实现
             
            
            
            
            
             
              1、与WebCore交互接口的实现
             
             
              在WebKit源代码目录结构中WebKit目录下分别包含gtk、mac、qt、win、wx目录，其分别对应不同的Port移植方式，在每一个目录下面都包括 WebCoreSupport目录，而在不同的WebCoreSupport目录下分别包含有对类接口WebCore::ChromeClient、 WebCore::ContextMenuClient、WebCore::DragClient、WebCore::EditorClient、 WebCore::FrameLoaderClient、WebCore::InspectorClient等的实现，它们代表外部程序提供给
 WebKit内部使用的接口实现，其中WebCore::ChromeClient、WebCore::FrameLoaderClient非常重要。
             
            
            
             
              
               
                初步了解其接口定义能基本了解其对应的含义，这些接口往往需要由Port移植部分来提供实现，往往由WebKit内部根据一定的条件来调用。下面初步来了解几个主要接口：
                
                 
                  WebCore::ChromeClient接口：
                 
                
               
              
             
            
            
            
            
             
              
               
                [cpp]
               
               
                view
 plain
               
               
                copy
               
               
               
              
             
             
              
               
                
                 //往往在运行window.open脚本时调用，以便由外部程序决定如何打开一个新页面如新建一个窗口、新建一个Tab页签等；
                
                
                
               
              
              
               
                
                 virtual
                
                
                 WebCore::Page* createWindow(WebCore::Frame*,
                
                
                 const
                
                
                 WebCore::FrameLoadRequest&,
                
                
                 const
                
                
                 WebCore::WindowFeatures&);
                
               
              
              
               
               
              
              
               
                
                 //通知外部程序显示页面；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 show();
                
               
              
              
               
               
              
              
               
                
                 virtual
                
                
                
                
                 bool
                
                
                 canRunModal();
                
               
              
              
               
               
              
              
               
                
                 //通知外部程序以Modal的方式显示页面；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 runModal();
                
               
              
              
               
               
              
              
               
                
                 //通知外部程序显示JS警告提示窗口；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 runJavaScriptAlert(WebCore::Frame*,
                
                
                 const
                
                
                 WebCore::String&);
                
               
              
              
               
               
              
              
               
                
                 //通知外部程序显示JS警告确认窗口；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 bool
                
                
                 runJavaScriptConfirm(WebCore::Frame*,
                
                
                 const
                
                
                 WebCore::String&);
                
               
              
              
               
               
              
              
               
                WebCore::FrameLoaderClient接口：
               
              
              
               
                
                 //检查是否拥有主页面窗口；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 bool
                
                
                 hasWebView()
                
                
                 const
                
                
                 ;
                
               
              
              
               
                
                 //检查是否拥有页面窗口；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 bool
                
                
                 hasFrameView()
                
                
                 const
                
                
                 ;
                
               
              
              
               
               
              
              
               
                
                 //通知外部程序有关http请求开始、结束、获取数据等，如通常浏览器状态栏显示的信息；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidReceiveResponse(WebCore::DocumentLoader*, unsigned
                
                
                 long
                
                
                 identifier,
                
                
                 const
                
                
                 WebCore::ResourceResponse&);
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidReceiveContentLength(WebCore::DocumentLoader*, unsigned
                
                
                 long
                
                
                 identifier,
                
                
                 int
                
                
                 lengthReceived);
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidFinishLoading(WebCore::DocumentLoader*, unsigned
                
                
                 long
                
                
                 identifier);
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidFailLoading(WebCore::DocumentLoader*, unsigned
                
                
                 long
                
                
                 identifier,
                
                
                 const
                
                
                 WebCore::ResourceError&);
                
               
              
              
               
               
              
              
               
                
                 //通知外部程序WebKit内部主要事件处理，以便外部程序及时响应或创建维护数据等
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidHandleOnloadEvents();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidReceiveServerRedirectForProvisionalLoad();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidCancelClientRedirect();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchWillPerformClientRedirect(
                
                
                 const
                
                
                 WebCore::KURL&,
                
                
                 double
                
                
                 interval,
                
                
                 double
                
                
                 fireDate);
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidChangeLocationWithinPage();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchWillClose();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidReceiveIcon();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidStartProvisionalLoad();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidReceiveTitle(
                
                
                 const
                
                
                 WebCore::String&);
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidCommitLoad();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidFinishDocumentLoad();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidFinishLoad();
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 dispatchDidFirstLayout();
                
               
              
              
               
               
              
              
               
                
                 //告诉外部程序需要提供切换到一个新页面状态。此时外部程序往往会新建FrameView，并将FrameView与Frame关联，设置原生窗口句柄及其消息处理机制等等；
                
                
                
               
              
              
               
                
                 virtual
                
                
                
                
                 void
                
                
                 transitionToCommittedForNewPage();
                
               
              
              
               
               
              
              
               
                
                 //告诉外部程序创建一个新的Frame，如遇到html中iframe标签时，需要外部程序创建一个新的Frame及原生窗口句柄等；
                
                
                
               
              
              
               
                
                 virtual
                
                
                 PassRefPtr createFrame(
                
                
                 const
                
                
                 WebCore::KURL& url,
                
                
                 const
                
                
                 WebCore::String& name, WebCore::HTMLFrameOwnerElement* ownerElement,
                
               
              
              
               
                
                 const
                
                
                 WebCore::String& referrer,
                
                
                 bool
                
                
                 allowsScrolling,
                
                
                 int
                
                
                 marginWidth,
                
                
                 int
                
                
                 marginHeight);
                
               
              
              
               
               
              
              
               
                
                 //告诉外部程序需要创建一个Plugin实例，从而创建其原生窗口等等；
                
                
                
               
              
              
               
                
                 virtual
                
                
                 WebCore::Widget* createPlugin(
                
                
                 const
                
                
                 WebCore::IntSize&, WebCore::Element*,
                
                
                 const
                
                
                 WebCore::KURL&,
                
                
                 const
                
                
                 Vector&,
                
                
                 const
                
                
                 Vector&,
                
                
                 const
                
                
                 WebCore::String&,
                
                
                 bool
                
                
                 loadManually);
                
               
              
             
            
            
             
              2、对WebCore中的page/loader等方面的类提供对应Port的实现支持
             
             
              如EventHandlerWin.cpp、FrameLoaderWin.cpp、DocumentLoaderWin.cpp、DocumentLoaderWin.cpp、WidgetWin.cpp、KeyEventWin.cpp等
              
               
                
                 3、实现WebView及WebFrame等以便外部程序嵌入WebKit
                
                
                 不同的Port移植对WebView及WebFrame的定义及实现有所不同，但其与WebCore中的Page、Frame之间的关系大致与浅谈WebKit之WebCore篇图一描述相一致。
                 
                  
                   具体关于WebView、WebFrame的定义与实现，特别是初始化时的动作可根据不同的Port移植而有所不同，同时初始化时会将上面提到的WebCore Port接口实现告诉WebKit内部。主要示例代码如下：
                   
                    
                    
                    
                    
                    
                     
                      
                       
                        [cpp]
                       
                       
                        view
 plain
                       
                       
                        copy
                       
                       
                       
                      
                     
                     
                      
                       
                        
                         static
                        
                        
                        
                        
                         void
                        
                        
                         webkit_web_view_init(WebKitWebView* webView)
                        
                       
                      
                      
                       
                        {
                       
                      
                      
                       
                        WebKitWebViewPrivate* priv = WEBKIT_WEB_VIEW_GET_PRIVATE(webView);
                       
                      
                      
                       
                        webView->;priv = priv;
                       
                      
                      
                       
                        priv->;corePage =
                        
                         new
                        
                        
                         Page(
                        
                        
                         new
                        
                        
                         WebKit::ChromeClient(webView),
                        
                        
                         new
                        
                        
                         WebKit::ContextMenuClient(webView),
                        
                        
                         new
                        
                        
                         WebKit::EditorClient(webView),
                        
                        
                         new
                        
                        
                         WebKit::DragClient,
                        
                        
                         new
                        
                        
                         WebKit::InspectorClient);
                        
                       
                      
                      
                       
                        priv->;mainFrame = WEBKIT_WEB_FRAME(webkit_web_frame_new(webView));
                       
                      
                      
                       
                        priv->;lastPopupXPosition = priv->;lastPopupYPosition = -1;
                       
                      
                      
                       
                        priv->;editable =
                        
                         false
                        
                        
                         ;
                        
                       
                      
                      
                       
                        ................................
                       
                      
                      
                       
                       
                      
                      
                       
                        priv->;webSettings = webkit_web_settings_new();
                       
                      
                      
                       
                        webkit_web_view_update_settings(webView);
                       
                      
                      
                       
                        ..................................
                       
                      
                      
                       
                        }
                       
                      
                      
                       
                       
                      
                      
                       
                        WebKitWebFrame* webkit_web_frame_new(WebKitWebView* webView)
                       
                      
                      
                       
                        {
                       
                      
                      
                       
                        g_return_val_if_fail(WEBKIT_IS_WEB_VIEW(webView), NULL);
                       
                      
                      
                       
                       
                      
                      
                       
                        WebKitWebFrame* frame = WEBKIT_WEB_FRAME(g_object_new(WEBKIT_TYPE_WEB_FRAME, NULL));
                       
                      
                      
                       
                        WebKitWebFramePrivate* priv = frame->;priv;
                       
                      
                      
                       
                        WebKitWebViewPrivate* viewPriv = WEBKIT_WEB_VIEW_GET_PRIVATE(webView);
                       
                      
                      
                       
                       
                      
                      
                       
                        priv->;webView = webView;
                       
                      
                      
                       
                        priv->;client =
                        
                         new
                        
                        
                         WebKit::FrameLoaderClient(frame);
                        
                       
                      
                      
                       
                        priv->;coreFrame = Frame::create(viewPriv->;corePage, 0, priv->;client).get();
                       
                      
                      
                       
                        priv->;coreFrame->;init();
                       
                      
                      
                       
                       
                      
                      
                       
                        
                         return
                        
                        
                         frame;
                        
                       
                      
                      
                       
                        }
                       
                      
                     
                    
                    
                     
                     
                     
                      
                       4、Chrome中对Port移植方面的实现
                      
                      
                       其基本上与其他Port移植类似，其主要代码在webkit/glue目录中，可重点关注带client_impl.cc后缀的文件、 webview_impl.cc、webwidget_impl.cc等；但是其究竟如何创建原生windows窗口、如何创建Render进程、 Render进程与创建的原生windows窗口的关系如何等需要更进一步深入研究Chrome，如果能从上面提到的Port部分入手也许很快就可得到答案，这一点以后有机会单独研究。
                      
                     
                     
                      
                       5、Android中对Port移植方面的实现
                      
                     
                     
                      
                       其实现有点特殊，由于Andriod将WebKit以一个Java类接口的方式提供给Java环境使用(不像上面提到的Chrome、Safari等都是将WebKit以 一个C++动态或静态库的方式供C/C++外部程序调用)，这样WebKit内部与外部即JavaVM的交互(如上面提到的ChromeClient、
 FrameLoaderClient接口实现)需要一个Bridge类来协调处理，同时WebView、WebFrame接口绑定给JavaVM的jni接口实现也需要通过这个Bridge来支持协调处理。具体可详细参考android源码代码中WebCore\platform\android目录下的源文件。
                      
                      
                       
                        
                         
                          6、通过进一步了解WebCore Port接口及其实现，可以加深这样一个认识：
                         
                        
                        
                         
                          如果从MVC的角度来看整个基于WebKit的浏览器(当然不尽合理)，WebKit的Port部分相当于V部分，它提供显示页面内容及其辅助信息(如提示状态)的场所(即原生窗口)以及控制该显示场所的状态变化及消息响应(如改变大小、鼠标移动等)；而M部分往往由WebCore来实现，至于WebCore如何组织DOM则往往由htmlparser部分根据DOM定义来组织，如何在提供的显示场所显示Web内容则往往由WebCore中的layout部分来实现，其中充分利用了Css定义来布局显示该显示的内容；一旦涉及控制或动态处理往往由Port部分发起而由Javascript脚本来实现处理，其任务由JavascriptCore或V8来完成。
                         
                         
                          
                           
                            一般说来新打开一个页面，Port部分需要提供一个主显示场所(即原生窗口)，如果页面中含有iframe标签，则需要在主显示场所内创建一个子显示场所，以显示iframe标签对应src的内容；如果页面中含有embed/object等插件标签同样往往也需要在主显示场所内创建一个子显示场所(除非windowless)，以交由插件实现在提供的显示场所中显示内容。
                           
                           
                            
                             
                              特别需要说明的是我们通常看到的页面表单元素input text field、textArea、button、radiobutton等往往不像window图形库中的按钮、菜单、输入框等会对应一个原生窗口，页面中的表单元素在一个显示场所(即原生窗口)中完全是利用Css等通过layout方式来达到我们所看到的类似原生按钮、输入框、列表框、滚动条等效果，其中特别是能准确定位元素大小、设置focus、光标显示、响应事件等，这充分的说明了浏览器引擎内部布局部分的威力所在。
                             
                             
                              
                               
                                从另外一个角度来看一个页面一般说来(除非遇到iframe或插件需要另外提供一块子画布)相当于一块画布，浏览器引擎能在其精确的位置绘制不同颜色的文字、图片、图标等，同时根据当前的鼠标及一个模拟的输入提示光标位置，接收键盘输入操作。页面中的绝大多数元素与原生的窗口元素几乎没有关联，完全通过组合、布局、准确定位来处理一切。。。
                               
                               
                                
                                 
                                  
                                   三、如何利用WebKit?
                                  
                                 
                                 
                                  
                                   了解WebKit Port部分，对我们如何利用WebKit有非常现实的意义，目前已经将WebKit移植到多种平台如windows、qt、gtk、mac、wx、java、framebuffer等，甚至移植到python、ruby及3D等环境中去。通过借鉴或利用这些已有的WebKit Port实现，完全可以将WebKit发扬广大。
                                  
                                  
                                   
                                    
                                     前一阶段正好得到一个网友抓取网页的需求，试想目前移植利用WebKit基本都用来显示页面，往往涉及图形显示方面，但随着ajax及动态页面的广泛使用，未来动态生成的页面越来越多，传统的搜索引擎仅仅抓取静态的页面内容显然是不够的，现代化的搜索引擎应该能抓取动态的页面内容，这样它从某种意义讲相当于一个能获取对应的动态页面但不真正显示出其内容的浏览器，这样一个搜索引擎不仅能分析DOM树，同时能运行Javascript脚本(如运行ajax)，以真正完整获取页面内容，其实这样一个搜索引擎如果利用WebKit来实现的话，应该是个不错的选择，在我们了解WebKit
 Port部分之后，我们是否可以来模拟一个不真正具备图形显示方面的Port，进而充分利用WebKit中的WebCore及Javascript实现方面的功能呢？一点想法，今后有机会可以试试，或许Google、Yahoo的搜索引擎已经有了相关的实现，不知是否使用的就是WebKit?应该不会，有谁清楚的话，烦请通知一声。
                                    
                                    
                                     
                                      
                                       但愿我们也能利用利用WebKit整出一个象模象样的东东如机顶盒浏览器、手机浏览器等等。。
                                      
                                      
                                       
                                        
                                         
                                          四、参考资源
                                          
                                          
                                         
                                        
                                        
                                         The WebKit Open Source Project
                                        
                                        
                                         
                                          Google Chrome Home
                                         
                                         
                                          
                                          
                                         
                                         
                                          Android - An Open Handset Alliance Project
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


