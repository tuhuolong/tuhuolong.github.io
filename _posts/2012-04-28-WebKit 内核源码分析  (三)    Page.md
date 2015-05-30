---
layout: post
title: WebKit 内核源码分析  (三)    Page
date: 2012-04-28 00:23:00
categories: [WebKit]
tags: [webkit, stylesheet, 浏览器, chrome, user, qt]
---
浏览器的请求一般是以页面请求为单位，当用户通过网址栏输入一个url，浏览器就开始一个页面请求。而一个页面请求可能包含有一到多个页面子帧，以及图片、CSS和插件等派生子资源。Page类就是用来对应这样的页面请求。Page类是WebKit中非常重要的一个类，它就像内核对外的一个聚合器。
关键词:WebKit内核源代码,WebCore,Page,Frame，WebKit架构
##1.    概述
浏览器的请求一般是以页面请求为单位，当用户通过网址栏输入一个url，浏览器就开始一个页面请求。而一个页面请求可能包含有一到多个页面子帧，以及图片、CSS和插件等派生子资源。Page类就是用来对应这样的页面请求。前进后退，导航，编辑，右键菜单，设置，Inspector等这些用户参与的动作，大部分是同Page相关的。而标记语言解析、排版、加载则更多地同Frame相关。
我们通过几个图来看下Qt移植中Page类同应用之间的关系。
 
![](http://hi.csdn.net/attachment/201103/9/0_1299648672b0K9.gif)
 
 
QWebPage通过QWebPagePrivate维护Page类的指针，并在QWebPagePrivate的构造函数中实例化Page对象。QWebPage类通过之后的createMainFrame调用实例化QwebFrame，而在QwebFrameData的构造函数中，以Page指针为参数调用了Frame::create 创建出Frame对象。
 
![](http://hi.csdn.net/attachment/201103/9/0_1299648680RXJs.gif)
  
 Page类通过组合其它类的方式，实现了很多功能，Page类本身并没有多少代码。
##2.    类关系
##![](http://hi.csdn.net/attachment/201103/9/0_1299648661Mm8q.gif)
##
##
##2.1  PageGroup
PageGroup并不是用来对Page进行管理的，而是设计用来将一些具有共同的属性或者设置的Page编成组的，以方便对这些属性进行管理。目前这样的属性包括localStorage的属性，IndexDB，User
 Script，User StyleSheet等。最常见的同PageGroup相关的操作是维护已访问链接（如addVisitedLink等接口）。根据地瓜的理解，假设WebKit内核之上架设多个应用（浏览器是一个应用），比较可能的是，一个应用独立一个PageGroup。这里同多tab页没有关系，多tab页属于同一个PageGroup。地瓜曾在mailing
 group上就这个问题咨询过，一位RIM的同学给我举了一个例子，比如一个基于WebKit的邮件程序，一方面他可能调用基于webkit的browser来显示网页，另外他本身也基于webkit来显示一些邮件，这两个之间的setting有很大可能不一样，他们就使用不同的PageGroup。
PageGroup中有这个Group已经安装并且使用的User
 Script和User StyleSheet的集合，一般在网页解析完毕后，这些User
 Script和User StyleSheet会插入到Document中。
PageGroup中还维护了Local Storage和Index
 DB相关的设置，比如它们的Path，上限等，通过GroupSettings类实现。
PageGroup创建以后，每次创建一个新的Page对象，会通过addPage接口加入到这个PageGroup的m_pages中。
每次有导航行为发生的时候，会调用addVisitedLink来将url加入到已访问链接中。如果浏览器要跟踪已访问的接口，则在初始化的时候必须调用PageGroup::setShouldTrackVisitedLinks，且参数为true。此处shouldTrackVisitedLinks是一个静态的全局变量，也就是说，所有应用维护一样的行为（一个应用将其设置为false会影响到其它同样基于此核的应用）？
Page类中维护了PageGroup的指针，并提供了group接口，这是个lazy接口，如果m_group不存在，会调用InitGroup来创建一个。对于Page类来说，如果没有设置GroupName，则在初始化的时候会生成一个空GroupName的PageGroup，由m_singlePageGroup维护，并把指针赋给m_group，如果以非空的名字调用了setGroupName，则会重新创建PageGroup，此时这个PageGroup由m_group来维护。
###2.2  Setting
WebCore中的设置相关的类，浏览器应用的不少配置、选项同该类相关，Qt移植中，应用在创建Page对象后，会根据Page::settings来实例化QwebSetting。
###2.3  Chrome
原生窗口接口类，参考地瓜写的”WebKit中的Chrome和ChromeClient”。
###2.4  其它
SelectionController :负责管理Page中的选取操作，绝大部分选取操作是基于Frame的，只在Frame的Selection为空的时候，对焦点游标的绘制工作才会使用到Page类的SelectionController。
SharedGraphicsContext3D:共享3D图形上下文，为了优化2D显示而加入。在加速的2D
 canvas中，引入的DrawingBuffer的概念，SharedGraphicsContext3D提供了createDrawingBuffer来创建DrawingBuffer。
DragController:拖拽控制器。Chrome的超级拖拽功能同这个有关？地瓜会在以后对此进行求证。
FocusController:焦点控制器。考虑到焦点会在各个frame之间切换，所以由Page类维护焦点控制器最合适不过。
ContextMenuController:右键下拉菜单控制器。
InspectorController:Inspector控制器，浏览器中的很多开发工具都同这个类相关。
GeolocationController:定位定位服务控制器。
DeviceMotionController:设备移动控制器
DeviceOrientationController:设备方向控制器
SpeechInputClient:语音输入Client。
ProgressTracker:进度跟踪。
BackForwardController:前进后退操作控制。
Frame:一个Page由至少一个主帧和若干个其它子帧构成。
HistoryItem:历史记录。
PluginData:插件相关，未来可能同PluginDatabase类合并。主要是初始化Plugin的信息。
PluginHalter:用来控制Plugin的停止和重新开始。
RenderTheme:这个类提供了控件的渲染和绘制接口。Qt移植中，RenderThemeQt是RenderTheme接口的具体实现。
EditorClient:同编辑功能相关，比如拷贝、剪切、删除等操作。
