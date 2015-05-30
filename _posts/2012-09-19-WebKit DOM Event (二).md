---
layout: post
title: WebKit DOM Event (二)
date: 2012-09-19 11:42:00
categories: [WebKit]
tags: [webkit]
---
![](http://img.my.csdn.net/uploads/201209/20/1348132967_2832.jpg)

 
【1】Event事件发生
【2】查找EventTarget 
【3】EventDispatcher分发事件
 - - - - EventDispatcher::dispatchEvent
 - - - -         Node::preDispatchEventHandler
 - - - -        【1】Event::CAPTURING_PHASE
 - - - -                 WindowEventContext::handleLocalEvents
 - - - -                 EventContext::handleLocalEvents （m_ancestors）
 - - - -        【2】Event::AT_TARGET
 - - - -                （Node*->handleLocalEvents）
 - - - -                 Node::handleLocalEvents
 - - - -                 EventTarget::fireEventListeners
 - - - -                         EventListenerMap::find(event)
 - - - -                         EventListener*->handleEvent
 - - - -                                 V8AbstractEventListener::handleEvent
 - - - - 
 - - - -        【3】Event::BUBBLING_PHASE
 - - - -                 EventContext::handleLocalEvents （m_ancestors）
 - - - -                 WindowEventContext::handleLocalEvents
 - - - -         Node*->defaultEventHandler   （默认事件处理） 或 m_ancestors*->defaultEventHandler （往上传） 
 - - - -                 Node::defaultEventHandler
 - - - -                 HTMLAnchorElement::defaultEventHandler （Anchor结点）
 - - - -                         WebCore::handleLinkClick
 - - - -                         FrameLoader::urlSelected  发送请求
 - - - -                 HTMLButtonElement::defaultEventHandler
 - - - -                 HTMLLabelElement::defaultEventHandler




![](http://img.my.csdn.net/uploads/201209/20/1348132930_6163.jpg)

----------------------------------------------------------


++++ 
++++
++++
++++
++++
++++
++++
++++ 
++++ 
 - - - - 
 - - - - 
 - - - - 
 - - - - 
 - - - - 
 - - - - 
 - - - - 
 - - - -  
 - - - -