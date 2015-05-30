---
layout: post
title: skipping incompatible
date: 2012-05-08 15:28:00
categories: [零碎]
tags: [编译器]
---
*bin/ld: skipping incompatible ./l*.a when searching for -l*
*/bin/ld: cannot find -l*
collect2: ld returned 1 exit status


个人非专业翻译：“跳过不匹配的库 * ，当查找库 * 时”
库链接失败。

可能的原因：
1.版本不兼容。
2.编译库的编译器和现在用的编译器不一致。