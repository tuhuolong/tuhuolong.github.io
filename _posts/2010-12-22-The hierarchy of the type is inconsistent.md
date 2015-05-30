---
layout: post
title: The hierarchy of the type is inconsistent
date: 2010-12-22 09:24:00
categories: [Java]
tags: [hierarchy, compiler, eclipse, library, class, build]
---
#
These errors happend because some[interface](http://www.velocityreviews.com/forums/t149067-the-hierarchy-of-the-type-is-inconsistent.html#)/class in the 
hierarchy cannot be resolve.
For example: the error is occur in your 
class - class x, x inherits y, and in turn, y inherits z. However, the[compiler![](http://kona.kontera.com/javascript/lib/imgs/grey_loader.gif)](http://www.velocityreviews.com/forums/t149067-the-hierarchy-of-the-type-is-inconsistent.html#)cannot resolve z (in above error is EventManager), because z 
is belong to a library that is not included.
Therefore, you have to 
add package containing z to the classpath/ or project's[Java](http://www.velocityreviews.com/forums/t149067-the-hierarchy-of-the-type-is-inconsistent.html#)Build Path (if you are using eclipse).
