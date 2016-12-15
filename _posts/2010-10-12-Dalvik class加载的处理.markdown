---
layout: post
title: "Dalvik class加载的处理"
date: 2010-10-12 18:54:00 
comments: true
categories: [class]
tags: [class]
description: "Dalvik class加载的处理"
keywords: class
---


 
  
   
    Java
   
   
    源代码经过编译后会生成后缀为
   
   
    class
   
   
    的文件，也即字节码文件。然后在
   
   
    Android
   
   
    中使用
   
   
    dx
   
   
    工具将其转换为后缀为
   
   
    jar
   
   
    的
   
   
    dex
   
   
    类型文件。
   
   
    Dalvik
   
   
    虚拟机负责解释并执行编译后的字节码。在解释执行字节码之前，当然要读取
文件，分析文件的内容，得到字节码，然后才能解释执行之。在整个的加载过程中，最为重要的就是对
   
   
    Class
   
   
    的加载
   
   
    
    
    – Class
   
   
    包含
   
   
    Method
   
   
    ，
   
   
    Method
   
   
    又包含
   
   
    code
   
   
    。通过对
   
   
    Class
   
   
    的加载，我们即可获得所需执行的字节码。
   
  
 
 
  
  
 
 
  
   
    本文
从
   
   
    dexfile
   
   
    文件分析及
   
   
    Class
   
   
    加载中的数据结构入手，结合主要流程，对整个加载过程进行分析，期望对大
家有所帮助。
   
  
 
 
  
  
 
 
  
  
 
 
  
   
    1. 
DexFile
   
   
    在内存中的映射
   
  
 
 
  
  
 
 
  
   
    在
   
   
    Android
   
   
    系统中，
   
   
    java
   
   
    源文件会被编译为后缀为
   
   
    jar
   
   
    的
   
   
    dex
   
   
    类
型文件，在代码中称为
   
   
    dexfile
   
   
    。在加载
   
   
    Class
   
   
    之前，必先读取相应的
   
   
    jar
   
   
    文件。通常我们使用
   
   
    read
   
   
    函数来读取文件中的内容。但在
   
   
    Dalvik
   
   
    中使用
   
   
    mmap
   
   
    函数，和
   
   
    read
   
   
    不同，
   
   
    mmap
   
   
    函数会将
   
   
    dex
   
   
    文件映射到内存中，这样通过普通的内存读取操作即可访问
   
   
    dex file
   
   
    中的内容。
   
  
 
 
  
  
 
 
  
   
    Dexfile
   
   
    的文件格式如下图所示，主要有三部分组成：
头部，索引，数据。通过头部可知索引的位置和数目，可知数据区的起始位置。其中
   
   
    classDefsOff
   
   
    指定了
   
   
    ClassDef
   
   
    在文件的起始位置，
   
   
    dataOff
   
   
    指定了数据在文件的起始位置，
   
   
    ClassDef
   
   
    即可理解为
   
   
    Class
   
   
    的索引。通过读取
   
   
    ClassDef
   
   
    可获知
   
   
    Class
   
   
    的基本信息，其中
   
   
    classDataOff
   
   
    指定了
   
   
    Class
   
   
    数据在数据区的位置。
   
  
 
 
  
  
 
 
  
   
    在将
   
   
    dexfile
   
   
    文件映射到内存后，即会调用
   
   
    dexFileParse
   
   
    函数对其分析，分析的结果存放于名为
   
   
    DexFile
   
   
    的数据结构中。
   
   
    DexFile
   
   
    中的
   
   
    baseAddr
   
   
    指向映射区的起始位置，
   
   
    pClassDefs
   
   
    指向
   
   
    ClassDefs
   
   
    （即
   
   
    class
   
   
    索引）的起始位置。由于在查找
   
   
    class
   
   
    时，都是使用
   
   
    class
   
   
    的名字进行查找，为了加快查找速度，创建了一个
   
   
    hash
   
   
    表。在
   
   
    hash
   
   
    表中对
   
   
    class
   
   
    名字进行
   
   
    hash
   
   
    ，并生成
   
   
    index
   
   
    。这些操作都是在对文件解析时所完成的，这样虽然在加载过程中比较耗时，
但是在运行过程中确可节省大量查找时间。
   
  
 
 
 
 
  
 
 
 
 
 
 
 
 
 
 
  
   
    2. ClassObject -  Class
   
   
    在加载后的表现形式
   
  
 
 
  
  
 
 
  
   在对文件解析完成后就要加载
  
  
   Class
  
  
   的具体内容了！在
  
  
   Dalvik
  
  
   中，由
  
  
   ClassObject
  
  
   这个数据结构负责存放加载的信息。如下图所示，加载过程会在内存中
  
  
   alloc
  
  
   几个区域，分别存放
  
  
   directMethods, virtualMethods, 
sfields, ifields
  
  
   。这些信息
正是从
  
  
   dex
  
  
   文件的数据区中读取。首先会读取
  
  
   Class
  
  
   的详细信息，从中获知
  
  
   directMethod, virtualMethod, 
sfield, ifield
  
  
   等的信息，然后
