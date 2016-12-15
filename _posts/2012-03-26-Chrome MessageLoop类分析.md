---
layout: post
title: "Chrome MessageLoop类分析"
date: 2012-03-26 16:16:00 
comments: true
categories: [chrome]
tags: [chrome]
description: "Chrome MessageLoop类分析"
keywords: chrome
---


 
  Windows程序是基于消息的，不管其封装形式如何，最后都要包含如下代码
 
 
  
   
    MSG msg;
   
   
    
     while(GetMesssage(&msg))
    
    
     
      {
     
     
      
       TranslateMessage(&msg);
      
      
       
        DispatchMessage(&msg);
       
       
        
         }
        
        
         
          
           大部分的工作都是在这个while循环里完成。 GetMessage获得一条消息，然后调用DispatchMessage分发消息。DispatchMessage首先找到消息对应的窗口，调用窗口的消息处理函数，在消息处理函数里通过switch/case执行对应的处理代码，然后返回继续获得下一个消息GetMessage。直到收到WM_QUIT消息，while循环才会退出，否则一直阻塞在GetMessage处等待消息。
          
          
           
            
             一条线程启动后，如果不是进入消息循环，那么很快就结束，比如以下线程入口函数
            
            
             
              unsigned int WINAPI ThreadFunc(LPVOID param)
             
             
              
               {
              
              
               
                ...
               
               
                
                 
                  
                   //执行到这里，线程就准备退出了
                  
                  
                   
                    }
                   
                   
                    
                     因为创建线程是有代价的，我们希望线程能一直运行，所以加入消息循环，比如
                    
                    
                     
                      unsigned int WINAPI ThreadFunc(LPVOID param)
                     
                     
                      
                       {
                      
                      
                       
                        ...
                       
                       
                        
                         
                          while(WaitForMessage())
                         
                         
                          
                           {
                          
                          
                           
                            DispatchMessage();
                           
                           
                            
                             }
                            
                            
                             
                              
                               }
                              
                              
                               
                                消息循环就是让线程一直运行在类似的while循环中，不断检查和分发消息，直到收到特定消息而退出循环，然后线程就结束了。
                               
                               
                                
                                 一般来说，除非只为特定工作而创建的线程之外，通用的公共线程都应该有一个消息队列和消息循环。 这样，我们可以通过把消息发送到指定线程的消息队列中，从而让消息在指定线程中处理，而避免多线程访问所带来的并发访问问题。
                                
                                
                                 
                                  
                                   在Chrome里，是使用MessageLoop对象来封装以上的消息处理循环代码。一条线程在启动后，就会创建MessageLoop对象，并调用MessageLoop.Run()方法来进入消息循环。当MessageLoop.Run()函数返回，即线程退出。
                                  
                                  
                                   
                                    class MessageLoop : public base::MessagePump::Delegate {
                                   
                                   
                                    
                                     {
                                    
                                    
                                     
                                      public:
                                     
                                     
                                      
                                       
                                        
                                         // Run the message loop.
                                        
                                        
                                         
                                          void Run();
                                         
                                         
                                          
                                           // Signals the Run method to return after it is done processing all pending
                                          
                                          
                                           
                                            // messages. This method may only be called on the same thread that called
                                           
                                           
                                            
                                             // Run, and Run must still be on the call stack.
                                            
                                            
                                             
                                              //
                                             
                                             
                                              
                                               // Use QuitTask if you need to Quit another thread's MessageLoop, but note
                                              
                                              
                                               
                                                // that doing so is fairly dangerous if the target thread makes nested calls
                                               
                                               
                                                
                                                 // to MessageLoop::Run. The problem being that you won't know which nested
                                                
                                                
                                                 
                                                  // run loop you are quiting, so be careful!
                                                 
                                                 
                                                  
                                                   //
                                                  
                                                  
                                                   
                                                    void Quit();
                                                   
                                                   
                                                    
                                                     // Returns the MessageLoop object for the current thread, or null if none.
                                                    
                                                    
                                                     
                                                      static MessageLoop* current();
                                                     
                                                     
                                                      
                                                       
                                                        // The "PostTask" family of methods call the task's Run method asynchronously
                                                       
                                                       
                                                        
                                                         // from within a message loop at some point in the future.
                                                        
                                                        
                                                         
                                                          //
                                                         
                                                         
                                                          
                                                           // With the PostTask variant, tasks are invoked in FIFO order, inter-mixed
                                                          
                                                          
                                                           
                                                            // with normal UI or IO event processing.
                                                           
                                                           
                                                            
                                                             //
                                                            
                                                            
                                                             
                                                              // The MessageLoop takes ownership of the Task, and deletes it after it has
                                                             
                                                             
                                                              
                                                               // been Run().
                                                              
                                                              
                                                               
                                                                //
                                                               
                                                               
                                                                
                                                                 // NOTE: These methods may be called on any thread. The Task will be invoked
                                                                
                                                                
                                                                 
                                                                  // on the thread that executes MessageLoop::Run().
                                                                 
                                                                 
                                                                  
                                                                   
                                                                    
                                                                     void PostTask( const tracked_objects::Location& from_here, Task* task);
                                                                    
                                                                    
                                                                     
                                                                      }
                                                                     
                                                                     
                                                                      
                                                                       调用MessageLoop::current()方法可以获得当前线程对应的MessageLoop对象，并可以调用其PostTask方法让该线程上执行一个Task。PostTask把Task放入队列后就返回了，Task会在稍后的消息循环中得到处理。
                                                                      
                                                                      
                                                                       
                                                                        MessageLoop除了执行Task，还可以处理常规的windows消息以及IO事件等，具体要看MessagLoop的类型。 MessageLoop内部使用使用MessagePump对象来处理消息，并根据不同的类型创建不同的MessagePump。
                                                                       
                                                                       
                                                                        
                                                                         MessageLoop::MessageLoop(Type type)
                                                                        
                                                                        
                                                                         
                                                                          {
                                                                         
                                                                         
                                                                          
                                                                           lazy_tls_ptr.Pointer()->;Set(this);
                                                                          
                                                                          
                                                                           
                                                                            
                                                                             
                                                                              if (type_ == TYPE_DEFAULT) {
                                                                             
                                                                             
                                                                              
                                                                               pump_ = new base::MessagePumpDefault();
                                                                              
                                                                              
                                                                               
                                                                                } else if (type_ == TYPE_IO) {
                                                                               
                                                                               
                                                                                
                                                                                 pump_ = new base::MessagePumpForIO();
                                                                                
                                                                                
                                                                                 
                                                                                  } else {
                                                                                 
                                                                                 
                                                                                  
                                                                                   DCHECK(type_ == TYPE_UI);
                                                                                  
                                                                                  
                                                                                   
                                                                                    pump_ = new base::MessagePumpForUI();
                                                                                   
                                                                                   
                                                                                    
                                                                                     }
                                                                                    
                                                                                    
                                                                                     
                                                                                      }
                                                                                     
                                                                                     
                                                                                      
                                                                                       
                                                                                        void MessageLoop::RunInternal()
                                                                                       
                                                                                       
                                                                                        
                                                                                         {
                                                                                        
                                                                                        
                                                                                         
                                                                                          pump_->;Run(this);
                                                                                         
                                                                                         
                                                                                          
                                                                                           }
                                                                                          
                                                                                          
                                                                                           
                                                                                            
                                                                                             MessagePumpDefault最简单，只执行Task。MessagePumpForUI可以处理windows消息队列，而MessagePumpForIO可以处理IO完成端口的消息。不同Pump获得消息的方式是不一样的，MessagePumpDefault等待Event信号量，PumpForUI从Windows的消息队列，而PumpForIO是从IO完成端口。
                                                                                            
                                                                                            
                                                                                             
                                                                                              
                                                                                               
                                                                                                void MessagePumpDefault::Run(Delegate* delegate) {
                                                                                               
                                                                                               
                                                                                                
                                                                                                 
                                                                                                  if (delayed_work_time_.is_null()) {
                                                                                                 
                                                                                                 
                                                                                                  
                                                                                                   event_.Wait();
                                                                                                  
                                                                                                  
                                                                                                   
                                                                                                    } else {
                                                                                                   
                                                                                                   
                                                                                                    
                                                                                                     TimeDelta delay = delayed_work_time_ - Time::Now();
                                                                                                    
                                                                                                    
                                                                                                     
                                                                                                      if (delay >; TimeDelta()) {
                                                                                                     
                                                                                                     
                                                                                                      
                                                                                                       event_.TimedWait(delay);
                                                                                                      
                                                                                                      
                                                                                                       
                                                                                                        } else {
                                                                                                       
                                                                                                       
                                                                                                        
                                                                                                         delayed_work_time_ = Time();
                                                                                                        
                                                                                                        
                                                                                                         
                                                                                                          }
                                                                                                         
                                                                                                         
                                                                                                          
                                                                                                           }
                                                                                                          
                                                                                                          
                                                                                                           
                                                                                                            }
                                                                                                           
                                                                                                           
                                                                                                            
                                                                                                             
                                                                                                              
                                                                                                               void MessagePumpForUI::WaitForWork() {
                                                                                                              
                                                                                                              
                                                                                                               
                                                                                                                
                                                                                                                 
                                                                                                                  DWORD result;
                                                                                                                 
                                                                                                                 
                                                                                                                  
                                                                                                                   result = MsgWaitForMultipleObjectsEx(0, NULL, delay, QS_ALLINPUT,
                                                                                                                  
                                                                                                                  
                                                                                                                   
                                                                                                                    MWMO_INPUTAVAILABLE);
                                                                                                                   
                                                                                                                   
                                                                                                                    
                                                                                                                     
                                                                                                                      
                                                                                                                       if (WAIT_OBJECT_0 == result) {
                                                                                                                      
                                                                                                                      
                                                                                                                       
                                                                                                                        MSG msg = {0};
                                                                                                                       
                                                                                                                       
                                                                                                                        
                                                                                                                         DWORD queue_status = GetQueueStatus(QS_MOUSE);
                                                                                                                        
                                                                                                                        
                                                                                                                         
                                                                                                                          if (HIWORD(queue_status) & QS_MOUSE &&
                                                                                                                         
                                                                                                                         
                                                                                                                          
                                                                                                                           !PeekMessage(&msg, NULL, WM_MOUSEFIRST, WM_MOUSELAST, PM_NOREMOVE)) {
                                                                                                                          
                                                                                                                          
                                                                                                                           
                                                                                                                            WaitMessage();
                                                                                                                           
                                                                                                                           
                                                                                                                            
                                                                                                                             }
                                                                                                                            
                                                                                                                            
                                                                                                                             
                                                                                                                              return;
                                                                                                                             
                                                                                                                             
                                                                                                                              
                                                                                                                               }
                                                                                                                              
                                                                                                                              
                                                                                                                               
                                                                                                                                
                                                                                                                                 
                                                                                                                                  
                                                                                                                                   
                                                                                                                                    bool MessagePumpForIO::GetIOItem(DWORD timeout, IOItem* item) {
                                                                                                                                   
                                                                                                                                   
                                                                                                                                    
                                                                                                                                     memset(item, 0, sizeof(*item));
                                                                                                                                    
                                                                                                                                    
                                                                                                                                     
                                                                                                                                      ULONG_PTR key = NULL;
                                                                                                                                     
                                                                                                                                     
                                                                                                                                      
                                                                                                                                       OVERLAPPED* overlapped = NULL;
                                                                                                                                      
                                                                                                                                      
                                                                                                                                       
                                                                                                                                        if (!GetQueuedCompletionStatus(port_.Get(), &item->;bytes_transfered, &key,
                                                                                                                                       
                                                                                                                                       
                                                                                                                                        
                                                                                                                                         &overlapped, timeout)) {
                                                                                                                                        
                                                                                                                                        
                                                                                                                                         
                                                                                                                                          if (!overlapped)
                                                                                                                                         
                                                                                                                                         
                                                                                                                                          
                                                                                                                                           return false; // Nothing in the queue.
                                                                                                                                          
                                                                                                                                          
                                                                                                                                           
                                                                                                                                            item->;error = GetLastError();
                                                                                                                                           
                                                                                                                                           
                                                                                                                                            
                                                                                                                                             item->;bytes_transfered = 0;
                                                                                                                                            
                                                                                                                                            
                                                                                                                                             
                                                                                                                                              }
                                                                                                                                             
                                                                                                                                             
                                                                                                                                              
                                                                                                                                               
                                                                                                                                                
                                                                                                                                                 item->;handler = reinterpret_cast<IOHandler*>;(key);
                                                                                                                                                
                                                                                                                                                
                                                                                                                                                 
                                                                                                                                                  item->;context = reinterpret_cast<IOContext*>;(overlapped);
                                                                                                                                                 
                                                                                                                                                 
                                                                                                                                                  
                                                                                                                                                   return true;
                                                                                                                                                  
                                                                                                                                                  
                                                                                                                                                   
                                                                                                                                                    }
                                                                                                                                                   
                                                                                                                                                   
                                                                                                                                                    
                                                                                                                                                     
                                                                                                                                                      
                                                                                                                                                       PostTask只是把Task放在一个Task队列里的，如果这时候线程处于等待消息的阻塞状态，那么还需要发送一条消息去唤醒线程。不同的Pump使用不同的方式，PumpDefault是等待Event信号量，PumpForUI是阻塞在GetMessage上等待Windows消息，所以可以发送Windows消息唤醒。而PumpForIO是阻塞在IO完成端口上，那么只要模拟发送一个IO完成信息过去即可唤醒线程。
                                                                                                                                                      
                                                                                                                                                      
                                                                                                                                                       
                                                                                                                                                        
                                                                                                                                                         void MessagePumpDefault::ScheduleWork() {
                                                                                                                                                        
                                                                                                                                                        
                                                                                                                                                         
                                                                                                                                                          // Since this can be called on any thread, we need to ensure that our Run
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                          
                                                                                                                                                           // loop wakes up.
                                                                                                                                                          
                                                                                                                                                          
                                                                                                                                                           
                                                                                                                                                            event_.Signal();
                                                                                                                                                           
                                                                                                                                                           
                                                                                                                                                            
                                                                                                                                                             }
                                                                                                                                                            
                                                                                                                                                            
                                                                                                                                                             
                                                                                                                                                              
                                                                                                                                                               
                                                                                                                                                                void MessagePumpForUI::ScheduleWork() {
                                                                                                                                                               
                                                                                                                                                               
                                                                                                                                                                
                                                                                                                                                                 PostMessage(message_hwnd_, kMsgHaveWork, reinterpret_cast<WPARAM>;(this), 0);
                                                                                                                                                                
                                                                                                                                                                
                                                                                                                                                                 
                                                                                                                                                                  }
                                                                                                                                                                 
                                                                                                                                                                 
                                                                                                                                                                  
                                                                                                                                                                   
                                                                                                                                                                    void MessagePumpForIO::ScheduleWork() {
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                    
                                                                                                                                                                     BOOL ret = PostQueuedCompletionStatus(port_, 0,
                                                                                                                                                                    
                                                                                                                                                                    
                                                                                                                                                                     
                                                                                                                                                                      reinterpret_cast<ULONG_PTR>;(this),
                                                                                                                                                                     
                                                                                                                                                                     
                                                                                                                                                                      
                                                                                                                                                                       reinterpret_cast<OVERLAPPED*>;(this));
                                                                                                                                                                      
                                                                                                                                                                      
                                                                                                                                                                       
                                                                                                                                                                        }
                                                                                                                                                                       
                                                                                                                                                                       
                                                                                                                                                                        
                                                                                                                                                                         MessagePump对Native消息（Windows消息或IO完成消息）有自己的处理方式（PumpDefault没有消息要处理），而Task则由MessageLoop来统一处理。
                                                                                                                                                                        
                                                                                                                                                                        
                                                                                                                                                                         
                                                                                                                                                                          void MessagePumpDefault::Run(Delegate* delegate) {
                                                                                                                                                                         
                                                                                                                                                                         
                                                                                                                                                                          
                                                                                                                                                                           
                                                                                                                                                                            
                                                                                                                                                                             for (;;) {
                                                                                                                                                                            
                                                                                                                                                                            
                                                                                                                                                                             
                                                                                                                                                                              ScopedNSAutoreleasePool autorelease_pool;
                                                                                                                                                                             
                                                                                                                                                                             
                                                                                                                                                                              
                                                                                                                                                                               
                                                                                                                                                                                bool did_work = delegate->;DoWork();
                                                                                                                                                                               
                                                                                                                                                                               
                                                                                                                                                                                
                                                                                                                                                                                 if (!keep_running_)
                                                                                                                                                                                
                                                                                                                                                                                
                                                                                                                                                                                 
                                                                                                                                                                                  break;
                                                                                                                                                                                 
                                                                                                                                                                                 
                                                                                                                                                                                  
                                                                                                                                                                                   
                                                                                                                                                                                    
                                                                                                                                                                                     if (did_work)
                                                                                                                                                                                    
                                                                                                                                                                                    
                                                                                                                                                                                     
                                                                                                                                                                                      continue;
                                                                                                                                                                                     
                                                                                                                                                                                     
                                                                                                                                                                                      
                                                                                                                                                                                       
                                                                                                                                                                                        did_work = delegate->;DoIdleWork();
                                                                                                                                                                                       
                                                                                                                                                                                       
                                                                                                                                                                                        
                                                                                                                                                                                         if (!keep_running_)
                                                                                                                                                                                        
                                                                                                                                                                                        
                                                                                                                                                                                         
                                                                                                                                                                                          break;
                                                                                                                                                                                         
                                                                                                                                                                                         
                                                                                                                                                                                          
                                                                                                                                                                                           
                                                                                                                                                                                            if (did_work)
                                                                                                                                                                                           
                                                                                                                                                                                           
                                                                                                                                                                                            
                                                                                                                                                                                             continue;
                                                                                                                                                                                            
                                                                                                                                                                                            
                                                                                                                                                                                             
                                                                                                                                                                                              
                                                                                                                                                                                               if (delayed_work_time_.is_null()) {
                                                                                                                                                                                              
                                                                                                                                                                                              
                                                                                                                                                                                               
                                                                                                                                                                                                event_.Wait();
                                                                                                                                                                                               
                                                                                                                                                                                               
                                                                                                                                                                                                
                                                                                                                                                                                                 } else {
                                                                                                                                                                                                
                                                                                                                                                                                                
                                                                                                                                                                                                 
                                                                                                                                                                                                  TimeDelta delay = delayed_work_time_ - Time::Now();
                                                                                                                                                                                                 
                                                                                                                                                                                                 
                                                                                                                                                                                                  
                                                                                                                                                                                                   if (delay >; TimeDelta()) {
                                                                                                                                                                                                  
                                                                                                                                                                                                  
                                                                                                                                                                                                   
                                                                                                                                                                                                    event_.TimedWait(delay);
                                                                                                                                                                                                   
                                                                                                                                                                                                   
                                                                                                                                                                                                    
                                                                                                                                                                                                     } else {
                                                                                                                                                                                                    
                                                                                                                                                                                                    
                                                                                                                                                                                                     
                                                                                                                                                                                                      // It looks like delayed_work_time_ indicates a time in the past, so we
                                                                                                                                                                                                     
                                                                                                                                                                                                     
                                                                                                                                                                                                      
                                                                                                                                                                                                       // need to call DoDelayedWork now.
                                                                                                                                                                                                      
                                                                                                                                                                                                      
                                                                                                                                                                                                       
                                                                                                                                                                                                        delayed_work_time_ = Time();
                                                                                                                                                                                                       
                                                                                                                                                                                                       
                                                                                                                                                                                                        
                                                                                                                                                                                                         }
                                                                                                                                                                                                        
                                                                                                                                                                                                        
                                                                                                                                                                                                         
                                                                                                                                                                                                          }
                                                                                                                                                                                                         
                                                                                                                                                                                                         
                                                                                                                                                                                                          
                                                                                                                                                                                                           // Since event_ is auto-reset, we don't need to do anything special here
                                                                                                                                                                                                          
                                                                                                                                                                                                          
                                                                                                                                                                                                           
                                                                                                                                                                                                            // other than service each delegate method.
                                                                                                                                                                                                           
                                                                                                                                                                                                           
                                                                                                                                                                                                            
                                                                                                                                                                                                             }
                                                                                                                                                                                                            
                                                                                                                                                                                                            
                                                                                                                                                                                                             
                                                                                                                                                                                                              
                                                                                                                                                                                                               keep_running_ = true;
                                                                                                                                                                                                              
                                                                                                                                                                                                              
                                                                                                                                                                                                               
                                                                                                                                                                                                                }
                                                                                                                                                                                                               
                                                                                                                                                                                                               
                                                                                                                                                                                                                
                                                                                                                                                                                                                 
                                                                                                                                                                                                                  void MessagePumpForUI::DoRunLoop() {
                                                                                                                                                                                                                 
                                                                                                                                                                                                                 
                                                                                                                                                                                                                  
                                                                                                                                                                                                                   for (;;) {
                                                                                                                                                                                                                  
                                                                                                                                                                                                                  
                                                                                                                                                                                                                   
                                                                                                                                                                                                                    bool more_work_is_plausible = ProcessNextWindowsMessage();
                                                                                                                                                                                                                   
                                                                                                                                                                                                                   
                                                                                                                                                                                                                    
                                                                                                                                                                                                                     if (state_->;should_quit)
                                                                                                                                                                                                                    
                                                                                                                                                                                                                    
                                                                                                                                                                                                                     
                                                                                                                                                                                                                      break;
                                                                                                                                                                                                                     
                                                                                                                                                                                                                     
                                                                                                                                                                                                                      
                                                                                                                                                                                                                       
                                                                                                                                                                                                                        if (more_work_is_plausible)
                                                                                                                                                                                                                       
                                                                                                                                                                                                                       
                                                                                                                                                                                                                        
                                                                                                                                                                                                                         continue;
                                                                                                                                                                                                                        
                                                                                                                                                                                                                        
                                                                                                                                                                                                                         
                                                                                                                                                                                                                          
                                                                                                                                                                                                                           more_work_is_plausible = state_->;delegate->;DoIdleWork();
                                                                                                                                                                                                                          
                                                                                                                                                                                                                          
                                                                                                                                                                                                                           
                                                                                                                                                                                                                            if (state_->;should_quit)
                                                                                                                                                                                                                           
                                                                                                                                                                                                                           
                                                                                                                                                                                                                            
                                                                                                                                                                                                                             break;
                                                                                                                                                                                                                            
                                                                                                                                                                                                                            
                                                                                                                                                                                                                             
                                                                                                                                                                                                                              
                                                                                                                                                                                                                               if (more_work_is_plausible)
                                                                                                                                                                                                                              
                                                                                                                                                                                                                              
                                                                                                                                                                                                                               
                                                                                                                                                                                                                                continue;
                                                                                                                                                                                                                               
                                                                                                                                                                                                                               
                                                                                                                                                                                                                                
                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                  WaitForWork(); // Wait (sleep) until we have work to do again.
                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                   }
                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                    }
                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                      void MessagePumpForIO::DoRunLoop() {
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                       for (;;) {
                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                        bool more_work_is_plausible = state_->;delegate->;DoWork();
                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                         if (state_->;should_quit)
                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                          break;
                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                            more_work_is_plausible |= WaitForIOCompletion(0, NULL);
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                             if (state_->;should_quit)
                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                              break;
                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                if (more_work_is_plausible)
                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                 continue;
                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                   more_work_is_plausible = state_->;delegate->;DoIdleWork();
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                    if (state_->;should_quit)
                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                     break;
                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                       if (more_work_is_plausible)
                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                        continue;
                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                          WaitForWork(); // Wait (sleep) until we have work to do again.
                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                           }
                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                            }
                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                              MessageLoop对象从队列里取出Task执行，即调用Task::Run，然后删除。
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                void MessageLoop::RunTask(Task* task) {
                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                 task->;Run();
                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                  delete task;
                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                   }
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                     当一条线程创建了MessageLoop对象之后，它就具备了运行Task的能力，我们就可以任意指派Task在该线程执行。就像这样
                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                      some_message_loop->;PostTask( some_task);
                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                        因为MessageLoop的本意是处理和分发消息的，是共用，所以消息处理代码不宜运行过长时间，包括Task。因为这会长时间阻塞住整条MessageLoop，影响其它消息的处理。对于需要长时间运行的，还是需要创建单独的工作线程，或者调用异步执行代码。 比如Socket, Pipe和File都是可以用非阻塞的方式操作，即异步IO--在Windows平台上可以使用IO完成端口来方便处理。MessageLoopForIO可以用于异步IO专用线程，调用者只有把IO对象的句柄交给MessageLoopForIO对象（MessagePumpForIO
 ::RegisterIOHandler），以后每当有IO对象的操作完成时，调用者就会从MessageLoopForIO收到回调通知（IOHandler::OnIOCompleted）。 MessageLoopForUI就更不用说了，因为Windows窗口是绑定到创建线程上的，所以只要有一个MessageLoopForUI对象就可以处理属于该线程的所有Windows消息，不需要额外的注册过程。
                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                          class MessagePumpForIO : public MessagePumpWin {
                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                                            class IOHandler {
                                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                             public:
                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                              virtual ~IOHandler() {}
                                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                               // This will be called once the pending IO operation associated with
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                // |context| completes. |error| is the Win32 error code of the IO operation
                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                                 // (ERROR_SUCCESS if there was no error). |bytes_transfered| will be zero
                                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                  // on error.
                                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                                   virtual void OnIOCompleted(IOContext* context, DWORD bytes_transfered,
                                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                    DWORD error) = 0;
                                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                     };
                                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                       void RegisterIOHandler(HANDLE file_handle, IOHandler* handler);
                                                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                        }
                                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                          所以，如果相关模块都可以异步调用，那么只要有两条线程（分别处理UI和IO消息）即可满足程序的需要，而主线程一般就是UI线程。 使用基于MessageLoop的消息分发机制，可以大大减少线程的数量，避免程序并发带来的各种问题。
                                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                
                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                
                                                                                                                                                                                                                               
                                                                                                                                                                                                                              
                                                                                                                                                                                                                             
                                                                                                                                                                                                                            
                                                                                                                                                                                                                           
                                                                                                                                                                                                                          
                                                                                                                                                                                                                         
                                                                                                                                                                                                                        
                                                                                                                                                                                                                       
                                                                                                                                                                                                                      
                                                                                                                                                                                                                     
                                                                                                                                                                                                                    
                                                                                                                                                                                                                   
                                                                                                                                                                                                                  
                                                                                                                                                                                                                 
                                                                                                                                                                                                                
                                                                                                                                                                                                               
                                                                                                                                                                                                              
                                                                                                                                                                                                             
                                                                                                                                                                                                            
                                                                                                                                                                                                           
                                                                                                                                                                                                          
                                                                                                                                                                                                         
                                                                                                                                                                                                        
                                                                                                                                                                                                       
                                                                                                                                                                                                      
                                                                                                                                                                                                     
                                                                                                                                                                                                    
                                                                                                                                                                                                   
                                                                                                                                                                                                  
                                                                                                                                                                                                 
                                                                                                                                                                                                
                                                                                                                                                                                               
                                                                                                                                                                                              
                                                                                                                                                                                             
                                                                                                                                                                                            
                                                                                                                                                                                           
                                                                                                                                                                                          
                                                                                                                                                                                         
                                                                                                                                                                                        
                                                                                                                                                                                       
                                                                                                                                                                                      
                                                                                                                                                                                     
                                                                                                                                                                                    
                                                                                                                                                                                   
                                                                                                                                                                                  
                                                                                                                                                                                 
                                                                                                                                                                                
                                                                                                                                                                               
                                                                                                                                                                              
                                                                                                                                                                             
                                                                                                                                                                            
                                                                                                                                                                           
                                                                                                                                                                          
                                                                                                                                                                         
                                                                                                                                                                        
                                                                                                                                                                       
                                                                                                                                                                      
                                                                                                                                                                     
                                                                                                                                                                    
                                                                                                                                                                   
                                                                                                                                                                  
                                                                                                                                                                 
                                                                                                                                                                
                                                                                                                                                               
                                                                                                                                                              
                                                                                                                                                             
                                                                                                                                                            
                                                                                                                                                           
                                                                                                                                                          
                                                                                                                                                         
                                                                                                                                                        
                                                                                                                                                       
                                                                                                                                                      
                                                                                                                                                     
                                                                                                                                                    
                                                                                                                                                   
                                                                                                                                                  
                                                                                                                                                 
                                                                                                                                                
                                                                                                                                               
                                                                                                                                              
                                                                                                                                             
                                                                                                                                            
                                                                                                                                           
                                                                                                                                          
                                                                                                                                         
                                                                                                                                        
                                                                                                                                       
                                                                                                                                      
                                                                                                                                     
                                                                                                                                    
                                                                                                                                   
                                                                                                                                  
                                                                                                                                 
                                                                                                                                
                                                                                                                               
                                                                                                                              
                                                                                                                             
                                                                                                                            
                                                                                                                           
                                                                                                                          
                                                                                                                         
                                                                                                                        
                                                                                                                       
                                                                                                                      
                                                                                                                     
                                                                                                                    
                                                                                                                   
                                                                                                                  
                                                                                                                 
                                                                                                                
                                                                                                               
                                                                                                              
                                                                                                             
                                                                                                            
                                                                                                           
                                                                                                          
                                                                                                         
                                                                                                        
                                                                                                       
                                                                                                      
                                                                                                     
                                                                                                    
                                                                                                   
                                                                                                  
                                                                                                 
                                                                                                
                                                                                               
                                                                                              
                                                                                             
                                                                                            
                                                                                           
                                                                                          
                                                                                         
                                                                                        
                                                                                       
                                                                                      
                                                                                     
                                                                                    
                                                                                   
                                                                                  
                                                                                 
                                                                                
                                                                               
                                                                              
                                                                             
                                                                            
                                                                           
                                                                          
                                                                         
                                                                        
                                                                       
                                                                      
                                                                     
                                                                    
                                                                   
                                                                  
                                                                 
                                                                
                                                               
                                                              
                                                             
                                                            
                                                           
                                                          
                                                         
                                                        
                                                       
                                                      
                                                     
                                                    
                                                   
                                                  
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


