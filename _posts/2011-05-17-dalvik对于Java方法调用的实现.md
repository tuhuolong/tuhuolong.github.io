---
layout: post
title: dalvik对于Java方法调用的实现
date: 2011-05-17 14:48:00
categories: [Android, Java]
tags: [java, jni, 汇编, frameworks, thread, object]
---
**(入口一)字节码OP_INVOKE_ (汇编实现)**
**1.****dvmResolveMethod:**(C函数)决议方法的Method**	:**方法**名字**字符串**找**ClassObject中**Method**
**2**.(设置调用环境)****创建****一个方法调用****栈帧****(设置参数)
**3**.分支----(汇编)
**->本地方法**:nativeFunc(C函数)
   ...........
   ...........
**->Java方法**:(1)设置interpState(字节码)初始执行环境
    (2)**跳转执行**:取(被调方法)下一字节码
算偏移,跳转



Get__MethodID:决议出方法的Method:方法**名字**字符串**查**ClassObject中**Method**
**(入口二)****JNI**->CallXXXMethod->(启动类的main方法通过JNI调用)
(dvm内部)直接调用 -> 
**dvmCallMethodV**(Thread*, Method*, Object*, JValue*, va_list args);**(C实现)**
 
**1.**(设置调用环境)**创建**一个方法调用**栈帧**(设置参数)
**2.**分支
**->本地方法**:(*method->nativeFunc)(self->curFrame,
pResult, method, self)
**(1).**(第一次调用)决议时先**查找本地函数指针**,再调用
****
(1).查找**类库**的本地**函数表**中的Dalvik**本地函数**
类库本地函数格式:**static void** XXX**(const u4* args,JValue* pResult,const Method* method,Thread* self);**
(一般Java程序的最终内部实现,用户透明)**直接调用**VM的本地函数
(2).查找已加载的**本地库**(dll/so)中标准**JNI****函数**
标准JNI函数声明格式:**JNIExport** jRetVal **JNICall**  **java_**ClassName**_**NativeMethodName**(JNIEnv*, jobject,** ...**);**
DLL中的本地函数可以通过JNI接口与VM交互(调用VM的本地函数)
(frameworks类的本地方法):加载类时已找到本地函数指针(注册此类的所有JNI函数到Method)
**(2)**.通过**JNI**的**调用桥**(call bridge)**调用**JNI函数 
(1)根据**JNI函数**的**格式**[即ABI标准(EABI-ARM/ABI-x86/FFI标准)]**设置参数**(寄存器/栈)
(2)调用**本地函数**   
(3)**设置返回值**(java对象)
 
**->Java方法**:dvmInterpret(Thread*, Method*, JValue* )
(1)设置interpState(字节码)初始执行环境
(2)进入口函数dvmMterpStdRun
(两种Java字节码执行机制)
1.C版的**Switch-Case**机制:
 
while (true) {
       u2 inst = pc[0];取下一字节码
       Handler handler = (Handler) gDvmMterpHandlers[inst & 0xff];查表(找到相应的)处理函数
       (*handler)(glue);调用处理函数
   }
2.汇编版的**跳转**线性**执行**(Threaded execution):每一个字节码对应汇编指令都占64字节,并按字节码顺序依次排列
每个字节码执行完后/第一个字节码
FETCH_ADVANCE_INST:**取**下一**字节码**
GET_INST_OPCODE
GOTO_OPCODE(ip):跳到下一字节码的汇编指令-根据字节码**算**出**偏移**(单位64)再**跳转**
 


跳转执行(少去了Switch-Case中的循环)便于指令流水线,更加快速,但每个字节码得汇编指令都占64字节会浪费空间
 

