---
layout: post
title: WebKit如何加载web页面
date: 2010-09-12 14:25:00
categories: [WebKit]
tags: [webkit, web, 文档, cache, 网络, 框架]
---
转自[http://blog.csdn.net/keensword007/archive/2010/09/03/5860837.aspx](http://blog.csdn.net/keensword007/archive/2010/09/03/5860837.aspx)
 
在WebKit能够渲染web页面之前，它必须从网络加载页面和子资源。从web加载资源涉及到许多层，本文将集中于WebCore，即WebKit的主要渲染组件，如何参与加载过程。
WebKit包含两条加载管道，一个用于加载文档到框架(frame)，另一个用于加载子资源（如图片和脚本）。下图总结了两条管道涉及的主要对象：
![](http://hi.csdn.net/attachment/201009/12/0_1284272607uEEu.gif)
**加载框架(Frame)
**FrameLoader负责加载文档到框架，当您点击链接的时候，FrameLoader开始创建一个新的处在“策略(policy)”状态的DocumentLoader对象，在该状态下等待webkit客户决定如何处理本次加载。通常客户指示FrameLoader将加载当作导航(navigator)（而不是阻塞式加载）。
一旦客户指示FrameLoader将加载当作导航，FrameLoader将DocumentLoader迁移到"临时(provision)"状态，启动网络请求，并等待以判断网络请求的结果是下载还是一个新的文档。
接着，DocumentLoader创建MainResourceLoader，它的任务是通过ResourceHandle接口和平台相关网络库交互。将MainResourceLoader从DocumentLoader分离出来有两个目的：(1) MainResourceLoader将DocumentLoader从处理ResourceHandle回调的细节隔离开来。(2) MainResourceLoader的生命周期与DocumentLoader的生命周期（与Document紧密关联）分离。
一旦加载系统从网络接收到足够的信息，判断出接收的资源确实表示一份文档，FrameLoader将DocumentLoader迁移到"已提交(committed)"状态，Frame开始显示新的文档。
 
**加载子资源(Subresource)
**当然，显示一个web页面需要的不仅仅是组成文档的HTML，还需要加载图像、脚本以及其它文档引用到的子资源。DocLoader就是负责加载这些子资源的（注意，虽然DocumentLoader和DocLoader名称上相似，但它们的职责却大不相同）。
我们以加载图片为例。要加载一个图像，DocLoader首先检查Cache是否在内存中已经有一份图像的副本（如CachedImage对象）。如果Cache中已经存在，DocLoader会立即响应。甚至进一步提高效率，Cache总是将解码过的图像放在显存，这样WebKit就不需要对同一图像做两次解码。
如果图像不在Cache中，Cache将创建一个新的CacheImage对象表示该图像。CacheImage对象将要求“Loader“对象创建SubresourceLoader来启动网络请求。SubresourceLoader在子资源加载管道中所起的作用和MainResourceLoader在主资源加载管道中所起的作用类似，都是直接和ResourceHanle的平台相关接口交互。
 
