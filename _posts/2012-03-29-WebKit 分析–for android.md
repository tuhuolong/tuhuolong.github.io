---
layout: post
title: WebKit 分析–for android
date: 2012-03-29 10:50:00
categories: [WebKit, Android]
tags: [webkit, java, android, 浏览器, ui, jni]
---
**一、WebKit简介
**WebKit是一个开源的浏览器网页排版引擎，包含WebCore排版引擎和JSCore引擎。WebCore和JSCore引擎来自于KDE项目的KHTML和KJS开源项目。Android平台的Web引擎框架采用了WebKit项目中的WebCore和JSCore部分，上层由Java语言封装，并且作为API提供给Android应用开发者，而底层使用WebKit核心库（WebCore和JSCore）进行网页排版。

**二、WebKit目录结构
**Android平台的WebKit模块分成Java和WebKit库两个部分，其目录结构如下表所示：
**Webkit 某块目录结构****Java层（根目录device\java\android\android\webkit）**BrowserFrame.javaBrowserFrame对象是对WebCore库中的Frame对象的java层封装，用于创建WebCore中定义的Frame，以及为该Frame对象提供Java层回调方法。ByteArrayBuilder.javaByteArrayBuilder辅助对象，用于byte块链表的处理。CacheLoader.javaURL Cache载入器对象，该对象实现SteadLoader抽象基类，用于通过CacheResult对象载入内容数据。CacheManager.javaCache管理对象，用于java层cache对象管理。CacheSyncManger.javaCache同步管理对象，负责同步RAM和FLASH之间的浏览器Cache数据。实际的物理数据操作在WebSyncManager对象中完成。CallbackProxy.java该对象用于处理WebCore与UI线程消息代理类。当有Web事件产生时WebCore线程会调用该回调代理类，代理类通过消息方式通知UI线程，并且调用设置的客户对象的回调函数。CellList.javaCellList定义图片集合中的Cell，管理Cell图片的绘制，状态改变以及索引。CookieManager.java根据RFC2109规范，管理cookies.DataLoader.java数据载入器对象，用于载入网页数据。DataSorter.java尚未使用。DownloadListener.java下载侦听器接口。DownloadManagerCore.java下载管理器对象，管理下载列表。该对象运行于WebKit的线程中，通过CallbackProxy对象与UI线程交互。FileLoader.java文件载入器，用于将文件载入到Frame中。FrameLoader.javaFrame载入器，用于载入网页Frame数据。HttpAuthHandler.javaHttp认证处理对象，该对象会作为参数传递给BrowserCallback.displayHttpAuthDialog方法，与用户交互。HttpDataTime.java该对象是处理HTTP日期的辅助对象。JsConfirmResult.javaJs确认请求对象。JsPromptResult.javaJs结果提示对象，用于向用户提示Javascript运行结果。JsResult.javaJs结果对象，用于用户交互。JWebCoreJavaBridge.java用Java与WebCore库中的Timer和Cookies对象交互的桥接代码。LoaderListener.java载入侦听器，用于处理载入器侦听消息。NetWork.java该对象封装网络连接逻辑，为调用者提供更高级的网络连接接口。PanZoom.java用于处理图片缩放、移动等操作。PanZoomCellList.java用于保存、缩放图片的Cell。PerfChecker.java用于效率测试的功能对象？？？？SslErrorHandler.java用于处理SSL错误消息。StreamLoader.javaStreamLoader抽象类是所有内容载入器对象的基类。该类是通过消息方式控制的状态机，用于将数据载入到Frame中。TextDialog.java用于处理html中文本区域叠加情况，可以使用标准的文本编辑而定义的特殊的EditText控件。URLUtil.javaURL处理功能函数，用于编码、解码URL字符串，以及提供附加的URL类型分析功能。WebBackForwardList.java该对象包含WebView对象中显示的历史数据。WebBackForwardListClient.java浏览历史处理的客户接口类，所有需要接收浏览历史改变的类都需要实现该接口。WebChromeClient.javaChrome客户基类，Chrome客户对象在浏览器文档标题、进度条、图标改变时会得到通知。WebHistoryItem.java该对象用于保存一条网页历史数据。WebIconDataBase.java图标数据库管理对象，所有的WebView均请求相同的图标数据库对象。WebSettings.javaWebView的管理设置数据，该对象数据是通过JNI接口从底层获取。WebSyncManager.java数据同步对象，用于RAM数据和Flash数据的同步。WebView.javaWeb视图对象，用于基本的网页数据载入、显示等UI操作。WebViewClient.javaWeb视图客户对象，在Web视图中有事件产生时，该对象可以获得通知。WebViewCore.java该对象对WebCore库进行了封装，将UI线程中的数据请求发送给WebCore处理，并且通过CallbackProxy的方式，通过消息通知UI线程数据处理结果。WebViewDatabase.java该对象使用SQLiteDatabase为WebCore模块提供数据存取操作。 
**三、WebKit模块框架**
Android平台的Webkit有java层和webkit库两部分组成，java层负责与android应用层进行通信，而webkit类库负责实际的网页排版处理。Java层和C库之间通过JNI和Bridge相互调用，如下图所示：
[![java-webkit](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201123145121.png "java-webkit")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201123136366.png)
 
