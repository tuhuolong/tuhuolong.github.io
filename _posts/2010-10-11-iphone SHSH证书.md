---
layout: post
title: iphone SHSH证书
date: 2010-10-11 00:27:00
categories: [iphone]
tags: [iphone, 服务器, apple, 百度, 手机, windows]
---
扫盲：SHSH是简称，全称应该为 ECID SHSH或者 SHSH Blob 或者 ECID SHSH Blob 。
    在说SHSH之前，我们先说什么是ECID ，ECID的英文全名是 Exclusive Chip ID ，也就是每一台[iphone](http://mobile.zol.com.cn/topic/491606.html)、Ipod、Ipad都根据自己的芯片有一个唯一的识别码。这个可以通过在[手机](http://detail.zol.com.cn/cell_phone_index/subcate57_list_1.html)查看、查看注册表，用软件读取等多种手段得到。
    然后我们说为什么苹果要做SHSH，以前的智能手机，大家刷了新版，不好用，就再刷回低版本。以前还无所谓，在Apple出了Iphone之后，这个问题就严重了。[苹果](http://bible.younet.com/typelist.php?type=topic&keyword=苹果)的新版本一般都会屏蔽旧版本的越狱，苹果不想让大家越狱，当然苹果就不想让大家再刷回去，所以用了一种新的验证技术，这就是SHSH了。SHSH是根据每台机的ECID和当前最新的版本经过复杂运算而得出的一个签名文件，SHSH不是存在你的机子上的，是存在Apple的服务器上的。当你刷机的时候，Apple会连上服务器来验证当前你的刷机版本和ECID所产生的SHSH和服务器上的是否匹配，如果不匹配，则不能刷机。
    那么，明白了SHSH是怎么来的，你就会很容易理解以下的问题：
    为什么新版本的程序一出来，就不能备份旧版本的SHSH了，因为一旦出了新版本，所有的SHSH都已经更新了，而更新的目的，就是防止你刷回低版本。如果你不想刷回旧版本，那么Shsh对你是没有任何用处的。无论你现在用的4.0还是4.0.1，当你备份你的SHSH的时候，你都只能备份到当前最新的版本的SHSH文件。
    其实不管验证技术有多高明，我们明白了验证机制，我们就可以为所欲为了，自己用一台服务器来代替[苹果](http://bible.younet.com/typelist.php?type=topic&keyword=苹果)（通过修改Hosts文件轻易实现），把自己以前备份的SHSH文件放在服务器上，这样就可以刷回以前的老版本了。（这一段是百度来的）

    步入正题
    请确定[电脑](http://detail.zol.com.cn/desktop_pc_index/subcate27_list_1.html)已按照iTunes客户端，windows系统下。
    请确定你的iPhone4已经成功越狱。
    需要用到的软件，umbrella-4.01.03.exe，百度搜索吧。
    1、越狱后进入cydia，请先点击一下make my life easier,thanks!,其实这一步就已搞定，但是保险起见，建议持续以下操作。
    2、下载umbrella-4.01.03.exe解压到自己的任意文件夹，一般是下面两个文件

[](http://sjbbs.zol.com.cn/tips/show_pic.php?picid=4262338)


 
    3、打开你的iPhone4，连接电脑，强烈建议使用原装数据线，然后打开umbrella-4.01.03.exe，勾选advanced options，device/version选择iPhone4 4.0.1，request from：cydia

[](http://sjbbs.zol.com.cn/tips/show_pic.php?picid=4262340)


 
    4、save my SHSH，大概10秒钟即可完成，保存路径一般为C:DocumentsandSettingsAdministrator.shsh格式，备份大小为59.5K

[](http://sjbbs.zol.com.cn/tips/show_pic.php?picid=4262341)

[](http://sjbbs.zol.com.cn/tips/show_pic.php?picid=4262342)


 
    5、至于后续的降级方法，需要更进一步操作，相对复杂些，首先要建立自己的服务器，stat TSS server，根据网速状况，可能要持续数分钟时间。具体教程留待以后发布吧，理论上备份过4.0.1SHSH的iPhone4都可以再降级至4.0.1的。
 

[](http://sjbbs.zol.com.cn/tips/show_pic.php?picid=4262343)
    欧若建议，再下一个越狱软件发布之前，不要贸然升级你的iPhone，即使升级，请先备份好自己的SHSH。祝大家玩机快乐！
 
 
