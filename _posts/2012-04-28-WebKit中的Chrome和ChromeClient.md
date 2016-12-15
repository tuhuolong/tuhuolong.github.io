---
layout: post
title: "WebKit中的Chrome和ChromeClient"
date: 2012-04-28 00:31:00 
comments: true
categories: [chrome]
tags: [chrome]
description: "WebKit中的Chrome和ChromeClient"
keywords: chrome
---


 
  
   浏览器的
  
  
   
    GUI
   
  
  
   接口分成两种，一种是控件的绘制，另一种则是同应用息息相关的窗口交互操作。本文主要介绍后一种，在
  
  
   
    WebKit
   
  
  
   里面，称之为宿主窗口。
  
  
   
    Chrome
   
  
  
   类为
  
  
   
    WebKit
   
  
  
   内核定义了一系列的宿主窗口相关的操作接口，并最终在不同的实现中由
  
  
   
    ChromeClient
   
  
  
   类的派生类来实现，比如，在
  
  
   
    Qt
   
  
  
   里面的
  
  
   
    ChromeClientQt
   
  
  
   类。本文将介绍
  
  
   
    Chrome
   
  
  
   类在
  
  
   
    WebKit
   
  
  
   中的作用，以及在移植
  
  
   
    WebKit
   
  
  
   的时候，如何来实现
  
  
   
    ChromeClient
   
  
  
   类。
  
 
 
  
   关键字：
  
  
   
    WebKit
   
  
  
   ，
  
  
   
    WebCore,Chrome,ChromeClient
   
  
  
   ，宿主窗口，
  
  
   
    WebKit
   
  
  
   移植
  
  
   
    ,ChromeClientQt
   
  
 
 
  
   
    1.
   
   
   
  
  
   
    Chrome
   
  
  
   类在
  
  
   
    WebKit
   
  
  
   中的作用
  
 
 
  
   先说个题外话，春节回家，亲戚朋友都会关心我在做什么。当我告诉它们，我在从事跟浏览器相关的工作的时候，他们迷惑的眼神分明在表示对我进一步解释的期待。这个时候，我会根据不同的人给出两种解释，对那些已经有网络使用背景的人，我告诉他们，我在做跟
  
  
   
    IE
   
  
  
   一样的事情。对不知道网络为何物的人，我会这么跟他们说，网站就像世界各地有很多的电视台，我们在做一个技术，这个技术让你的电视可以跟别的电视不一样，你可以找到很多的电视台，这些电视台提供各种各样的节目，内容，这个技术可以帮你把这个电视台的节目抓下来，并展现给你看，如果没有这个技术，即使你找到这个电台，把东西拿下来你也看不了，同时你可以通过我们这个技术，同这个电视台进行双向交流，就像打进电视热线一样。他们似懂非懂，也许会竖起大拇指，说“你这个科学很先进！我觉得很有前途，你要好好干！以后我们家的电视也要用你们那样的技术！“。
  
 
 
  
   后来我为了这个浏览器的定义，特意查了百度百科，我综合一下，比较好的解释可能是这样，”浏览器是
  
  
   
    Web/Wap
   
  
  
   服务的客户端浏览程序，可向
  
  
   
    Web/Wap
   
  
  
   服务器发送各种请求，并对服务器发回的超文本信息和各种多媒体数据格式进行解释、显示和播放，并让用户与此些文件互动“。
  
 
 
  
   从上面这个定义里面，我简单提炼出了浏览器需要的几个功能件：发送请求（
  
  
   
    http
   
  
  
   ），解释超文本信息和各种多媒体数据（解析），显示和播放这些信息（排版，渲染，以及可能存在的插件），互动（交互）。这几个模块里面，同平台
  
  
   
    GUI
   
  
  
   相关的是排版、渲染和互动。而
  
  
   
    Chrome
   
  
  
   类就是
  
  
   
    WebCore
   
  
  
   内核渲染网页以及互动所需的并定义出来会由移植实现的同平台相关的接口，这个接口不包括控件的渲染。
  
  
   
    ChromeClient
   
  
  
   的具体实现（比如
  
  
   
    ChromeClientQt
   
  
  
   ），则是移植对这些接口的实现。如果以
  
  
   
    MVC
   
  
  
   的角度来看，
  
  
   
    Chrome
   
  
  
   就是
  
  
   
    V
   
  
  
   ，当然
  
  
   
    WebKit
   
  
  
   并非
  
  
   
    MVC
   
  
  
   的架构。
  
 
 
  
   
    2.
   
   
   
  
  
   类关系
  
 
 
  
   
    Chrome
   
  
  
   是对应于
  
  
   
    Page
   
  
  
   的，每个
  
  
   
    Page
   
  
  
   都会在构造函数中创建出一个
  
  
   
    Chrome
   
  
  
   对象，并将对象指针赋给
  
  
   
    Page
   
  
  
   类
  
  
   
    m_chrome
   
  
  
   成员。
  
 
 
 
 
  
  
 
 
 
 
  
   
    Chrome
   
  
  
   类继承自
  
  
   
    HostWindow
   
  
  
   类，
  
  
   
    HostWindow
   
  
  
   类定义了宿主窗口必须实现的一系列接口，包括刷窗口（及内容），滚动，窗口相对坐标和屏幕坐标之间的相互转换等接口。
  
  
   
    HostWindow
   
  
  
   是个抽象接口类，没有构造函数，无法实例化，
  
  
   
    Chrome
   
  
  
   通过继承对这个类进行了实现。
  
 
 
 
 
 
 
  
  
 
 
  
   在类
  
  
   
    Chrome
   
  
  
   的实现中，通过了组合的方式，将具体实现委托给
  
  
   
    ChromeClient
   
  
  
   。如
  
 
 
  
   
    void Chrome::invalidateContentsAndWindow(const IntRect& updateRect, bool immediate)
   
  
 
 
  
   
    {
   
  
 
 
  
   
    m_client->;invalidateContentsAndWindow(updateRect, immediate);
   
  
 
 
  
   
    }
   
  
 
 
  
   
    ChromeClient
   
  
  
   也是抽象接口类，没有构造函数，必须在
  
  
   
    porting
   
  
  
   的时候，进行继承，由不同的移植依托自己的平台进行实现。以
  
  
   
    Qt
   
  
  
   移植为例，由
  
  
   
    ChromeClientQt
   
  
  
   类来最终实现（
  
  
   
    google
   
  
  
   的
  
  
   
    Chrome
   
  
  
   分支主要由
  
  
   
    ChromeClientImpl
   
  
  
   类实现）。
  
 
 
 
 
 
 
  
  
 
 
  
   而在
  
  
   
    ChromeClientQt
   
  
  
   的具体实现中，很多又是通过
  
  
   
    Page
   
  
  
   类的内部
  
  
   
    QwebPageClient
   
  
  
   类数据成员（
  
  
   
    client
   
  
  
   ）提供的接口来实现的。同样的，
  
  
   
    QwebPageClient
   
  
  
   是抽象接口类，无法实例化，通过继承类
  
  
   
    PageClientQWidget
   
  
  
   来实现。而
  
  
   
    PageClientQWidget
   
  
  
   的实现又最终通过
  
  
   
    qwebview
   
  
  
   来实现，这个过程有点绕弯弯。
  
 
 
 
 
  
   
    
    
   
  
 
 
  
  
  
   
    PageClientQWidget
   
  
  
   的实例化在
  
  
   
    QwebPage::setView
   
  
  
   接口中完成。
  
 
 
  
   在代码结构上，
  
  
   
    ChromeClientQt.cpp
   
  
  
   和
  
  
   
    PageClientQt.cpp
   
  
  
   都位于
  
  
   
    WebKit/qt/WebCoreSupport
   
  
  
   目录下，表面他们为了实现
  
  
   
    WebCore
   
  
  
   需要实现的移植层代码。
  
 
 
  
   
    3.
   
   
   
  
  
   主要接口
  
 
 
  
   
    3.1.
   
   
   
  
  
   
    ChromeClientQt
   
  
 
 
  
   
    ChromeClientQt(QWebPage* webPage);
   
  
 
 
  
   描述：构造函数，以
  
  
   
    QWebPage
   
  
  
   为依赖对象创建。一般在创建
  
  
   
    Page
   
  
  
   对象前调用这个构造函数实例化
  
  
   
    ChromeClientQt
   
  
  
   ，并以之为参数创建
  
  
   
    Page
   
  
  
   对象。
  
 
 
  
   
    3.2.
   
   
   
  
  
   
    windowRect
   
  
 
 
  
   
    FloatRect windowRect();
   
  
 
 
  
   描述：获得浏览器窗口区域的大小，这个区域不止包括显示区域，还包括状态条，菜单栏，工具栏等等。
  
 
 
  
   
    3.3.
   
   
   
  
  
   
    pageRect
   
  
 
 
  
   
    FloatRect pageRect();
   
  
 
 
  
   描述：获取显示区域的大小，在排版的时候会经常调用到。
  
 
 
  
   
    3.4.
   
   
   
  
  
   
    createWindow
   
  
 
 
  
   
    Page* createWindow(Frame*, const FrameLoadRequest&, const WindowFeatures&, const NavigationAction&);
   
  
 
 
  
   描述：创建一个窗口。一般是在新建一个新窗口或者
  
  
   
    tab
   
  
  
   页的时候调用。一个新窗口的创建意味着会有一个
  
  
   
    qwebview
   
  
  
   的创建和
  
  
   
    qwebpage
   
  
  
   的创建。窗口创建成功以后，还要在
  
  
   
    qwebframe
   
  
  
   主帧中发起网页请求。这个接口在移植中非常重要。
  
 
 
  
   
    3.5.
   
   
   
  
  
   
    setToolbarsVisible
   
  
 
 
  
   
    setToolbarsVisible(bool visible);
   
  
 
 
  
   
    bool toolbarsVisible();
   
  
 
 
  
   
    void setStatusbarVisible(bool);
   
  
 
 
  
   
    statusbarVisible();
   
  
 
 
  
   
    setScrollbarsVisible(bool);
   
  
 
 
  
   
    scrollbarsVisible();
   
  
 
 
  
   
    setMenubarVisible(bool);
   
  
 
 
  
   
    menubarVisible();
   
  
 
 
  
   描述：不用多说，这些接口是用来控制这些区域的显示与否。
  
 
 
  
   
    3.6.
   
   
   
  
  
   
    addMessageToConsole
   
  
 
 
  
   
    void addMessageToConsole(MessageSource, MessageType,
   
  
 
 
  
   
    MessageLevel, const String& message,
   
  
 
 
  
   
    unsigned int lineNumber,
   
  
 
 
  
   
    const String& sourceID);
   
  
 
 
  
   描述：很多浏览器都在提供了
  
  
   
    javascript
   
  
  
   控制台工具，方便开发人员进行调试，这个接口就是要把信息在控制台中打印出来。
  
 
 
  
   
    3.7.
   
   
   
  
  
   
    runJavaScriptAlert
   
  
 
 
  
   
    void runJavaScriptAlert(Frame*, const String&);
   
  
 
 
  
   
    bool runJavaScriptConfirm(Frame*, const String&);
   
  
 
 
  
   
    bool runJavaScriptPrompt(Frame*, const String& message,
   
  
 
 
  
   
    const String& defaultValue, String& result);
   
  
 
 
  
   描述：用来实现
  
  
   
    javascript
   
  
  
   中的
  
  
   
    alert
   
  
  
   框，确认框，提示框，完成同用户的交互。
  
 
 
  
   
    3.8.
   
   
   
  
  
   
    setStatusbarText
   
  
 
 
  
   
    void setStatusbarText(const String&);
   
  
 
 
  
   描述：设置状态条显示信息。
  
 
 
  
   
    3.9.
   
   
   
  
  
   
    invalidateContentsAndWindow
   
  
 
 
  
   
    void invalidateContentsAndWindow
   
   
   
   
    (const IntRect& windowRect, bool immediate);
   
  
 
 
  
   描述：非常重要的一个移植接口，用来刷屏（包含内容和窗体）。一般情况下，平台趋向于一个异步的调用（并不马上调用），也就是说，可能多次的
  
  
   
    invalidateContentsAndWindow
   
  
  
   调用的结果才导致一次屏幕的刷新。
  
  
   
    Immediate
   
  
  
   参数则用来表示是否要立即进行屏幕刷新，不过很多移植都不对这个参数进行支持。
  
 
 
  
   
    3.10.
   
   
   
  
  
   
    scroll
   
  
 
 
  
   
    void scroll(const IntSize& delta, const IntRect& scrollViewRect,
   
  
 
 
  
   
    const IntRect&);
   
  
 
 
  
   描述：滚动支持。移植一般调用
  
  
   
    native widget
   
  
  
   缺省的
  
  
   
    scroll
   
  
  
   功能来实现这个接口。
  
 
 
  
   
    3.11.
   
   
   
  
  
   
    windowToScreen
   
  
 
 
  
   
    IntRect  windowToScreen(const IntRect& rect) const
   
  
  
   ；
  
 
 
  
   
    IntPoint  screenToWindow(const IntPoint& point) const;
   
  
 
 
  
   描述：非常重要的移植接口，用来实现基于控件或者小窗口的相对坐标和屏幕坐标之间的转换。在排版的时候，会经常用到这两个转换。
  
 
 
  
   
    3.12.
   
   
   
  
  
   
    requestGeolocationPermissionForFrame
   
  
 
 
  
   
    void requestGeolocationPermissionForFrame(
   
  
 
 
  
   
    Frame* frame, Geolocation* geolocation)
   
  
  
   ；
  
 
 
  
   
    void cancelGeolocationPermissionRequestForFrame(
   
  
 
 
  
   
    Frame* frame, Geolocation* geolocation);
   
  
 
 
  
   描述：在
  
  
   
    Geolocation
   
  
  
   （基于浏览器的地理定位技术）的时候，浏览器在调用
  
  
   
    Geolocation
 API
   
  
  
   获取您的地理位置之前，会有一个用户确认，这两个接口就是用来实现这个确认以及确认的取消。
  
 
 
  
   
    3.13.
   
   
   
  
  
   
    createPopupMenu
   
  
 
 
  
   
    QWebSelectMethod* createSelectPopup() const;
   
  
 
 
  
   
    PassRefPtr<PopupMenu>; createPopupMenu(
   
  
 
 
  
   
    PopupMenuClient* client) const;
   
  
 
 
  
   描述：在
  
  
   
    WebKit
   
  
  
   中，网页中的下拉框
  
  
   
    (select+option)
   
  
  
   并不是作为一个控件来实现，而是结合画
  
  
   
    input
   
  
  
   ，画下来三角和弹出选项来实现。这两个接口就是用来弹出选项的。画框和
  
  
   
    input
   
  
  
   则在
  
  
   
    RenderThemeQt
   
  
  
   中实现。
  
 
 
  
   
    3.14.
   
   
   
  
  
   
    show
   
  
 
 
  
   
    void show();
   
  
 
 
  
   描述：用来显示窗体和内容，立即执行刷屏。
  
 


