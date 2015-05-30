---
layout: post
title: Android Makefile分析
date: 2011-03-09 10:14:00
categories: [Android]
tags: [makefile, android, module, class, path, include]
---
首先是可能用得到的基础知识，必须了解基础Makefile的语法，然后下面是Andriod用来编译相应模块使用的核心makefile，当然如果要速成的话也可以不看这些东西，直接按照后面的例子添加就可以了：
     1、prebuilt
      /build/core/base_rules.mk
      /build/core/prebuilt.mk
      /build/core/multi_prebuilt.mk
     2、.so/
      /build/core/base_rules.mk
      /build/core/shared_library.mk
      /build/core/dynamic_library.mk
      /build/core/binary.mk
     3、.a
      /build/core/base_rules.mk
      /build/core/static_library.mk
      /build/core/binary.mk
     编写可执行文件基本上和.so是差不多的，现在分为两类来仔细讲一下，一类是prebuilt files的编译，另外就是.so/.a/elf的编译。
     在所有这许makefile中最重要的是base_rules.mk，它是对module进行处理的核心过程，下面先看看这个文件的内容：
      ![](http://hiphotos.baidu.com/lucky900apple/pic/item/912c98f81ef6bcbffc037fd9.jpg)

每个模块在编译的时候都会产生一个编译目录和一个安装目录，编译目录就是这个模块编译以后生成的目标文件，安装目录就代表着这个模块是否会编译进文件系统，就是是否编译进IMG啦～～在base_rules.mk提供了两个变量来定义你要输出的目录，仔细弄懂对你了解编译后的生成目录是很有帮助的～～
      built_module_path := $(intermediates)
      LOCAL_BUILT_MODULE := $(built_module_path)/$(LOCAL_BUILT_MODULE_STEM)
      LOCAL_INSTALLED_MODULE := $(LOCAL_MODULE_PATH)/$(LOCAL_MODULE_SUBDIR)$(LOCAL_INSTALLED_MODULE_STEM)
      built_module_path是编译生成的中间文件所在的目录，LOCAL_BUILT_MODULE_STEM就是你要生成的编译目标啦，如果本地模块指定了LOCAL_MODULE_STEM的话，它的值就是$(LOCAL_MODULE_STEM)$(LOCAL_MODULE_SUFFIX)，如果没有指定了的话就是$(LOCAL_MODULE)$(LOCAL_MODULE_SUFFIX)。由此可以看到LOCAL_MODULE的定义是很有讲究的，比如什么＊.so，一般＊就用来作模块名。LOCAL_MODULE_SUFFIX在编译不同的模块时，GOOGLE内置会给你加上相应的值，如果你不了解的话还是尽量自己来指定，不然可能编译出来的东西被篡改了文件名哦噢～～
      将编译目录的文件拷贝到安装目录就是我们的LOCAL_INSTALLED_MODULE了，是否会安装就要看你定义的 LOCAL_MODULE_TAGS了，当然你也可以通过修改LOCAL_MODULE_PATH来自定义安装。LOCAL_MODULE_PATH是有个很有用的变量，首先我们看看当我们在本地模块没有指定这个值的时候，它的值实际上是：LOCAL_MODULE_PATH := $($(my_prefix)OUT$(use_data)_$(LOCAL_MODULE_CLASS))，如果你的模块定义了TAGS ：＝ TESTS则user_data的值是DATA，这样的模块会被安装在data/目录下，那么通过替换我们就知道这个LOCAL_MODULE_PATH := TARGET_OUT_$(LOCAL_MODULE_CLASS)。这个LOCAL_MODULE_CLASS在特定的类型编译会被google赋值成固定内容，但是在prebuilt的编译中它是由你自己来赋值的，它的值就会用来定义生成的目录，比如LOCAL_MODULE_CLASS := ETC的时候，则就会被安装在/system/etc目录。那么我们就知道如何来定义prebuilt模块里面的CLASS了，google还提供了一个 LOCAL_MODULE_SUBDIR可以让你来定义子目录，但是要记得的是在每个模块的最后要将这个值清空，因为默认CLEAR_VARS是不会清空这个值的。
      当然前面说的是LOCAL_MODULE_PATH的默认值，我们可以通过给它赋值来强制指定安装的目录，比如说要安装在system/etc /permissions目录，则可以强制指定它的值为$(TARGET_OUT_ETC)/permissions，这样模块就会被强制安装在这个目录了，给LOCAL_MODULE_PATH赋值的情况主要应用在prebuilt模块的编译上，其他的应该尽量采用其默认值。
      下面我们就具体看看我写的一个如何编译自己的.so *.a elf的例子，具体能使用到的变量，和要注意的地方我都写出来了：
         ![](http://hiphotos.baidu.com/lucky900apple/pic/item/26ca4bd1442fa28651da4ba2.jpg)

这里要说明的是这个prelink，prelinke只有在编译.so的时候才会有的选项，主要是通过预链接的方式来加快程序启动和执行的速度，如果你的本地模块prelink是真的话，那你要在build/core/prelink-linux-arm.map中定义你的库需要使用的空间，空间不够的话会报错(具体使用没仔细研究过)
       下面再看看如何来将我们自己的prebuilt files编译进工程，先让我们和这个相关的最重要的两个makefile：
        ![](http://hiphotos.baidu.com/lucky900apple/pic/item/bdf481cc6acfb6557f3e6faa.jpg)

multi_prebuilt.mk只用来加入需要加入的各种库，仔细注意它里面的那个对加入文件进行处理的函数，尤其是 LOCAL_MODULE，LOCAL_MODULE_SUFFIX ，LOCAL_SRC_FILES这3个变量是如何得到的，使用multi_prebuilt编译进工程的文件都会自动打上USER的tag。而且它最后还是会掉用prebuilt.mk来执行真正的编译操作。
        prebuilt.mk实际上可以编译任何文件到我们的工程中，下面是我写的一个例子，可以自动将目录下相应格式的文件编译进工程：
      ![](http://hiphotos.baidu.com/lucky900apple/pic/item/6b97d222dcb361558a82a1b0.jpg)

此外添加prebuilt.mk也有一个粗暴的方式，可以通过下面的规则简单的将所有prebuilt files添加到你工程中去，必须一个一个添加：
        LOCAL_PATH := $(call my-dir)
        include $(CLEAR_VARS)
        # your prebuilt file name
        LOCAL_MODULE := example.so
        LOCAL_MODULE_TAGS := user eng
        # your prebuilt file (must be relative directory )
        LOCAL_SRC_FILES := lib/example.so
        # the path your prebuilt file will be installed   $(TARGET_OUT) is the system directory
        LOCAL_MODULE_PATH := $(TARGET_OUT)/lib  
        include (BUILD_PREBUILT)
在最后说一下Android对模块唯一性检测的规则，在base_rules.mk里面通过module_id里来检测这个模块是否已经存在，我们看看这个值是如何定义的：
module_id := MODULE.$(if /
    $(LOCAL_IS_HOST_MODULE),HOST,TARGET).$(LOCAL_MODULE_CLASS).$(LOCAL_MODULE)
所以它是通过LOCAL_MODULE_CLASS和LOCAL_MODULE这两个变量来检测模块的唯一性，因此当你定义同一样的CLASS和 MODULE的时候，Android就会报错，提示模块必须是唯一的。因此如果你将LOCAL_MODULE_CALSS进行修改，使用 LOCAL_MODULE_PATH来指定安装目录的时候就会逃过Android对模块唯一性的检测，但是这样导致的问题就是同一个目标会有多个规则来实现它，而且每个规则都是相同的命令。这样导致的结果就是MAKE会将所有的依赖放在一起，然后使用命令来将其生成目标，这样是非常危险的，会造成库的覆盖，而且很多时候你不知道它就究竟使用的是哪个依赖来最终生成目标，只能用md5sum来检查。
      因此当你使用prebuilt来覆盖系统原有的文件的时候应该特别小心，如果你确认你的文件在覆盖系统原有文件以后系统能正常运行的话，好的方法是还是打开Android对module_id的检测，同时把原来生成这个文件的Makefile进行修改，让其不编译出这个文件～
      最后补充一个Android如何来存放模块的编译中间文件：
1、如果你的LOCAL_MODULE_CLASS包含COMMON_MODULE_CLASS := JAVA_LIBRARIES NOTICE_FILES，则你编译出的中间文件会放在：
   $(TARGET__OUT_COMMON_INTERMEDIATES)/$(LOCAL_MODULE_CLASS)/$(LOCAL_MODULE)_intermediates/
TARGET_OUT_COMMON_INTERMEDIATES := out/target/common/obj
2、如果你的LOCAL_MODULE_CLASS不包含COMMON_MODULE_CLASS，则你编译出的中间文件会放在：
     $(TARGET__OUT_INTERMEDIATES)/$(LOCAL_MODULE_CLASS)/$(LOCAL_MODULE)_intermediates/
TARGET_OUT_INTERMEDIATES := out/target/product/msm7627_ffa/ob
