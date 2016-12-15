---
layout: post
title: "WinLaucher启动"
date: 2013-01-03 17:54:00 
comments: true
categories: []
tags: []
description: "WinLaucher启动"
keywords: 
---


 
  
   extern "C" __declspec(dllexport) int WINAPI dllLauncherEntryPoint(HINSTANCE, HINSTANCE, LPTSTR, int nCmdShow)
   
    {
    
     
      MyRegisterClass(hInst);
      
       注册Main窗口类
      
     
     
      hMainWnd = CreateWindow(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      
       CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, 0, 0, hInst, 0);
       
        
         创建Main窗口
        
       
       
        hURLBarWnd = CreateWindow(L"EDIT", 0,
        
         WS_CHILD | WS_VISIBLE | WS_BORDER | ES_LEFT | ES_AUTOVSCROLL,
         
          0, 0, 0, 0,
          
           hMainWnd,
           
            0,
            
             hInst, 0);
             
              创建地址栏
             
             
              ShowWindow(hMainWnd, nCmdShow);
              
               UpdateWindow(hMainWnd);
               
                
                 HRESULT hr = WebKitCreateInstance(CLSID_WebView, 0, IID_IWebView, reinterpret_cast<void**>;(&gWebView));
                 
                  创建WebView
                 
                 
                  
                   gWebHost = new WinLauncherWebHost();
                   
                    创建FrameLoadDelegate
                   
                   
                    hr = gWebView->;setFrameLoadDelegate(gWebHost);
                    
                     
                      gPrintDelegate = new PrintWebUIDelegate;
                      
                       创建UIDelegate
                      
                      
                       hr = gWebView->;setUIDelegate(gPrintDelegate);
                       
                        
                         hr = gWebView->;setHostWindow(reinterpret_cast<OLE_HANDLE>;(hMainWnd));
                         
                          
                           hr = gWebView->;initWithFrame(clientRect, 0, 0);
                           
                            WebView初始化
                           
                           
                            registerWebViewWindowClass();
                            
                             m_viewWindow = CreateWindowEx(0, kWebViewWindowClassName);
                             
                              创建ViewWindow
                             
                             
                              
                               
                               
                               ViewWindow窗口过程函数(
                              
                              
                               WebView::WebViewWndProc
                              
                              
                               )
                              
                              
                               m_page = new Page(pageClients);
                               
                                创建Page
                               
                               
                                
                                 WebFrame* webFrame = WebFrame::createInstance();
                                 
                                  创建WebFrame
                                 
                                 
                                 
                                 
                                  
                                   
                                   
                                   SetWindowLongPtr(m_viewWindow, 0, (LONG_PTR)this);
                                   
                                    将WebView指针保持在窗口里
                                   
                                  
                                  
                                   
                                   
                                  
                                 
                                
                                
                                 resizeSubViews();
                                 
                                  调整ViewWindow和地址栏位置
                                 
                                 
                                  
                                   ShowWindow(gViewWindow, nCmdShow);
                                   
                                    UpdateWindow(gViewWindow);
                                    
                                     
                                      while (GetMessage(&msg, NULL, 0, 0)) { 主窗口消息循环
                                      
                                       if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg)) {
                                       
                                        TranslateMessage(&msg);
                                        
                                         DispatchMessage(&msg);
                                         
                                          }
                                          
                                           }
                                           
                                            
                                             
                                              }
                                              
                                               
                                                
                                                 
                                                  
                                                   
                                                    
                                                    
                                                   
                                                   
                                                    
                                                    
                                                   
                                                   
                                                    
                                                     }
                                                    
                                                   
                                                  
                                                 
                                                
                                                
                                                 
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


