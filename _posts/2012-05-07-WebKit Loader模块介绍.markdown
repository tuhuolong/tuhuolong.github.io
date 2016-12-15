---
layout: post
title: "WebKit Loader模块介绍"
date: 2012-05-07 14:57:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "WebKit Loader模块介绍"
keywords: webkit
---


 
  
   
    一．下面重点介绍一下与
   
   Loader
   
    相关的数据结构和模块。
   
  
 
 
  
  
 
 
  
   Frame
  
  
   ：可以看做是浏览器外壳调用
  
  Loader
  
   的总入口，它就像我们印象中的一个网页，它关注的是页面的显示
  
  (FrameView)
  
   、页面数据的加载
  
  (FrameLoader)
  
   、页面内的各种控制器
  
  (Editor,
 EventHandler, ScriptController, etc.)
  
   等等，它包含以下模块（只列出重点）：
  
 
 
  Document
 
 
  Page
 
 
  FrameView
 
 
  RenderView
 
 
  FrameLoader
 
 
  DOMWindow
 
 
 
 
  
   下面分别介绍
  
  (PS:
  
   必须要了解这些概念，不然后面的东东都无法理解
  
  ):
 
 
 
 
  1)
  
   Document
  
  :
  
   这个类的爷爷类是
  
  Node
  
   ，它是
  
  DOM
  
   树各元素的基类；
  
  Document
  
   有个子类是
  
  HTMLDocument
  
   ，它是整个文档
  
  DOM
  
   树的根结点，这样就明白了：原来
  
  Document
  
   就是描述具体文档的代码，看一下它的头文件，就更明白了，它的属性与方法就是围绕着各种各样的结点：
  
  Text
  
   ，
  
  Comment
  
   ，
  
  CDATASection
  
   ，
  
  Element
  
   ……
  
 
 
 
 
  2)
  
   Page
  
  :
  
   我的理解是，
  
  Page
  
   与
  
  Frame(
  
   严格说是
  
  FrameView)
  
   是一一对应的，
  
  Frame
  
   关注
  
  UI
  
   ，
  
  Page
  
   关注数据。现在的浏览器一般都提供同时打开多个窗口，每一个窗口对应的数据就是这个
  
  Page
  
   在管理了。
  
 
 
  
   在
  
  page.cpp
  
   文件里，还有个重要的全局指针变量：
  
  static HashSet<Page*>;* allPages;
  
   这个变量包含了所有的
  
  page
  
   实例。
  
 
 
 
 
  3)
  
   FrameView
  
  :
  
   可以理解为为一个网页的
  
  ViewPort,
  
   它提供一个显示区域，同时包含的有
  
  Render
  
   根节点、
  
  layout
  
   排版相关接口、
  
  Scroll
  
   相关等。
  
  FrameView
  
   是
  
  Layout
  
   排版的总入口。
  
 
 
 
 
  4)
  
   RenderView
  
  :
  
   与
  
  FrameView
  
   差不多，只是分工不同，它管理与
  
  Render
  
   树相关的东东。
  
 
 
 
 
  5)
  
   FrameLoader
  
  
   ：重点，
  
  FrameLoader
  
   类将
  
  Documents
  
   加载到
  
  Frames
  
   。当点击一个链接时，
  
  FrameLoader
  
   创建一个新的处于“
  
  policy
  
   ”状态的
  
  DocumentLoader
  
   对象，一旦
  
  webkit
  
   指示
  
  FrameLoader
  
   将本次加载视为一个导航
  
  (navigation)
  
   ，
  
  FrameLoader
  
   就推动
  
  DocumentLoader
  
   进入“
  
  provisional
  
   ”状态，（在该状态，
  
  DocumentLoader
  
   发调用
  
  CURL
  
   发起一个网络请求，并等待是
  
  html
  
   还是下载文件。）同时
  
  ,DocumentLoader
  
   会创建一个
  
  MainResourceLoader
  
   对象（该对象在后面单独介绍）。
  
 
 
  6
  
   ）。
  
  
   DOMWindow:
  
  
   实现了
  
  Dom
  
   的一些接口，如
  
  CreateNode
  
   等。后面可以详细讲讲。
  
 
 
 
 
  
   上面介绍的概念比较多，我也不晓得好不好理解，没理解也不怕，多去看看代码，这是必须的。
  
 
 
 
 
  另外看看下面的图，就会清晰很多的
 
 
  
   
    
    
   
  
 
 
 
 
  
   
    二、
   
   Webkit
   
    的
   
   Loader
   
    有两条加载数据的主线：
   
   (
   
    从上图可以看到
   
   )
  
 
 
  1. MainResourceLoader
  
   ：
  
  
   该模块主要加载主网页请求。后面称为
  
  MainResource
  
   。
  
 
 
  2. DocLoader
  
   ：
  
  
   该模块除了主网页外的所有子请求，如
  
  :.js
  
   文件，图片资源，
  
  .css
  
   文件。
  
  
   后面称为
  
  SubResource
  
   。
  
 
 
 
 
  MainResource
  
   部分：
  
 
 
  FrameLoader->;DocumentLoader->;MainResourceLoader-ResourceHandleDocumentLoader
  
   经历状态：
  
  1)"policy" 2) "provisional" 3)"commited"
  
   分别是等待、作为
  
  navigation
  
   发送
  
  network
 request
  
   、文件下载完毕
  
 
 
 
 
  Subresource
  
   部分：
  
 
 
  DocLoader->;Cache->;[CacheObjects]example: CacheImage->;SubresourceLoader->;ResourceHandle
  
   当请求一个资源时，首先查看
  
  Cache
  
   中是否存在该对象，如果存在直接返回；如果不存在，创建该
  
  Cache
  
   对象（如
  
  CacheImage
  
   ），然后创建一个
  
  SubresourceLoader
  
   ，加载资源。
  
 
 
  
   举例说明：
  
 
 
  
   加载图片时，
  
  DocLoader
  
   首先询问
  
  Cache,
  
   在内存中是否也存在（
  
  CachedImage
  
   对象），如果已存在，则直接加载，即省了时间又省了流量。如果图片不在
  
  Cache
  
   中，
  
  Cache
  
   首先创建一个新的
  
  CachedImage
  
   对象来代表该图片，然后由
  
  CachedImage
  
   对象调用
  
  Loader
  
   对象发起一个网络请求，
  
  Loader
  
   对象创建
  
  SubResourceLoader
  
   。后面的流程就一样了，
  
  SubResourceLoader
  
   也是直接把
  
  ResourceHandle
  
   打交道的。
  
  
  
 
 
  
  
 
 
  
   
    接下来跟踪一下
   
   Loader
   
    发送请求的代码实现：
   
  
 
 
  1.
  
   用户输入
  
  URL
  
   后，最先调用的接口是：
  
 
 
  FrameLoader::load(constResourceRequest& request)
 
 
 
 
  ResourceRequest
  
   包含了：
  
 
 
  KURL(
  
   处理
  
  url
  
   的一个类
  
  )
  
   、
  
  setHTTPHeaderField
  
   、
  
  setHTTPContentType
  
   等与
  
  HTTP
  
   头部相关的函数
  
 
 
 
 
  2.Load()
  
   通过
  
  ResourceRequest
  
   数据调用
  
  createDocumentLoader(request, substituteData)
  
   来创建一个
  
  DocumentLoader
  
   。
  
 
 
 
 
  3.Load()
  
   函数继续给
  
  request
  
   设置
  
  HttpAccept,Cache-Control HTTP
  
   头等信息。
  
 
 
 
 
  4.
  
   设置
  
  FrameLoader::checkNavigationPolicy
  
   函数进入
  
  "Policy"
  
   状态。
  
 
 
 
 
  5.
  
   判断该
  
  url
  
   是否在
  
  Cache
  
   中等一系列状态判断后，进入
  
  DocumentLoader::startLoadingMainResource
  
   函数准备加载
  
  MainResource
  
   。该函数首先会创建调用
  
  MainResourceLoader
  
   。
  
 
 
 
 
  6.
  
   进入
  
  MainResourceLoader::load
  
   函数，调用
  
  illSendRequest(r,ResourceResponse());
  
   做发送请求前的准备。
  
 
 
 
 
  7.
  
   调用
  
  PolicyCheck
  
   检查
  
  policy
  
   的状态后，进入
  
  FrameLoader::callContinueLoadAfterNavigationPolicy
  
   继续往下走。
  
 
 
 
 
  8:
  
   在
  
  MainResourceLoader::loadNow(ResourceRequest&r)
  
   函数里创建
  
  ResourceHandle,
  
   在创建
  
  ResourceHandle
  
   函数中，调用
  
  start
  
   函数，
  
  start
  
   函数把
  
  ResourceHandle
  
   自已添加到
  
  ResourceHandleManager
  
   的
  
  m_resourceHandleList
  
   队列里。
  
 
 
  
   同时，调用
  
  m_downloadTimer.startOneShot
  
   激活网页请求下载的定时器。
  
  (
  
   这是个毫秒级的定时器，采用定时器的原因也是为了实现异步的请法
  
  )
 
 
 
 
  
   可以看到
  
  m_downloadTimer
  
   的定义：
  
  Timer<ResourceHandleManager>; m_downloadTimer;
 
 
  m_downloadTimer
  
   是实现的一个定时器模块类，在它的构造函数里已经传入了回调函数的地址：
  
  ResourceHandleManager::downloadTimerCallback
  
   。
  
 
 
 
 
  9.
  
   一路返回到
  
  Load()
  
   函数，并返回到调用源，函数执行完毕。
  
 
 
 
 
  10.  ResourceHandleManager::downloadTimerCallback
  
   回调函数被定时器调用。
  
 
 
 
 
  11.
  
   可以看到
  
  downloadTimerCallback
  
   函数的代码：
  
 
 
  
   调用
  
  libcurl
  
   库的接口
  
  curl_multi_fdset,curl_multi_perform
  
   等查询数据。
  
 


