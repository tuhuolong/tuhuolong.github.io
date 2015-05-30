---
layout: post
title: jarsigner签名过程
date: 2012-12-28 13:53:00
categories: [Android, Java]
tags: []
---
【1】Hash: 每一个文件的摘要(Hash值)，并写入MANIFEST.MF文件
【2】二次Hash: (MANIFEST.MF文件的摘要(Hash值))+MANIFEST.MF文件每一个项(如下)的摘要(Hash值)
 写入.SF文件
------------------------
Name: xxxx
SHA1-Digest: xxxx


------------------------
【3】签名: 签名信息+私钥+证书链+(SF文件的签名) 写入签名文件.DSA/.RSA文件 
