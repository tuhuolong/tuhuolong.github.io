---
layout: post
title: Java Annotation
date: 2010-10-15 09:54:00
categories: [Java]
tags: [java, annotations, 编译器, class, compiler, processing]
---
**1. 定义**
annotate 英文里面注解的意思。Java annotation 又称为java 内注( 内建) 
的意思。从sun 官方网站上面看到关于java 内注的定义：java 内注提供了关于代码的一些数据( 注解), 它本身不是java 
代码的一部分。它不能直接影响它注释的代码的运行。java 在jdk5 之前本身提供了一些专门的注解机制( 如transient ：防止序列化) 。
网上看到的另外的一些定义：Annotation 
提供了一条与程序元素关联任何信息或者任何元数据（metadata ）的途径。从某些方面看，annotation 
就像修饰符一样被使用，并应用于包、类型、构造方法、方法、成员变量、参数、本地变量的声明中。这些信息被存储在annotation 
的“name=value” 结构对中。 annotation 类型是一种接口，能够通过java 反射API 的方式提供对其信息的访问。
****
**2. 按功能分类 Annotations**
java 的内注有些是给编译器使用的，有些是运行时可调用的关于其功能的分类可参考： 
Sun 官网里关于 Annotations 的分 类：
1.( 为编译起提供信息)information for the compiler 
：为编译器提供信息，如编译器可以用来检查错误和忽略警告。
2.( 编译时或部署时处理)Compiler-time and deployment-time 
processing ：可以让软件工具通过处理内注信息来产生代码、xml 文件等等。
3.( 运行时处理)runtime processing( 运行时处理) 
：有一些特殊的内注可以在运行时进行处理。
java5 本身提供了供编译器自己使用的几个内注，先来理解下这几个内注: 
Annotations Used by the Compiler: 
@Deprecated : 用来告诉编译器这个方法已经被遗弃了不再维护，当有代码引用该方法时，会提示。
@Override : 
用来告诉编译器，该方法重写了父类的某个方法。这个有的时候有助于查找拼写错误。如果用了才标记，但是父类却没有该方法，则会报错。
@SuppressWarnings : 
用来告诉编译器忽略一些警告。@SuppressWarnings({"unchecked", "deprecation"}) 
用于忽略unchecket 和deprecation 的警告。
这几个是java 
内部定义好的一些内注，我们平常在编写代码时候应该都有接触到。我们也可以自己定义内注，请继续看下面的章节。
****
**3. 语法和使用**
此功能由一个定义annotation 类型的语法和一个描述annotation 
声明的语法，读取annotaion 的API ，一个使用annotation 修饰的class 文件，一个annotation 处理工具（apt
 ）组成。
** 3.1 格式**
Annotation 类型声明于一般的接口声明极为类似，区别只在于它在interface 
关键字前面使用“@” 符号。Annotation 类型可以由>=0 个的类型成员组成。Annotation 
的类型成员由类型和成员方法组成，成员方法是一个空的方法，方法名即是其成员名。类型只能是primitives 、String 、Class 
、enums 、annotation 和前面类型的数组。可以有默认值。 如下几个例子涵盖了基本的语法：
例子1: 
1. package annotation; 
  
2. public @interface Copyright {    
3. String value();    
4. }    


定义了一个内注，只有一个名为 value, 类型为 String 的成员。 
例子2: 
1. package annotation; 
   
2. public @interface Preliminary {    
3. }    


定义了一个空的内注，又称为 MarkAnnotation, 标记内注， (java 的 @overide 
就是一个标记内注 ) 
例子 3: 


package annotation; 
    
public @interface annotest {    
int id(); 
   
String synopsis();    
String engineer() default "[unassigned]" ;// 默认值是 unassigned    
String date() default "[unimplemented]" ;    
}    

定义了一个有多成员的内注，且有些有默认值。 
例子 4: 

import java.lang.annotation.*;     
/**    
* Indicates that the annotated method is a test method. This annotation should 
  
* be used only on parameterless static methods. 
  
*/  

  
@Retention (RetentionPolicy. RUNTIME )// 声明了这是一个运行时的內注， java 虚拟机不会抛弃它    
@Target (ElementType. METHOD )// 声明了该内注只对方法有用    
public @interface Test {    
}    

例子 5: 
1. package test; 
   
2.   
3. public @interface UncheckedExceptions {      
4. Class<? extends RuntimeException>[] value(); 
   
5. }    


内注是个 Class
 的数组类型。 
