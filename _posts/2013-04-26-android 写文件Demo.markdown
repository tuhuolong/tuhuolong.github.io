---
layout: post
title: "android 写文件Demo"
date: 2013-04-26 12:12:00 
comments: true
categories: []
tags: []
description: "android 写文件Demo"
keywords: 
---


 
  File file = new File("/mnt/sdcard/tuhuolong.txt");
  
   FileOutputStream outputStream = null;
   
    try {
    
     
     
     outputStream = new FileOutputStream(file,true);
     
      
      
      int len = 0;
      
       
       
       byte[] buf = new byte[1024];
       
        
        
        InputStream inputstream =entity.getContent();
        
         
         
         while ((len = inputstream.read(buf)) >; 0) {
         
          
          
          outputStream.write(buf, 0, len);
          
           
           
           }
           
            
            
            outputStream.close();
            
             } catch (Exception e) {}
             
              
               
                
                 
                  File file = new File("/mnt/sdcard/tuhuolong.txt");
                  
                   FileOutputStream outputStream = null;
                   
                    String result = "";
                    
                     try {
                     
                      outputStream = new FileOutputStream(file,true);
                      
                       byte[] buf = result.getBytes();
                       
                        int len = buf.length;
                        
                         outputStream.write(buf, 0, len);
                         
                          outputStream.close();
                          
                           } catch (Exception e) {}
                          
                         
                        
                       
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 
 
 


