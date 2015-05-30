---
layout: post
title: JavaScriptCore API 和V8 API
date: 2011-12-21 15:26:00
categories: [JavaScript]
tags: [api, javascript, primitive, accessor, exception, integer]
---
JavaScriptCore主要正调接口

*JavaScriptCore中的主要正调API*
*API描述*
JSClassCreate
创建一个JavaScript对象的类，这个类可以用以创建JavaScript对象。
JSObjectMake
使用JSClassCreate创建的类创建一个JavaScript对象。
JSObjectMakeFunctionWithCallback
使用一个本地方法作为回调创建一个JavaScript方法，当这个JavaScript方法被调用时，将触发这个回调。
JSObjectMakeConstructor
创建一个JavaScript构造器对象，构造器可以用以在JavaScript环境中使用new操作符创建新的对象。
JSObjectMakeArray
创建一个数组对象。
JSObjectMakeDate
创建一个日期对象。
JSObjectMakeError
创建一个错误对象。
JSObjectHasProperty
验证对象是否具有某个属性。
JSObjectGetProperty
根据属性名称获得对象的某个属性。
JSObjectSetProperty
设置对象的某个属性。
JSObjectDeleteProperty
从对象上删除某个属性。
JSObjectGetPrivate
获取对象的私有数据。
JSObjectSetPrivate
设置对象的私有数据
JSObjectCallAsFunction
将对象作为JavaScript方法调用。
JSObjectIsConstructor
验证对象是否为JavaScript构造器。
JSObjectCallAsConstructor
将对象作为JavaScript构造器调用。
JavaScriptCore中主要回调接口

*JavaScriptCore中的回调API*
*API描述*
JSObjectInitializeCallback
当JavaScript对象被创建时被调用。
JSObjectFinalizeCallback
当JavaScript对象被销毁时被调用。
JSObjectHasPropertyCallback
当查询JavaScript对象是否具有某个属性时被调用。
JSObjectGetPropertyCallback
当获取JavaScript对象的某个属性时被调用。
JSObjectSetPropertyCallback
当对JavaScript对象设置某个属性时被调用。
JSObjectDeletePropertyCallback
当移除JavaScript对象的某个属性时被调用。
JSObjectGetPropertyNamesCallback
当获取JavaScript对象的所有属性名时被调用。
JSObjectCallAsFunctionCallback
当JavaScript对象被作为方法被调用时被调用。
JSObjectCallAsConstructorCallback
当JavaScript对象被作为构造器被调用时被调用。
JSObjectHasInstanceCallback
当JavaScript对象被作为instanceof操作符的操作数时被调用。
JSObjectConvertToTypeCallback
当JavaScript对象进行显式的类型转换时被调用。
--------------------------------------------------------------------------------------------------------------

V8主要正调接口

*V8中的主要正调API*
*API描述*
v8::ObjectTemplat
对象模板，用以生成JavaScript对象。
v8::FunctionTemplat
方法模板，用以生成JavaScript方法。
v8::Value
JavaScript值，是所有不同类型的JavaScript对象的基类。
v8::Primitive
原始数据对象，用以封装原始数据类型。
v8::Boolean
布尔型数据对象，用以封装布尔型数据类型。
v8::Number
数字型数据对象，用以封装数字型数据类型。
v8::String
字符串数据对象，用以封装字符串数据类型。
v8::Integer
整数型数据对象，用以封装整数型数据类型。
v8::Object
对象类型数据对象，用以封装对象数据类型。
v8::Array
数组类型数据对象，用以封装数组数据类型。
v8::String
字符串数据对象，用以封装字符串数据类型。
v8::Function
方法类型数据对象，用以封装方法数据类型。
v8::Exception
异常工厂类，用以创建各种类型的异常。
v8::Arguments
参数对象，包含了方法调用时的所有信息。
v8主要回调接口

*V8中的回调API*
*API描述*
Accessor
针对对象的某个属性的回调接口，当某个特定的属性被访问时触发回调。
Interceptors
针对对象的全部属性的回调接口，当对象的任意属性被访问时触发回调。



