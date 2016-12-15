---
layout: post
title: "The hierarchy of the type is inconsistent"
date: 2010-12-22 09:24:00 
comments: true
categories: [hierarchy]
tags: [hierarchy]
description: "The hierarchy of the type is inconsistent"
keywords: hierarchy
---


 
  
  
 
 
  
   These errors happend because some
   
    
     
      interface
     
    
   
   /class in the 
hierarchy cannot be resolve.
   
   For example: the error is occur in your 
class - class x, x inherits y, and in turn, y inherits z. However, the
   
    
     
      compiler
     
    
    
     
     
     
      
       
      
     
    
   
   cannot resolve z (in above error is EventManager), because z 
is belong to a library that is not included.
   
   Therefore, you have to 
add package containing z to the classpath/ or project's
   
    
     
      Java
     
    
   
   Build Path (if you are using eclipse).
  
 


