---
layout: post
title: WebKit Frame对象分析
date: 2012-05-07 15:03:00
categories: [浏览器, WebKit]
tags: [webkit, iframe, 数据结构, html, 脚本, uml]
---
Frame应该是整个webcore里面最核心的对象之一,前面在讲Page对象的文章中，也提到过，从Page的角度来看，其实一个Page就是由一个或者多个Frame构成，而这些Frame是以树状的关系组织，我们来看一段HTML代码

**[css]** [view
 plain](http://blog.csdn.net/shunzi__1984/article/details/6244003# "view plain")[copy](http://blog.csdn.net/shunzi__1984/article/details/6244003# "copy")1. <html>  
2.   <body>  
3.     hello, everyone!  
4.      <iframe src="..."/>  
5.      <iframe src ="..."/>  
6.   </body>  
7. </html>  


在以上的html代码中，一个Page里面就会有三个Frame，首先任何一个Page都会至少对应一个Frame，也就是rootFrame，这个Frame不对应iframe节点（Frame中的m_ownerElement对象为空），也是frame树的根节点,还有两个iframe标签对应两个Frame，如果在iframe的属性src里面指定的页面中还含有iframe标签的话，那么也会对应一个frame（一个iframe标签对应一个frame），这样frame之间就形成了父子关系
以下是Frame的一个类图关系
![](http://hi.csdn.net/attachment/201103/12/0_129992088111nS.gif)
 
其中有很多非常重要的对象，下面我们挑出几个来分析
1. Page
Page前面文章中已经提到过。这里表示Frame所在的Page是哪个
2. EventHander
事件处理者，我们在网页上（确切的说是在一个frame上）点击一个按钮，或者移动鼠标，首先底层会将该事件截获然后报给WebView（Page专题中有提到）,然后通过Page找到RootFrame，将该事件交给RootFrame的EventHandler，RootFrame的EventHandler如果发现其所在Frame有subFrame，会交给subFrame的EventHandler来处理，如果处理成功就返回，否则自己继续处理。
3.FrameView
FrameView对应着一个Frame的显示，这个数据结构也是与render引擎连接的纽带，该结构里面有一个非常重要的接口，叫layout, 接口里面的内容就是render dom树的过程。也有一个非常重要的数据成员 RenderObject* mlayoutRootObject;表示render树的根节点.
在32平台FrameView是继承自ScrollableView
4.ScriptController
脚本相关操作的入口，我们知道，一个页面可以执行很多js函数，这个结构就是与js脚本相关的一个对象。
5 Document
表示一个文档，一个html页面解析完成，会形成一个dom树（相关的知识请看dom规范），该结构就是表示html页面解析完成后的结构，每个标签都会在document上对应一个节点，这也是非常核心的一个数据结构，从MVC的角度来看，应该是M部分。
6 DOMWindow
 在bom规范中，js可以访问一个对象，window（bom规范定义了一系列的标准接口），通过该window的接口，js用户可以动态的控制页面的内容，DOMWindow就是表示那个window接口的实现，用一个UML的图来表示，可以表示成如下
![](http://hi.csdn.net/attachment/201103/12/0_1299922835R2Rn.gif)
7. FrameLoader
页面加载器，与页面相关的加载都由其负责，里面又包含了很多其他的对象，这也是非常重要的一个对象，后面会专门讨论。
