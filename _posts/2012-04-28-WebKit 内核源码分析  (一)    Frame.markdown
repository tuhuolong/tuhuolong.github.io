---
layout: post
title: "WebKit 内核源码分析  (一)    Frame"
date: 2012-04-28 00:21:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "WebKit 内核源码分析  (一)    Frame"
keywords: webkit
---


 
  在分析内核的时候，Frame是首当其冲的一个类，本文将分析Frame类的代码。
 
 
  1.    描述
 
 
  Frame类是WebCore内核同应用之间联系的一个重要的类。它有点像设计模式中的Façade，将内核的各个不同的零配件组装在了一起，但又不是Façade，因为用户很多时候还是要直接去操作里面的组件。除了设计上的考虑，Frame还有语法上的意义，它对应于Page里面的帧。
 
 
  2.    类结构
 
 
  
 
 
 
 
 
 
  1）              FrameTree对象用来协助管理父帧和子帧的关系，常见的比如main frame之中有iframe元素，就会调用FrameLoaderClientQt::createFrame来产生子帧，产生的子帧会通过appendChild添加到主帧的树状结构中。Frame通过FrameTree对象，可以方便地访问它的父帧，子帧，兄弟帧。
 
 
  2）              维护FrameLoader对象用来完成frame的加载，FrameLoader是一个非常重要的类，后续进行进一步的分析。
 
 
  3）              维护NavigationScheduler对象用来管理页面跳转调度（比如重定向，meta refresh等）。
 
 
  4）              DOMWindow用来管理同DOM相关的事件、属性和消息。
 
 
  5）              FrameViwe类用于Frame的排版。
 
 
  6）              Frame文档解析后，对每一个tag或者attr，会有对应的dom节点关联，Document类用来管理这些dom节点。不同的文档类型继承出不同的子类，比如HTML文档对应子类HTMLDocument,XML文档对应于XMLDocument。
 
 
  7）              SciptController对象，脚本控制器，用来管理脚本的执行和操作。
 
 
  8）              Editor对象用来处理页面的编辑相关的操作，比如拷贝，粘贴，输入等，Editor对象，它同Page类的EditorClient对象紧密合作。和EditorClient的关系就如同Page同Frame的关系。
 
 
  9）              SelectionController用来管理Frame中的选取操作。
 
 
  10）         AnimationControlle,动画控制，控制动画的播放，暂停，继续（同HTML video标签是否有关系？）
 
 
  11）         EventHandler，事件处理对象，这里的对象主要是同上层应用也就是用户参与的事件，比如鼠标事件，按键事件（快捷键等），滚动事件，resize事件等。这是一个浏览器外壳经常需要打交道的类。
 
 
  3.    主要接口
 
 
  3.1   Create
 
 
  static PassRefPtr<Frame>; create(Page*,HTMLFrameOwnerElement*,FrameLoaderClient*)
  
   描述： 调用Frame构造函数，创建出Frame对象。有两个地方会创建Frame对象，一是要加载一个新的页面请求，这个时候会创建main frame，一是在加载子帧的时候，通过FrameLoaderClientQt的createFrame接口，创建子帧对应的Frame对象，在第一种情况中，HTMLFrameOwnerElement参数为NULL，第二种情况传子帧的父元素。在一个tab页内，main frame会重用。
  
 
 
  调用系列：
 
 
  àQwebPage::setView
 
 
  àQwebPage::setViewportSize
 
 
  àQwebPage::mainFrame
 
 
  àQwebPagePrivate::createMainFrame
 
 
  àQwebFrameData::QwebFrameData
 
 
  àFrame::create
 
 
  
   àFrameLoader::finishedLoading
   
    à……
   
  
 
 
  àHTMLDocumentParser::append
 
 
  à……
 
 
  àHTMLTreeBuilder::processToken
 
 
  à……
 
 
  àHTMLElementBase::openURL
 
 
  àSubFrameLoader::requestFrame
 
 
  à……
 
 
  àFrameLoaderClientQt::creatFrame
 
 
  àQwebFrameData::QwebFrameData
 
 
  àFrame::create
 
 
  3.2  createView
 
 
  void createView(const IntSize&, const Color&, bool, const IntSize&, bool,
 
 
  ScrollbarMode = ScrollbarAuto, bool horizontalLock = false,
 
 
  ScrollbarMode = ScrollbarAuto, bool verticalLock = false)
 
 
  描述：创建出FrameView对象，以用于之后的排版。应用调用这个函数的时候需要传入同排版有关的一些信息，如初始视窗大小，背景色，滚动条模式等。创建出FrameView以后，即调用Frame::setView设置成当前的FrameView。
  
   函数调用系列：
   
    àFrameLoader::commitProvisionalLoad
   
  
 
 
  àFrameLoader::transitionToCommitted
 
 
  àFrameLoaderClientQt::transitionToCommittedForNewPage
 
 
  àFrame::createView
 
 
  3.3  setDocument
 
 
  void setDocument(PassRefPtr<Document>;)
 
 
  描述：设置同Frame关联的Document对象(一般是DocumentWriter创建的)。
 
 
  函数调用系列：
 
 
  àQWebFrame::QwebFrame
 
 
  àQwebFramePrivate::init
 
 
  àFrame::init
 
 
  àFrameLoader::init
 
 
  àDocumentWriter：：begin
 
 
  àFrame::setDocument
 
 
  
   àDocumentLoader::receivedData
   
    àDocumentLoader::commitLoad
   
  
 
 
  àFrameLoaderClientQt::committedLoad
 
 
  àDocumentLoader::commitData
 
 
  àDocumentWriter::setEncoding
 
 
  àDocumentWriter::willSetEncoding
 
 
  àFrameLoader::receivedFirstData
 
 
  àDocumentWriter::begin
 
 
  àFrameLoader::clear
 
 
  àFrame::setDocument
 
 
  3.4  init
 
 
  void Frame::init
 
 
  描述：Frame对象初始化，会调用FrameLoader::init初始化FrameLoader对象。
 
 
  调用系列：
  
   àQWebFrame::QWebFrame
  
 
 
  àQwebFramePrivate::init
 
 
  àFrame::init
 
 
  3.5  setPageAndTextZoomFactors
 
 
  void setPageAndTextZoomFactors(float pageZoomFactor, float textZoomFactor)
 
 
  描述：设置页面放大因子和文字放大因子。在网页缩放或者改变网页字体大小的时候调用。
 


