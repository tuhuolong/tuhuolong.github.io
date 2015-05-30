---
layout: post
title: Android JNI_OnLoad()函数
date: 2012-04-25 15:51:00
categories: [Android]
tags: [jni, android, initialization, c, jvm]
---
JVM加载完本地库会调用JNI_OnLoad()函数

当Android的 VM(VirtualMachine)执行到C组件(即*so文件)里的**System**.loadLibrary()函数时，首先会去执行C组件里的JNI_OnLoad()函数。它的用途有二：
1.       告诉VM此C组件使用那一个JNI版本。如果你的*.so文件没有提供JNI_OnLoad()函数，VM会默认该*.so檔是使用最老的JNI 1.1版本。由于新版的JNI做了许多扩充，如果需要使用JNI的新版功能，例如JNI 1.4的 java.nio.ByteBuffer, 就必须藉由JNI_OnLoad()函数来告知VM。
2.       由于VM执行到System.loadLibrary()函数时，就会立即先呼叫JNI_OnLoad()，所以C组件的开发者可以藉由JNI_OnLoad()来进行C组件内的初期值之设定(Initialization)。
 
例如，在Android的/system/lib/libmedia_jni.so档案里，就提供了JNI_OnLoad()函数，其程序代码片段为：
 
//#define LOG_NDEBUG 0
#define LOG_TAG "MediaPlayer-JNI"
………
**jint **JNI_OnLoad(JavaVM* vm, void* reserved)
{
    JNIEnv* env = NULL;
    jint result = -1;
 
    if (vm->GetEnv((void**) &env, JNI_VERSION_1_4) != JNI_OK) {
        LOGE("ERROR: GetEnv failed\n");
        goto bail;
    }
    assert(env != NULL);
    if (register_android_media_MediaPlayer(env) < 0) {
        LOGE("ERROR: MediaPlayer native registration failed\n");
        goto bail;
    }
    if (register_android_media_MediaRecorder(env) < 0) {
        LOGE("ERROR: MediaRecorder native registration failed\n");
        goto bail;
    }
    if (register_android_media_MediaScanner(env) < 0) {
        LOGE("ERROR: MediaScanner native registration failed\n");
        goto bail;
    }
    if (register_android_media_MediaMetadataRetriever(env) < 0) {
        LOGE("ERROR: MediaMetadataRetriever native registration failed\n");
        goto bail;
    }
    /* success -- return valid version number */
    result = JNI_VERSION_1_4;
bail:
    return result;
}
// KTHXBYE
 
    此函数回传JNI_VERSION_1_4值给VM，于是VM知道了其所使用的JNI版本了。此外，它也做了一些初期的动作(可呼叫任何本地函数)，例如指令：
 
   if (register_android_media_MediaPlayer(env) < 0) {
        LOGE("ERROR: MediaPlayer native registration failed\n");
        goto bail;
    }
 
就将此组件提供的各个本地函数(NativeFunction)登记到VM里，以便能加快后续呼叫本地函数之效率。
    JNI_OnUnload()函数与JNI_OnLoad()相对应的。在加载C组件时会立即呼叫JNI_OnLoad()来进行组件内的初期动作；而当VM释放该C组件时，则会呼叫JNI_OnUnload()函数来进行善后清除动作。当VM呼叫JNI_OnLoad()或JNI_Unload()函数时，都会将VM的指标(Pointer)传递给它们，其参数如下：
 
**jint **JNI_OnLoad(JavaVM* vm, void* reserved)
{
      ………
}
 
**jint **JNI_OnUnload(JavaVM* vm, void* reserved)
{
      ………
}
 
在JNI_OnLoad()函数里，就透过VM之指标而取得JNIEnv之指标值，并存入env指针变量里，如下述指令：
 
**jint **JNI_OnLoad(JavaVM* vm, void* reserved)
{
    JNIEnv* env = NULL;
    jint result = -1;
 
    if (vm->GetEnv((void**) &env, JNI_VERSION_1_4) != JNI_OK) {
        LOGE("ERROR: GetEnv failed\n");
        goto bail;
    }
 
   由于VM通常是多执行绪(Multi-threading)的执行环境。每一个执行绪在呼叫JNI_OnLoad()时，所传递进来的JNIEnv指标值都是不同的。为了配合这种多执行绪的环境，C组件开发者在撰写本地函数时，可藉由JNIEnv指标值之不同而避免执行绪的数据冲突问题，才能确保所写的本地函数能安全地在Android的多执行绪VM 里安全地执行。基于这个理由，当在呼叫C组件的函数时，都会将JNIEnv指标值传递给它，如下：
**jint **JNI_OnLoad(JavaVM* vm, void* reserved)
{
JNIEnv* env = NULL;
  ……….
    if (register_android_media_MediaPlayer(env) < 0) {
       …….
    }
     }
这JNI_OnLoad()呼叫register_android_media_MediaPlayer(env)函数时，就将env指标值传递过去。如此，在register_android_media_MediaPlayer()函数就能藉由该指标值而区别不同的执行绪，以便化解数据冲突的问题。
      例如，在register_android_media_MediaPlayer()函数里，可撰写下述指令：
       if ((*env)->MonitorEnter(env, obj) != JNI_OK) {
            ………
 }
 
    查看是否已经有其它执行绪进入此对象，如果没有，此执行绪就进入该对象里执行了。还有，也可撰写下述指令：
 
        if ((*env)->MonitorExit(env, obj) != JNI_OK) {
             ………
         }
 
查看是否此执行绪正在此对象内执行，如果是，此执行绪就会立即离开。
