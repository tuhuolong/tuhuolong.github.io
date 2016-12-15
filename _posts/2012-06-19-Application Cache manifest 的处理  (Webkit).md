---
layout: post
title: "Application Cache manifest 的处理  (Webkit)"
date: 2012-06-19 16:18:00 
comments: true
categories: [webkit]
tags: [webkit]
description: "Application Cache manifest 的处理  (Webkit)"
keywords: webkit
---


 
  
   <html manifest="xxx">;
  
 
 
  
   
    
     缓存构成
    
    
    
   
   
    url+.manifest+manifest
   
   
    或者
 url (没有manifest)
   
  
 
 
  
   
   
  
 
 
  
   
    webkit解析html标签,发送(
   
   
    异步
   
   
    ).manifest请求
   
  
 
 
  
   ->;HTMLTreeBuilder::processStartTag
  
 
 
  
   ->;HTMLConstructionSite::insertHTMLHtmlStartTagBeforeHTML
   
   
  
 
 
  
   ->;HTMLHtmlElement::insertedByParser
  
 
 
  
   ->;documentLoader->;applicationCacheHost()->;selectCacheWithManifest或者selectCacheWithoutManifest
  
 
 
  
   (创建ApplicationCacheGroup)
  
 
 
  
   ->;ApplicationCacheGroup::update->;ApplicationCacheGroup::createResourceHandle
  
 
 
  
   发送manifest请求,ApplicationCacheGroup作为ResourceHandleClient
  
 
 
  
   -----------------异步-------------------
  
 
 
  
   
    根据.manifest发送(
   
   
    异步
   
   
    )缓存请求
   
  
 
 
  
   ->;ApplicationCacheGroup::didReceiveResponse
  
 
 
  
   ->;ApplicationCacheGroup::didReceiveData
  
 
 
  
   收到manifest
  
 
 
  
   ->;ApplicationCacheGroup::didFinishLoading
  
 
 
  
   解析manifest   (缓存队列)
  
 
 
  
   -----------------------------------------
  
 
 
  
   ->;ApplicationCacheGroup::startLoadingEntry
  
 
 
  
   ->;ApplicationCacheGroup::createResourceHandle
   
   
  
 
 
  
   
    依次发送请求获取缓存资源
   
   
   
  
 
 
  
   
   
  
 
 
  
   ->;ApplicationCacheGroup::didReceiveResponse
  
 
 
  
   ->;ApplicationCacheResource::create (创建CacheResource)
  
 
 
  
   ->;ApplicationCacheGroup::didReceiveData
  
 
 
  
   ->;(填充CacheResource)
  
 
 
  
   ->;ApplicationCacheGroup::didFinishLoading
   
   
  
 
 
  
   
    -----------------------------------------
   
  
 
 
  
   
    
     保存缓存
    
   
  
 
 
  
   
    ->;ApplicationCacheGroup::deliverDelayedMainResources
   
  
 
 
  
   
    ->;finishedLoadingMainResource
   
  
 
 
  
   
    ->;创建主url的CacheResource
   
  
 
 
  
   
    ->;ApplicationCacheGroup::checkIfLoadIsComplete
   
  
 
 
  
   
    ->;ApplicationCacheStorage::storeNewestCache
   
  
 
 
  
   
    保存在.db文件里面
   
  
 
 
  
   *******************************************
  
 
 
  
   
    
     第二次访问
    
    根据url映射从本地数据库取
    
     缓存
    
   
  
 
 
  
   
    ->;MainResourceLoader::load
    
    
   
  
 
 
  
   
    ->;ApplicationCacheHost::maybeLoadMainResource
   
  
 
 
  
   
    ->;ApplicationCacheGroup::cacheForMainRequest
   
  
 
 
  
   
    ->;SubstituteData(resource->;data(), resource->;response().mimeType(), resource->;response().textEncodingName(), KURL());
   
  
 
 
  
   
    获得缓存数据
   
  
 
 
  
   
    ->;(
    
     同步
    
    )MainResourceLoader::didReceiveResponse
   
  
 
 
  
   
    ->;(
    
     同步
    
    )MainResourceLoader::didReceiveData
    
    
   
  
 


