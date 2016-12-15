---
layout: post
title: "apktool + keytool + jarsigner"
date: 2014-12-24 01:39:00 
comments: true
categories: []
tags: []
description: "apktool + keytool + jarsigner"
keywords: 
---


 
 
 
  
   
    apktool d <apk>;
   
  
 
 
  
   
    apktool b <app_path>;
   
  
 
 
  
   
    keytool -list -v -keystore xxx.keystore
   
  
 
 
  
   
   
  
 
 
  
   keytool -printcert -file CERT.RSA  打印证书
  
 
 
  
   
    keytool -genkey -v -keystore <keystore>; -alias <alias_name>; -keyalg RSA -keysize 2048 -validity 10000
    
    
   
  
 
 
  
   
    jarsigner -verbose -keystore <keystore>; -signedjar <out_signed_apk>; <src_apk>; <key_alias>;
   
  
 
 
  
   
   
  
 
 
  
  
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  keytool+jarsigner ->; jdk
 
 
  
  
 
 
 
 
  
  
 
 
 
 
 
 
  
  
 
 
  
  
 