****
**3.2 内注的类型**
关于内注的类型主义以下几点: 
1. 普通annotation 
有成员的annotation 
2. marker annotation 类型 
一
个没有成员定义的annotation 类型被称为marker annotation 。这种annotation 
类型仅使用自身的存在与否来为我们提供信息。如Override 。
3.meta-annotation ： 
meta
 -annotation 也称为元annotation ，它是被用来声明annotation 类型的annotation 。Java5.0 
提供了一些标准的元-annotation 类型。如：
[target] 
annotation 
的target 是一个被标注的程序元素。target 说明了annotation 所修饰的对象范围：annotation 
可被用于packages 、 types （类、接口、枚举、annotation 
类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch 参数）。在annotation 
类型的声明中使用了target 可更加明晰其修饰的目标。
[retention] 
annotation 的retention 
定义了该annotation 被保留的时间长短：某些annotation 仅出现在源代码中，而被编译器丢弃；而另一些却被编译在 class 
文件中；编译在class 文件中的annotation 可能会被虚拟机忽略，而另一些在class 被装载时将被读取（请注意并不影响class 
的执行，因为annotation 与class 在使用上是被分离的）。使用这个meta-annotation 可以对annotation 
的“生命周期” 限制。
上面的例4 里声明的test 
就是使用了@Retention (RetentionPolicy. RUNTIME ) 和 
@Target (ElementType. 
METHOD ) 两个元内注进行声明的内注。前者是为了让内注能在运行时被处理，后者说明了该内注是用来修饰方法的。 
****
**3.3 内注的使用**
标注的使用很简单，就是在要修饰的地方前面加上内
注。
使用标准Annotation 
@Override
public 
String toSting() {   // 注意方法名拼写错了
    return "[" + super.toString() +
 "]";
}
本方法编译器会报错，因为使用了@Override标注之后编译器会检查父类是否有被重写的方法。显然上面的方法拼写错误，
所以就会报错。
自定义内注使用
定义：

package annotation;    
  
import java.lang.annotation.*; // import this to use @Documented    
  
@Documented 
   
public @interface ClassPreamble {    
String author();    
String date();    
int currentRevision() default 1; 
   
String lastModified() default "N/A" ; 
   
String lastModifiedBy() default "N/A" ; 
   
String[] reviewers(); // Note use of array    
}    

使用： 

@ClassPreamble (author = "John Doe" , date = "3/17/2002" , currentRevision = 6, lastModified = "4/12/2004" , lastModifiedBy = "Jane Doe" , reviewers = {     
"Alice" , "Bob" , "Cindy" } // Note array notation    
)    
public class TestAnno {      
}    

该内注提供了一个注
释的模板。 
3. 运行时处理内注 
内注定义：

package test; 
    

import java.lang.annotation.*; 
   
  
/**    
* Indicates that the annotated method is a test method. This annotation should 
  
* be used only on parameterless static methods. 
  
*/    
  
@Retention (RetentionPolicy. RUNTIME ) 
   
@Target (ElementType. METHOD ) 
   
public @interface Test {    
  
}    

 
使用 :    
package test; 
   
   
public class Sample {    
@Test 
    
public static void m1() { 
   
}
    
public static void m2() { 
   
}    
  
@Test 
   
public static void m3() { 
   
throw new RuntimeException( "Boom" );    
}    
 
public static void m4() { 
   
}    
  
@Test 
     
public static void m5() { 
   
}    
  
public static void m6() { 
   
}    
  
@Test 
   
public static void m7() { 
   
throw new RuntimeException( "Crash" );    
}    
  
public static void m8() { 
   
}    
  
public static void m9() { 
   
}    
 
}    
  

测试： 

package test;  
   
  
import java.lang.reflect.*;  
   
  
public class RunTests {     

public static void main(String[] args) throws Exception {    
int passed = 0, failed = 0; 
   
for (Method m : Class. forName (args[0]).getMethods()) {    
if (m.isAnnotationPresent( Test . class )) {    
try { 
   
m.invoke( null ); 
   
passed++;    
} catch (Throwable ex) { 
   
System. out    
.printf( "Test %s failed: %s %n" , m, ex.getCause()); 
   
failed++;    
}    
}    
}    
  
System. out .printf( "Passed: %d, Failed %d%n" , passed, failed); 
   
}    
}    
  

运行 :java RunTests test.Sample 的结果如下：

Test public static void test.Sample.m3() failed: java.lang.RuntimeException : Boom 
    
Test public static void test.Sample.m7() failed: java.lang.RuntimeException : Crash 
   
Passed: 2, Failed 2  

 
从代码和结果可以看到，通过 Java
 反射可以访问声明为 RetentionPolicy.RUNTIME 类型的 java 内注 . 
