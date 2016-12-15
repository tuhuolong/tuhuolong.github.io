---
layout: post
title: "WebKit 内核源码分析  (二)    FrameLoader"
date: 2012-04-28 00:22:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "WebKit 内核源码分析  (二)    FrameLoader"
keywords: webkit
---


 
  FrameLoader类负责一个Frame的加载，在Frame的流程中起到非常重要的重要，同很多组件都有交互，本文将分析FrameLoader类的代码。
 
 
  1. 概述
 
 
  顾名思义，FrameLoader是一个Frame的loader，它的作用就是为客户提供一个下载一个Frame的一系列接口。这里的客户指的是类的客户，比如Frame类,间接客户是上层应用，比如qwebframe。
  
   从它的定义看，最容易想到的是一个load接口，用来将一个frame load下来。任何一个页面至少都需要一个mainframe，因此一个页面的下载一般就是从load一个mainframe开始。
   
    在load frame的过程中，通过FrameLoaderClient接口将load过程的不同阶段告知客户。
    
     FrameLoader通过setDocumentLoader相当于把load的工作委托给了DocumentLoader类。
     
      FrameLoader同DocumentLoader是has-a的关系。一般在load的时候创建DocumentLoader。Frame调用DocumentLoader的startLoadingMainResource开始load frame。
     
    
   
  
 
 
  2. 类关系
 
 
  
   
    
     1）Frame和FrameLoader是contain-a的关系，在Frame的构造函数中调用FrameLoader的构造函数，调用时传入参数Frame指针和FrameLoaderClient指针。
     
      2）Frame有可能有子Frame，所以维护SubFrameLoader对象m_subframeLoader来管理子Frame的load。Frame可以对应xml document，也可对应html document，等等。跟Document相关的子resource的load不在FrameLoader的职责范围内。
      
       3）包含一个DocumentWriter类对象m_writer，当Frame的数据load finish的时候，将数据传给DocumentWriter类，进行下一步的处理（比如解码）
       
        4）FrameLoader维护了三个DocumentLoader对象，分别对应于不同的阶段，m_policyDocumentLoader对应于收到用户load调用，进行policy check阶段，m_provisionalDocumentLoader对应于policy check通过以后，Frame数据还没有到来之前，它会负责startLoadingMainResource的调用。m_documentLoader则是Frame第一个数据到来以后使用的DocumentLoader，这个时候，前一个主Frame的DocumentLoader已经不能再用(user
 agent开始白屏，刷掉前一个页面的显示)。
        
         5）包含一个HistoryController对象，用于操作历史记录相关的接口，保存或者恢复Document和View相关的状态，维护前进后退队列，以实现前进后退功能，前进后退本质上是同Page对象关联的，FrameLoader通过HistoryController操作m_backFowardController对象
         
          6）包含一个ResourceLoadNotifier对象，主要用于同ResourceLoader及FrameLoaderClient打交道，可以理解为ResourceLoader有事件变化或者发生的时候，通知FrameLoader的一个手段
          
           7）包含一个SubframeLoader对象，当FrameLoader下载的Document有子帧需要请求的时候（比如HTMLDocument中解析到iframe 元素），用来处理子帧请求
           
            8）将FrameLoader的状态封装到FrameLoaderStateMachine中，这个状态同FrameState不同，FrameState倾向于判断Frame涉及的Document的下载状态，是出于发起状态（Provisional），还是出于已经收到响应但不全（CommittedPage）,还是响应收全的状态，倾向于同http相关。而FramLoaderStateMachine倾向于同DocumentLoader相关，用来描述FrameLoader处理DocumentLoader的节点，是处于已经创建，还是显示的状态。
            
             9）PolicyChecker主要用来对FrameLoader进行一些校验。包括三种校验：NewWindow,Navigation和Content。NewWindow对应于浏览器需要新开一个tab页或窗口的时候，Navigation对应于一个页面请求发起的时候，Content校验对应于收到数据以后（判断Mime type等），PolicyChecker通过提供对应的接口，由FrameLoaderClient来对这些请求进行校验，以确定是否允许继续，或者需要其它的动作。
            
           
          
         
        
       
      
     
    
   
  
 
 
  3.   主要接口
 
 
  Frame::init
 
 
  功能：FrameLoader的初始化
 
 
  函数调用系列
  
   QWebFrame::QWebFrame(QwebPage* parent,QWebFrameData *frameData)
   
    QWebFramePrivate::init(QWebFrame* qwebframe,QWebFrameData* frameData)
    
     Frame::init()
     
      FrameLoader::init()
     
    
   
  
 
 
  说明：主要做一些自身的初始化工作，比如初始化状态机，Sandbox Flags，创建DocumentLoader被设置为Policy DocumentLoader和Provisional DocumentLoader，调用DocumentLoader和documentWriter等的接口进行初始化操作
 
 
  FrameLoader::commitProvisionalLoad
 
 
  功能：提交Provisional阶段下载的数据
 
 
  函数调用系列：
  
   DocumentLoader::finishLoading
   
    DocumentLoader::commitIfReady
    
     FrameLoader::commitProvisionalLoad
     
      
       或者
       
        ResourceLoader::didReceiveData
        
         MainResourceLoader::addData
         
          DocumentLoader::receiveData
          
           DocumentLoader::commitLoad
           
            DocumentLoader::commitIfReady
            
             DocumentLoader::commitProvisionalLoad
            
           
          
         
        
       
      
     
    
   
  
 
 
  说明：这个接口主要的操作是将Provisional DocumentLoader设置成DocumentLoader，因为已经收到数据，所以FrameState也会跃迁到FrameStateCommittedPage。还有历史记录，PageCache相关的操作。另外，这个接口会间接调用FrameLoaderClientQt::transitionToCommittedForNewPage，通过Frame::createView创建出FrameView来。
 
 
  Frame::finishedLoading
 
 
  功能：frame请求网络加载完成的时候调用此接口
 
 
  函数调用系列
  
   ResourceLoader::didFinishLoading
   
    MainResourceLoader::didFinishLoading
    
     FrameLoader::finishedLoading
     
      FrameLoader::init()
     
    
   
  
 
 
  说明：检查是否有网络错误，告诉DocumentLoader和DocumentWriter下载完成，以便进行后续操作（提交数据，解析）。
 
 
  FrameLoader::finishedParsing
 
 
  功能：解析完成调用此接口
 
 
  函数调用系列
  
   DocumentWritter::end
   
    ….
    
     Document::finishParsing
     
      ….
      
       Document::finishedParsing
       
        FrameLoader::finishedParsing
       
      
     
    
   
  
 
 
  FrameLoader::load(const ResourceRequest& request,bool lockHistory)
 
 
  功能：加载一个frame请求，Frame请求相关的数据，封装成ResourceRequest传入。
 
 
  函数调用系列：一般由应用触发调用
 
 
  说明：这个接口调用FrameLoaderClientQt::createDocumentLoader创建出DocumentLoader，并以此DocumentLoader为Policy Document Loader，进入Policy check流程。
 


