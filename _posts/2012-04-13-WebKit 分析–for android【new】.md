---
layout: post
title: WebKit 分析–for android【new】
date: 2012-04-13 16:07:00
categories: [Android, 浏览器, WebKit]
tags: [webkit, android, java, jni, html5, 浏览器]
---
**Java调WebView,WebView(UI线程)向WebViewCore(**WebCore线程)发消息,再由**WebViewCore正调BrowserFrame,回调CallbackProxy**

网上有许多webkit的分析文章，其中针对android porting的一篇文章[WebKit – WebKit For Android](http://www.jjos.org/android/2010/05/10/312_webkit-webkit-for-android.html)，写的非常好，分析得非常深入。不过这篇文章针对的Android版本比较老（具体版本无从考究），因此本文将在这篇文章的基础上，加入android
 4.0 webkit porting的一些内容。
###一、Android WebKit简介
Webkit是一个开源的浏览器排版和渲染引擎，包含WebCore和JavascriptCore。WebKit有众多的实现(Qt、Gtk, windows, chromium, android, etc)。Android 4.0平台的Web引擎框架采用了WebKit中的WebCore，javascript引擎则是采用google的V8引擎。Android 4.0的webkit采用了和chromium 12.0.742.130中webkit相同的codebase，webkit版本为534.30。
###二、Android WebKit模块框架
Android平台的WebKit上层由Java语言封装，并且作为API提供给Android应用开发者，而底层使用WebKit核心库（WebCore）进行网页排版。WebKit模块分为两个部分: Java层和C层（webkit库）。Java层和C层通过JNI相互调用，如图1所示：
[![AndroidWebKitArchitecture](http://mogoweb.net/wp-content/uploads/2012/03/AndroidWebKitArchitecture_thumb.png)](http://mogoweb.net/wp-content/uploads/2012/03/AndroidWebKitArchitecture.png)
图1 Android WebKit模块框架
在webkit其它平台的移植中，webkit层就是封装WebCore，为上层应用提供接口的。Android的平台具有一定的特殊性，需要提供Java API接口，应用程序框架也是基于Java的，所以在Android的移植中，webkit层实际上被拆成两部分，Java部分和C++部分，它们之间通过JNI接口进行通讯。JNI是一种双向通讯机制，Java代码可以调用C/C++代码，C/C++代码也可以调用Java代码。
通常，WebCore中回调Java的代码都位于WebKit(Android Implementation)层，但有一个例外，就是Source/WebCore/platform/android/GeolocationServiceBridge.cpp，该文件也包含回调到Java的代码。

####2.1 Java层框架
#####2.1.1 Java层源码说明
Java层的代码位于frameworks/base/core/java/android/webkit目录下。各文件的简单说明如下：
AccessibilityInjector.java为WebView注入AccessibilityBrowserFrame.java对WebCore中Frame对象的Java层封装，用于创建WebCore中定义的Frame，以及为该Frame对象提供Java层回调方法ByteArrayBuilder.java辅助对象，用于byte块链表的处理~~CacheLoader.java~~~~android 4.0 WebKit中不再使用~~CacheManager.javaCache管理对象，负责Java层Cache对象管理CallbackProxy.java该对象是用于处理WebCore与UI线程消息的代理类。当有Web事件产生时WebCore线程会调用该回调代理类，代理类会通过消息的方式通知UI线程，并且调用设置的客户对象的回调函数。CertTool.java证书工具ClientCertRequestHandler.java处理客户端证书请求ConsoleMessage.java来自WebCore的Javascript控制台消息~~ContentLoader.java~~android 4.0 WebKit中不再使用CookieManager.java根据RFC2109规范，管理cookiesCookieSyncManager.javaCookies同步管理对象，该对象负责同步RAM和Flash之间的Cookies数据。实际的物理数据操作在基类WebSyncManager中完成。~~DataLoader.java~~android 4.0 WebKit中不再使用DateSorter.java日期排序DebugFlags.java定义调试标志DeviceMotionAndroidOrientationManager.java用于实现DeviceMotion和DeviceOrientationDeviceMotionService.java实现SensorEventListener接口，处理动作DeviceOrientationService.java实现SensorEventListener接口，处理方向变化DownloadLister.java下载侦听器接口~~FileLoader.java~~android 4.0 WebKit中不再使用FindActionModeCallback.java？FrameLoader.javaFrame载入器，用于载入网页Frame数据GeolocationPermission.java用于管理浏览器UI的位置信息权限GeolocationService.java实现java侧的GeolocationServiceAndroidHTML5Audio.javaHTML5 audio支持类HTML5VideoFullScreen.java全屏视频视图，仅提供给浏览器使用HTML5VideoInline.java内嵌视频视图，仅提供给浏览器使用HTML5VideoView.java视频视图，仅提供给浏览器使用HTML5VideoViewProxy.javaHTML5视频视图代理类HttpAuthHandler.javaHTTP认证请求，需要用户处理HttpAuthHandlerImpl.javaHttpAuthHandler实现，仅用于Android Java HTTP stackJniUtil.java供JNI使用的实用类，用于获取cache目录等C代码无法直接获取的信息，以及读取资源包中的文件等JsPromptResult.javaJs结果提示对象，用于向用户提示Javascript运行结果。JsResult.javaJs结果对象，用于用户交互JWebCoreJavaBridge.java用Java与WebCore库中Timer和Cookies对象交互的桥接代码。KeyStoreHandler.javahttps相关处理L10nUtils.java字符串国际化，在使用chrome http stack时用到~~LoadListener.java~~载入器侦听器，用于处理载入器侦听消息。android 4.0 WebKit中不再使用MimeTypeMap.javaMIME类型映射MockGeolocation.java模拟地理位置信息Network.java该对象封装网络连接逻辑，为调用者提供更为高级的网络连接接口。OverScrollGlow.java用于实现OverScroller效果PerfChecker.java性能测试Plugin.java插件处理相关PluginData.java插件处理相关PluginFullScreenHolder.java插件处理相关PluginList.java插件处理相关PluginManager.java插件处理相关PluginStub.java插件处理相关SearchBox.java定义搜索对话框接口SearchBoxImpl.java搜索对话框接口实现SelectActionModeCallback.java？SslCertLookupTable.javahttps相关处理SslClientCertLookupTable.javahttps相关处理SslErrorHandler.javahttps相关处理SslErrorHandlerImpl.javahttps相关处理~~StreamLoader.java~~android 4.0 WebKit中不再使用~~UrlInterceptHandler.java~~用于google gears，已废弃~~UrlInterceptRegistry.java~~用于google gears，已废弃URLUtil.javaURL处理实用类ValueCallback.java回调接口，用于异步返回数据值ViewManager.java子视图管理类，主要用于管理插件视图ViewStateSerializer.javaWebView视图序列化和反序列化WebBackForwardList.java该对象包含WebView对象中显示的历史数据。WebBackForwardListClient.java浏览历史处理的客户接口类，所有需要接收浏览历史改变的类都需要实现该接口。WebChromeClient.javaChrome客户基类，Chrome客户对象在浏览器文档标题、进度条、图标改变时候会得到通知。WebHistoryItem.java该对象用于保存一条网页历史数据WebIconDatabase.java图标数据库管理对象，所有的WebView均请求相同的图标数据库对象WebResourceResponse.java封装某个资源的响应信息WebSettings.javaWebView的管理设置数据，该对象数据是通过JNI接口从底层获取。WebStorage.java处理webstorage数据库WebSyncManager.java数据同步对象，用于RAM数据和FLASH数据的同步操作。WebTextView.java在html文本输入控件激活时，显示系统原生编辑组件WebView.javaWeb视图对象，用于基本的网页数据载入、显示等UI操作。WebViewClient.javaWeb视图客户对象，在Web视图中有事件产生时，该对象可以获得通知。WebViewCore.java该对象对WebCore库进行了封装，将UI线程中的数据请求发送给WebCore处理，并且通过CallbackProxy的方式，通过消息通知UI线程数据处理的结果。WebViewDatabase.java该对象使用SQLiteDatabase为WebCore模块提供数据存取操作。WebViewFragment.java实现WebView嵌入到Fragment中WebViewWorker.java实现html5 workers，在UI线程和webkit线程开启单独的线程ZoomControlBase.java缩放控件接口ZoomControlEmbedded.java内置缩放控件ZoomControlExternal.java扩展缩放控件，已废弃ZoomManager.java维护WebView的缩放状态#####
#####2.1.2 Java层主要类关系图
WebKit Java层包含79个Java文件，主要的类关系图如下：
[![Java layer class diagram](http://mogoweb.net/wp-content/uploads/2012/02/Javalayerclassdiagram_thumb.png)](http://mogoweb.net/wp-content/uploads/2012/02/Javalayerclassdiagram.png)
#####1）WebView
WebView类是WebKit模块Java层的视图类，所有需要使用Web浏览功能的Android应用程序都要创建该视图对象显示和处理请求的网络资源。目前，WebKit模块支持HTTP、HTTPS、FTP以及javascript请求。WebView作为应用程序的UI接口，为用户提供了一系列的网页浏览、用户交互接口，客户程序通过这些接口访问WebKit核心代码。
**2）WebViewDatabase**
WebViewDatabase是WebKit模块中针对SQLiteDatabase对象的封装，用于存储和获取运行时浏览器保存的缓冲数据、历史访问数据、浏览器配置数据等。该对象是一个单实例对象，通过getInstance方法获取WebViewDatabase的实例。WebViewDatabase是WebKit模块中的内部对象，仅供WebKit框架内部使用。
**3）WebViewCore**
WebViewCore类是Java层与C层WebKit核心库的交互类，客户程序调用WebView的网页浏览相关操作会转发给BrowserFrame对象。当WebKit核心库完成实际的数据分析和处理后会回调WebViweCore中定义的一系列JNI接口，这些接口会通过CallbackProxy将相关事件通知相应的UI对象。
**4）CallbackProxy**
CallbackProxy是一个代理类，用于UI线程和WebCore线程交互。该类定义了一系列与用户相关的通知方法，当WebCore完成相应的数据处理，则会调用CallbackProxy类中对应的方法，这些方法通过消息方式间接调用相应处理对象的处理方法。
**5）**BrowserFrame****
BrowserFrame类负责URL资源的载入、访问历史的维护、数据缓存等操作，该类会通过JNI接口直接与WebKit C层库交互。
**6）**JWebCoreJavaBridge****
该类为Java层WebKit代码提供与C层WebKit核心部分的Timer和Cookies操作相关的方法。
**7）**WebSettings****
该对象描述了WEB浏览器访问相关的用户配置信息。
**8）**DownloadListener****
下载侦听接口，如果客户代码实现该接口，则在下载开始、失败、挂起、完成等情况下，DownloadManagerCore对象会调用客户代码中实现的DwonloadListener方法。
**9）**WebBackForwardList****
WebBackForwarList对象维护着用户访问历史记录，该类为客户程序提供操作访问浏览器历史数据的相关方法。
**10）**WebViewClient****
WebViewClient类定义了一系列事件方法，如果Android应用程序设置了WebViewClient派生对象，则在页面载入、资源载入、页面访问错误等情况发生时，该派生对象的相应方法会被调用。
**11）**WebBackForwardListClient****
WebBackForwardListClient对象定义了对访问历史操作时可能产生的事件接口，当用户实现了该接口，则在操作访问历史时（访问历史移除、访问历史清空等）用户会得到通知。
**12）**WebChromeClient****
WebChromeClient类定义了与浏览窗口修饰相关的事件。例如接收到Title、接收到Icon、进度变化时，WebChromeClient的相应方法会被调用。
#####2.1.3 流载入器（已废弃）
在Android 4.0之前的版本，数据载入都是在Java层实现的，从4.0开始，Android webkit引入了chromium的部分代码，输入载入走的是C++代码。不过原有的Java代码仍然保留，可以在编译webkit时用USE_CHROME_NETWORK_STACK宏进行切换。
####2.2 C层框架
#####2.2.1 C类与Java类的关系
WebKit类一般被拆成两个，Java类和C++类。比如在Java API部分，有一个WebView类，在C++部分，也有一个WebView类。WebViewCore, WebSettings等等也是同样的。
需要注意的是，JNI是C语言接口，所以Java类并不能直接调用C++代码，需要在C++代码中export出C语言接口。所以代码中使用了一个技巧，在Java类中定义一个int成员变量（实际上是一个指针），指向对应的C++类，如下图所示：
**[![AndroidNativeClass](http://mogoweb.net/wp-content/uploads/2012/03/AndroidNativeClass_thumb.png)](http://mogoweb.net/wp-content/uploads/2012/03/AndroidNativeClass.png)**
**1．BrowserFrame**
与BrowserFrame Java类相对应的C++类为WebFrame(文件名为WebCoreFrameBridge.cpp)，该类为Dalvik虚拟机回调BrowserFrame类中定义的本地方法进行了封装。与BrowserFrame中回调函数（Java层）相对应的C层结构定义如下：
struct WebFrame::JavaBrowserFrame 
{ 
    jweak mObj; 
    jweak mHistoryList; // WebBackForwardList object 
    jmethodID mStartLoadingResource; 
    jmethodID mMaybeSavePassword; 
    jmethodID mShouldInterceptRequest; 
    jmethodID mLoadStarted; 
    jmethodID mTransitionToCommitted; 
    jmethodID mLoadFinished; 
    jmethodID mReportError; 
    jmethodID mSetTitle; 
    jmethodID mWindowObjectCleared; 
    jmethodID mSetProgress; 
    jmethodID mDidReceiveIcon; 
    jmethodID mDidReceiveTouchIconUrl; 
    jmethodID mUpdateVisitedHistory; 
    jmethodID mHandleUrl; 
    jmethodID mCreateWindow; 
    jmethodID mCloseWindow; 
    jmethodID mDecidePolicyForFormResubmission; 
    jmethodID mRequestFocus; 
    jmethodID mGetRawResFilename; 
    jmethodID mDensity; 
    jmethodID mGetFileSize; 
    jmethodID mGetFile; 
    jmethodID mDidReceiveAuthenticationChallenge; 
    jmethodID mReportSslCertError; 
    jmethodID mRequestClientCert; 
    jmethodID mDownloadStart; 
    jmethodID mDidReceiveData; 
    jmethodID mDidFinishLoading; 
    jmethodID mSetCertificate; 
    jmethodID mShouldSaveFormData; 
    jmethodID mSaveFormData; 
    jmethodID mAutoLogin; 
    AutoJObject frame(JNIEnv* env) { 
        return getRealObject(env, mObj); 
    } 
    AutoJObject history(JNIEnv* env) { 
         return getRealObject(env, mHistoryList); 
    } 
};
该结构作为WebFrame（C层）的一个成员变量（mJavaFrame），在WebFrame构造函数中，用BrowserFrame（Java层）类的回调方法的method ID初始化JavaBrowserFrame结构的各个域。初始后，当WebCore（C层）在剖析网页数据时，有Frame相关的资源改变，比如WEB页面的主题变化，则会通过mJavaFrame结构，调用指定BrowserFrame对象的相应方法，通知Java层处理。
**2．JWebCoreJavaBridge**
与该对象相对应的C层对象为JavaBridge，JavaBridge对象继承了TimerClient, CookieClient, KeyGenerateorClient, FileSystemClient类，主要负责WebCore中的定时器和Cookie管理。与Java层JWebCoreJavaBridge类中方法method ID相关的是JavaBridege中几个成员变量，在构造JavaBridge对象时，会初始化这些成员变量，之后有Timer或者Cookies事件产生，WebCore会通过这些ID值，回调对应JWebCoreJavaBridge的相应方法。
**3．LoadListener**
与该对象相关的C层对象为WebCoreResourceLoader，与LoaderListener中回调函数（Java层）相对应的C层结构是struct resourceloader_t，该结构保存了LoadListener对象ID、CancelMethod ID以及DownloadFiledMethod ID等值。当有Cancel或者Download事件产生，WebCore会回调LoadListener类中的CancelMethod或者DownloadFileMethod。
**4．WebViewCore**
与WebViewCore相关的C类是WebViewCorel，定义了两个数据结构，一个是WebViewCoreFields，对应于Java层WebViewCore对象的成员变量，另一个是WebViewCore::JavaGlue，对应于Java层WebViewCore对象的成员方法。定义如下：
// Field ids for WebViewCore 
struct WebViewCoreFields { 
    jfieldID m_nativeClass; 
    jfieldID m_viewportWidth; 
    jfieldID m_viewportHeight; 
    jfieldID m_viewportInitialScale; 
    jfieldID m_viewportMinimumScale; 
    jfieldID m_viewportMaximumScale; 
    jfieldID m_viewportUserScalable; 
    jfieldID m_viewportDensityDpi; 
    jfieldID m_webView; 
    jfieldID m_drawIsPaused; 
    jfieldID m_lowMemoryUsageMb; 
    jfieldID m_highMemoryUsageMb; 
    jfieldID m_highUsageDeltaMb; 
} gWebViewCoreFields;
// —————————————————————————-
struct WebViewCore::JavaGlue { 
    jweak m_obj; 
    jmethodID m_scrollTo; 
    jmethodID m_contentDraw; 
    jmethodID m_layersDraw; 
    jmethodID m_requestListBox; 
    jmethodID m_openFileChooser; 
    jmethodID m_requestSingleListBox; 
    jmethodID m_jsAlert; 
    jmethodID m_jsConfirm; 
    jmethodID m_jsPrompt; 
    jmethodID m_jsUnload; 
    jmethodID m_jsInterrupt; 
    jmethodID m_didFirstLayout; 
    jmethodID m_updateViewport; 
    jmethodID m_sendNotifyProgressFinished; 
    jmethodID m_sendViewInvalidate; 
    jmethodID m_updateTextfield; 
    jmethodID m_updateTextSelection; 
    jmethodID m_clearTextEntry; 
    jmethodID m_restoreScale; 
    jmethodID m_needTouchEvents; 
    jmethodID m_requestKeyboard; 
    jmethodID m_requestKeyboardWithSelection; 
    jmethodID m_exceededDatabaseQuota; 
    jmethodID m_reachedMaxAppCacheSize; 
    jmethodID m_populateVisitedLinks; 
    jmethodID m_geolocationPermissionsShowPrompt; 
    jmethodID m_geolocationPermissionsHidePrompt; 
    jmethodID m_getDeviceMotionService; 
    jmethodID m_getDeviceOrientationService; 
    jmethodID m_addMessageToConsole; 
    jmethodID m_formDidBlur; 
    jmethodID m_getPluginClass; 
    jmethodID m_showFullScreenPlugin; 
    jmethodID m_hideFullScreenPlugin; 
    jmethodID m_createSurface; 
    jmethodID m_addSurface; 
    jmethodID m_updateSurface; 
    jmethodID m_destroySurface; 
    jmethodID m_getContext; 
    jmethodID m_keepScreenOn; 
    jmethodID m_sendFindAgain; 
    jmethodID m_showRect; 
    jmethodID m_centerFitRect; 
    jmethodID m_setScrollbarModes; 
    jmethodID m_setInstallableWebApp; 
    jmethodID m_enterFullscreenForVideoLayer; 
    jmethodID m_setWebTextViewAutoFillable; 
    jmethodID m_selectAt; 
    AutoJObject object(JNIEnv* env) { 
        // We hold a weak reference to the Java WebViewCore to avoid memeory 
        // leaks due to circular references when WebView.destroy() is not 
        // called manually. The WebView and hence the WebViewCore could become 
        // weakly reachable at any time, after which the GC could null our weak 
        // reference, so we have to check the return value of this method at 
        // every use. Note that our weak reference will be nulled before the 
        // WebViewCore is finalized. 
        return getRealObject(env, m_obj); 
    } 
};
WebViewCore类有个JavaGlue对象作为成员变量，在构建WebViewCore对象时，用WebViewCore（Java层）中的方法ID值初始化该成员变量。并且会将构建的WebViewCore对象指针复制给WebViewCore（Java层）的mNativeClass，这样将WebViewCore（Java层）和WebViewCore（C层）关联起来。
**5．WebSettings**
与WebSettings相关的C层结构是struct FieldIds(文件名WebSettings.cpp)，该结构保存了WebSettings类中定义的属性ID以及方法ID，在构建FieldIds对象时，会设置这些方法和属性的ID值。
**6．WebView**
与WebView相关的C层类是WebView，该类中的m_javaGlue中保存着WebView（Java层）中定义的属性和方法ID，在WebView（C层）构造方法中初始化，并且将构造的WebView对象（C层）的指针，赋值给WebView类（Java层）的mNativeClass变量，这样WebView（Java层）和WebView对象（C层）建立了关系。
####三、基本流程分析
#####3.1 webkit初始化
Android提供了WebView类，该类提供客户化浏览显示的功能。如果客户需要加入浏览器的支持，可像使用其它视图类一样加入应用程序，显示给用户。当客户代码中第一次生成WebView对象时，会初始化WebKit库（包括Java层和C层两个部分），之后用户可以操作WebView对象完成网络或者本地资源的访问。
WebView对象的生成主要涉及4个类CallbackProxy、WebViewCore、WebViewDatabase以及BrowserFrame。其中CallbackProxy对象为WebKit模块中UI线程和WebKit类库提供交互功能，WebViewCore是WebKit的核心层，负责与C层交互以及WebKit模块C层类库初始化，WebViewDatabase为WebKit模块运行时缓存、cookie等数据存储提供支持，BrowserFrame用于创建WebCore中的Frame，并为Frame提供Java层回调方法。WebKit模块初始化流程如下：
实例化WebView
- 创建CallbackProxy对象
- 创建WebViewCore对象1. 调用System.loadLibrary载入webcore相关类库（C层）
2. 如果是第一次初始化WebViewCore对象，创建WebCoreTherad线程
3. 创建EventHub对象，处理WebViewCore事件
4. 获取WebIconDatabase对象实例
5. 向WebCoreThread发送初始化消息- 创建BrowserFrame对象
- 向WebView发送WEBCORE_INTIALIZED_MSG_ID消息，通知初始化完成




- 获取WebViewDatabase实例
- 调用init初始化WebView
- 收到WEBCORE_INITIALIZED_MSG_ID消息后，调用nativeCreate

######3.1.1 JNI native方法注册
在创建WebViewCore时进行，调用System.loadLibrary方法载入webcore相关类库，该过程由Dalvik虚拟机完成，它会从动态链接库目录中寻找libWebCore.so类库，载入到内存中，并且调用WebKit初始化模块的JNI_OnLoad方法（代码见WebCoreJniOnLoad.cpp）。WebKit模块的JNI_OnLoad方法中完成了如下初始化操作：
1. 初始化JavaBridge[*registerJavaBridge*]
获取JWebCoreJavaBridge类的mNativeBridge成员变量的fieldID，以及注册JWebCoreJavaBridge类中的native方法
2. 初始化JniUtil[*registerJniUtil*]
注册JniUtil类中的native方法
3. 初始化WebFrame[*registerWebFrame*]
获取BrowserFrame类的mNativeFrame成员变量的ID，以及注册BrowserFrame类中的native方法
4. 初始化WebCoreResourceLoader[*registerResourceLoader*]
获取LoadListener类的mNativeLoader成员的ID，以及注册LoadListener类中的native方法
5. 初始化WebViewCore[*registerWebViewCore*]
获取WebViewCore类的java成员的ID，以及注册WebViewCore类中的native方法
6. 初始化WebHistory[*registerWebHistory*]
获取WebHistoryItem类的java成员的ID，以及注册WebBackForwardList和WebHistoryItem类中的native方法
7. 初始化WebIconDatabase[*registerWebIconDatabase*]
注册WebIconDatabase类中的native方法
8. 初始化WebSettings[*registerWebSettings*]
获取WebSettings类的java成员的ID，以及注册native方法
9. 初始化WebStorage[*registerWebStorage*]
注册WebStorage类的native方法
10. 初始化WebView[*registerWebView*]
获取WebView类的mNativeClass成员的ID，以及注册native方法
11. 初始化ViewStateSerializer[*registerViewStateSerializer*]
注册ViewStateSerializer类的native方法
12. 初始化GeolocationPermissions[*registerGeolocationPermissions*]
注册GeolocationPermissions类的native方法
13. 初始化MockGeolocation[*registerMochGeolocation*]
注册MockGeolocation类的native方法
14. 初始化HTML5Audio[*registerMediaPlayerAudio*]
注册HTML5 Audio类的native方法
15. 初始化HTML5Video[*registerMediaPlayerVideo*]
注册HTML5VideoViewProxy类的native方法
16. 初始化DeviceMotionAndOrientationManager[*registerDeviceMotionAndOrientationManager*]
注册DeviceMotionAndOrientationManager类的native方法
17. 初始化CookieManager[registerCookieManager]
注册CookieManager类的native方法
18. 初始化CacheManager[registerCacheManager]
注册CacheManager类的native方法
######3.1.2  UI线程和webcore线程
webcore线程在第一次创建WebViewCore对象时创建， 且只创建一次，该线程负责处理WebCore初始化事件。WebViewCore构造函数会被阻塞，直到WebCoreThread初始化完成。在WebViewCore对象构造最后一步，发送INITIALIZE消息给WebCoreThread，执行webcore相关的初始化（WebViewCore::initialize）。在WebViewCore::initialize方法中，会创建BrowserFrame对象，并且向WebView对象发送WEBCORE_INITIALIZED_MSG_ID消息。WebView收到消息后，会执行nativeCreate方法，创建c层的WebView对象。
######3.1.3 初始化过程序列图
[![Initialize Sequence](http://mogoweb.net/wp-content/uploads/2012/02/InitializeSequence_thumb.png)](http://mogoweb.net/wp-content/uploads/2012/02/InitializeSequence.png)
#####3.2 loadData
loadData用于加载"data:”形式的url，通过该方法，可以将文件内容读入到字符串，然后通过loadData进行加载，是最简单的一种数据加载方法。比如：
> webview.loadData(“<html><body>hello</body></html>”, "text/html”, "utf-8”);
######3.2.1 loadData序列图
[![LoadData Sequence](http://mogoweb.net/wp-content/uploads/2012/02/LoadDataSequence_thumb.png)](http://mogoweb.net/wp-content/uploads/2012/02/LoadDataSequence.png)
