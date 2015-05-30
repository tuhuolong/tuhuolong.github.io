---
layout: post
title: webkit如何实现JS DOM binding—基于V8分析
date: 2010-09-08 18:41:00
categories: [WebKit]
tags: [webkit, binding, js引擎, web, 编译器, 语言]
---
##转自[http://blog.sunchrome.com/?p=33](http://blog.sunchrome.com/?p=33)
##一个web页面需要使用到JS的场景
###Parser阶段
HTMLDocumentParser 中的HTMLTokenizer在解析到<script>开始标签时，创建HTMLScriptElement对象，在解析</script>时，开始解析JS代码啊，如果<script src=”">中含有src,那么发出一个异步请求(在异步请求过程中，parser会中断，等待JS的解析结果，期间Webkit会来做一些DNS预取，资源预解析等工作，总之WebKit是不会浪费这些宝贵时间的)，等src资源返回后开始解析。
###用户事件驱动
比如鼠标点击一个按钮
###解析状态
onload等函数
##WebKit中的JS执行的代码所在的位置
WebKit中所有的JS执行都是通过Frame中的ScriptController m_script;来完成，也就是说，所有的每个Frame只有一个JS入口。
ScriptController中含有一个V8Proxy的V8代理，所有执行过程都是通过V8Proxy来完成的。
如何把DOM接口的相关接口暴露给V8的？
要把DOM传递给V8调用就要解决两个问题，DOM对象怎么传递给V8？
DOM对象的中的方法怎么传递给V8？
还有诸如其他的Screen等对象是如何传递给V8的？
一个Frame中的V8运行环境，也就是上面文章所提到的Context,
W3C—WEB IDL
由该工作草案定义的接口定义语言（interface definition language）叫作Web IDL，它可被用来描述要在Web浏览器里实现的接口。Web IDL是一种IDL的变体，它所具有的很多特性使之能够更容易地对Web平台里的常用脚本对象的行为进行规定。为了支持过去只能以文字描述的常用功能，该IDL在很多方面得到了扩展。另外，它还为ECMAScript第3版和Java给出了精确的语言绑定。
回归WebKit核心代码WebCore中的源代码，可以发现有许多xxx.idl文件，这些文件就是需要暴露给JS解析引擎的接口。
以WebKit/WebCore/page/DOMWindow.h 为列，改文件主要在C++层次描述了一个Web页面的所有特性，包括 DOM,screen,History等等相关信息。这些信息都是通过WebKit/WebCore/page/DOMWindow.idl描述文件来描述的，DOMWindow.idl主要分为两个部分，属性和方法，DOMWindow.idl的代码太多了，就不在这里列举IDL文件内容了。其实xxx.idl文件在C++运行时并不起任何作用，真正起作用的是编译器利用 xxx.idl文件来生成的对应的C++源文件，由于通过xxx.idl接口描述的属性和功能规则比较简单，所以通过编译器通过perl语言，然后按照一定规则来生成对应的供JS引擎调用的C++源文件。
Chromium生成的V8接口对应的目录在src/chrome/Debug/obj/global_intermediate/webkit/bindings或者
src/chrome/Release/obj/global_intermediate/webkit/bindings.
通过分析这些动态生成的文件可以发现，他们的组成结构非常简单，
namespace WebCore {
class V8DOMWindow {
public:
static bool HasInstance(v8::Handle value);
static v8::Persistent GetRawTemplate();
static v8::Persistent GetTemplate();
static DOMWindow* toNative(v8::Handle);
static v8::Handle wrap(DOMWindow*);
static void derefObject(void*);
static WrapperTypeInfo info;
…忽略了下面的部分代码
};
v8::Handle toV8(DOMWindow*);
v8::Handle toV8(PassRefPtr);
}
然后CPP文件中是这样实现的
static const BatchedAttribute shadowAttrs[] = {
// Attribute ‘location’ (Type: ‘attribute’ ExtAttr: ‘V8DisallowShadowing DoNotCheckDomainSecurity CPPCustom V8CustomSetter JSCCustom’)
{“location”, DOMWindowInternal::locationAttrGetter, V8DOMWindow::locationAccessorSetter, 0 /* no data */, static_cast(v8::ALL_CAN_READ | v8::ALL_CAN_WRITE | v8::PROHIBITS_OVERWRITING), static_cast(v8::None | v8::DontDelete), 0 /* on instance */},
// Attribute ‘window’ (Type: ‘readonly attribute’ ExtAttr: ‘V8DisallowShadowing DoNotCheckDomainSecurity’)
{“window”, DOMWindowInternal::windowAttrGetter, 0, 0 /* no data */, static_cast(v8::ALL_CAN_READ | v8::PROHIBITS_OVERWRITING), static_cast(v8::None | v8::DontDelete), 0 /* on instance */},
// Attribute ‘top’ (Type: ‘attribute’ ExtAttr: ‘V8DisallowShadowing DoNotCheckDomainSecurityOnGet Replaceable V8ReadOnly’)
{“top”, DOMWindowInternal::topAttrGetter, 0, 0 /* no data */, static_cast(v8::ALL_CAN_READ | v8::PROHIBITS_OVERWRITING), static_cast(v8::None | v8::DontDelete), 0 /* on instance */},
};
static const BatchedAttribute DOMWindowAttrs[] = {
// Attribute ‘screen’ (Type: ‘readonly attribute’ ExtAttr: ”)
{“screen”, DOMWindowInternal::screenAttrGetter, 0, 0 /* no data */, static_cast(v8::DEFAULT), static_cast(v8::None), 0 /* on instance */},
// Attribute ‘history’ (Type: ‘readonly attribute’ ExtAttr: ‘JSCCustomGetter DoNotCheckDomainSecurity’)
{“history”, DOMWindowInternal::historyAttrGetter, 0, 0 /* no data */, static_cast(v8::ALL_CAN_READ), static_cast(v8::None), 0 /* on instance */}
……下面的代码忽略,
}
static const BatchedCallback DOMWindowCallbacks[] = {
{“getSelection”, DOMWindowInternal::getSelectionCallback},
{“print”, DOMWindowInternal::printCallback},
{“stop”, DOMWindowInternal::stopCallback},
{“open”, V8DOMWindow::openCallback},
{“showModalDialog”, V8DOMWindow::showModalDialogCallback},
{“alert”, DOMWindowInternal::alertCallback},
{“confirm”, DOMWindowInternal::confirmCallback},
{“prompt”, DOMWindowInternal::promptCallback},
{“find”, DOMWindowInternal::findCallback},
{“scrollBy”, DOMWindowInternal::scrollByCallback},
{“scrollTo”, DOMWindowInternal::scrollToCallback},
{“scroll”, DOMWindowInternal::scrollCallback},
{“moveBy”, DOMWindowInternal::moveByCallback},
{“moveTo”, DOMWindowInternal::moveToCallback},
{“resizeBy”, DOMWindowInternal::resizeByCallback},
{“resizeTo”, DOMWindowInternal::resizeToCallback},
{“getMatchedCSSRules”, DOMWindowInternal::getMatchedCSSRulesCallback},
{“setTimeout”, V8DOMWindow::setTimeoutCallback},
{“clearTimeout”, DOMWindowInternal::clearTimeoutCallback},
{“setInterval”, V8DOMWindow::setIntervalCallback},
{“clearInterval”, DOMWindowInternal::clearIntervalCallback},
{“atob”, DOMWindowInternal::atobCallback},
{“btoa”, DOMWindowInternal::btoaCallback},
{“addEventListener”, V8DOMWindow::addEventListenerCallback},
{“removeEventListener”, V8DOMWindow::removeEventListenerCallback},
{“captureEvents”, V8DOMWindow::captureEventsCallback},
{“releaseEvents”, V8DOMWindow::releaseEventsCallback},
};
注意到这两部分分别定义了JS所能调用的 属性和所使用的方法。
然后再把这些属性和方法注册到V8执行时所需要的Context中，其流程大致是这样的。
ScriptController::executeScript
ScriptController::evaluate
v8::Handle v8Context = V8Proxy::mainWorldContext(m_proxy->frame());
proxy->mainWorldContext();
V8DOMWindowShell::initContextIfNeeded
V8DOMWindowShell::createNewContext
V8DOMWindow::GetShadowObjectTemplate
static v8::Persistent ConfigureShadowObjectTemplate
void batchConfigureAttributes
void configureAttribute
(attribute.onProto ? proto : instance)->SetAccessor(v8::String::New(attribute.name),
attribute.getter,
attribute.setter,
v8::External::Wrap(attribute.data),
attribute.settings,
attribute.attribute);
注册函数的执行流程和上面这个流程类似。
这样V8就可以在指定的Context中调用所有WEB IDL中所规则的属性和方法了。
