---
layout: post
title: "WebKit Page对象的分析"
date: 2012-05-07 15:01:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "WebKit Page对象的分析"
keywords: webkit
---


 
  
   
    Page对象是webkit的核心对象之一,顾名思义，Page数据结构就是描述览器上我们打开的一个页面,这样一个页面包括很多部分,比如菜单控制，拖拽控制，页面显示，以及一些参数设置等。所以webkit中的Page对象里面也对应很多这样相关的对象，下面是一张表示这些对象关系的类图。
   
  
 
 
  
   
    
   
  
 
 
 
 
  
   
    从上图中可以看出，Page里面主要是一些对象的组合，Page自身的代码并不是很多,里面的对象根据名字大概能够猜出个一二,下面主要讲一下两个比较重要的对象.
   
  
 
 
  
   
    1. Frame
   
  
 
 
  
   
    Frame应该是webcore里面最重要的一个核心对象，从page的角度上来看，页面就是有众多的Frame构成，当然，一定会有一个主Frame,Page中持有的这个Frame就是主Frame,也就是RootFrame,这个对象比较复杂，后面我会单独拿出来分析。
   
  
 
 
  
   
    2. chrome
   
  
 
 
  
   
    chrome是与显示相关的一个对象，比如刷新页面窗口，滚动窗口等，就会用到chrome中的接口，chrome也是连接webcore与webkit（指webkit的port部分）的核心对象。下面是一个关于chrome的类图
   
  
 
 
  
   
    
   
  
 
 
  
  
 
 
 
 
  
   
    大家看到，chrome是继承自HostWindow，就是表示一个宿主窗口，这里贴出HostWindow的代码
   
  
 
 
  
   
   
  
 
 
  
   
    
     [cpp]
    
    
     view
 plain
    
    
     copy
    
    
    
   
  
  
   
    
     
      class
     
     
      HostWindow :
     
     
      public
     
     
      Noncopyable {
     
    
   
   
    
     
      public
     
     
      :
     
    
   
   
    
     
      virtual
     
     
      ~HostWindow() { }
     
    
   
   
    
    
   
   
    
     
      // Requests the host invalidate the window, not the contents.  If immediate is true do so synchronously, otherwise async.
     
     
     
    
   
   
    
     
      virtual
     
     
     
     
      void
     
     
      invalidateWindow(
     
     
      const
     
     
      IntRect& updateRect,
     
     
      bool
     
     
      immediate) = 0;
     
    
   
   
    
    
   
   
    
     
      // Requests the host invalidate the contents and the window.  If immediate is true do so synchronously, otherwise async.
     
     
     
    
   
   
    
     
      virtual
     
     
     
     
      void
     
     
      invalidateContentsAndWindow(
     
     
      const
     
     
      IntRect& updateRect,
     
     
      bool
     
     
      immediate) = 0;
     
    
   
   
    
    
   
   
    
     
      // Requests the host scroll backingstore by the specified delta, rect to scroll, and clip rect.
     
     
     
    
   
   
    
     
      virtual
     
     
     
     
      void
     
     
      scroll(
     
     
      const
     
     
      IntSize& scrollDelta,
     
     
      const
     
     
      IntRect& rectToScroll,
     
     
      const
     
     
      IntRect& clipRect) = 0;
     
    
   
   
    
    
   
   
    
     
      // Requests the host invalidate the contents, not the window.  This is the slow path for scrolling.
     
     
     
    
   
   
    
     
      virtual
     
     
     
     
      void
     
     
      invalidateContentsForSlowScroll(
     
     
      const
     
     
      IntRect& updateRect,
     
     
      bool
     
     
      immediate) = 0;
     
    
   
   
    
    
   
   
    
     
      #if ENABLE(TILED_BACKING_STORE)
     
     
     
    
   
   
    
     
      // Requests the host to do the actual scrolling. This is only used in combination with a tiled backing store.
     
     
     
    
   
   
    
     
      virtual
     
     
     
     
      void
     
     
      delegatedScrollRequested(
     
     
      const
     
     
      IntSize& scrollDelta) = 0;
     
    
   
   
    
     
      #endif
     
     
     
    
   
   
    
    
   
   
    
     
      // Methods for doing coordinate conversions to and from screen coordinates.
     
     
     
    
   
   
    
     
      virtual
     
     
      IntPoint screenToWindow(
     
     
      const
     
     
      IntPoint&)
     
     
      const
     
     
      = 0;
     
    
   
   
    
     
      virtual
     
     
      IntRect windowToScreen(
     
     
      const
     
     
      IntRect&)
     
     
      const
     
     
      = 0;
     
    
   
   
    
    
   
   
    
     
      // Method for retrieving the native client of the page.
     
     
     
    
   
   
    
     
      virtual
     
     
      PlatformPageClient platformPageClient()
     
     
      const
     
     
      = 0;
     
    
   
   
    
    
   
   
    
     
      // To notify WebKit of scrollbar mode changes.
     
     
     
    
   
   
    
     
      virtual
     
     
     
     
      void
     
     
      scrollbarsModeDidChange()
     
     
      const
     
     
      = 0;
     
    
   
   
    
    
   
   
    
     
      // Request that the cursor change.
     
     
     
    
   
   
    
     
      virtual
     
     
     
     
      void
     
     
      setCursor(
     
     
      const
     
     
      Cursor&) = 0;
     
    
   
   
    
     };
    
   
  
 
 
 
 
  
   
    chrome中有一个ChromeClient，这个chromeClient都是由客户实现，这样webcore就能够和外面交互
   
  
 
 
  
   
    附chrome代码
   
  
 
 
  
   
   
  
 
 
  
   
    
     [cpp:collapse]
    
    
     +
 expand source
    
    
     view
 plain
    
    
     copy
    
    
    
   
  
  
  
 
 
 
 
  
   
    其实chrome的大部分功能都是委托给chromeclient实现。
   
  
 
 
 
 
  
   chrome中有一个接口名如下:
  
 
 
 
 
  
   
    Page* Chrome::createWindow(Frame* frame, const FrameLoadRequest& request, const WindowFeatures& features, const NavigationAction& action) ;
   
  
 
 
  
   
    其实就是创建一个新的窗口,那么在什么时候会用到此接口,比如打开一个新的标签页，这时候就需要创建一个新的窗口，所以，从这个角度来说，chromeclient就是为webcore里需要显示的内容提供一个场所，对需要涉及到的显示操作，提供一种实现。
   
  
 
 
  
  
 
 
  
   
    WebChromeClient提供一种ChromeClient的实现，而实际的窗口相关的操作，都会定义在webview里面，所以不同的平台应该有不同WebView的实现。
   
  
 


