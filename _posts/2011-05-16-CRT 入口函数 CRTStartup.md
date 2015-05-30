---
layout: post
title: CRT 入口函数 CRTStartup
date: 2011-05-16 14:53:00
categories: [C++]
tags: [windows, exe, library, 语言, blog]
---

windows启动一个程序的过程大概是这样的：首先由外壳程序（比如Explorer）调用CreateProcess这个系统调用，CreateProcess为这个进程创建虚拟地址，然后将代码和数据载入，然后系统再创建一个主线程开始执行run
time startup函数的代码，run
time startup 函数会最终调用入口点函数（main,
WinMain）。
run time startup会做很多事情，比如全局变量的创建和销毁，入口点函数的调用等。这里我就要有问题要问了，run
time startup函数对于我编程来讲是不可见的，那么这个函数到底在哪里呢？起初我还以为它在一个动态连接库里，但马上我就发现不对。在连接程序的时候我发现下面一条信息：
  Found _printf
        Referenced in temporary.obj
        Loaded LIBCD.lib(printf.obj)
      Found _mainCRTStartup
        Loaded LIBCD.lib(crt0.obj)
很明显，它是由libcd.lib静态的连接到我的exe程序的。在网上搜了下相关的信息，发现了一个很有趣的程序，这个程序没有WinMain函数，但却能够输出字符串。程序如下：
//文件名:a.c
//不是原创
#include < windows.h >
const char str[] = "This
is Zouxiao's blog";
void DrawString(HDC hDC, int x,
int y)
{    
    SetBkMode(hDC, TRANSPARENT);
    SetTextColor(hDC, RGB(255, 0, 0));
    TextOut(hDC, x, y, str, strlen(str));
}
void WinMainCRTStartup()
{
    HDC hDC = GetDC(NULL);
   
DrawString(hDC, GetSystemMetrics(SM_CXSCREEN) / 2,
                   
GetSystemMetrics(SM_CYSCREEN) / 2);
    
    ReleaseDC(NULL, hDC);
}
成功的连接这个文件需要注意两点：1，文件的后缀名必须为.c而不能是.cpp。 2，必须打开/SUBSYSTEM:WINDOWS的连接开关。
根据这个程序可以看出：
WinMainCRTStartup这个函数名是由连接器选定的，如果是/SUBSYSTEM指定的是windows，那么就是WinMainCRTStartup,如果/SUBSYSTEM指定的是console,那么就是mainCRTStartup。如果没有指定的话连接器就自动根据入口点函数（main,WinMain）自动选择，在这种情况下如果没有入口点函数连接器就会报错。CRTStartup函数并不用我们写，在libc.lib(libcd.lib)中有一份编译好了的CRTStartup函数（源代码visual studio目录中可以找到(crt0.c,wincrt0.c)），这个函数会默认的调用我们的入口点函数，而且连接器会自动将其连接到我们的exe程序（在我们文件里没有这些函数的前提下）
大概我的理解就这么多，至于为什么它必需要以.c为后缀名，是因为它必须以c语言编译，或许是因为它属于c run
time library ，连接器只识别以c语言编译的函数的缘故吧
