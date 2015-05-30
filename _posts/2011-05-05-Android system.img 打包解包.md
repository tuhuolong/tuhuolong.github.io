---
layout: post
title: Android system.img 打包解包
date: 2011-05-05 15:51:00
categories: [Android]
tags: [android, image, download, 工具]
---
system.img为yaffs2镜像

   1.解包   对system.img)的解包需要用到unyaffs这工具,下载地址：[http://code.google.com/p/unyaffs/downloads/list](http://code.google.com/p/unyaffs/downloads/list)
    下载以后和system.img放入同一个文件夹
   解包命令：

1. ./unyaffs system.img



   2.打包
   这里要用到编译yaffs2镜像制作工具：mkyaffs2image,位于android/out/host/linux-x86/bin,或者下载地址：[http://www.aleph1.co.uk/cgi-bin/viewcvs.cgi/yaffs2/](http://www.aleph1.co.uk/cgi-bin/viewcvs.cgi/yaffs2/)

   点击左下角的 Download tarball 下整个tar包，
   下载完成后，进入utils目录运行1. make

*复制代码*以后生成了mkyaffs2image和mkyaffs两个文件
   mkyaffs2image命令格式：
1. ./mkyaffs2image <要打包的目录> <生成的文件>

*
**
*
 
