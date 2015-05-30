---
layout: post
title: android类加载: 内部引用类同dex文件(不包括BootClassLoader加载类), 否则加载失败
date: 2015-04-22 16:57:00
categories: [Android]
tags: []
---

Resolve.cpp
	if (!fromUnverifiedConstant &&
	    IS_CLASS_FLAG_SET(referrer, CLASS_ISPREVERIFIED))
	{
	    ClassObject* resClassCheck = resClass;
	    if (dvmIsArrayClass(resClassCheck))
	        resClassCheck = resClassCheck->elementClass;
	
	    if (referrer->pDvmDex != resClassCheck->pDvmDex &&
	        resClassCheck->classLoader != NULL)
	    {
	        ALOGW("Class resolved by unexpected DEX:"
	             " %s(%p):%p ref [%s] %s(%p):%p",
	            referrer->descriptor, referrer->classLoader,
	            referrer->pDvmDex,
	            resClass->descriptor, resClassCheck->descriptor,
	            resClassCheck->classLoader, resClassCheck->pDvmDex);
	        ALOGW("(%s had used a different %s during pre-verification)",
	            referrer->descriptor, resClass->descriptor);
	        dvmThrowIllegalAccessError(
	            "Class ref in pre-verified class resolved to unexpected "
	            "implementation");
	        return NULL;
	    }
	}