**3.1 java层框架**
**3.1.1 主要类关系**
      WebKit模块的java层一共由41个文件组成，其中主要的类关系如下图所示：
[![class-relation](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201132343126.png "class-relation")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201132248676.png)
 
**1. WebView**
    WebView类是WebKit模块Java层的视图类，所有需要使用Web浏览器功能的Android都需要创建该视类对象显示和处理请求的网络资源。目前WebKit模块支持HTTP、HTTPS、FTP以及javascript请求。WebView作为应用程序的UI接口，为用户提供一系列的网页浏览、用户交互接口，客户程序通过这些接口访问WebKit核心代码。
**2. WebViewDatabase**
    WebViewDatabase是WebKit模块针对SQLiteDatabase对象的封装，用于存储和获取运行时浏览器保存的缓冲数据、历史访问数据、浏览器配置数据等。该对象是个单实例对象，通过getInstance方法获取WebViewDatabase的实例。WebViewDatabase是WebKit模块中的内部对象，仅供WebKit框架内部使用。
**3. WebViewCore**
    WebViewCore类是java层和C层WebKit核心库的交互类，客户程序调用WebView的网页浏览相关操作会转发给BrowserFrame对象。当WebKit核心库完成实际的数据分析和处理后会回调WebViewCore中定义的一系列JNI接口，这些接口会通过CallbackProxy将相关事件通知相应的UI对象。
**4. CallbackProxy**
     CallbackProxy是一个代理类，用于UI线程和WebCore线程交互。该类定义了一系列与用户相关的通知方法，当WebCore完成相应的数据处理，则会调用CallbackProxy类中对应的方法，这些方法通过消息方式间接调用相应的处理对象的处理方法。详细的处理流程在下文中会具体分析。
**5. BrowserFrame**
     BrowserFrame类负责URL资源的载入、访问历史的维护、数据缓存等操作，该类通过JNI接口直接与WebKit的C库交互。
**6. JWebCoreJavaBridge**
**      **该类为java层Webkit代码提供与C层WebKit核心部分的Timer和Cookies操作相关的方法。
**7. DownloadManagerCore**
    下载管理核心类，该类负责管理网络资源下载，所有Web下载操作均由该类统一管理。该类实例运行在WebKit线程当中，与UI线程的交互是通过调用CallbackProxy对象中相应的方法完成。
**8. WebSettings**
    WEB浏览器通过该对象访问相关的用户配置信息。
**9. DownloadListener**
    下载侦听接口，如果用户代码实现该接口，则在下载开始、失败、挂起、完成等情况下，DownloadManagerCore对象会调用客户代码中实现的DownloadListener方法。
