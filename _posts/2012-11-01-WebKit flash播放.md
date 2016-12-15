---
layout: post
title: "WebKit flash播放"
date: 2012-11-01 17:16:00 
comments: true
categories: []
tags: []
description: "WebKit flash播放"
keywords: 
---


 
  HTMLPlugInImageElement::attach
  
   HTMLPlugInImageElement::updateWidgetCallback
   
    HTMLEmbedElement::updateWidget
    
     SubframeLoader::requestObject
     
      SubframeLoader::requestPlugin
      
       SubframeLoader::loadPlugin
       
        FrameLoaderClientAndroid::createPlugin
        
         RenderPart::setWidget
         
         
        
       
      
     
    
   
  
 
 
  
   
   
  
 
 
  
   
    flash区域产生消息，WebCore发送消息给PluginView
   
  
 
 
  
   
    
    
   
  
 
 
  
   
    PluginView::handleEvent
    
    
   
  
 
 
  
   
    
    
   
  
 
 
  
   
    
    
   
  
 
 
  
   
    
    
   
  
 


