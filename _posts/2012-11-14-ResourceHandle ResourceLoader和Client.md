---
layout: post
title: "ResourceHandle ResourceLoader和Client"
date: 2012-11-14 20:46:00 
comments: true
categories: []
tags: []
description: "ResourceHandle ResourceLoader和Client"
keywords: 
---


 
  
   ResourceHandle
   
   
   
    |
    
     
      ResourceHandleClient :
      
       ResourceLoader
       
        MainResourceLoader
       
      
     
     
      
       SubresourceLoader
      
      
       |
       
        
         SubresourceLoaderClient  :
         
          CachedResourceRequest
         
        
        
         
         
         
          IconLoader
          
           
            
             
              DocumentThreadableLoader
             
            
           
          
          
           
           
           
            |
           
           
            
             
             
             
              ThreadableLoaderClient : XMLHttpRequest
              
              
              
               
               
              
             
            
           
          
         
         
          
          
          
           
           
          
         
        
       
      
     
    
   
  
  
  
 