**10. WebBackForwardList**
    WebBackForwardList 对象维护用户访问历史记录，该类为客户程序提供操作访问浏览器历史数据的相关方法。
**11. WebViewClient**
    WebViewClient类定义了一系列事件方法，如果android应用程序设置了WebVieClient派生对象，则在网页载入、资源载入、页面访问错误等情况发生时，该派生对象的相应方法会被调用。
**12. WebBackForwardListClient**
    WebBackForwardListClient对象定义了对访问历史操作时可能产生的事件接口，当用户实现该接口，则在操作访问历史时（访问历史移除、访问历史 清空等）用户会得到通知。
**13. WebChromeClient**
    WebChromeClient类定义了与浏览器窗口修饰相关的事件。例如接收到Title、接收到Icon、进度变化时，WebChromeClient的相应方法会被调用。
 
**3.1.2 主要类的设计**
**3.1.2.1 数据载入器的设计**
    WebKit模块的java部分框架中使用数据载入器来加载相应类型的数据，目前有CacheLoader、DataLoader以及FileLoader三类载入器，他们分别用于缓存数据、内存数据、和文件数据的载入操作。java层（WebKit模块）所有的载入器都从StreamLoader继承（其父类为Handler），由于SteamLoader 类的基类为Handler类，因此在构建载入器时，会开启一个事件处理线程，该线程负责实际的数据载入操作，而请求线程通过消息的方式驱动数据的载入。下图是数据载入器相关类的类图结构：
[![loader](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201123296077.png "loader")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201123289764.png)
    StreamLoader类定义了4个不同的消息：
> MSG_STATUS、MSG_HEADERS、MSG_DATA、MSG_END
    分别表示发送状态信息、发送消息头消息、发送数据消息以及数据发送完毕消息。该类提供了2个抽象保护方法以及一个公有方法：setupStreamAndSendStatus 保护方法主要是用于构建与通信协议相关的数据流，以及向LoadListener发送状态。 buildHeaders方法是向子类提供构造特定协议消息头功能。所有载入器只有一个公有方法（load），因此当需要载入数据时，调用该方法即可。与数据载入流程相关的类还有LoaderListener以及BrowserFrame，当数据载入事件发生时，WebKit的C库会更新载入进度，并且会通知BrwoserFrame，BrowserFrame接收到进度条变更事件后通过CallbackProxy对象，通知View类进度条数据变更。下买你以DataLoader类为例子，说明数据载入以及与UI交互过程（转载者注：下图看不清，等我搞清楚这个流程之后我再画一个换上。）：
[![dataLoader](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201132519948.png "dataLoader")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201132414102.png)
    上图中绿色部分是BrowserFrame处理进度变更事件时，调用CallbackProxy对象通知视图变更状态的操作，在这里省略。图中灰色部分表示C层代码，而白色部分表示Java层代码。
 
