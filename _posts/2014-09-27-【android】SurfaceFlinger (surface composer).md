---
layout: post
title: 【android】SurfaceFlinger (surface composer)
date: 2014-09-27 18:49:00
categories: [Android]
tags: []
---
SurfaceFlinger is an Android system service, responsible for compositing all the application and system surfaces into a single buffer that is finally to be displayed by display controller.
Let's zoom in above statement.
SurfaceFlinger is a system wide service but it is not directly available to application developer as Sensor or other services can be. Every time you want to update your UI, SurfaceFlinger will kick in. This explains why SurfaceFlinger is a battery drainer.
Besides your application surfaces, there are system surfaces, including status bar, navigation bar and, when rotation happens, surfaces created by the system for rotation animation. Most applications have only one active surface - the one of current foreground
 activity, others have more than one when SurfaceView is used in the view hierarchy or Presentation mode is used.
SurfaceFlinger is responsible for COMPOSITING all those surfaces. A common misunderstanding is that SurfaceFinger is for DRAWING. It is not correct. Drawing is the job of OpenGL. The interesting thing is SurfaceFlinger used openGL for compositing as well.
The composition result will be put in a system buffer, or native window, which is the source for display controller to fetch data from. This is what you see in the screen.
