---
layout: post
title: "Android滑屏 mScrollX mScrollY scrollTo() scrollBy()"
date: 2012-09-07 10:56:00 
comments: true
categories: [android]
tags: [android]
description: "Android滑屏 mScrollX mScrollY scrollTo() scrollBy()"
keywords: android
---


 
  
   首先 ，我们必须明白在
   
    
     Android View视图是没有边界的，Canvas是没有边界的
    
   
   ，只不过我们通过绘制特定的View时对
  
 
 
  
   Canvas对象进行了一定的操作，例如 : translate(平移)、clipRect(剪切)等，以便达到我们的对该Canvas对象绘制的要求 ，
  
 
 
  
   我们可以将这种无边界的视图称为“
   
    
     视图坐标
    
   
   ”-----它不受物理屏幕限制。通常我们所理解的一个Layout布局文件只是该视
  
 
 
  
   图的显示区域，超过了这个显示区域将不能显示到父视图的区域中 ，对应的，我们可以将这种无边界的视图称为“
   
    
     布局坐标
    
   
   ”
  
 
 
  
   ------ 父视图给子视图分配的布局(layout)大小。 而且， 一个视图的在屏幕的其实坐标位于视图坐标起始处，如下图所示。
  
 
 
  
  
 
 
  
   这么来说吧 ，世界本是无边无界的，可是我们的眼睛我们的心约束了我们所看到的“世界” 。
  
 
 
  
  
 
 
  
   如下所示：
  
 
 
  
   
  
 
 
  
  
 
 
  
   黑色框框表示该子视图的布局坐标， 褐色框框表示该子视图的视图坐标--该坐标是无限的，超过了父视图给子视图
  
 
 
  
   规定的区域后，不再显示该超出内容。
  
 
 
  
  
 
 
  
   那么下面的问题就是：如何将我们的视图的任意坐标能显示到该视图的中心坐标上呢？ 由于该布局位置是只能显示特定的
  
 
 
  
   一块视图内容 ，因此我们需要通过scrollTo()或者scrollBy()方法将我们期望的视图“滚动”至布局坐标上。
  
 
 
  
  
 
 
  
   在View.java中提供了了如下两个变量以及相应的属性方法去读取滚动值 ，如下： View.java类中
  
  
  
 
 
  
   
    
     
      
       
        
         01
        
       
       
        
         /**
        
       
      
     
    
   
   
    
     
      
       
        
         02
        
       
       
        
        
        
         *
 The offset, in pixels, by which the content of this view is scrolled
        
       
      
     
    
   
   
    
     
      
       
        
         03
        
       
       
        
        
        
         *
 horizontally.
        
       
      
     
    
   
   
    
     
      
       
        
         04
        
       
       
        
        
        
         *
 {@hide}
        
       
      
     
    
   
   
    
     
      
       
        
         05
        
       
       
        
        
        
         */
        
       
      
     
    
   
   
    
     
      
       
        
         06
        
       
       
        
        
        
         protected
        
        
         int
        
        
         mScrollX;
        
        
         //该视图内容相当于视图起始坐标的偏移量  
 ， X轴 方向
        
       
      
     
    
   
   
    
     
      
       
        
         07
        
       
       
        
        
        
         /**
        
       
      
     
    
   
   
    
     
      
       
        
         08
        
       
       
        
        
        
         *
 The offset, in pixels, by which the content of this view is scrolled
        
       
      
     
    
   
   
    
     
      
       
        
         09
        
       
       
        
        
        
         *
 vertically.
        
       
      
     
    
   
   
    
     
      
       
        
         10
        
       
       
        
        
        
         *
 {@hide}
        
       
      
     
    
   
   
    
     
      
       
        
         11
        
       
       
        
        
        
         */
        
       
      
     
    
   
   
    
     
      
       
        
         12
        
       
       
        
        
        
         protected
        
        
         int
        
        
         mScrollY;
        
        
         //该视图内容相当于视图起始坐标的偏移量  
 ， Y轴方向
        
       
      
     
    
   
   
    
     
      
       
        
         13
        
       
       
       
      
     
    
   
   
    
     
      
       
        
         14
        
       
       
        
        
        
         /**
        
       
      
     
    
   
   
    
     
      
       
        
         15
        
       
       
        
        
        
         *
 Return the scrolled left position of this view. This is the left edge of
        
       
      
     
    
   
   
    
     
      
       
        
         16
        
       
       
        
        
        
         *
 the displayed part of your view. You do not need to draw any pixels
        
       
      
     
    
   
   
    
     
      
       
        
         17
        
       
       
        
        
        
         *
 farther left, since those are outside of the frame of your view on
        
       
      
     
    
   
   
    
     
      
       
        
         18
        
       
       
        
        
        
         *
 screen.
        
       
      
     
    
   
   
    
     
      
       
        
         19
        
       
       
        
        
        
         *
        
       
      
     
    
   
   
    
     
      
       
        
         20
        
       
       
        
        
        
         *
 @return The left edge of the displayed part of your view, in pixels.
        
       
      
     
    
   
   
    
     
      
       
        
         21
        
       
       
        
        
        
         */
        
       
      
     
    
   
   
    
     
      
       
        
         22
        
       
       
        
        
        
         public
        
        
         final
        
        
         int
        
        
         getScrollX()
 {
        
       
      
     
    
   
   
    
     
      
       
        
         23
        
       
       
        
        
        
         return
        
        
         mScrollX;
        
       
      
     
    
   
   
    
     
      
       
        
         24
        
       
       
        
        
        
         }
        
       
      
     
    
   
   
    
     
      
       
        
         25
        
       
       
       
      
     
    
   
   
    
     
      
       
        
         26
        
       
       
        
        
        
         /**
        
       
      
     
    
   
   
    
     
      
       
        
         27
        
       
       
        
        
        
         *
 Return the scrolled top position of this view. This is the top edge of
        
       
      
     
    
   
   
    
     
      
       
        
         28
        
       
       
        
        
        
         *
 the displayed part of your view. You do not need to draw any pixels above
        
       
      
     
    
   
   
    
     
      
       
        
         29
        
       
       
        
        
        
         *
 it, since those are outside of the frame of your view on screen.
        
       
      
     
    
   
   
    
     
      
       
        
         30
        
       
       
        
        
        
         *
        
       
      
     
    
   
   
    
     
      
       
        
         31
        
       
       
        
        
        
         *
 @return The top edge of the displayed part of your view, in pixels.
        
       
      
     
    
   
   
    
     
      
       
        
         32
        
       
       
        
        
        
         */
        
       
      
     
    
   
   
    
     
      
       
        
         33
        
       
       
        
        
        
         public
        
        
         final
        
        
         int
        
        
         getScrollY()
 {
        
       
      
     
    
   
   
    
     
      
       
        
         34
        
       
       
        
        
        
         return
        
        
         mScrollY;
        
       
      
     
    
   
   
    
     
      
       
        
         35
        
       
       
        
        
        
         }
        
       
      
     
    
   
  
 
 
  
  
 
 
  
   注意，所谓的“by which the content of this view is scrolled”表示该偏移量只针对于该View中onDraw()方法里的
  
 
 
  
   具体内容实现，而不针对背景图片等 。具体原因可参考<
   
    
     
      
       Android中View绘制流程以及invalidate()等相关方法分析
      
     
    
   
   >;
  
 
 
  
  
 
 
  
   提示：下文中提到的当前视图内容是在绘制在布局坐标处的内容。
  
 
 
 
 
  
   public void
   
    scrollTo
   
   (int x, int y)
  
 
 
  
   说明：在当前视图内容偏移至(x , y)坐标处，即显示(可视)区域位于(x , y)坐标处。
  
 
 
  
   方法原型为： View.java类中
  
 
 
  
   
    
     查看源码
    
    
    
    
     打印
    
    
     ?
    
   
  
  
   
    
     
      
       
        
         01
        
       
       
        
         /**
        
       
      
     
    
   
   
    
     
      
       
        
         02
        
       
       
        
        
        
         *
 Set the scrolled position of your view. This will cause a call to
        
       
      
     
    
   
   
    
     
      
       
        
         03
        
       
       
        
        
        
         *
 {@link #onScrollChanged(int, int, int, int)} and the view will be
        
       
      
     
    
   
   
    
     
      
       
        
         04
        
       
       
        
        
        
         *
 invalidated.
        
       
      
     
    
   
   
    
     
      
       
        
         05
        
       
       
        
        
        
         *
 @param x the x position to scroll to
        
       
      
     
    
   
   
    
     
      
       
        
         06
        
       
       
        
        
        
         *
 @param y the y position to scroll to
        
       
      
     
    
   
   
    
     
      
       
        
         07
        
       
       
        
        
        
         */
        
       
      
     
    
   
   
    
     
      
       
        
         08
        
       
       
        
         public
        
        
         void
        
        
         scrollTo(
        
        
         int
        
        
         x,
        
        
         int
        
        
         y)
 {
        
       
      
     
    
   
   
    
     
      
       
        
         09
        
       
       
        
        
        
         //偏移位置发生了改变
        
       
      
     
    
   
   
    
     
      
       
        
         10
        
       
       
        
        
        
         if
        
        
         (mScrollX
 != x || mScrollY != y) {
        
       
      
     
    
   
   
    
     
      
       
        
         11
        
       
       
        
        
        
         int
        
        
         oldX
 = mScrollX;
        
       
      
     
    
   
   
    
     
      
       
        
         12
        
       
       
        
        
        
         int
        
        
         oldY
 = mScrollY;
        
       
      
     
    
   
   
    
     
      
       
        
         13
        
       
       
        
        
        
         mScrollX
 = x;
        
        
         //赋新值，保存当前便宜量
        
       
      
     
    
   
   
    
     
      
       
        
         14
        
       
       
        
        
        
         mScrollY
 = y;
        
       
      
     
    
   
   
    
     
      
       
        
         15
        
       
       
        
        
        
         //回调onScrollChanged方法
        
       
      
     
    
   
   
    
     
      
       
        
         16
        
       
       
        
        
        
         onScrollChanged(mScrollX,
 mScrollY, oldX, oldY);
        
       
      
     
    
   
   
    
     
      
       
        
         17
        
       
       
        
        
        
         if
        
        
         (!awakenScrollBars())
 {
        
       
      
     
    
   
   
    
     
      
       
        
         18
        
       
       
        
        
        
         invalidate();
        
        
         //一般都引起重绘
        
       
      
     
    
   
   
    
     
      
       
        
         19
        
       
       
        
        
        
         }
        
       
      
     
    
   
   
    
     
      
       
        
         20
        
       
       
        
        
        
         }
        
       
      
     
    
   
   
    
     
      
       
        
         21
        
       
       
        
         }
        
       
      
     
    
   
  
 
 
 
 
  
  
  
   public void
   
    scrollBy
   
   (int
 x, int y)
  
 
 
  
   说明：在当前视图内容继续偏移(x , y)个单位，显示(可视)区域也跟着偏移(x,y)个单位。
  
 
 
  
   方法原型为： View.java类中
  
 
 
  
   
    
     
      
       
        
         01
        
       
       
        
         /**
        
       
      
     
    
   
   
    
     
      
       
        
         02
        
       
       
        
        
        
         *
 Move the scrolled position of your view. This will cause a call to
        
       
      
     
    
   
   
    
     
      
       
        
         03
        
       
       
        
        
        
         *
 {@link #onScrollChanged(int, int, int, int)} and the view will be
        
       
      
     
    
   
   
    
     
      
       
        
         04
        
       
       
        
        
        
         *
 invalidated.
        
       
      
     
    
   
   
    
     
      
       
        
         05
        
       
       
        
        
        
         *
 @param x the amount of pixels to scroll by horizontally
        
       
      
     
    
   
   
    
     
      
       
        
         06
        
       
       
        
        
        
         *
 @param y the amount of pixels to scroll by vertically
        
       
      
     
    
   
   
    
     
      
       
        
         07
        
       
       
        
        
        
         */
        
       
      
     
    
   
   
    
     
      
       
        
         08
        
       
       
        
        
        
         //
 看出原因了吧 。。 mScrollX 与 mScrollY 代表我们当前偏移的位置 ， 在当前位置继续偏移(x ,y)个单位
        
       
      
     
    
   
   
    
     
      
       
        
         09
        
       
       
        
        
        
         public
        
        
         void
        
        
         scrollBy(
        
        
         int
        
        
         x,
        
        
         int
        
        
         y)
 {
        
       
      
     
    
   
   
    
     
      
       
        
         10
        
       
       
        
        
        
         scrollTo(mScrollX
 + x, mScrollY + y);
        
       
      
     
    
   
   
    
     
      
       
        
         11
        
       
       
        
        
        
         }
        
       
      
     
    
   
  
 
 
  
  
 
 
  
  
 
 
  
   第一个小Demo非常简单 ，大家重点理解与掌握scrollTo() 与 scrollBy()函数的用法和区别。
  
 
 
  
  
 
 
  
   第二个小Demo则有了Launcher的模样，能够左右切换屏幕 。实现功能如下： 采用了一个自定义ViewGroup，该ViewGroup
  
 
 
  
   对象包含了3个LinearLayout子视图，并且以一定的布局坐标(由layout()方法指定)显示在ViewGroup上。 接下来，即可调用该
  
 
 
  
   ViewGroup对象的scrollTo或者scrollBy()方法切换指定视图内容了，即切换屏幕。 呵呵 ，挺好玩的吧 。
  
 
 
 
 
  
   如果对View绘制流程不懂的，可以参考我的这篇博客
   
    <
   
   
    
     
      
       Android中View绘制流程以及invalidate()等相关方法分析
      
     
    
   
   >;
 。
  
 