**3.2 C层框架**
**3.2.1 C类与Java类的关系**
**1. BrowserFrame**
    与BrowserFrame Java类相对应的C++类为FrameBridge，该类对Dalvik虚拟机回调BrowserFrame类中定义的本地方法进行了封装。与BrowserFrame中回调函数（java层）相对应的C层结构定义如下：
	struct FrameBridge::JavaBrowserFrame
	{
	    JavaVM*   mJVM;
	    jobject   mObj;
	    jmethodID mStartLoadingResource;
	    jmethodID mLoadStarted;
	    jmethodID mUpdateHistoryForCommit;
	    jmethodID mUpdateCurrentHistoryData;
	    jmethodID mReportError;
	    jmethodID setTitle;
	    jmethodID mWindowObjectCleared;
	    jmethodID mDidReceiveIcon;
	    jmethodID mUpdateVisiteHistory;
	    jmethodID mHandleUrl;
	    jmethodID mCreateWindow;
	    jmethodID mCloseWindow;
	    jmethodID mDecidePolicyForFormResubmission;
	};
    该结构作为FrameBridge（C 层）的一个成员变量（mJavaFrame），在FrameBridge构造函数中，用BrowserFrame（java 层）类的回调方法的偏移量初始化JavaBrowserFrame结构的各个域。初始后，当WebCore（C层）在剖析网页数据时，有Frame相关的资源改变，比如WEB页面的主题变化，则会通过mJavaFrame结构，调用指定BrowserFrame对象的相应方法，通知Java层处理。**2. JWebCoreJavaBridge**    与该对象相对应的C层对象为JavaBridge，javaBridge对象继承了TimerClient和CookieClient类，负责WebCore中的定时器和Cookie管理。与Java层JWebCoreJavaBridge类中方法偏移量相关的是JavaBridge中几个成员变量，在构造JavaBridge对象时，会初始化这些成员变量，之后有Timer或者Cookies事件产生时，WebCore会通过这些ID值，回调对应JWebCoreJavaBridge的相应方法。**3. LoadListener**    与该对象相关的C层结构是 struct resourceloader_t, 该结构保存了LoadListener对象ID、CancelMethod ID以及DownloadFileMethod ID值。当有Cancel或者Download事件产生，WebCore会回调LoadListener类中的CancelMethod或者DownloadFileMethod。**4. WebViewCore**    与WebViewCore相关的C类是WebCoreViewImpl， WebViewCoreImpl类有个JavaGlue对象作为成员变量，在构建WebCoreViewImpl对象时，用WebViewCore（java层）中的方法ID值初始化该成员变量。并且将构建的WebCoreViewImpl对象指针赋值给WebViewCore（java层）的mNativeClass,这样将WebViewCore（java层）和WebViewCoreImpl（C层）关联起来。**5. WebSettings**    与WebSettings相关的C层结构是struct FieldIds，该结构保存了WebSettings类中定义的属性ID以及方法ID，在WebCore初始化时（WebViewCore的静态方法中使用System.loadLibrary载入）会设置这些方法和属性的ID值。**6. WebView**    与WebView相关的C层类是WebViewNative，该类中的mJavaGlue中保存了WebView中定义的属性和方法ID，在WebViewNative构造方法中初始化，并且将构造的WebViewNative对象的指针，赋值给WebView类的mNativeClass变量，这样WebView和WebViewNative对象就建立了关系。 3.2.2 主要类关系    与java层相关的C层类如下表所示：ChromeClientAndroid该类主要处理WebCore中与Frame装饰相关的操作。例如设置状态栏、滚动条、Javascript脚本提示框等。当浏览器中有关事件产生时，ChromeClientAndroid类的相应方法会被调用，该类会将相关的UI事件通过Bridge传递给Java层，由Java层负责绘制以及用户交互方面的处理。EditorClientAndroid该类负责处理页面中文本相关的处理，比如文本输入、取消、输入法数据处理、文本粘贴、文本编辑等操作。不过目前该类只对按键相关的事件进行了处理，其他操作均未支持。ContextMenuClient该类提供页面相关的功能菜单。比如图片拷贝、朗读、查找等功能。但是，目前项目中未实现具体功能。DragClient该类定义了与页面拖拽相关的处理，但是目前该类没有实现具体功能。FrameLoaderClientAndroid该类提供与Frame加载相关的操作，当用户请求加载一个页面时，WebCore分析完网页数据后，会通过该类调用Java层的回调方法，通知UI相关的组件处理。InspectorClientAndroid该类提供与窗口相关的操作，比如窗口显示、关闭窗口、附加窗口等。不过目前该类的各个方法均为空实现。Page该类提供与页面相关的操作，比如网页页面的前进、后退等操作。FrameBridge该类对Frame相关的Java层方法进行了封装，当有Frame事件产生时，WebCore通过FrameBridge回调Java的回调函数，完成用户交互过程。AssetManager该类为浏览器提供本地资源访问功能。RenderSkinAndroid该类与控件绘制相关，所有的绘制控件类都需要从该类派生，目前WebKit模块中有Button、Combo、Radio三类控件。以上几个类会在Java层请求创建WebFrame的时候被建立，他们的关系如下图所示：
