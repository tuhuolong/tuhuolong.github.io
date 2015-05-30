---
layout: post
title: Android Canvas和Paint
date: 2012-07-18 13:45:00
categories: [Android]
tags: [android, float, colors, path, string, dst]
---
**Canvas**

drawArc(RectF oval, float startAngle, floatsweepAngle, boolean useCenter, Paint paint) **弧线**
drawBitmap(Bitmap bitmap, Rect src, Rectdst, Paint paint) **位图**
drawCircle(float cx, float cy, floatradius,Paint paint) **圆**
drawOval(RectF oval, Paint paint) **椭圆**
drawPoint(float x, float y, Paint paint) **点**
drawLine(float startX, float startY, floatstopX, float stopY, Paint paint) **线**
drawRect(RectF rect, Paint paint) **矩形**
drawText(String text, float x, floaty,Paint paint) **文本**
drawPath(Path path, Paint paint) **路径**
drawVertices(Canvas.VertexMode mode,intvertexCount,float[] verts,int vertOffset,float[] texs,int texOffset,int[]colors,int colorOffset,short[] indices,int indexOffset,int indexCount,Paintpaint)**顶点**
drawRoundRect (RectF rect, float rx, floatry, Paint paint) **圆角矩形**
drawPicture(Picture picture,RectF dst)**图片**
**
**
translate(x,y) **平移**，原点移动到（X+x，Y+y）；
rotate(a) **旋转**，旋转a度角；
scale(x,y) **伸缩**
save()，restore()：提供和一个堆栈，**保存**和**恢复**绘图状态，save将当前绘图状态压入堆栈，restore出栈，恢复绘图状态；
   
    
**Paint**
setARGB(int a, int r, int g, int b) // 设置 Paint对象颜色，参数一为alpha透明值
setAlpha(int a) // 设置alpha不透明度，范围为0~255
setAntiAlias(boolean aa) // 是否抗锯齿
setColor(int color)  // 设置颜色，这里Android内部定义的有Color类包含了一些常见颜色定义
setTextScaleX(float scaleX)  // 设置文本缩放倍数，1.0f为原始
setTextSize(float textSize)  // 设置字体大小
setUnderlineText(booleanunderlineText) // 设置下划线
 
[http://blog.csdn.net/rhljiayou/article/details/7212620](http://blog.csdn.net/rhljiayou/article/details/7212620)
  
  
