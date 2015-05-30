---
layout: post
title: C++强制类型转换：dynamic_cast、const_cast 、static_cast、reinterpret_cast
date: 2011-06-23 16:08:00
categories: [C++]
tags: [c++, 编译器, class, 编程, c]
---
static_cast(可能不安全):一般、对象指(引)上行/下行转换
dynamic_cast(**运行时**的**安全检查--**抛出异常/NULL):对象指(引)上行/下行转换
const_cast:   主要针对const和volatile的转换. 
reinterpret_cast:   用于进行没有任何关联之间的转换，比如一个字符指针转换为一个整形数。


**[http://www.cplusplus.com/doc/tutorial/typecasting/](http://www.cplusplus.com/doc/tutorial/typecasting/)
**
[http://msdn.microsoft.com/en-us/library/cby9kycs%28v=vs.71%29.aspx](http://msdn.microsoft.com/en-us/library/cby9kycs%28v=vs.71%29.aspx)
[http://stackoverflow.com/questions/1781763/what-exactly-is-dynamic-casting-in-c](http://stackoverflow.com/questions/1781763/what-exactly-is-dynamic-casting-in-c)

**      **
**1）static_cast<T*>(a)**
编译器在编译期处理
将地址a转换成类型T，T和a必须是指针、引用、算术类型或枚举类型。
表达式static_cast<T*>(a), a的值转换为模板中指定的类型T。在运行时转换过程中，不进行类型检查来确保转换的安全性。
static_cast它能在内置的数据类型间互相转换，对于类只能在有联系的指针类型间进行转换。可以在继承体系中把指针转换来、转换去，但是不能转换成继承体系外的一种类型
class A { ... };
class B { ... };
class D : public B { ... };
void f(B* pb, D* pd)
{
    D* pd2 = static_cast<D*>(pb);        // 不安全, pb可能只是B的指针
    B* pb2 = static_cast<B*>(pd);        // 安全的
    A* pa2 = static_cast<A*>(pb);        //错误A与B没有继承关系
    ...
}
**2）dynamic_cast<T*>(a)**
在运行期，会检查这个转换是否可能。
完成类层次结构中的提升。T必须是一个指针、引用或无类型的指针。a必须是决定一个指针或引用的表达式。
dynamic_cast 仅能应用于指针或者引用，不支持内置数据类型
表达式dynamic_cast<T*>(a) 将a值转换为类型为T的对象指针。如果类型T不是a的某个基类型，该操作将返回一个空指针。
它不仅仅像static_cast那样，检查转换前后的两个指针是否属于同一个继承树，它还要检查被指针引用的对象的实际类型，确定转换是否可行。
如果可以，它返回一个新指针，甚至计算出为处理多继承的需要的必要的偏移量。如果这两个指针间不能转换，转换就会失败，此时返回空指针（NULL）。
很明显，为了让dynamic_cast能正常工作，必须让编译器支持运行期类型信息（RTTI）。
**3）const_cast<T*>(a)**
编译器在编译期处理
去掉类型中的常量，除了const或不稳定的变址数，T和a必须是相同的类型。
表达式const_cast<T*>(a)被用于从一个类中去除以下这些属性：const, volatile, 和 __unaligned。
class A { ... };
void f()
{
    const A *pa = new A;//const对象
    A *pb;//非const对象
    //pb = pa; // 这里将出错，不能将const对象指针赋值给非const对象
    pb = const_cast<A*>(pa); // 现在OK了
    ...
}
对于本身定义时为const的类型，即使你去掉const性，在你操作这片内容时候也要小心，只能r不能w操作，否则还是会出错
const char* p = "123"; 
char* c = const_cast<char*>(p); 
c[0] = 1;   //表面上通过编译去掉了const性，但是操作其地址时系统依然不允许这么做。
const_cast操作不能在不同的种类间转换。相反，它仅仅把一个它作用的表达式转换成常量。它可以使一个本来不是const类型的数据转换成const类型的，或者把const属性去掉。
尽量不要使用const_cast,如果发现调用自己的函数，竟然使用了const_cast，那就赶紧打住，重新考虑一下设计吧。
**4）reinterpret_cast<T*>(a)**
编译器在编译期处理
任何指针都可以转换成其它类型的指针，T必须是一个指针、引用、算术类型、指向函数的指针或指向一个类成员的指针。
表达式reinterpret_cast<T*>(a)能够用于诸如char* 到 int*，或者One_class* 到 Unrelated_class*等类似这样的转换，因此可能是不安全的。
class A { ... };
class B { ... };
void f()
{
    A* pa = new A;
    void* pv = reinterpret_cast<A*>(pa);
    // pv 现在指向了一个类型为B的对象，这可能是不安全的
    ...
}
使用reinterpret_cast 的场合不多，仅在非常必要的情形下，其他类型的强制转换不能满足要求时才使用。
== ===========================================
== static_cast .vs. reinterpret_cast 
== ================================================
reinterpret_cast是为了映射到一个完全不同类型的意思，这个关键词在我们需要把类型映射回原有类型时用到它。我们映射到的类型仅仅是为了故弄玄虚和其他目的，这是所有映射中最危险的。(这句话是C++编程思想中的原话) 
static_cast 和 reinterpret_cast 操作符修改了操作数类型。它们不是互逆的； 
static_cast 在编译时使用类型信息执行转换，在转换执行必要的检测(诸如指针越界计算, 类型检查). 其操作数相对是安全的。
另一方面；reinterpret_cast是C++里的强制类型转换符,操作符修改了操作数类型,但仅仅是重新解释了给出的对象的比特模型而没有进行二进制转换。
例子如下：
int n=9; 
double d=static_cast < double > (n); 
上面的例子中, 我们将一个变量从 int 转换到 double。这些类型的二进制表达式是不同的。 要将整数 9 转换到 双精度整数 9，static_cast 需要正确地为双精度整数 d 补足比特位。其结果为 9.0。
而reinterpret_cast 的行为却不同: 
int n=9; 
double d=reinterpret_cast<double & > (n);
这次, 结果有所不同. 在进行计算以后, d 包含无用值. 这是因为 reinterpret_cast 仅仅是复制 n 的比特位到 d, 没有进行必要的分析. 
因此, 你需要谨慎使用 reinterpret_cast.
reinterpret_casts的最普通的用途就是在函数指针类型之间进行转换。
例如，假设你有一个函数指针数组：
typedefvoid(*FuncPtr)();//FuncPtr is一个指向函数的指针，该函数没有参数,返回值类型为void
FuncPtrfuncPtrArray[10];//funcPtrArray是一个能容纳10个FuncPtrs指针的数组
让我们假设你希望（因为某些莫名其妙的原因）把一个指向下面函数的指针存入funcPtrArray数组：
int doSomething();
你不能不经过类型转换而直接去做，因为doSomething函数对于funcPtrArray数组来说有一个错误的类型。在FuncPtrArray数组里的函数返回值是void类型，而doSomething函数返回值是int类型。
funcPtrArray[0] = &doSomething;//错误！类型不匹配
reinterpret_cast可以让你迫使编译器以你的方法去看待它们：
funcPtrArray[0] = reinterpret_cast<FuncPtr>(&doSomething);
转换函数指针的代码是不可移植的（C++不保证所有的函数指针都被用一样的方法表示），在一些情况下这样的转换会产生不正确的结果
 