[![webFrame](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/20110420113259202.jpg "webFrame")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201132535323.jpg)
上图中标注为深绿色的FrameAndroid是浏览器Frame，一个BrowserFrame对象对应着一个FrameAndroid对象。而其他8个标注为淡绿色的类，是与该Frame显示、布局等相关的类。WebKit模块中所有WebCore核心代码与用户交互的操作使用FrameAndroid对象中的Briedge处理（回调相应的Java方法）。
 
**四、基本操作分析**
**4.1 WebKit模块初始化**
    Android SDK中提供了WebView类，该类为客户提供客户化浏览显示的功能，如果客户需要加入浏览器支持，可将该类的实例或者派生类的实例作为视图，调用Activity类的SetContentView显示给用户。当客户代码中第一次生成WebView对象时，会初始化WebKit库（包括Java层和C层两个部分），之后用户可以操作WebView对象完成网络或者本地资源的访问。
    WebView对象的生成主要涉及3个类CallbackProxy、WebViewCore以及WebViewDatabase。其中CallbackProxy对象为WebKit模块中UI线程和WebKit类库提供交互功能，WebViewCore是WebKit的核心层，负责与C层交互以及WebKit模块C层类库初始化，而WebViewDatabase为WebKit模块运行时缓存、数据存储提供支持。WebKit模块初始化流程如下：
 
**WebView**
- **+- 创建CallbackProxy对象**
- **+- 创建WebViewCore对象**1. 调用System.loadLibrary载入webcore类相关库（C层）
2. 如果是第一次初始化WebViewCore对象，创建WebCoreThread线程
3. 创建EventHub对象，处理WebViewCore事件
4. 获取WebIconDatabase对象实例
5. 向WebCoreThread发送初始化消息


- **+- 获取WebViewDatabase实例**


    如上所述，第一步调用System.loadLIbrary方法载入webcore相关类库，该过程由Dalvik虚拟机完成，它会动态从链接库目录中寻找libWebCore.so类库，载入内存中，并且调用WebKit初始化模块的JNI_OnLoad方法。WebKit模块的JNI_OnLoad方法中完成如下初始化操作：
> a) 初始化framebridge[register_android_webcore_framebridge]
> 初始化gFrameAndroidField静态变量，以及注册BrowserFrame类中的本地方法表。
> b) 初始化javabridge[register_android_webcore_javabridge]
> 初始化gJavaBridge.mObject对象，以及注册JWebCoreJavaBridge类中的本地方法
> c) 初始化资源loader[register_android_webcore_resource_loader]
> 初始化gResourceLoader静态变量，以及注册LoadListener类的本地方法
> d) 初始化webviewcore[register_android_webkit_webviewcore]
> 初始化gWebCoreViewImplField静态变量，以及注册WebViewCore类的本地方法
> e) 初始化webhistory[register_android_webkit_webhistory]
> 初始化gWebHistoryItem结构，以及注册WebBackForwardList和WebHistoryItem类的本地方法
> f) 初始化webicondatabase[register_android_webkit_webicondatabase]
> 注册WebIconDatabase类的本地方法
> g) 初始化websettings[register_android_webkit_websettings]
> 初始化gFieldIds静态变量，以及注册WebSettings类的本地方法
> h) 初始化webview[register_android_webkit_webview]
> 初始化gWebViewNativeField静态变量，以及注册WebView类的本地方法
    第二步是WebCoreThread初始化，该初始化只在第一次创建WebViewCore对象时完成，当用户代码第一次生成WebView对象，会在初始化WebViewCore类时创建WebCoreThread线程，该线程负责处理WebCore初始化事件。此时WebViewCore构造函数会被阻塞，直到一个WebView初始化请求完毕时，会在WebCoreThread线程中唤醒。
    第三步创建EventStub对象，该对象处理WebView类的事件，当WebCore初始化完成后会向WebView对象发送事件，WebView类的EventStub对象处理该事件，并且完成后续初始化工作。
    第四步获取WebIconDatabase对象实例。
    第五步向WebViewCore发送INITIALIZE事件，并且将this指针作为消息内容传递。WebView类主要负责处理UI相关的事件，而WebViewCore主要负责与WebCore库交互。在运行时期，UI线程和WebCore数据处理线程是运行在两个独立的线程当中。WebCoreThread线程接收到INITIALIZE线程后，会调用消息对象参数的initialize方法，而后唤醒阻塞的WebViewCore Java线程（该线程在WebViewCore的构造函数中被阻塞）。不同的WebView对象实例有不同的WebViewCore对象实例，因此通过消息的方式可以使得UI线程和WebViewCore线程解耦合。WebCoreThread的事件处理函数，处理INITIALIZE消息时，调用的是不同WebView中WebViewCore实例的initialize方法。WebViewCore类中的initialize方法中会创建BrowserFrame对象（该对象管理整个WEB窗体，以frame相关事件），并且向WebView对象发送WEBCORE_INITIALIZED_MSG_ID消息。WebView消息处理函数，会根据消息参数1初始化指定的WebViewCore对象，并且更新WebViewCore的Frame缓冲。
    初始化过程的序列图如下图所示：
