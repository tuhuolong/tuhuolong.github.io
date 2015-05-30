---
layout: post
title: gtest的使用 (android)
date: 2012-08-09 17:25:00
categories: [Android, Tuhuolong]
tags: [android, module, extension, testing, include, build]
---
Android.mk
LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)

LOCAL_CPP_EXTENSION := .cc
LOCAL_MODULE := gtest2
LOCAL_MODULE_CLASS := EXECUTABLES
LOCAL_MODULE_TAGS := optional
LOCAL_SRC_FILES := \
    test1.cc
LOCAL_C_INCLUDES := \
external/gtest/include \
browser-os/stlport/stlport \
bionic
LOCAL_STATIC_LIBRARIES += libgtest libgtest_main libstlport_zeus_static
#LOCAL_STATIC_LIBRARIES += libgtest libstlport_zeus_static
include $(BUILD_EXECUTABLE)
=================================================================
test1.cc
#include <gtest/gtest.h>
int Foo(int a, int b)
{
    return a+b;
}
TEST(FooTest, IntAdd)
{
    EXPECT_EQ(9, Foo(4, 5));
}
/*
int main(int argc, char **argv)
{
    testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}*/
=================================================================

int main(int argc, char **argv)
{
    testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();

}
和 libgtest_main使用一个
=================================================================

gtest库位于android/external目录
     
"gtest最终编译成可执行文件在手机/PC里运行"

    
   
