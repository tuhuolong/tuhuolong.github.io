---
layout: post
title: "VS2005+cygwin编译WebKit"
date: 2012-11-25 14:12:00 
comments: true
categories: []
tags: []
description: "VS2005+cygwin编译WebKit"
keywords: 
---


 
  
   
    首先，如果按照WebKit官网上步骤是可以编过去的，但是要求
    
     VS2005默认装在C:\
     
      Program Files，cygwin默认装在
      
       C:\，QuickTimeSDK默认装在
       
        C:\
       
       
        Program
 Files
       
      
     
    
   
  
 
 
  
   
    
     
      
       如果选择安装在其他地方，同志们，苦日子就来了。。。
      
     
    
   
  
 
 
  
   
    1. 安装VS2005英文版
   
  
 
 
  
   
    
     2. Install
    
    
     Microsoft
 Visual Studio 2005 Team Suite Service Pack 1
    
    
     .
    
    
    
   
  
 
 
  
   
    
     
      3. Install
     
     
      Visual
 Studio 2005 Service Pack 1 ATL Security Update
     
     
      .
     
     
     
    
   
  
 
 
  
   
    
     
     
    
   
  
 
 
  
   
    4. Install the following hotfixes to improve Visual Studio's performance and responsiveness:
   
  
 
 
  
   
    
     
      KB918559
     
    
   
  
  
   
    
     
      KB935225
     
    
   
  
  
   
    
     
      KB943969
     
    
   
  
  
   
    
     
      KB947315
     
    
   
  
 
 
  
   5. 安装cygwin
  
 
 
  
   
    
     
      到
      
       
        cygwin官网
       
      
      下载setup.exe
      
      
     
    
   
  
 
 
  
   
    
     
      下载Packages
     
    
   
  
 
 
  
   
    
     
      "
     
    
    
     Download
 Without Installing" ->; "Direct Connection" ->;
    
   
  
 
 
  
   
    
     
      ->; 一般选择http://mirrors.163.com镜像 ,勾选Devel和Libs "Install"
     
    
   
  
 
 
  
   
    
     
      ->; 但是curl这个包需要ftp://mirrors.xmission.com ,勾选Net->;curl "Install"
     
    
   
  
 
 
  
   
    
     
      ->; "Install from Local Directory"
     
    
   
  
 
 
  
   
    
     
      6.
      
       Install
       
        
         QuickTime
 SDK
        
       
      
     
    
   
  
 
 
  
   
    
     
      7.
      
       Install DirectX SDK
       
        February
 2010 DirectX SDK
       
       (随意选择安装位置，它会自动创建系统变量DXSDK_DIR)
      
     
    
   
  
 
 
  
   
    
     
      
      
      
       fatal error C1083: Cannot open include file: 'd3d9.h':
 No such file or directory
      
     
    
   
  
 
 
  
   
    
     
      8. 取WebKit代码
      
       Nightly
 Build
      
      
       , 放到home目录 : webkit使用svn下载后差不多有1G多，这里面的大部分代码是测试代码，由于网速慢加上现在不需要这些测试代码，使用
      
      
       Nightly Builds
      
      
       ，下载后只有十几兆。
      
     
    
   
  
 
 
  
   
    
     
      
       9. 下载
       
        WebKit
 Support Libraries
       
       , 放到WebKit根目录
      
     
    
   
  
 
 
  
   
    
     
      
       10. 运行Tools/Scripts/
       
        update-webkit
 ,会下载一些头文件和库(到WebKitLibraries/win)，并自动创建以下两个系统变量
       
      
     
    
   
  
 
 
  
   
    
     
      
      
      
       
        WEBKITLIBRARIESDIR
 = E:\Program Files\cygwin\home\chenhao\WebKit-r135516\WebKitLibraries\win
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          WEBKITOUTPUTDIR = E:\Program Files\cygwin\home\chenhao\WebKit-r135516\WebKitBuild
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           11.
          
         
        
       
      
     
    
   
  
  
   修改VS2005路径, Tools/Scripts/webkitdirs.pm
  
  
   
    
     
      
       
       
      
     
    
   
  
 
 
  
   
    
     
      
       #$vsInstallDir = File::Spec->;catdir($programFilesPath,
 "Microsoft Visual Studio 8");
      
      
       
        $vsInstallDir = File::Spec->;catdir(
       
       
        "E:\\Program Files"
       
       
        , "Microsoft Visual Studio 8");
       
      
     
    
   
  
 
 
  
   
    
     
      
       12. 修改QuickTime
 SDK路径, Source/WebCore/WebCore.vcproj/QTMovieWinCommon.vsprops
      
     
    
   
  
 
 
  
   
    
     
      
       "VCCLCompilerTool"
      
     
    
   
  
 
 
  
   
    
     
      
       $(ProgramFiles)/QuickTime SDK/cincludes
      
     
    
   
  
 
 
  
   
    
     
      
      
      
       E:\Program
 Files
      
      
       /QuickTime SDK/cincludes
      
     
    
   
  
 
 
  
   
    
     
      
       
        fatal error C1083: Cannot open include file: 'CFData.h': No such file or directory
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        "VCLinkerTool"
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        $(ProgramFiles)\QuickTime SDK\Libraries
       
      
     
    
   
  
 
 
  
   
    
     
      
       
       
       
        
         E:\Program Files
        
       
       
        \QuickTime SDK\Libraries
       
       
       
      
     
    
   
  
 
 
  
   
    
     
      
       
       
       
        fatal error LNK1104: cannot open file 'QTMLClient.lib'
       
      
     
    
   
  
 
 
  
   
    
     
      
       13.
      
      
       运行
      
      
       Tools/Scripts/build
      
      
       
        -webkit
 ,它会动态生成一些代码(
       
       
        
         stdint.h和stdbool.h等到WebKitLibraries/win)
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         build
        
        -webkit
 --debug    或者
       
      
     
    
   
   
    build
   
   -webkit --release
  
 
 
  
   
    fatal error C1083: Cannot open include file: 'stdint.h': No such file or directory
   
  
  
  
 
 
  
   14.
   
    使用VS2005打开 Source/WebKit/win/WebKit.vcproj/WebKit.sln
   
  
 
 
  
   15. 将所有工程 Configuration->;"Debug"
  
 
 
  
   
    16.
    
     所有C/C++工程 Treat Warnings As Errors 设为"No"
    
   
  
 
 
  
   
    
    
    
     error C2220: warning treated as error - no 'object' file generated
    
   
  
 
 
  
   
    
     
      
       
        
         
          
          
         
        
       
      
     
    
   
  
 
 
  
   17.修改cygwin路径
  
 
 
  
   
    (1)所有非C/C++工程Configuration->;NMake->;Build Command Line,Rebuild All Command Line和Clean Command Line
   
   
   
  
 
 
  
   
    set PATH=%SystemDrive%\cygwin\bin;%PATH%
   
  
 
 
  
   
    set PATH=
   
   
    E:\Program Files
   
   
    \cygwin\bin;%PATH%
   
  
 
 
  
   
   
   
    
     fatal error U1077: 'touch' : return code '0x1'
    
   
  
  
   (cygwin路径不对,
 .make文件找不到touch命令。)
  
 
 
  
   (2)部分C/C++工程的PreBuild.cmd
  
 
 
  
   
    
    
   
   Interfaces工程
  
  
   Source\WebKit\win\WebKit.vcproj\InterfacesPreBuild.cmd
  
 
 
  
   
    
     
      
       
        
         
          
           
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     set PATH=%SystemDrive%\cygwin\bin;%PATH%
    
   
  
 
 
  
   
    
     set PATH=
     
      E:\Program Files
     
     \cygwin\bin;%PATH%
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           WebKitLib工程 Source\WebKit\win\WebKit.vcproj\WebKitLibPreBuild.cmd
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           set PATH=%SystemDrive%\cygwin\bin;%PATH%
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           set PATH=
           
            E:\Program Files
           
           \cygwin\bin;%PATH%
           
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           WebKit工程 Source\WebKit2\win\WebKitPreBuild.cmd
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           
            set PATH=%SystemDrive%\cygwin\bin;%PATH%
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           
            set PATH=
            
             E:\Program Files
            
            \cygwin\bin;%PATH%
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           
            
             'bash' 不是内部或外部命令，也不是可运行的程序
            
            
            
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           18.
 Build Solution （我第一次是按照工程的依赖链一个一个编译的，
           
            容易发现哪个工程出错了，也可以直接Build Solution
           
           ）
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           19.
 下载
           
            Safari
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           安装之后将Safari根目录下的所有dll（PubSubDLL.dll，Safari.dll，SafariTheme.dll，Search.dll和SpellChecker.dll）
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           拷贝到WebKitBuild\Debug\bin目录。
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           20.
 点击WinLauncher.exe ,搞定
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           
            
            
           
          
         
        
       
      
     
    
   
  
 
 
  
   
    
     
      
       
        
         
          
           
           
          
         
        
       
      
     
    
   
  
 
 
  
  
 
 
  
  
 
 
  
   
    
     
     
    
   
  
 
 
  
   
    
     其他编译错误：
    
   
  
 
 
  
   
    
     
      1.
     
     
      error C2001: newline in constant
      
      
      Source\WebCore\platform\LocalizedStrings.cpp
     
     
      Source\WebCore\platform\LocalizedStrings.cpp
     
    
   
  
 
 
  
   
    
     
      return formatLocalizedString(WEB_UI_STRING("Look Up 鈥?@鈥?, "Look Up context
 menu item with selected word"), selectedCFString.get());
     
    
   
  
 
 
  
   
    
     
      
       return formatLocalizedString(WEB_UI_STRING("Look Up
      
      
       “%@”"
      
      
       ,
 "Look Up context menu item with selected word"), selectedCFString.get());
      
      
      
     
    
   
  
 
 
  
   
    
     
      return WEB_UI_STRING("Look Up 鈥?selection>;鈥?, "Look Up context menu item with
 selected word").replace("<selection>;",
      
      
     
    
   
  
 
 
  
   
    
     
      
       return WEB_UI_STRING("Look Up “
      
      
       <selection>;”"
      
      
       ,
 "Look Up context menu item with selected word").replace("<selection>;",
      
     
    
   
  
 
 
  
   
    
     
     
    
   
  
 