[![webcore](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/20110420113312277.png "webcore")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201133042988.png)
    初始化完成后Java层和C层类图关系如下图所示:
[![WebView_c](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/20110420113319597.jpg "WebView_c")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201133138716.jpg)
上图中淡绿色的类表示Java层，而灰色类表示C层。
 
4.2 数据载入
**4.2.1 载入网路数据**
    客户代码中可以使用WebView类的loadUrl方法，请求访问指定的URL网页数据。WebView对象中保存着WebViewCore的引用，由于WebView属于UI线程，而WebViewCore属于后台线程，因此WebView对象的loadUrl被调用时，会通过消息的方式将URL信息传递给WebViewCore对象，该对象会调用成员变量mBrowserFrame的loadUrl方法，进而调用WebKit库完成数据的载入。其调用函数序列如下所示：
[![loadurl](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/20110420113332606.png "loadurl")](http://images.cnblogs.com/cnblogs_com/hibraincol/201104/201104201133255477.png)
    网络数据的载入分别由Java层和C层共同完成，Java层完成用户交互、资源下载等操作，而C层主要完成数据分析（建立DOM树、分析页面元素等）操作。由于UI线程和WebCore线程运行在不同的两个线程中，因此当用户请求访问网络资源时，通过消息的方式向WebViewCore对象发送载入资源请求。在Java层的WebKit模块中，所有与资源载入相关的操作都是由BrowserFrame类中对应的方法完成，这些方法是本地方法，会直接调用WebCore库的C层函数完成数据载入请求，以及资源分析等操作。如上图所示，C层的FrameLoader类是浏览框架的资源载入器，该类负责检查访问策略以及向Java层发送下载资源请求等功能。在FrameLoader中，当用户请求网络资源时，经过一系列的策略检查后会调用FrameBridge的startLoadingResource方法，该方法会回调BrowserFrame（Java）类的startLoadingResource方法，完成网络数据的下载，而后BrowserFrame（Java）类的startLoadingResource方法会返回一个LoadListener的对象，FrameLoader会删除原有的FrameLoader对象，将LoadListener对象封装成ResourceLoadHandler对象，并且将其设置为新的FrameLoader。到此完成了一次资源访问请求，接下来的任务即是WebCore库会根据资源数据进行分析和构建DOM，以及相关的数据结构。
 
**4.2.2 载入本地数据**
本地数据是以data://开头的URL表示，载入过程和网络数据一样，只不过在执行FrameLoader类的executeLoad方法时，会根据URL的SCHEME类型区分，调用DataLoader的requestUrl方法（参看3.1.2.1节对载入器的分析），而不是调用handleHTTPLoad建立实际的网络通信连接。
 
**4.2.3 载入文件数据**
文件数据是以file://开头的URL，载入的基本流程与网络数据载入流程基本一致，不同的是在运行FrameLoader类的executeLoad方法时，根据SCHEME类型，调用FileLoader的requestUrl方法，完成数据加载（参看3.1.2.1节对载入器的分析）。
 
**4.3 刷新绘制**
    当用户拖动滚动条、有窗口遮盖、或者有页面事件触发都会向WebViewCore（Java层）对象发送背景重绘消息，该消息会引起网页数据的绘制操作。WebKit的数据绘制可能出于效率上的考虑，没有通过Java层，而是直接在C层使用SGL库完成。与Java层图形绘制相关的Java对象有如下几个：
    1. Picture类
该类对SGL封装，其中变量mNativePicture实际上是保存着SkPicture对象的指针。WebViewCore中定义了两个Picture对象，当作双缓冲处理，在调用webKitDraw方法时，会交换两个缓冲区，加速刷新速度。
    2. WebView类
该类接受用户交互相关的操作，当有滚屏、窗口遮盖、用户点击页面按钮等相关操作时，WebView对象会与之相关的WebViewCore对象发送VIEW_SIZE_CHANGED消息。当WebViewCore对象接收到该消息后，将构建时建立的mContentPictureB刷新到屏幕上，然后将mContentPictureA与之交换。
    3. WebViewCore类
该类封装了WebKit C层代码，为视图类提供对WebKit的操作接口，所有对WebKit库的用户请求均由该类处理，并且该类还为视图类提供了两个Picture对象，用于图形数据刷新。
    下面以Web页面被鼠标拖拽的情况为例子，分析网页数据刷新过程。
    当用户使用手指点击触摸屏，并且移动手指，则会引发touch事件的产生，Android平台会将touch事件传递给最前端的视图相应（dispatchTouchEvent方法处理）。在WebView类中定义了5种touch模式，在手指拖动Web页面的情况下，会触发mMotionDragMode，并且会调用View类的scrollBy方法，触发滚屏事件以及使视图无效（重绘，会调用View的onDraw方法）。WebView视图中的滚屏事件由onScrollChanged方法响应，该方法向WebViewCore对象发送SET_VISIBLE_RECT事件。
    WebViewCore对象接收到SET_VISIBLE_RECT事件后，将消息参数中保存的新视图的矩形区域大小传递给nativeSetVisibleRect方法，通知WebCoreViewImpl对象（C层）视图矩形变更（WebCoreViewImpl::setVisibleRect方法）。在setVisibleRect方法中，会通过虚拟机调用WebViewCore的contentInvalidate方法，该方法会引发webkitDraw方法的调用（通过WEBKIT_DRAW消息）。在webkitDraw方法里，首先会将mContentPictureB对象传递给本地方法nativeDraw绘制，而后将mContentPictureB的内容与mContentPictureA的内容对调。在这里mContentPictureA
 缓冲区是供给WebViewCore的draw方法使用，如果用户选择某个控件，绘制焦点框时候WebViewCore对象的draw方法会调用，绘制的内容保存在mContentPictureA中，之后会通过Canvas对象（Java层）的drawPicture方法将其绘制到屏幕上，而mContentPictureB缓冲区是用于built操作的，nativeDraw方法中首先会将传递的mContentPictureB对象数据重置，而后在重新构建的mContentPictureB画布上，将层上相关的元素绘制到该画布上。上面提到，之后会将mContentPictureB和mContentPictureA的内容对调，这样一次重绘事件产生时（会调用WebView.onDraw方法）会将mContentPictureA的数据使用Canvas类的drawPicture绘制到屏幕上。当webkitDraw方法将mContentPictureA与mContentPictureB指针对调后，会向WebView对象发送NEW_PICTURE_MSG_ID消息，该消息会引发WebViewCore的VIEW_SIZE_CHANGED消息的产生，并且会使当前视图无效产生重绘事件(invalidate())，引发onDraw方法的调用，完成一次网页数据的绘制过程。
