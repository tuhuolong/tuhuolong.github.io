---
layout: post
title: "VS2005+cygwin编译WebKit分析"
date: 2012-11-27 10:13:00 
comments: true
categories: []
tags: []
description: "VS2005+cygwin编译WebKit分析"
keywords: 
---


 
  
   1. build-webkit
  
 
 
  
   buildVisualStudioProject
   
   
  
 
 
  
   2. webkitdirs.pm
  
 
 
  
   @command = ($vcBuildPath, $project, $action, $config);
  
 
 
  
   system @command;
  
 
 
  
   /home/chenhao/WebKit-r135516/Tools/Scripts/pdevenv win\WebKit.vcproj\WebKit.sln /build Debug
  
 
 
  
   3. pdevenv
  
 
 
  
   
    call "%VS80COMNTOOLS%\vsvars32.bat" 设置VS的环境变量
    
     set PATH=E:\Program Files\cygwin\home\chenhao\WebKit-r135516\Tools\vcbin;E:\Program Files\cygwin\home\chenhao\WebKit-r135516\Tools\Scripts;%PATH%
     
      IF EXIST "%VSINSTALLDIR%\Common7\IDE\devenv.com" (devenv.com /useenv win\WebKit.vcproj\WebKit.sln /build Debug) ELSE VCExpress.exe /useenv win\WebKit.vcproj\WebKit.sln /build Debug
     
    
   
  
 
 
  
   
    
     devenv.com /useenv win\WebKit.vcproj\WebKit.sln /build Debug
    
   
  
 
 
  
   
    
     
     
    
   
  
 
 
  
   依赖关系
  
 
 
  
   JavaScriptCore->;JavaScriptCoreGenerated->;WTF->;WTFGenerated
  
 
 
  
   ->; LLIntAssembly ->; LLIntOffsetsExtactor ->; LLIntDesiredOffsets ->;
  
 


