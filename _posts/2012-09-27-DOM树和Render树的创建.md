---
layout: post
title: "DOM树和Render树的创建"
date: 2012-09-27 17:07:00 
comments: true
categories: []
tags: []
description: "DOM树和Render树的创建"
keywords: 
---


 
  
   解析HTML Token
  
  
   
    - - - - HTMLTreeBuilder::processToken
   
   
    解析Token
   
   
    
     - - - -         HTMLTreeBuilder::processStartTag   （
    
    
     解析到开始标签
    
    
     ）
    
    
     
      - - - -                 HTMLConstructionSite::insertHTMLHtmlStartTagBeforeHTML
     
     
      
       - - - -
       
        HTMLConstructionSite::attach
       
      
      
       
        - - - -                                 ContainerNode::parserAddChild
       
       
        
         - - - -
         
          Element::attach HTML
         
         
          - - - -                                 。。。。。。
         
        
        
         
          - - - -                 HTMLConstructionSite::insertHTMLHeadElement  head
         
         
          
           - - - -                         HTMLConstructionSite::createHTMLElement  head
          
          
           
            - - - -                                 HTMLElementFactory::createHTMLElement  head
            
             
              - - - -
              
               HTMLConstructionSite::attach
              
             
             
              
               - - - -
               
                ContainerNode::parserAddChild
               
              
              
               
                - - - -
                
                 Element::attach  HEAD
                
               
              
             
            
           
           
            
             - - - -                                 。。。。。。
            
            
             
              - - - -                 HTMLConstructionSite::insertHTMLBodyElement  body
              
               - - - -                         HTMLConstructionSite::createHTMLElement  body
               
                - - - -                                 HTMLElementFactory::createHTMLElement  body
                
                 
                  - - - -
                  
                   HTMLConstructionSite::attach
                  
                  
                   - - - -
                   
                    ContainerNode::parserAddChild
                   
                   
                    - - - -
                    
                     Element::attach  BODY
                    
                   
                  
                 
                 
                  - - - -                                 。。。。。。
                  
                   
                    - - - -                 HTMLConstructionSite::insertHTMLElement  div
                   
                   
                    
                     - - - -                         HTMLConstructionSite::createHTMLElement  div
                     
                      创建Node结点
                     
                    
                    
                     
                      - - - -                                 HTMLElementFactory::createHTMLElement  div
                      
                       
                        - - - -                         HTMLConstructionSite::attach
                       
                       
                        
                         - - - -                                 ContainerNode::parserAddChild
                         
                          加入DOM树
                         
                        
                        
                         
                          - - - -
                          
                           Element::attach DIV
                           
                            attach
                            
                             渲染
                            
                           
                           
                            
                             - - - -                                         Node::createRendererIfNeeded  DIV
                            
                            
                             
                              - - - -                                                 Node::createRendererAndStyle  DIV
                              
                               - - - -                                                         Node::styleForRenderer DIV
                               
                                - - - -                                                                 CSSStyleSelector::styleForElement  DIV
                                
                                 CSS选择器创建RenderStyle
                                
                                
                                 - - - -                                                                         RenderStyle::create
                                 
                                  - - - -                                                         Node*->;createRenderer  DIV
                                  
                                   创建RenderObject
                                  
                                  
                                   
                                    - - - -                                                         Node::setRenderer  DIV
                                    
                                     保存
                                    
                                    RenderObejct
                                    
                                     指针
                                    
                                    ->;Node结点
                                    
                                     
                                      - - - -                                                         RenderObject::setAnimatableStyle  DIV
                                      
                                       - - - -                                                                 RenderObject::setStyle       保存RenderStyle指针
                                       
                                        ->;RenderObject结点
                                       
                                       
                                        
                                         - - - -
                                         
                                          parentNode.RenderObject::addChild
                                          
                                           加入Render树
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                         
                          - - - -
                          
                           HTMLConstructionSite::insertHTMLElement    。。
                          
                          
                           - - - -                         。。。。。。
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
             
              
               - - - -         HTMLTreeBuilder::processCharacter   （
              
              
               解析到Text
              
              
               ）
              
              
               
                - - - -                 HTMLConstructionSite::insertTextNode
                
                 
                  - - - -                         Text::create
                  
                   创建Text结点
                  
                  
                   
                    - - - -                         HTMLConstructionSite::attachAtSite
                   
                   
                    
                     - - - -                                 ContainerNode::parserAddChild
                     
                      加入DOM树
                     
                    
                    
                     
                      - - - -
                      
                       Text::attach
                       
                        attach
                        
                         渲染
                        
                       
                      
                     
                     
                      
                       
                        - - - -                                         Node::createRendererIfNeeded  #text
                       
                       
                        
                         - - - -                                                 Node::createRendererAndStyle
                         
                          #text
                         
                         
                          - - - -                                                         Node::styleForRenderer
                          
                           #text
                          
                          
                           - - - -                                                                 parentNode.RenderObject.RenderStyle
                           
                            继承父节点的RenderStyle
                           
                           
                            - - - -                                                         Node*->;createRenderer
                            
                             #text
                            
                            创建RenderObject
                            
                             
                              - - - -                                                         Node::setRenderer
                              
                               #text
                              
                              保存RenderObejct指针->;Node结点
                              
                               
                                - - - -                                                         RenderObject::setAnimatableStyle
                                
                                 #text
                                
                                
                                 - - - -                                                                 RenderObject::setStyle       保存RenderStyle指针 ->;RenderObject结点
                                 
                                  - - - -                                                parentNode.RenderObject::addChild    加入Render树
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                        
                         
                          
                           - - - -         HTMLTreeBuilder::processComment
                           
                            （
                           
                           
                            解析到注释
                           
                           
                            ）
                           
                           
                            
                             - - - -
                             
                              - - - -
                              
                               - - - -
                               
                                - - - -
                                
                                 - - - -
                                 
                                  
                                   - - - -
                                   
                                    - - - -
                                    
                                     - - - -
                                     
                                      - - - -
                                      
                                       - - - -
                                       
                                        
                                         - - - -
                                         
                                          - - - -
                                          
                                           - - - -
                                           
                                            - - - -
                                            
                                             - - - -
                                             
                                              
                                               - - - -
                                               
                                                - - - -
                                                
                                                 - - - -
                                                 
                                                  - - - -
                                                  
                                                   - - - -
                                                   
                                                    
                                                     - - - -
                                                     
                                                      - - - -
                                                      
                                                       - - - -
                                                       
                                                        - - - -
                                                        
                                                         - - - -
                                                         
                                                          
                                                           - - - -
                                                           
                                                            - - - -
                                                            
                                                             - - - -
                                                             
                                                              - - - -
                                                              
                                                               - - - -
                                                              
                                                             
                                                            
                                                           
                                                          
                                                         
                                                        
                                                       
                                                      
                                                     
                                                    
                                                   
                                                  
                                                 
                                                
                                               
                                              
                                             
                                            
                                           
                                          
                                         
                                        
                                       
                                      
                                     
                                    
                                   
                                  
                                 
                                
                               
                              
                             
                            
                           
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


