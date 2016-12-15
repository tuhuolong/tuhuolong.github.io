---
layout: post
title: "WebKit DOM Event (一)"
date: 2012-06-04 14:25:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "WebKit DOM Event (一)"
keywords: webkit
---


 
  
   DOM Event 规范
  
  
   DOM Event 主要定义了三类接口：
  
  
   
    EventTarget
   
   , 所有DOM节点和XMLHttpRequest 都实现EventTarget接口
  
  
   
    
    
   
   
    
     
     
     
     
     class EventTarget  {
    
   
   
    
     
     
     
     
     void addEventListener(in DOMString type,inEventListener listener, in boolean useCapture);
    
   
   
    
     
     
     
     
     void removeEventListener(in DOMString type,in EventListener listener, in boolean useCapture);
    
   
   
    
     
     
     
     
     boolean dispatchEvent(in Event event)
    
   
   
    
    
    
    
    };
   
  
  
   
   
   
    
     这个类是一个纯虚基类，因此是一个接口类
    
   
   
    
     1.添加和删除Listener，这些是虚函数，子类中可以重新实现
    
   
   
    
     2.分发Event，有一个虚函数，和一个函数，这样子类可以有自己的个性
    
   
   
    
     3.添加，删除和获取属性Listener
    
   
   
    
     4.fire触发Listener函数，这个函数不需要有子类的实现
    
   
   
   
  
  
   
    EventListener
   
   
    , 用户实现该接口，并且在一个EventTarget上注册，使用addEventListener
 完成注册；
   
  
  
   
   
   
    
     
      void handleEvent(in Event evt);
     
    
   
   
    
     
      EventListener::Type {
      
       JSEventListenerType,
       
        ImageEventListenerType,
        
         InspectorDOMAgentType,
         
          InspectorDOMStorageResourceType,
          
           ObjCEventListenerType,
           
            CPPEventListenerType,
            
             ConditionEventListenerType,
             
              GObjectEventListenerType,
              
               NativeEventListenerType
               
                };
               
              
             
            
           
          
         
        
       
      
     
     
     
    
   
   
    
     Event
    
    
     给EventHandle提供上下文信息
    
   
   
   
   
    
     
      readonly attribute DOMString        type;
     
    
   
   
    
     
      readonly attribute EventTarget      target;
     
    
   
   
    
     
      readonly attribute EventTarget      currentTarget;
     
    
   
   
    
     
      readonly attribute unsigned short   eventPhase;
     
    
   
   
    
     
      readonly attribute boolean          bubbles;
     
    
   
   
    
     
      readonly attribute boolean          cancelable;
     
    
   
   
    
     
      readonly attribute DOMTimeStamp     timeStamp;
     
    
   
   
    
     
     
    
   
   
    
     
      void               stopPropagation();
     
    
   
   
    
     
      void               preventDefault();
     
    
   
   
    
     
      [OldStyleObjC] void initEvent(in DOMStringeventTypeArg,
     
    
   
   
    
     
      in boolean canBubbleArg,
     
    
   
   
    
     
      in boolean cancelableArg);
     
    
   
   
    
     
     
    
   
   
    
     
      // DOM Level 3 Additions.
     
    
   
   
    
     
      readonly attribute boolean defaultPrevented;
     
    
   
   
    
     
      void stopImmediatePropagation();
     
    
   
   
    
     
     
    
   
   
    
     
      // IE Extensions
     
    
   
   
    
     
      readonly attribute EventTarget      srcElement;
     
    
   
   
    
     
      attribute boolean          returnValue;
     
    
   
   
    
     
      attribute boolean          cancelBubble;
     
    
   
   
    
     
     
    
   
   
    
     
      #if defined(LANGUAGE_JAVASCRIPT) &&LANGUAGE_JAVASCRIPT
     
    
   
   
    
     
      readonly attribute [Custom] Clipboard        clipboardData;
     
    
   
   
    
     
      #endif
     
    
   
   
   
  
  
   
   
  
  
   事件流
  
  
   
    基本事件流
    
     每个事件都对应一个事件目标（EventTarget）（也是一个node 节点），EventTarget 有event 的target 属性指定。 每个事件目标注册有若干事件监听者(EventListerner), 这些监听者在事件到达后激活，激活的顺序在DOM规范中没有定义。如果没有定义事件capture或者bubbling，则当事件目标上的所有事件监听者响应完毕后，则这个事件处理完毕。
    
   
   
    事件捕获
    
     事件捕获发生在如下情况下： 一个事件监听者注册在某个事件的目标节点的祖先节点上，当该事件发生后，在其到达目标节点之前，祖先节点的事件监听者首先捕获并处理，然后将事件逐级下传，直到目标节点。
    
   
   
    事件冒泡
    
     事件冒泡初始阶段同基本事件流相同，然而在事件的目标节点所有事件监听者响应完毕后，是将将会沿着节点向其祖先方向上传，直到document点，上传过程中将会逐级激发在遇到节点上注册的的所有事件监听者（捕获事件除外）。
    
   
   
    事件取消
    
     一些事件可以规定为可取消的，这些事件一般都会应有一个缺省的动作。当此类事件发生时，首先传递给目标节点上的事件监听者，事件监听者可以选择是否取消该事件的缺省动作。
    
   
  
  
   下面两图为事件流图：
  
  
   
    
   
  
  
   
    
   
  
  
   WebKit DOM Event 类图
  
  
   
    
   
  
  
   WebKit 实现逻辑
  
  
   实现逻辑（以鼠标事件为例）:
  
  
   
    鼠标事件发生
   
   
    根据鼠标事件发生的位置， 找到对应的EventTarget 节点
   
   
    在EventTarget的 dispatchGenericEvent函数中，获取到所有的父节点，保存到list中；
   
   
    进入事件捕获阶段
   
   
    触发当前EventTarget的当前事件的EventListen
   
   
    进入事件冒泡阶段
   
  
  
   
   
   
    
     bool EventTargetNode::dispatchGenericEvent(PassRefPtr<Event>;e, ExceptionCode&, bool tempEvent)
    
   
   
    
     {
    
   
   
    
     RefPtr<Event>;evt(e);
    
   
   
    
     ASSERT(!eventDispatchForbidden());
    
   
   
    
     ASSERT(evt->;target());
    
   
   
    
     ASSERT(!evt->;type().isNull()); // JavaScript code could create anevent with an empty name
    
   
   
    
     //work out what nodes to send event to
    
   
   
    
     DeprecatedPtrList<Node>; nodeChain;
    
   
   
    
    
   
   
    
     if(inDocument()) {
    
   
   
    
     for (Node* n = this; n; n = n->;eventParentNode()) {
    
   
   
    
     n->;ref();
    
   
   
    
     nodeChain.prepend(n);
    
   
   
    
     }
    
   
   
    
     }else {
    
   
   
    
     // if node is not in the document just send event to itself
    
   
   
    
     ref();
    
   
   
    
     nodeChain.prepend(this);
    
   
   
    
     }
    
   
   
    
    
   
   
    
     DeprecatedPtrListIterator<Node>; it(nodeChain);
    
   
   
    
     //Before we begin dispatching events, give the target node a chance to do somework prior
    
   
   
    
     //to the DOM event handlers getting a crack.
    
   
   
    
     void*data = preDispatchEventHandler(evt.get());
    
   
   
    
     //trigger any capturing event handlers on our way down
    
   
   
    
     evt->;setEventPhase(Event::CAPTURING_PHASE);
    
   
   
    
     it.toFirst();
    
   
   
    
     //Handle window events for capture phase, except load events, this quirk isneeded
    
   
   
    
     //because Mozilla used to never propagate load events to the window object
    
   
   
    
     if(evt->;type() != loadEvent && it.current()->;isDocumentNode() &&!evt->;propagationStopped())
    
   
   
    
     static_cast<Document*>;(it.current())->;handleWindowEvent(evt.get(),true);
    
   
   
    
     for(; it.current() && it.current() != this && !evt->;propagationStopped();++it) {
    
   
   
    
     evt->;setCurrentTarget(EventTargetNodeCast(it.current()));
    
   
   
    
     EventTargetNodeCast(it.current())->;handleLocalEvents(evt.get(), true);
    
   
   
    
     }
    
   
   
    
    
   
   
    
     //dispatch to the actual target node
    
   
   
    
     it.toLast();
    
   
   
    
     if(!evt->;propagationStopped()) {
    
   
   
    
     evt->;setEventPhase(Event::AT_TARGET);
    
   
   
    
     evt->;setCurrentTarget(EventTargetNodeCast(it.current()));
    
   
   
    
     // We do want capturing event listeners to be invoked here, even though
    
   
   
    
     // that violates the specification since Mozilla does it.
    
   
   
    
     EventTargetNodeCast(it.current())->;handleLocalEvents(evt.get(), true);
    
   
   
    
     EventTargetNodeCast(it.current())->;handleLocalEvents(evt.get(), false);
    
   
   
    
     }
    
   
   
    
     --it;
    
   
   
    
     //ok, now bubble up again (only non-capturing event handlers will be called)
    
   
   
    
     //### recalculate the node chain here? (e.g. if target node moved in document byprevious event handlers)
    
   
   
    
     //no. the DOM specs says:
    
   
   
    
     //The chain of EventTargets from the event target to the top of the tree
    
   
   
    
     //is determined before the initial dispatch of the event.
    
   
   
    
     //If modifications occur to the tree during event processing,
    
   
   
    
     //event flow will proceed based on the initial state of the tree.
    
   
   
    
     //
    
   
   
    
     //since the initial dispatch is before the capturing phase,
    
   
   
    
     //there's no need to recalculate the node chain.
    
   
   
    
     //(tobias)
    
   
   
    
     if(evt->;bubbles()) {
    
   
   
    
     evt->;setEventPhase(Event::BUBBLING_PHASE);
    
   
   
    
     for (; it.current() && !evt->;propagationStopped() && !evt->;cancelBubble();--it) {
    
   
   
    
     evt->;setCurrentTarget(EventTargetNodeCast(it.current()));
    
   
   
    
     EventTargetNodeCast(it.current())->;handleLocalEvents(evt.get(), false);
    
   
   
    
     }
    
   
   
    
     // Handle window events for bubbling phase, except load events, thisquirk is needed
    
   
   
    
     // because Mozilla used to never propagate load events at all
    
   
   
    
     it.toFirst();
    
   
   
    
     if (evt->;type() != loadEvent && it.current()->;isDocumentNode()&& !evt->;propagationStopped() && !evt->;cancelBubble()) {
    
   
   
    
     evt->;setCurrentTarget(EventTargetNodeCast(it.current()));
    
   
   
    
     static_cast<Document*>;(it.current())->;handleWindowEvent(evt.get(),false);
    
   
   
    
     }
    
   
   
    
     }
    
   
   
    
     evt->;setCurrentTarget(0);
    
   
   
    
     evt->;setEventPhase(0); // I guess this is correct, the spec does notseem to say
    
   
   
    
     // anything aboutthe default event handler phase.
    
   
   
    
     //Now call the post dispatch.
    
   
   
    
     postDispatchEventHandler(evt.get(), data);
    
   
   
    
     //now we call all default event handlers (this is not part of DOM - it isinternal to khtml)
    
   
   
    
     it.toLast();
    
   
   
    
     if(evt->;bubbles())
    
   
   
    
     for (; it.current() && !evt->;defaultPrevented() && !evt->;defaultHandled();--it)
    
   
   
    
     EventTargetNodeCast(it.current())->;defaultEventHandler(evt.get());
    
   
   
    
     elseif (!evt->;defaultPrevented() && !evt->;defaultHandled())
    
   
   
    
     EventTargetNodeCast(it.current())->;defaultEventHandler(evt.get());
    
   
   
    
     //deref all nodes in chain
    
   
   
    
     it.toFirst();
    
   
   
    
     for(; it.current(); ++it)
    
   
   
    
     it.current()->;deref(); // this may delete us
    
   
   
    
     Document::updateDocumentsRendering();
    
   
   
    
     //If tempEvent is true, this means that the DOM implementation
    
   
   
    
     //will not be storing a reference to the event, i.e.  there is no
    
   
   
    
     //way to retrieve it from javascript if a script does not already
    
   
   
    
     //have a reference to it in a variable.  Sothere is no need for
    
   
   
    
     //the interpreter to keep the event in it's cache
    
   
   
    
     Frame *frame = document()->;frame();
    
   
   
    
     if(tempEvent && frame && frame->;scriptProxy())
    
   
   
    
     frame->;scriptProxy()->;finishedWithEvent(evt.get());
    
   
   
    
     return!evt->;defaultPrevented(); // ### what if defaultPrevented was called beforedispatchEvent?
    
   
   
    }
   
  
  
   
    
    
   
  
  
   
    Event::Type {
   
  
  
   
   
   
    abort
   
   
    beforecopy
   
   
    beforecut
   
   
    beforeload
   
   
    beforepaste
   
   
    beforeprocess
   
   
    beforeunload
   
   
    blocked
   
   
    blur
   
   
    cached
   
   
    change
   
   
    checking
   
   
    click
   
   
    close
   
   
    complete
   
   
    compositionend
   
   
    compositionstart
   
   
    compositionupdate
   
   
    connect
   
   
    contextmenu
   
   
    copy
   
   
    cut
   
   
    dblclick
   
   
    devicemotion
   
   
    deviceorientation
   
   
    display
   
   
    downloading
   
   
    drag
   
   
    dragend
   
   
    dragenter
   
   
    dragleave
   
   
    dragover
   
   
    dragstart
   
   
    drop
   
   
    error
   
   
    focus
   
   
    focusin
   
   
    focusout
   
   
    hashchange
   
   
    input
   
   
    invalid
   
   
    keydown
   
   
    keypress
   
   
    keyup
   
   
    load
   
   
    loadstart
   
   
    message
   
   
    mousedown
   
   
    mousemove
   
   
    mouseout
   
   
    mouseover
   
   
    mouseup
   
   
    mousewheel
   
   
    noupdate
   
   
    obsolete
   
   
    offline
   
   
    online
   
   
    open
   
   
    overflowchanged
   
   
    pagehide
   
   
    pageshow
   
   
    paste
   
   
    popstate
   
   
    readystatechange
   
   
    reset
   
   
    resize
   
   
    scroll
   
   
    search
   
   
    select
   
   
    selectstart
   
   
    selectionchange
   
   
    storage
   
   
    submit
   
   
    textInput
   
   
    unload
   
   
    updateready
   
   
    versionchange
   
   
    write
   
   
    writeend
   
   
    writestart
   
   
    zoom
   
   
   
   
    DOMActivate
   
   
    DOMFocusIn
   
   
    DOMFocusOut
   
   
    DOMAttrModified
   
   
    DOMCharacterDataModified
   
   
    DOMNodeInserted
   
   
    DOMNodeInsertedIntoDocument
   
   
    DOMNodeRemoved
   
   
    DOMNodeRemovedFromDocument
   
   
    DOMSubtreeModified
   
   
    DOMContentLoaded
   
   
   
   
    webkitBeforeTextInserted
   
   
    webkitEditableContentChanged
   
   
   
   
    canplay
   
   
    canplaythrough
   
   
    durationchange
   
   
    emptied
   
   
    ended
   
   
    loadeddata
   
   
    loadedmetadata
   
   
    pause
   
   
    play
   
   
    playing
   
   
    ratechange
   
   
    seeked
   
   
    seeking
   
   
    timeupdate
   
   
    volumechange
   
   
    waiting
   
   
   
   
    webkitbeginfullscreen
   
   
    webkitendfullscreen
   
   
   
   
    progress
   
   
    stalled
   
   
    suspend
   
   
   
   
    webkitAnimationEnd
   
   
    webkitAnimationStart
   
   
    webkitAnimationIteration
   
   
   
   
    webkitTransitionEnd
   
   
   
   
    orientationchange
   
   
   
   
    timeout
   
   
   
   
    touchstart
   
   
    touchmove
   
   
    touchend
   
   
    touchcancel
   
   
    *#if PLATFORM(ANDROID)
   
   
    touchlongpress
   
   
    touchdoubletap
   
   
    *#endif *
   
   
   
   
    success
   
   
   
   
    loadend
   
   
   
   
    webkitfullscreenchange
   
   
   
   
    webkitspeechchange
   
   
   
   
    webglcontextlost
   
   
    webglcontextrestored
   
   
    webglcontextcreationerror
   
   
   
   audioprocess
   
   
  
  
   
    }
   
  
  
  
 