再读取。下图为加载完成后的示意。
  
  
   
   
  
  
   这里并未介绍加载的每个细节，感兴趣的同学可通过此二图自行分析。
  
 
 
  
   还请大家注意的是在
  
  
   ClassObject
  
  
   结构中有个名为
  
  
   super
  
  
   的成员。通过
  
  
   super
  
  
   成员来指向它的超类。
  
 
 
 
 
  
 
 
 
 
 
 
  
   
    3.
 findClassNoInit –
   
   
    负责
加载
   
   
    Class
   
   
    并生成相应
   
   
    ClassObject
   
   
    的函数。
   
  
 
 
  
  
 
 
  
   
    第二
节介绍了加载后的数据结构，本节会分析负责加载工作的函数
   
   
    - findClassNoInit
   
   
    。
   
   
    
    
   
   
    请
注意在获取
   
   
    Class
   
   
    索引时，会分为基本类库文件和用户类文件两种情况。在
   
   
    Dalvik
   
   
    分析之准备篇中，
   
   
    grund.sh
   
   
    中有一语句
   
   
   
   
    “
   
   
    export 
BOOTCLASSPATH=$bootpath/core.jar:$bootpath/ext.jar:$bootpath/framework.jar:$bootpath/android.police.jar”
   
   
    。
   
   
    
    
   
   
    这条语句指定了
   
   
    Dalvik
   
   
    所需的基本库文件，如果没有此语句，
   
   
    Dalvik
   
   
    在启动过程中就会报错退出。
   
  
 
 
  
  
 
 
  
   
    LoadClassFromDex
   
   
    函数先会读取
   
   
    Class
   
   
    的具体数据（从
   
   
    ClassDataoff
   
   
    处），然后按图索骥，分别加载
   
   
    directMethod
   
   
    ，
   
   
    virtualMethod
   
   
    ，
   
   
    ifield
   
   
    ，
   
   
    sfield
   
   
    。
   
  
 
 
  
  
 
 
  
   
    作为
业界
   
   
    TOP
   
   
    公司出品的东东，当然要关注执行效率了
   
   
    ^_^
   
   
    。首先，在加载后我们要将其缓存起来，以便以后使用方便。其次，在查找过
程中，如果我们顺序查找的话，当然是很慢的拉。这当然是俺们
   
   
    senior engineer
   
   
    所不能容忍的，所以
   
   
    gDvm.loadedClasses
   
   
    这个
   
   
    Hash
   
   
    表就隆重出场了。什么，这位同学说没有几个
   
   
    Class
   
   
    ，至于这么大动干戈么。让我们看看，通过在准备篇中介绍的方法，我们在
   
   
    main.c 249
   
   
    行设置断点，这个时候基本库已加载完毕。当程序停下时，我们看看
   
   
    gDvm
   
   
    的值，可以看到
   
   
    numLoadedClasses
   
   
    这个成员的值为
   
   
    212
   
   
    ！也即意味着这个时候我们什么也没做，用户类也未加载，
   
   
    Dalvik
   
   
    已加载的
   
   
    Class
   
   
    数目已达到
   
   
    212
   
   
    。
   
  
 
 
  
  
 
 
  
   
    dvmLinkClass
   
   
    ，
   
   
    
    
   
   
    好长，但是其最终好像会再次调用
   
   
    findClassNoInit
   
   
    。嗯，也是可以理解的嘛。如果一个子类需要调用超类的函数，那它当然要先
加载超类了，可能的话，甚至会加载超类的超类
   
   
    ^_^
   
   
    。
   
   
    
    
   
  
 
 
  
  
 
 
  
   
  
 
 
 
 
 
 
 
 
  
   
    眼见
为虚，实践出真知，拿
   
   
    gdb
   
   
    调试下。
   
  
 
 
  
   
    在
   
   
    findClassNoInit
   
   
    函数处设置断点
   
   
    (
   
   
    在
   
   
    gdb
   
   
    提示符后输入
   
   
    ”b findClassNoInit”)
   
   
    ，在
   
   
    gdb
   
   
    提
示符后连续几次执行
   
   
    ”c”
   
   
    和
   
   
    ”bt”
   
   
    。可出现如下信息，可以看到在函数调用栈上可以多次看到
   
   
    findClassNoInit
   
   
    的身影。
   
  
 
 
  
  
 
 
  
   (gdb) bt
  
 
 
  
   #0 
 findClassNoInit (descriptor=0xfef4c7f4 "??????%", loader=0x0, 
pDvmDex=0x0)
  
 
 
  
   
    
     at 
dalvik/vm/oo/Class.c:1373
    
   
  
 
 
  
   
    
     #1  0xf6fc4d53 in 
dvmFindClassNoInit (descriptor=0xf5046a63 "Ljava/lang/Object;", 
loader=0x0)
    
   
  
 
 
  
   
    
     at dalvik/vm/oo/Class.c:1194
    
   
  
 
 
  
   
    
     #2  0xf6fc6c0a in 
dvmResolveClass (referrer=0xf5837400, classIdx=290,
    
   
  
 
 
  
   
    
     fromUnverifiedConstant=false)
 at dalvik/vm/oo/Resolve.c:94
    
   
  
 
 
  
   
    
     #3  0xf6fc3476 in dvmLinkClass 
