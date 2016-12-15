---
layout: post
title: "Native Client 资料"
date: 2011-12-05 15:35:00 
comments: true
categories: [浏览器]
tags: [浏览器]
description: "Native Client 资料"
keywords: 浏览器
---


 
 
 
  
  
 
 
  
   
   
  
 
 
  Native Client:
  
   本地程序
  
  (C/C++和目前不支持的其他程序)
  
   沙箱
  
 
 
  [JavaScript通过浏览器(解释引擎)来完成功能,HTML5只是扩展了部分功能]
 
 
  Native Client 适合
  
   纯计算(CPU+内存)
  
  本地程序,不适合 创建进程/直接访问文件/无限制访问网络 程序
 
 
  
   
   
  
 
 
  
  
 
 
  ========================================================================================================
 
 
  本地程序的生命周期
 
 
 
 
  （1）浏览器加载一个带有“application/x-nacl-srpc”MIME类型的embed标记的页面。
 
 
  （2）渲染引擎：调用RenderView::CreatePluginDelegate函数，创建一个针对application/x-nacl-srpc类型的NaCl插件。同时验证浏览器是否打开了Native Client功能，如果没有则阻止创建NaCl插件。
 
 
  （3）渲染引擎：调用NaCl插件的NP_Initialize函数。
 
 
  （4）NaCl插件：下载NaCl模块（即本地程序）
 
 
  当浏览器开始下载NaCl模块时调用NPP_NewStream函数，进一步调用PluginNpapi::NewStream函数，返回NP_NORMAL,表示NaCl插件已经准备好接受二进制数据。由于NaCl插件放入了浏览器沙箱，所以不能访问文件系统通过返回NP_ASFILEONLY。
 
 
  浏览器调用NPP_Write，NaCl插件则将二进制数据写入一个StreamShmBuffer对象。
 
 
  下载完成以后，浏览器调用NPP_DestroyStream函数销毁输入流。调用Plugin::Load函数，传入参数包括nexe二进制程序的StreamShmBuffer对象的指针。
 
 
  （5）NaCl插件：Plugin::Load函数加载NaCl模块
 
 
  NaCl插件检验nexe文件是否在白名单里面（不允许加载本地存储的NaCl模块），再检验nexe文件的合法ELF格式，之后创建一个plugin::ServiceRuntime对象来抽象NaCl加载器的实例。
 
 
  创建一个nacl::SelLdrLauncher对象，包含nexe文件的URL和文件描述符。调用nacl::SelLdrLauncher::Start函数，进而调用LaunchNaClProcess函数，进而发送ViewHostMsg_LaunchNaCl的IPC消息给浏览器，通知创建一个加载器进程。
 
 
  （6）浏览器：创建一个加载器进程
 
 
  创建一个channel5的已连接socket对，创建一个Chrome IPC 通道。创建一个加载器进程，之后触发NaClProcessHost::OnProcessLaunched回调函数，发送一个ViewHostMsg_LaunchNacl消息，包含channel5句柄、加载器进程ID和加载进程句柄。之后再发送NaClProcessMsg_Start消息，这时，NaCl插件和加载器有了彼此的channel5句柄，可以相互通信了。
 
 
  （7）加载器：NaClProcessMsg_Start消息的处理函数调用加载器的SelMain函数
 
 
  SelMain函数加载NaCl模块，创建一个BoundSocket，等待NaCl插件的连接。
 
 
  （8）NaCl插件：通过SharedMemory将NaCl模块传给加载器进程，向加载器发送start_module消息
 
 
  （9）加载器：收到start_module消息后，启动NaCl模块，等待调用。
 
 
  （10）NaCl插件：Plugin::Load调用OnLoad处理函数通知JavaScript引擎NaCl模块已准备好。至此，NaCl插件已经准备好处理JavaScript与NaCl模块之间的调用。
 
 
  
   ========================================================================================================
   
   
  
  
   
    Native Client目前支持2D图形、立体音频、URL获取、沙箱化的本地文件访问和与JavaScript的异步消息通信
   
   
   
  
  
   ========================================================================================================
  
  
  
  
   
    
     
      
       
        http://www.chromium.org/nativeclient
       
       
       
      
     
    
   
  
  
   
    
     
      
       
        https://developers.google.com/native-client/
       
      
     
    
   
  
  
   
    
     
      
       http://www.chromium.org/nativeclient/design-documents/native-client-integration-with-chrome
      
      
      
     
    
   
  
  
   
    
     
      http://www.chromium.org/nativeclient/getting-started/the-life-of-a-native-client-module
     
     
     
    
   
  
  
   
    
     http://www.chromium.org/nativeclient/getting-started/getting-started-background-and-basics
    
    
    
   
  
  
   内层沙箱：控制系统调用和跳转
  
  
   外层沙箱：系统调用白名单
  
  
   服务运行时：模拟系统调用
  
  
   
   
  
  
   服务运行时与NaCl程序同进程，后者通过API访问前者，前者通过x86的段页式内存阻止NaCl程序的非法内存访问
   
   
  
  
   
   
  
  
   服务运行时(64K)：前4K受读写保护，用于检查空指针；剩下的60K实现“trampoline”调用门和“springboard”返回门
  
  
   ========================================================================================================
   
   
  
  
   Native Client线程通过PepperAPI与浏览器的代理线程通信
  
  
   浏览器线程通过夹层API-系统API与OS内核线程通信
  
  
   CAR可以起到扩展浏览器的作用(一旦OS有变化,不需重新编译浏览器,只需发行CAR)
  
  
   由JS调NaCl程序
  
  
   
    NativeClient本身是平台相关的和可信的，可以访问所有系统调用接口
   
   
   
  
  
   
    
     
     
    
   
  
  
   ========================================================================================================
   
   
  
  
   执行过程(本地程序编译成中间码->;解码/验证->;受控运行)
  
  
   
    To the Native Client runtime system, a Native Client module is simply a set of machine code, formatted to adhere to a few special rules. No matter whether the code starts out as C or C++ or any other language, the Native Client
 runtime system performs the steps shown in the following figure:
   
  
  
   
  
  
   To ensure that system resources are untouched, the Native Client runtime system prevents the following unsafe activities:
  
  
   
    Manipulating devices or files directly (instead, a special file system API is provided)
   
   
    Directly accessing the operating system
   
   
    Using self-modifying code to hide the code's intent (such as attempts to write to protected memory)
   
  
  
   
    ========================================================================================================
   
  
  
   
    
     
     
    
   
  
  
   Native Client is ideal for application components requiring pure computation. It is not appropriate for modules requiring process creation, direct file system access, or unrestricted access to the network
   
   
  
  
   
   
  
  
   Native Client executes code that is compiled by a special compiler that prevents you from using certain types of operations and provides some sandboxing and such; it is intended to allow you to write native code but still enforce many of the security restrictions
 that the browser already follows. For things that are possible, it's great -- as long as you can get it to work in the browser you want to use. Currently it's supported by Google Chrome, but I haven't heard for sure that it works in any other browser.
  
  
   Conversely, with NPAPI or ActiveX (and see
   
    FireBreath
   
   which
 allows you to target both types simultaneously) you write native code and do more or less anything that a normal application could do (except in IE on Vista/Win7 w/ UAC enabled where you are in low integrity mode).
  
  
   The biggest disadvantage to NaCl is probably that you can't access hardware; since it's sandboxed, you're a bit more limited as to what networking things you can do and a lot more limited as to what devices you can interface with.
  
  
   
    
     
      
       ======================================================================
      
     
    
   
  
  
   
    
     
      
       
        Java’s
 security measures are chaperones. They’re always there and always checking your actions. NaCl’s mechanisms are just rules. They’re checked once, and then the program is on its own. NaCl promises to be faster than Java
       
       
       
      
     
    
   
  
  
   
    
     
      
       
        
         ======================================================================
        
        
        
       
      
     
    
   
  
  
   
    
     
      
       running a subset of Intel x86 or ARM native code using software-based
 fault isolation
      
     
    
   
  
  
   
    
     
      
       ======================================================================
      
     
    
   
  
  
   
    
     
      
       Native Client is specifically designed to run native code
 securely inside web browsers,
       
        it puts web applications on "the same playing field" as local applications, providing the raw speed needed to compete with traditional
 software on 3D games, video editing, and more
       
      
      
      
     
    
   
  
  
   
    
     
      
       
        ======================================================================
       
      
     
    
   
  
  
   
    
     
      
       
        
         "If
 we're successful with this [Native Client] project, we will make other languages more useful in the context of the web. We want to create a system that gives languages like C and C++ – but eventually others as well – the same excellent level of portability
 and safety that JavaScript provides on the web today."
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          
           Native
 Client
          
          is a software "sandbox" meant to
          
           securely run native code inside a browser
          
         
         
         
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          native
 languages such as C and C++ – which have access to a machine's underlying components – were not. Native Client seeks to add such protection.
         
         
         
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          
          
         
         
          google_native_client_from_all_sides
         
        
        
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         google_native_client_from_all_sides_2
        
       
      
     
    
   
  
  
   
    
     
      
       
        google_native_client_from_all_sides_3
       
      
     
    
   
  
  
   
    
     
      
       
        google_native_client_from_all_sides_4
       
      
      
      
     
    
   
  
  
   
    
     
      
       
        
         With
 the 32-bit x86 instruction set, Native Client uses the segment registers to restrict where in memory a program can read and write data and to ensure that a program doesn't jump to code outside a certain range of memory. But it also includes a modifiedcompiler
 and a code verifier that work to keep code jumps in line???????????????
        
       
       
       
      
     
    
   
  
  
   
    
     
      
       
        
         An
 ordinary program will read a data value from memory into a register and then jump to the address that value represents. But with Native Client, the compiler performs a bit of arithmetic on that value
         
          before
         
         the jump to ensure it doesn't target bad
 instructions, and then the code verifier double-checks the compiler's work.
        
        
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          
           This
 proposition fits quite nicely with Chrome OS, the fledgling Google operating system that puts
           
            all applications inside the browser
           
           . With Chrome
 OS, running existing 3D games and other desktop applications isn't really an option. But the Native Client project pre-dates Google's operating system effort, and the ultimate goal is to bring a new breed of applications to the entire web.????????????????????
          
         
         
         
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          "Our
 goal is to have an execution arm that can have no side effects – zero interaction with the outside world – and that's what we think we have achieved with the sandbox," says Brad Chen. "But the thing is that if you can't interact with the outside world, including
 the browser, you can't actually do anything. That's where these Pepper interfaces come in. They're designed to expose to Native Client exactly what is also being exposed via JavaScript."
         
         
         
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         =====================================================================
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          native
 code’s primary benefit lies in memory layout and access patterns, not instruction set benefits such as SIMD
         
         
         
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          
           mozillas-rejection-of-nativeclient-hurts-the-open-web
          
          
          
         
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         =====================================================================
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
         
        
       
      
     
    
   
  
 


