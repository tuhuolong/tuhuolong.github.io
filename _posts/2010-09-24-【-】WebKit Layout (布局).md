---
layout: post
title: "【-】WebKit Layout (布局)"
date: 2010-09-24 22:23:00 
comments: true
categories: [layout]
tags: [layout]
description: "【-】WebKit Layout (布局)"
keywords: layout
---


 
  
  
  
   WebKit 在渲染页面之前，需要确定各个元素的位置、大小，而这个过程就是layout(布局)。下面，我们对layout的主要过程进行一番说明。
  
 
 
  
   
    
     
      
       
        一、FrameView::layout方法
       
       
        FrameView作为与View相关的类，其主要涉及与显示相关的内容，而其中对页面元素的布局至关重要，这也是浏览器的核心处理部分。
        
         
          我们都知道浏览器从Web服务器获得数据后，经解析会构建DOM树、Render树，然后进行布局处理，进而为渲染页面作好准备，其中的布局处理往往由FrameView::layout方法发起，让我们来具体看看其实现，一窥其主要实现过程。
         
        
       
      
     
     
     
    
    
    
   
  
 
 
  
   
    
     
      
       
        
         void FrameView::layout
         
          (
         
         bool allowSubtree
         
          )
         
         
          
           {
          
          
           if
           
            (
           
           m_midLayout
           
            )
           
           
            return
            
             ;
            
            
             
              
               /
              
              
               /
              
              Always ensure our
              
               style
              
              
               info
              
              is up-to-date
              
               .
              
              This can happen in situations where
 the layout beats any sort of
              
               style
              
              recalc update that needs to occur
              
               .
              
              
               
                
                 /
                
                
                 /
                
                进行CSSStyleSelector的更新处理，因为一旦CSS发生变化，布局的结果也可能 发生相关变化，所以在开始布局之前，需要检查CSS是否发生变化，如果有则需要作相应调整，进而可能影响Render树等。
                
                 
                  if
                  
                   (
                  
                  m_frame-
                  
                   >;
                  
                  needsReapplyStyles
                  
                   (
                  
                  
                   )
                  
                  
                   )
                  
                  
                   m_frame-
                   
                    >;
                   
                   reapplyStyles
                   
                    (
                   
                   
                    )
                   
                   
                    ;
                   
                   
                    else if
                    
                     (
                    
                    document-
                    
                     >;
                    
                    childNeedsStyleRecalc
                    
                     (
                    
                    
                     )
                    
                    
                     )
                    
                    
                     document-
                     
                      >;
                     
                     recalcStyle
                     
                      (
                     
                     
                      )
                     
                     
                      ;
                     
                     
                      
                       bool subtree
                       
                        =
                       
                       m_layoutRoot
                       
                        ;
                       
                       
                        RenderObject
                        
                         *
                        
                        root
                        
                         =
                        
                        subtree ? m_layoutRoot : document-
                        
                         >;
                        
                        renderer
                        
                         (
                        
                        
                         )
                        
                        
                         ;
                        
                        
                         if
                         
                          (
                         
                         !root
                         
                          )
                         
                         
                          {
                         
                         
                          
                           /
                          
                          
                           /
                          
                          FIXME: Do we need to set m_size here?
                          
                           m_layoutSchedulingEnabled
                           
                            =
                           
                           true
                           
                            ;
                           
                           
                            return
                            
                             ;
                            
                            
                             
                              }
                             
                             
                              
                               
                                /
                               
                               
                                /
                               
                               布局的处理可能相互嵌套，这与发起布局处理的时机相关。
                               
                                
                                 /
                                
                                
                                 /
                                
                                设置滚动条相关
                                
                                 if
                                 
                                  (
                                 
                                 m_canHaveScrollbars
                                 
                                  )
                                 
                                 
                                  {
                                 
                                 
                                  hMode
                                  
                                   =
                                  
                                  ScrollbarAuto
                                  
                                   ;
                                  
                                  
                                   vMode
                                   
                                    =
                                   
                                   ScrollbarAuto
                                   
                                    ;
                                   
                                   
                                    
                                     }
                                    
                                    else
                                    
                                     {
                                    
                                    
                                     hMode
                                     
                                      =
                                     
                                     ScrollbarAlwaysOff
                                     
                                      ;
                                     
                                     
                                      vMode
                                      
                                       =
                                      
                                      ScrollbarAlwaysOff
                                      
                                       ;
                                      
                                      
                                       
                                        }
                                       
                                       
                                        
                                         if
                                         
                                          (
                                         
                                         !subtree
                                         
                                          )
                                         
                                         
                                          {
                                         
                                         
                                          
                                           /
                                          
                                          
                                           /
                                          
                                          Now set our scrollbar state
                                          
                                           for
                                          
                                          the layout
                                          
                                           .
                                          
                                          
                                           
                                            if
                                            
                                             (
                                            
                                            
                                             body
                                            
                                            
                                             -
                                            
                                            
                                             >;
                                            
                                            hasTagName
                                            
                                             (
                                            
                                            framesetTag
                                            
                                             )
                                            
                                            
                                             &
                                            
                                            
                                             &
                                            
                                            !m_frame-
                                            
                                             >;
                                            
                                            settings
                                            
                                             (
                                            
                                            
                                             )
                                            
                                            
                                             -
                                            
                                            
                                             >;
                                            
                                            frameFlatteningEnabled
                                            
                                             (
                                            
                                            
                                             )
                                            
                                            
                                             )
                                            
                                            
                                             {
                                            
                                            
                                             
                                              /
                                             
                                             
                                              /
                                             
                                             
                                              frameset
                                             
                                             而且设置了frameFlatteningEnabled，则关闭滚动条
                                             
                                              
                                               body
                                              
                                              
                                               -
                                              
                                              
                                               >;
                                              
                                              renderer
                                              
                                               (
                                              
                                              
                                               )
                                              
                                              
                                               -
                                              
                                              
                                               >;
                                              
                                              setChildNeedsLayout
                                              
                                               (
                                              
                                              true
                                              
                                               )
                                              
                                              
                                               ;
                                              
                                              
                                               vMode
                                               
                                                =
                                               
                                               ScrollbarAlwaysOff
                                               
                                                ;
                                               
                                               
                                                hMode
                                                
                                                 =
                                                
                                                ScrollbarAlwaysOff
                                                
                                                 ;
                                                
                                                
                                                 
                                                  }
                                                 
                                                 else if
                                                 
                                                  (
                                                 
                                                 
                                                  body
                                                 
                                                 
                                                  -
                                                 
                                                 
                                                  >;
                                                 
                                                 hasTagName
                                                 
                                                  (
                                                 
                                                 bodyTag
                                                 
                                                  )
                                                 
                                                 
                                                  )
                                                 
                                                 
                                                  {
                                                 
                                                 
                                                  
                                                   /
                                                  
                                                  
                                                   /
                                                  
                                                  设置滚动条
                                                  
                                                   applyOverflowToViewport
                                                   
                                                    (
                                                   
                                                   o
                                                   
                                                    ,
                                                   
                                                   hMode
                                                   
                                                    ,
                                                   
                                                   vMode
                                                   
                                                    )
                                                   
                                                   
                                                    ;
                                                   
                                                   
                                                    
                                                     }
                                                    
                                                    
                                                     
                                                      }
                                                     
                                                     
                                                      
                                                       
                                                        /
                                                       
                                                       
                                                        /
                                                       
                                                       root往往为RenderView对象
                                                       
                                                        RenderLayer
                                                        
                                                         *
                                                        
                                                        layer
                                                        
                                                         =
                                                        
                                                        root-
                                                        
                                                         >;
                                                        
                                                        enclosingLayer
                                                        
                                                         (
                                                        
                                                        
                                                         )
                                                        
                                                        
                                                         ;
                                                        
                                                        
                                                         
                                                          m_midLayout
                                                          
                                                           =
                                                          
                                                          true
                                                          
                                                           ;
                                                          
                                                          
                                                           beginDeferredRepaints
                                                           
                                                            (
                                                           
                                                           
                                                            )
                                                           
                                                           
                                                            ;
                                                           
                                                           
                                                            
                                                             root-
                                                            
                                                            
                                                             
                                                              >;
                                                             
                                                            
                                                            
                                                             layout
                                                            
                                                            
                                                             
                                                              ();
                                                             
                                                            
                                                            
                                                             endDeferredRepaints
                                                             
                                                              (
                                                             
                                                             
                                                              )
                                                             
                                                             
                                                              ;
                                                             
                                                             
                                                              m_midLayout
                                                              
                                                               =
                                                              
                                                              false
                                                              
                                                               ;
                                                              
                                                              
                                                               
                                                                m_layoutSchedulingEnabled
                                                                
                                                                 =
                                                                
                                                                true
                                                                
                                                                 ;
                                                                
                                                                
                                                                 
                                                                  if
                                                                  
                                                                   (
                                                                  
                                                                  !subtree
                                                                  
                                                                   &
                                                                  
                                                                  
                                                                   &
                                                                  
                                                                  !static_cast
                                                                  
                                                                   (
                                                                  
                                                                  root
                                                                  
                                                                   )
                                                                  
                                                                  
                                                                   -
                                                                  
                                                                  
                                                                   >;
                                                                  
                                                                  printing
                                                                  
                                                                   (
                                                                  
                                                                  
                                                                   )
                                                                  
                                                                  
                                                                   )
                                                                  
                                                                  
                                                                   adjustViewSize
                                                                   
                                                                    (
                                                                   
                                                                   
                                                                    )
                                                                   
                                                                   
                                                                    ;
                                                                   
                                                                   
                                                                    
                                                                     
                                                                      /
                                                                     
                                                                     
                                                                      /
                                                                     
                                                                     Now update the positions of all layers
                                                                     
                                                                      .
                                                                     
                                                                     
                                                                      
                                                                       /
                                                                      
                                                                      
                                                                       /
                                                                      
                                                                      对当前Render树布局完后，设置RenderLayer树的布局信息，其中m_doFullRepaint描述是否需要发起渲染处理。
                                                                      
                                                                       
                                                                        beginDeferredRepaints
                                                                        
                                                                         (
                                                                        
                                                                        
                                                                         )
                                                                        
                                                                        
                                                                         ;
                                                                        
                                                                        
                                                                         layer-
                                                                         
                                                                          >;
                                                                         
                                                                         updateLayerPositions
                                                                         
                                                                          (
                                                                         
                                                                         
                                                                          .
                                                                         
                                                                         
                                                                          .
                                                                         
                                                                         
                                                                          .
                                                                         
                                                                         
                                                                          )
                                                                         
                                                                         
                                                                          ;
                                                                         
                                                                         
                                                                          endDeferredRepaints
                                                                          
                                                                           (
                                                                          
                                                                          
                                                                           )
                                                                          
                                                                          
                                                                           ;
                                                                          
                                                                          
                                                                           
                                                                            
                                                                             /
                                                                            
                                                                            
                                                                             /
                                                                            
                                                                            设置快速blit
                                                                            
                                                                             setCanBlitOnScroll
                                                                             
                                                                              (
                                                                             
                                                                             !useSlowRepaints
                                                                             
                                                                              (
                                                                             
                                                                             
                                                                              )
                                                                             
                                                                             
                                                                              )
                                                                             
                                                                             
                                                                              ;
                                                                             
                                                                             
                                                                              
                                                                               
                                                                                /
                                                                               
                                                                               
                                                                                /
                                                                               
                                                                               因为在布局的过程中，可能进一步获得网页数据，则需要继续布局处理。
                                                                               
                                                                                if
                                                                                
                                                                                 (
                                                                                
                                                                                !m_postLayoutTasksTimer
                                                                                
                                                                                 .
                                                                                
                                                                                isActive
                                                                                
                                                                                 (
                                                                                
                                                                                
                                                                                 )
                                                                                
                                                                                
                                                                                 )
                                                                                
                                                                                
                                                                                 {
                                                                                
                                                                                
                                                                                 
                                                                                  /
                                                                                 
                                                                                 
                                                                                  /
                                                                                 
                                                                                 Calls resumeScheduledEvents
                                                                                 
                                                                                  (
                                                                                 
                                                                                 
                                                                                  )
                                                                                 
                                                                                 
                                                                                  performPostLayoutTasks
                                                                                  
                                                                                   (
                                                                                  
                                                                                  
                                                                                   )
                                                                                  
                                                                                  
                                                                                   ;
                                                                                  
                                                                                  
                                                                                   
                                                                                    if
                                                                                    
                                                                                     (
                                                                                    
                                                                                    !m_postLayoutTasksTimer
                                                                                    
                                                                                     .
                                                                                    
                                                                                    isActive
                                                                                    
                                                                                     (
                                                                                    
                                                                                    
                                                                                     )
                                                                                    
                                                                                    
                                                                                     &
                                                                                    
                                                                                    
                                                                                     &
                                                                                    
                                                                                    needsLayout
                                                                                    
                                                                                     (
                                                                                    
                                                                                    
                                                                                     )
                                                                                    
                                                                                    
                                                                                     )
                                                                                    
                                                                                    
                                                                                     {
                                                                                    
                                                                                    
                                                                                     
                                                                                      /
                                                                                     
                                                                                     
                                                                                      /
                                                                                     
                                                                                     Post-layout widget updates or an event handler made us need layout again
                                                                                     
                                                                                      .
                                                                                     
                                                                                     Lay out again
                                                                                     
                                                                                      ,
                                                                                     
                                                                                     but this time
                                                                                     
                                                                                      defer
                                                                                     
                                                                                     widget updates and event dispatch until after we return
                                                                                     
                                                                                      .
                                                                                     
                                                                                     
                                                                                      m_postLayoutTasksTimer
                                                                                      
                                                                                       .
                                                                                      
                                                                                      startOneShot
                                                                                      
                                                                                       (
                                                                                      
                                                                                      0
                                                                                      
                                                                                       )
                                                                                      
                                                                                      
                                                                                       ;
                                                                                      
                                                                                      
                                                                                       pauseScheduledEvents
                                                                                       
                                                                                        (
                                                                                       
                                                                                       
                                                                                        )
                                                                                       
                                                                                       
                                                                                        ;
                                                                                       
                                                                                       
                                                                                        layout
                                                                                        
                                                                                         (
                                                                                        
                                                                                        
                                                                                         )
                                                                                        
                                                                                        
                                                                                         ;
                                                                                        
                                                                                        
                                                                                         
                                                                                          }
                                                                                         
                                                                                         
                                                                                          
                                                                                           }
                                                                                          
                                                                                          else
                                                                                          
                                                                                           {
                                                                                          
                                                                                          
                                                                                           resumeScheduledEvents
                                                                                           
                                                                                            (
                                                                                           
                                                                                           
                                                                                            )
                                                                                           
                                                                                           
                                                                                            ;
                                                                                           
                                                                                           
                                                                                            
                                                                                             }
                                                                                            
                                                                                            
                                                                                             
                                                                                              }
                                                                                             
                                                                                            
                                                                                           
                                                                                          
                                                                                         
                                                                                        
                                                                                       
                                                                                      
                                                                                     
                                                                                    
                                                                                   
                                                                                  
                                                                                 
                                                                                
                                                                               
                                                                              
                                                                             
                                                                            
                                                                           
                                                                          
                                                                         
                                                                        
                                                                       
                                                                      
                                                                     
                                                                    
                                                                   
                                                                  
                                                                 
                                                                
                                                               
                                                              
                                                             
                                                            
                                                           
                                                          
                                                         
                                                        
                                                       
                                                      
                                                     
                                                    
                                                   
                                                  
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
  
  
 
 
  
   
    FrameView::layout方法，简单的说来就是发起对Render树中的每一个节点按照从父节点到子节点的方式进行x、y、width、height计算，当每一个树节点的位置及大小确定之后就可以进行后面的渲染。
    
     
      FrameView::layout往往会调用Render树根的layout方法即RenderView::layout。
      
       
        
         二、RenderView::layout方法
        
        
        
       
      
     
    
   
   
   
  
  
   
    
     
      
       
        
         
          void RenderView::layout
          
           (
          
          
           )
          
          
           
            {
           
           
            if
            
             (
            
            printing
            
             (
            
            
             )
            
            
             )
            
            
             m_minPrefWidth
             
              =
             
             m_maxPrefWidth
             
              =
             
             m_width
             
              ;
             
             
              
               
                /
               
               
                /
               
               Use calcWidth
               
                /
               
               
                Height
               
               to get the new
               
                width
               
               
                /
               
               
                height
               
               
                ,
               
               since this will take the full
               
                page
               
               zoom factor into account
               
                .
               
               
                bool relayoutChildren
                
                 =
                
                !printing
                
                 (
                
                
                 )
                
                
                 &
                
                
                 &
                
                
                 (
                
                !m_frameView
                
                 |
                
                
                 |
                
                m_width !
                
                 =
                
                viewWidth
                
                 (
                
                
                 )
                
                
                 |
                
                
                 |
                
                m_height !
                
                 =
                
                viewHeight
                
                 (
                
                
                 )
                
                
                 )
                
                
                 ;
                
                
                 if
                 
                  (
                 
                 relayoutChildren
                 
                  ) {
                 
                
               
              
             
            
           
          
         
        
       
      
      
       
        
         
          
           setChildNeedsLayout(true, false);
           
            for (RenderObject* child = firstChild(); child; child = child->;nextSibling()) {
            
             if (child->;style()->;height().isPercent() || child->;style()->;minHeight().isPercent() || child->;style()->;maxHeight().isPercent())
             
              child->;setChildNeedsLayout(true, false);
              
               }
               
               
              
             
            
           
          
         
        
       
      
      
       
        
         
          
           }
          
          
           
            ASSERT
            
             (
            
            !m_layoutState
            
             )
            
            
             ;
            
            
             LayoutState state
             
              ;
             
             
              
               /
              
              
               /
              
              FIXME: May be better to push
              
               a
              
              clip and avoid issuing offscreen repaints
              
               .
              
              
               state
               
                .
               
               m_clipped
               
                =
               
               false
               
                ;
               
               
                m_layoutState
                
                 =
                
                
                 &
                
                state
                
                 ;
                
                
                 
                  if
                  
                   (
                  
                  needsLayout
                  
                   (
                  
                  
                   )
                  
                  
                   )
                  
                  
                   RenderBlock::layout
                   
                    (
                   
                   
                    )
                   
                   
                    ;
                   
                   
                    /
                   
                   
                    /
                   
                   类继承的好处，直接调用父类的layout
                   
                    
                     // Reset overflow and then replace it with docWidth and docHeight.
                     
                      m_overflow.clear();
                      
                       addLayoutOverflow(IntRect(0, 0, docWidth(), docHeight()));
                       
                        
                         ASSERT
                         
                          (
                         
                         m_layoutStateDisableCount
                         
                          =
                         
                         
                          =
                         
                         0
                         
                          )
                         
                         
                          ;
                         
                         
                          ASSERT
                          
                           (
                          
                          m_layoutState
                          
                           =
                          
                          
                           =
                          
                          
                           &
                          
                          state
                          
                           )
                          
                          
                           ;
                          
                          
                           m_layoutState
                           
                            =
                           
                           0
                           
                            ;
                           
                           
                            setNeedsLayout
                            
                             (
                            
                            false
                            
                             )
                            
                            
                             ;
                            
                            
                             
                              }
                             
                             
                              
                               void RenderBlock::layout
                               
                                (
                               
                               
                                )
                               
                               
                                
                                 {
                                
                                
                                 
                                  /
                                 
                                 
                                  /
                                 
                                 Update our first letter
                                 
                                  info
                                 
                                 now
                                 
                                  .
                                 
                                 
                                  updateFirstLetter
                                  
                                   (
                                  
                                  
                                   )
                                  
                                  
                                   ;
                                  
                                  
                                   
                                    /
                                   
                                   
                                    /
                                   
                                   
                                    Table
                                   
                                   cells call layoutBlock directly
                                   
                                    ,
                                   
                                   so don
                                   
                                    't add any logic here. Put code into layoutBlock().
                                    
                                     
                                      layoutBlock
                                     
                                     (false);
                                     
                                      
                                       // It'
                                      
                                     
                                    
                                   
                                   
                                    s
                                   
                                   safe to check
                                   
                                    for
                                   
                                   control clip here
                                   
                                    ,
                                   
                                   since controls can never be
                                   
                                    table
                                   
                                   cells
                                   
                                    .
                                   
                                   If we have a lightweight clip, there can never be any overflow from children.
                                   
                                    if (hasControlClip() && m_overflow)
                                    
                                     clearLayoutOverflow();
                                     
                                      
                                       }
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
  
   
    
     
      
       三、RenderBlock::layoutBlock方法
      
      
      
     
    
    
     layoutBlock 会分成两个分支：
     
      
       
        layoutInlineChildren
       
       
        和
       
      
     
     
      
       
        layoutBlockChildren
        
         
         
        
       
      
     
    
   
  
  
   
    
     
      
       
        
         
          void RenderBlock::layoutBlock
          
           (
          
          bool relayoutChildren
          
           )
          
          
           
            {
           
           
            
             ...
            
            
             calcWidth
             
              (
             
             
              )
             
             
              ;
             
             
              /
             
             
              /
             
             先计算宽度
             
              calcColumnWidth
              
               (
              
              
               )
              
              
               ;
              
              
               
               
              
             
            
           
          
         
        
       
      
      
       
        
         
          m_overflow.clear();
          
          
         
        
       
      
      
       
        
         
          if
          
           (
          
          oldWidth !
          
           =
          
          width()
          
           |
          
          
           |
          
          oldColumnWidth !
          
           =
          
          desiredColumnWidth
          
           (
          
          
           )
          
          
           )
          
          
           relayoutChildren
           
            =
           
           true
           
            ;
           
           
            clearFloats
            
             (
            
            
             )
            
            
             ;
            
            
             
              int previousHeight
              
               =
              
              height()
              
               ;
              
              
               setHeight(0);
               
                
                 
                  /
                 
                 
                  /
                 
                 这就是在布局基本概念中提到的Block-level元素的子节点要么是Block-level元素要么为Inline-level元素。
                 
                  if
                  
                   (
                  
                  childrenInline
                  
                   (
                  
                  
                   )
                  
                  
                   )
                  
                  
                   
                    layoutInlineChildren
                   
                   
                    (
                   
                   relayoutChildren
                   
                    ,
                   
                   repaintTop
                   
                    ,
                   
                   repaintBottom
                   
                    )
                   
                   
                    ;
                   
                   
                    else
                    
                     
                      layoutBlockChildren
                     
                     
                      (
                     
                     relayoutChildren
                     
                      ,
                     
                     maxFloatBottom
                     
                      )
                     
                     
                      ;
                     
                     
                      
                       
                        /
                       
                       
                        /
                       
                       Expand our intrinsic
                       
                        height
                       
                       to encompass floats
                       
                        .
                       
                       
                        int toAdd
                        
                         =
                        
                        borderBottom
                        
                         (
                        
                        
                         )
                        
                        
                         +
                        
                        paddingBottom
                        
                         (
                        
                        
                         )
                        
                        
                         +
                        
                        horizontalScrollbarHeight
                        
                         (
                        
                        
                         )
                        
                        
                         ;
                        
                        
                         if
                         
                          (
                         
                         floatBottom
                         
                          (
                         
                         
                          )
                         
                         
                          >;
                         
                         
                          (
                         
                         m_height - toAdd
                         
                          )
                         
                         
                          &
                         
                         
                          &
                         
                         
                          (
                         
                         isInlineBlockOrInlineTable
                         
                          (
                         
                         
                          )
                         
                         
                          |
                         
                         
                          |
                         
                         isFloatingOrPositioned
                         
                          (
                         
                         
                          )
                         
                         
                          |
                         
                         
                          |
                         
                         hasOverflowClip
                         
                          (
                         
                         
                          )
                         
                         
                          |
                         
                         
                          |
                         
                         
                          
                           (
                          
                          parent
                          
                           (
                          
                          
                           )
                          
                          
                           &
                          
                          
                           &
                          
                          parent
                          
                           (
                          
                          
                           )
                          
                          
                           -
                          
                          
                           >;
                          
                          isFlexibleBox
                          
                           (
                          
                          
                           )
                          
                          
                           |
                          
                          
                           |
                          
                          m_hasColumns
                          
                           )
                          
                          
                           )
                          
                          
                           )
                          
                          
                           setHeight(floatBottom
                           
                            (
                           
                           
                            )
                           
                           
                            +
                           
                           toAdd
                           
                            );
                           
                           
                            
                             
                              /
                             
                             
                              /
                             
                             Now lay out our columns within this intrinsic
                             
                              height
                             
                             
                              ,
                             
                             since they can slightly affect the intrinsic
                             
                              height
                             
                             as we adjust
                             
                              for
                             
                             clean column breaks
                             
                              .
                             
                             
                              int singleColumnBottom
                              
                               =
                              
                              layoutColumns
                              
                               (
                              
                              
                               )
                              
                              
                               ;
                              
                              
                               
                                
                                 /
                                
                                
                                 /
                                
                                Calculate our new
                                
                                 height
                                
                                
                                 .
                                
                                
                                 /
                                
                                
                                 /
                                
                                布局完子节点后确定父节点高度
                                
                                 int oldHeight
                                 
                                  =
                                 
                                 height()
                                 
                                  ;
                                 
                                 
                                  calcHeight
                                  
                                   (
                                  
                                  
                                   )
                                  
                                  
                                   ;
                                  
                                  
                                   
                                    if
                                    
                                     (
                                    
                                    previousHeight !
                                    
                                     =
                                    
                                    height()
                                    
                                     )
                                    
                                    
                                     relayoutChildren
                                     
                                      =
                                     
                                     true
                                     
                                      ;
                                     
                                     
                                      
                                       
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
      
       
        
         
          
           /
          
          
           /
          
          另外布局属性为Fixed和absolute的元素
          
           layoutPositionedObjects
           
            (
           
           relayoutChildren
           
            |
           
           
            |
           
           isRoot
           
            (
           
           
            )
           
           
            )
           
           
            ;
           
           
           
          
         
        
       
      
      
       
        
         
          // Update our scroll information if we're overflow:auto/scroll/hidden now that we know if we overflow or not.
          
           updateScrollInfoAfterLayout();
           
           
          
         
        
       
      
      
       
        
         
          // Repaint with our new bounds if they are different from our old bounds.
          
           bool didFullRepaint = repainter.repaintAfterLayout();
           
            if (!didFullRepaint && repaintTop != repaintBottom && (style()->;visibility() == VISIBLE || enclosingLayer()->;hasVisibleContent())) {
            
             // 设置repaintRect
             
             
            
           
          
         
        
       
      
      
       
        // Make sure the rect is still non-empty after intersecting for overflow above
        
         if (!repaintRect.isEmpty()) {
         
          
           repaintRectangle
          
          (repaintRect); // We need to do a partial repaint of our content.
          
           if (hasReflection())
           
            
             repaintRectangle
            
            (reflectedRect(repaintRect));
            
             }
             
              }
              
               setNeedsLayout(false);
               
                }
               
              
             
            
           
          
         
        
       
      
     
    
   
  
  
   
    
     
      
       四、RenderBlock::layoutBlockChildren方法
      
      
       
       
      
     
    
   
   
   
  
  
   
    
     
      
       
        
         
          void RenderBlock::layoutBlockChildren
          
           (
          
          bool relayoutChildren
          
           ,
          
          int
          
           &
          
          maxFloatBottom
          
           )
          
          
           
            {
           
           
            int top
            
             =
            
            borderTop
            
             (
            
            
             )
            
            
             +
            
            paddingTop
            
             (
            
            
             )
            
            
             ;
            
            
             int bottom
             
              =
             
             borderBottom
             
              (
             
             
              )
             
             
              +
             
             paddingBottom
             
              (
             
             
              )
             
             
              +
             
             horizontalScrollbarHeight
             
              (
             
             
              )
             
             
              ;
             
             
             
            
           
          
         
        
       
      
      
       
        
         
          
          
         
        
       
      
      
       
        
         
          // Fieldsets need to find their legend and position it inside the border of the object.
          
           // The legend then gets skipped during normal layout.
           
            RenderObject* legend = layoutLegend(relayoutChildren);
            
            
           
          
         
        
       
      
      
       
        
         
          
           
            /
           
           
            /
           
           遍历子节点
          
         
        
       
      
      
       
        
         
          RenderBox* next = firstChildBox();
          
           while (next) {
           
            RenderBox* child = next;
            
             next = child->;nextSiblingBox();
            
           
          
         
        
       
      
      
       
        
         
          
           if (legend == child)
           
            continue; // Skip the legend, since it has already been positioned up in the fieldset's border.
            
             // Make sure we layout children if they need it.
             
              // FIXME: Technically percentage height objects only need a relayout if their percentage isn't going to be turned into
              
               // an auto value.  Add a method to determine this, so that we can avoid the relayout.
               
                if (relayoutChildren || ((child->;style()->;height().isPercent() || child->;style()->;minHeight().isPercent() || child->;style()->;maxHeight().isPercent()) && !isRenderView()))
                
                 child->;setChildNeedsLayout(true, false);
                 
                  
                   // If relayoutChildren is set and we have percentage padding, we also need to invalidate the child's pref widths.
                   
                    if (relayoutChildren && (child->;style()->;paddingLeft().isPercent() || child->;style()->;paddingRight().isPercent()))
                    
                     child->;setPrefWidthsDirty(true, false);
                     
                      
                       // Handle the four types of special elements first.  These include positioned content, floating content, compacts and
                       
                        // run-ins.  When we encounter these four types of objects, we don't actually lay them out as normal flow blocks.
                        
                         if (handleSpecialChild(child, marginInfo))
                         
                          continue;
                          
                           
                            // Lay out the child.
                            
                             
                              layoutBlockChild
                             
                             (child, marginInfo, previousFloatBottom, maxFloatBottom);
                             
                              }
                              
                               
                                // Now do the handling of the bottom of the block, adding in our bottom border/padding and
                                
                                 // determining the correct collapsed bottom margin information.
                                 
                                  handleBottomOfBlock(top, bottom, marginInfo);
                                  
                                   }
                                   
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
  
   
    layoutBlockChild 方法就比较复杂了，具体可以自己看代码。
    
     
      
       
        五、RenderBlock::layoutInlineChildren方法
       
       
        这个方法相当复杂，其作用就是布局文字、图像等，对文字行高确定、断行等处理，同时还包括 文字从左到右或从右到左的布局处理。具体可以参考bidi.cpp中的源码实现。
        
         
          
           六、调用FrameView::layout方法的时机
          
          
           由于从Web服务器获取的网页数据不可能一次性完成，往往需要边获取数据，边布局，然后渲染，这样才可能获得良好的用户感受。
           
            
             所以一旦获得主要数据如css数据及body等标签后，就可以开始布局，布局完后会根据当前条件决定是否将布局的数据渲染出来，或者继续布局处理后来获取的数据，这样也增加了布局处理过程的复杂度。
             
              
               而调用layout方法的时机也至关重要，因为layout本身就可能需要花费大量的时间如
               
                layoutBlockChildren、
               
               
                layoutInlineChildren
               
               等处理，其往往与网页的内容有关，而网页的内容却由网页开发者来确定，对浏览器来讲是千变万化的，这就对layout方法的实现及调用时机提出更高的要求，同时确定了其复杂性。
               
                
                 调用layout的时机主要有获得一定DOM文档数据后调用Document::updateLayout()、需要重新使用CSS数据时调用Document::recalcStyle()、改变窗口大小后调用Frame::forceLayout()等来实现。。。
                 
                  
                   
                    七、总结
                   
                   
                    其实WebKit涉及网页布局方面的layout方法蛮复杂的，如其他RenderObject子类也会根据自身情况重载实现layout，还有对float、fixed、absolute、inline元素等的处理，但其主要逻辑就象上面所提，这里只是汇总一下主要流程及概念，针对每一个具体标签或RenderObject的布局实现则需要更深一步的了解，希望大家能对了解WebKit的网页布局过程有一个清晰而准确的认识。。
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


