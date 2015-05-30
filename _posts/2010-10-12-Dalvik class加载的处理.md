---
layout: post
title: Dalvik class加载的处理
date: 2010-10-12 18:54:00
categories: [Java]
tags: [class, descriptor, 数据结构, jni, android, jar]
---
Java
 源代码经过编译后会生成后缀为class的文件，也即字节码文件。然后在Android中使用dx工具将其转换为后缀为jar 的dex类型文件。Dalvik 虚拟机负责解释并执行编译后的字节码。在解释执行字节码之前，当然要读取
文件，分析文件的内容，得到字节码，然后才能解释执行之。在整个的加载过程中，最为重要的就是对Class的加载– Class包含Method，Method 又包含code。通过对Class的加载，我们即可获得所需执行的字节码。
 
本文
从dexfile 文件分析及Class加载中的数据结构入手，结合主要流程，对整个加载过程进行分析，期望对大
家有所帮助。
 
 
#1. 
DexFile 在内存中的映射
 
在Android 系统中，java源文件会被编译为后缀为jar的dex类
型文件，在代码中称为dexfile。在加载Class之前，必先读取相应的jar文件。通常我们使用read函数来读取文件中的内容。但在Dalvik中使用mmap函数，和read不同，mmap函数会将dex文件映射到内存中，这样通过普通的内存读取操作即可访问dex file 中的内容。
 
Dexfile
 的文件格式如下图所示，主要有三部分组成：
头部，索引，数据。通过头部可知索引的位置和数目，可知数据区的起始位置。其中classDefsOff指定了ClassDef在文件的起始位置，dataOff指定了数据在文件的起始位置，ClassDef即可理解为Class的索引。通过读取ClassDef可获知Class的基本信息，其中classDataOff指定了Class数据在数据区的位置。
 
在将dexfile文件映射到内存后，即会调用dexFileParse函数对其分析，分析的结果存放于名为DexFile 的数据结构中。DexFile中的baseAddr指向映射区的起始位置，pClassDefs指向ClassDefs（即class索引）的起始位置。由于在查找class时，都是使用class的名字进行查找，为了加快查找速度，创建了一个hash表。在hash表中对class名字进行hash，并生成index。这些操作都是在对文件解析时所完成的，这样虽然在加载过程中比较耗时，
但是在运行过程中确可节省大量查找时间。
 
![](http://hi.csdn.net/attachment/201007/6/0_127838235177K1.gif) 
 
 
 
 
#2. ClassObject -  Class在加载后的表现形式
 
在对文件解析完成后就要加载Class的具体内容了！在Dalvik中，由ClassObject 这个数据结构负责存放加载的信息。如下图所示，加载过程会在内存中alloc几个区域，分别存放directMethods, virtualMethods, 
sfields, ifields。这些信息
正是从dex 文件的数据区中读取。首先会读取Class的详细信息，从中获知directMethod, virtualMethod, 
sfield, ifield等的信息，然后
再读取。下图为加载完成后的示意。这里并未介绍加载的每个细节，感兴趣的同学可通过此二图自行分析。
还请大家注意的是在ClassObject结构中有个名为super的成员。通过super成员来指向它的超类。
 
![](http://hi.csdn.net/attachment/201007/6/0_12783829334yaJ.gif)
 
 
#3.
 findClassNoInit – 负责
加载Class并生成相应ClassObject的函数。
 
第二
节介绍了加载后的数据结构，本节会分析负责加载工作的函数- findClassNoInit。请
注意在获取Class索引时，会分为基本类库文件和用户类文件两种情况。在Dalvik分析之准备篇中，grund.sh中有一语句   “export 
BOOTCLASSPATH=$bootpath/core.jar:$bootpath/ext.jar:$bootpath/framework.jar:$bootpath/android.police.jar”
 。这条语句指定了Dalvik所需的基本库文件，如果没有此语句，Dalvik在启动过程中就会报错退出。
 
LoadClassFromDex
 函数先会读取Class的具体数据（从ClassDataoff处），然后按图索骥，分别加载 directMethod， virtualMethod，ifield，sfield。
 
作为
业界TOP公司出品的东东，当然要关注执行效率了^_^。首先，在加载后我们要将其缓存起来，以便以后使用方便。其次，在查找过
程中，如果我们顺序查找的话，当然是很慢的拉。这当然是俺们senior engineer所不能容忍的，所以gDvm.loadedClasses这个Hash表就隆重出场了。什么，这位同学说没有几个Class，至于这么大动干戈么。让我们看看，通过在准备篇中介绍的方法，我们在main.c 249行设置断点，这个时候基本库已加载完毕。当程序停下时，我们看看gDvm的值，可以看到numLoadedClasses这个成员的值为212 ！也即意味着这个时候我们什么也没做，用户类也未加载，Dalvik 已加载的Class数目已达到212。
 
dvmLinkClass，好长，但是其最终好像会再次调用findClassNoInit。嗯，也是可以理解的嘛。如果一个子类需要调用超类的函数，那它当然要先
加载超类了，可能的话，甚至会加载超类的超类^_^。  
 
![](http://hi.csdn.net/attachment/201007/6/0_1278382976sWxw.gif)
 
 
 
眼见
为虚，实践出真知，拿gdb调试下。
在findClassNoInit函数处设置断点(在gdb提示符后输入”b findClassNoInit”)，在gdb提
示符后连续几次执行”c”和”bt”。可出现如下信息，可以看到在函数调用栈上可以多次看到findClassNoInit的身影。
 
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
从另一个角度去观察。在class.c 2575行设置断点，然后等待程序停下。看下clazz的内容。
(gdb) p clazz->super->descriptor
$6 = 0xf5046a63 "Ljava/lang/Object;"
(gdb) p clazz->descriptor
$7 = 0xf5046121 "Ljava/lang/Class;"
 
 
#4. 基本类库文件的加载
 
先在findClassNoInit函数处设置断点，然后运行程序，等待程序的停下。
 
(gdb) b 
findClassNoInit
Breakpoint 2 at 
0xf6fc13e0: file dalvik/vm/oo/Class.c, line 1373.
(gdb) c
Continuing.
 
看看
谁是第一个加载的Class，及其调用关系。
 
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
调用顺序清晰可见：main -> JNI_CreateJavaVM-> dvmStartup-> 
dvmThreadObjStartup-> dvmFindSystemClassNoInit-> findClassNoInit 观察仔细的同学可能会问在调用栈中没有看到dvmFindSystemClassNoInit啊，为何你这样写啊？我估计编译器将其作为inline优化了，导致gdb看不到有dvmFindSystemClassNoInit的栈。从回溯栈中也可清晰的看到
 
 
#5. 用户类文件的加载
用户
类文件的加载颇为曲折，它会先加载一个Class。然后这个Class去负责用户类文件的加载，当然了，这个Class又会通过JNI的方式去曲折调用到findClassNoInit。这个就留给大家自己分析了，其实楼主自己也不是很明白为什么要这么大费周折^_^。
