---
layout: post
title: WebKit Loader模块介绍
date: 2012-05-07 14:57:00
categories: [浏览器, WebKit]
tags: [webkit, cache, layout, 数据结构, scroll, 浏览器]
---
**一．下面重点介绍一下与Loader相关的数据结构和模块。**
** **
**Frame**：可以看做是浏览器外壳调用Loader的总入口，它就像我们印象中的一个网页，它关注的是页面的显示 (FrameView) 、页面数据的加载(FrameLoader) 、页面内的各种控制器 (Editor,
 EventHandler, ScriptController, etc.) 等等，它包含以下模块（只列出重点）：
Document
Page
FrameView
RenderView
FrameLoader
DOMWindow
 
下面分别介绍(PS: 必须要了解这些概念，不然后面的东东都无法理解):
 
1)**Document**:这个类的爷爷类是 Node，它是 DOM 树各元素的基类； Document 有个子类是 HTMLDocument ，它是整个文档 DOM 树的根结点，这样就明白了：原来 Document 就是描述具体文档的代码，看一下它的头文件，就更明白了，它的属性与方法就是围绕着各种各样的结点： Text ， Comment ， CDATASection ， Element……
 
2)**Page**:我的理解是，Page与Frame(严格说是FrameView)是一一对应的，Frame关注UI，Page关注数据。现在的浏览器一般都提供同时打开多个窗口，每一个窗口对应的数据就是这个Page在管理了。
  在 page.cpp 文件里，还有个重要的全局指针变量： static HashSet<Page*>* allPages; 这个变量包含了所有的page实例。
 
3)**FrameView**:可以理解为为一个网页的ViewPort, 它提供一个显示区域，同时包含的有Render根节点、layout排版相关接口、Scroll相关等。 FrameView是Layout排版的总入口。
 
4)**RenderView**:与FrameView差不多，只是分工不同，它管理与Render树相关的东东。
 
5) **FrameLoader**：重点，FrameLoader类将Documents加载到Frames。当点击一个链接时，FrameLoader创建一个新的处于“policy”状态的DocumentLoader对象，一旦webkit指示FrameLoader将本次加载视为一个导航(navigation)，FrameLoader就推动DocumentLoader进入“provisional”状态，（在该状态，DocumentLoader发调用CURL发起一个网络请求，并等待是html还是下载文件。）同时,DocumentLoader会创建一个MainResourceLoader对象（该对象在后面单独介绍）。
6）。**DOMWindow:**实现了Dom的一些接口，如CreateNode等。后面可以详细讲讲。
 
上面介绍的概念比较多，我也不晓得好不好理解，没理解也不怕，多去看看代码，这是必须的。
 
另外看看下面的图，就会清晰很多的
![](http://hiphotos.baidu.com/ellzu/pic/item/707d23acd933c895d57daa34d11373f0830200e5.jpg)


**二、Webkit的Loader有两条加载数据的主线： (从上图可以看到)**
1. MainResourceLoader：该模块主要加载主网页请求。后面称为MainResource。
2. DocLoader：该模块除了主网页外的所有子请求，如:.js文件，图片资源，.css文件。  后面称为SubResource。
 
MainResource部分：
FrameLoader->DocumentLoader->MainResourceLoader-ResourceHandleDocumentLoader经历状态：1)"policy" 2) "provisional" 3)"commited"分别是等待、作为navigation发送network
 request、文件下载完毕
 
Subresource部分：
DocLoader->Cache->[CacheObjects]example: CacheImage->SubresourceLoader->ResourceHandle 当请求一个资源时，首先查看Cache中是否存在该对象，如果存在直接返回；如果不存在，创建该Cache对象（如CacheImage），然后创建一个SubresourceLoader，加载资源。
举例说明：
  加载图片时，DocLoader首先询问Cache, 在内存中是否也存在（CachedImage对象），如果已存在，则直接加载，即省了时间又省了流量。如果图片不在Cache中， Cache首先创建一个新的CachedImage对象来代表该图片，然后由CachedImage对象调用Loader对象发起一个网络请求，Loader对象创建SubResourceLoader。后面的流程就一样了，SubResourceLoader也是直接把ResourceHandle打交道的。****
** **
**接下来跟踪一下Loader发送请求的代码实现：**
1. 用户输入URL后，最先调用的接口是：
FrameLoader::load(constResourceRequest& request)
 
ResourceRequest包含了：
KURL(处理url的一个类)、setHTTPHeaderField、setHTTPContentType等与HTTP头部相关的函数
 
2.Load()通过ResourceRequest数据调用createDocumentLoader(request, substituteData)来创建一个DocumentLoader。
 
3.Load()函数继续给request设置HttpAccept,Cache-Control HTTP头等信息。
 
4. 设置FrameLoader::checkNavigationPolicy函数进入"Policy"状态。
 
5.判断该url是否在Cache中等一系列状态判断后，进入DocumentLoader::startLoadingMainResource函数准备加载MainResource。该函数首先会创建调用MainResourceLoader。
 
6.进入MainResourceLoader::load函数，调用illSendRequest(r,ResourceResponse());做发送请求前的准备。
 
7.调用PolicyCheck检查policy的状态后，进入FrameLoader::callContinueLoadAfterNavigationPolicy继续往下走。
 
8:在MainResourceLoader::loadNow(ResourceRequest&r)函数里创建ResourceHandle,在创建ResourceHandle函数中，调用start函数，start函数把ResourceHandle自已添加到ResourceHandleManager的m_resourceHandleList队列里。
  同时，调用m_downloadTimer.startOneShot激活网页请求下载的定时器。(这是个毫秒级的定时器，采用定时器的原因也是为了实现异步的请法)
 
  可以看到m_downloadTimer的定义：Timer<ResourceHandleManager> m_downloadTimer;
  m_downloadTimer是实现的一个定时器模块类，在它的构造函数里已经传入了回调函数的地址：ResourceHandleManager::downloadTimerCallback。
 
9. 一路返回到Load()函数，并返回到调用源，函数执行完毕。
 
10.  ResourceHandleManager::downloadTimerCallback回调函数被定时器调用。
 
11. 可以看到downloadTimerCallback函数的代码：
           调用libcurl库的接口curl_multi_fdset,curl_multi_perform等查询数据。
