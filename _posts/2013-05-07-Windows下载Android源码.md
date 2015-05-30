---
layout: post
title: Windows下载Android源码
date: 2013-05-07 15:27:00
categories: [Android]
tags: []
---

下载msysgit，安装
官方下载：[http://code.google.com/p/msysgit/downloads/list](http://code.google.com/p/msysgit/downloads/list)，
 
打开Git Bash，执行命令
	cd D:
	git clone https://android.googlesource.com/platform/manifest.git
 ![](http://pic002.cnblogs.com/images/2011/364051/2011122514291766.jpg)
输入命令，切换到manifest目录
	cd manifest
git tag 列出android各个分支版本
	git tag
下载android-4.0.4_r2.1 manifest
	git checkout android-4.0.4_r2.1

 
执行脚本 download-src.py
	import xml.dom.minidomimport osfrom subprocess import call#downloaded source pathrootdir = "D:/android-source/android-4.0.4_r2.1"#git program pathgit = "E:/Program Files/Git/bin/git.exe"dom = xml.dom.minidom.parse("D:/manifest/default.xml")
	root = dom.documentElement
	
	prefix = git + " clone https://android.googlesource.com/"suffix = ".git"ifnot os.path.exists(rootdir):
	    os.mkdir(rootdir)for node in root.getElementsByTagName("project"):
	    os.chdir(rootdir)
	    d = node.getAttribute("path")
	    last = d.rfind("/")
	    if last != -1:
	        d = rootdir + "/" + d[:last]
	        ifnot os.path.exists(d):
	            os.makedirs(d)
	        os.chdir(d)
	    cmd = prefix + node.getAttribute("name") + suffix
	    call(cmd)

