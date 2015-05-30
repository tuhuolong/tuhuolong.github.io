---
layout: post
title: dalvik Java类库中本地类
date: 2010-12-24 13:49:00
categories: [Java]
tags: [java, system, apache, constructor, class, security]
---
dalvik中Java类库有许多本地类(全本地方法)
当(Java/C++)调用(Java类库中)本地类的本地方法时,ResolveNativeMethod:查映射表-->本地类的本地函数集-->本地函数指针
 
类库的本地函数集
static DalvikNativeClass gDvmNativeMethodSet[] = {
    { "Ljava/lang/Object;",               dvm_java_lang_Object, 0 },
    { "Ljava/lang/Class;",                dvm_java_lang_Class, 0 },
    { "Ljava/lang/Runtime;",              dvm_java_lang_Runtime, 0 },
    { "Ljava/lang/String;",               dvm_java_lang_String, 0 },
    { "Ljava/lang/System;",               dvm_java_lang_System, 0 },
    { "Ljava/lang/SystemProperties;",     dvm_java_lang_SystemProperties, 0 },
    { "Ljava/lang/Throwable;",            dvm_java_lang_Throwable, 0 },
    { "Ljava/lang/VMClassLoader;",        dvm_java_lang_VMClassLoader, 0 },
    { "Ljava/lang/VMThread;",             dvm_java_lang_VMThread, 0 },
    { "Ljava/lang/reflect/AccessibleObject;",dvm_java_lang_reflect_AccessibleObject, 0 },
    { "Ljava/lang/reflect/Array;",        dvm_java_lang_reflect_Array, 0 },
    { "Ljava/lang/reflect/Constructor;",dvm_java_lang_reflect_Constructor, 0 },
    { "Ljava/lang/reflect/Field;",        dvm_java_lang_reflect_Field, 0 },
    { "Ljava/lang/reflect/Method;",       dvm_java_lang_reflect_Method, 0 },
    { "Ljava/lang/reflect/Proxy;",        dvm_java_lang_reflect_Proxy, 0 },
    { "Ljava/security/AccessController;",dvm_java_security_AccessController, 0 },
    { "Ljava/util/concurrent/atomic/AtomicLong;",dvm_java_util_concurrent_atomic_AtomicLong, 0 },
    { "Ldalvik/system/VMDebug;",          dvm_dalvik_system_VMDebug, 0 },
    { "Ldalvik/system/DexFile;",          dvm_dalvik_system_DexFile, 0 },
    { "Ldalvik/system/VMRuntime;",        dvm_dalvik_system_VMRuntime, 0 },
    { "Ldalvik/system/Zygote;",           dvm_dalvik_system_Zygote, 0 },
    { "Ldalvik/system/VMStack;",          dvm_dalvik_system_VMStack, 0 },
    { "Lorg/apache/harmony/dalvik/ddmc/DdmServer;",dvm_org_apache_harmony_dalvik_ddmc_DdmServer, 0 },
    { "Lorg/apache/harmony/dalvik/ddmc/DdmVmInternal;", dvm_org_apache_harmony_dalvik_ddmc_DdmVmInternal, 0 },
    { "Lorg/apache/harmony/dalvik/NativeTestTarget;",dvm_org_apache_harmony_dalvik_NativeTestTarget, 0 },
    { "Lsun/misc/Unsafe;",                dvm_sun_misc_Unsafe, 0 },
    { "Ldalvik/CAR/CARCallbackFunc;",     dvm_dalvik_CAR_CARCallbackFunc, 0 },
    { NULL, NULL, 0 },
};
 
具体的本地类(java.lang.Class)的本地函数集
const DalvikNativeMethod dvm_java_lang_Class[] = {
    { "classForName",       "(Ljava/lang/String;ZLjava/lang/ClassLoader;)Ljava/lang/Class;",
        Dalvik_java_lang_Class_classForName },
    { "getClassLoader",     "(Ljava/lang/Class;)Ljava/lang/ClassLoader;",
        Dalvik_java_lang_Class_getClassLoader },
    ......,
};