(clazz=0xf5837400, classesResolved=false)
    
   
  
 
 
  
   
    
     at dalvik/vm/oo/Class.c:2537
    
   
  
 
 
  
   
    
     #4  0xf6fc1b67 in 
findClassNoInit (descriptor=0xf6ff0df6 "Ljava/lang/Class;", loader=0x0,
    
   
  
 
 
  
   
    
     pDvmDex=0xa04c720) at 
dalvik/vm/oo/Class.c:1489
    
   
  
 
 
  
   
   
  
 
 
  
   
   
  
 
 
  
   
    现在
从另一个角度去观察。在
   
   
    
     class.c 2575
    
   
   
    行设置断点，然后等待程序停下。看下
   
   
    
     clazz
    
   
   
    的内容。
   
  
 
 
  
   
    
     (gdb) p clazz->;super->;descriptor
    
   
  
 
 
  
   
    
     $6 = 0xf5046a63 "Ljava/lang/Object;"
    
   
  
 
 
  
   
    
     (gdb) p clazz->;descriptor
    
   
  
 
 
  
   
    
     $7 = 0xf5046121 "Ljava/lang/Class;"
    
   
  
 
 
  
   
   
  
 
 
  
  
 
 
  
   
    4.
   
   
    基本类库文件的加载
   
  
 
 
  
  
 
 
  
   
    先在
   
   
    findClassNoInit
   
   
    函数处设置断点，然后运行程序，等待程序的停下。
   
  
 
 
  
  
 
 
  
   (gdb) b 
findClassNoInit
  
 
 
  
   Breakpoint 2 at 
0xf6fc13e0: file dalvik/vm/oo/Class.c, line 1373.
  
 
 
  
   (gdb) c
  
 
 
  
   Continuing.
  
 
 
  
  
 
 
  
   
    看看
谁是第一个加载的
   
   
    Class
   
   
    ，及其调用关系。
   
  
 
 
  
  
 
 
  
   (gdb) bt
  
 
 
  
   #0 
 findClassNoInit (descriptor=0x0, loader=0x0, pDvmDex=0x0) at 
dalvik/vm/oo/Class.c:1373
  
 
 
  
   #1 
 0xf6fc32a1 in dvmLinkClass (clazz=0xf5837350, 
classesResolved=false)
  
 
 
  
   
    
     at 
dalvik/vm/oo/Class.c:2491
    
   
  
 
 
  
   
    
     #2  0xf6fc1b67 in 
findClassNoInit (descriptor=0xf6ff1ded "Ljava/lang/Thread;", loader=0x0,
    
   
  
 
 
  
   
    
     pDvmDex=0xa04c720) at 
dalvik/vm/oo/Class.c:1489
    
   
  
 
 
  
   #3 
 0xf6f92692 in dvmThreadObjStartup () at dalvik/vm/Thread.c:328
  
 
 
  
   #4 
 0xf6f800e6 in dvmStartup (argc=2, argv=0xa041190, 
ignoreUnrecognized=false, pEnv=0xa0411a0)
  
 
 
  
   
    
     at dalvik/vm/Init.c:1155
    
   
  
 
 
  
   
    
     #5  0xf6f8b8e3 in 
JNI_CreateJavaVM (p_vm=0xf6ff0df6, p_env=0xf6ff0df6, vm_args=0xfef4d0b0)
    
   
  
 
 
  
   
    
     at dalvik/vm/Jni.c:4198
    
   
  
 
 
  
   
    
     #6  0x08048893 in main (argc=3, 
argv=0xfef4d168) at dalvik/dalvikvm/Main.c:212
    
   
  
 
 
  
   
   
  
 
 
  
   
    函数
调用顺序清晰可见：
   
   
    
     main ->; JNI_CreateJavaVM->; dvmStartup->; 
dvmThreadObjStartup->; dvmFindSystemClassNoInit->; findClassNoInit
    
   
   
    观察仔细的同学可能会问在调用栈中没有看到
   
   
    
     dvmFindSystemClassNoInit
    
   
   
    啊，为何你这样写啊？我估计编译器将其作为
   
   
    
     inline
    
   
   
    优化了，导致
   
   
    
     gdb
    
   
   
    看不到有
   
   
    
     dvmFindSystemClassNoInit
    
   
   
    的栈。从回溯栈中也可清晰的看到
   
  
 
 
  
   
    
    
   
  
 
 
  
   
   
  
 
 
  
   
    5.
   
   
    用户类文件的加载
   
  
 
 
  
   
    用户
类文件的加载颇为曲折，它会先加载一个
   
   
    Class
   
   
    。然后这个
   
   
    Class
   
   
    去负责用户类文件的加载，当然了，这个
   
   
    Class
   
   
    又会通过
   
   
    JNI
   
   
    的方式去曲折调用到
   
   
    findClassNoInit
   
   
    。这个就留给大家自己分析了，
   
   
    
    
   
   
    其实楼主自己也不是很明白为什么要这么大费周折
   
   
    ^_^
   
   
    。
   
  
 


