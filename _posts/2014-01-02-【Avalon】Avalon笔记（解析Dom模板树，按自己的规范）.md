---
layout: post
title: 【Avalon】Avalon笔记（解析Dom模板树，按自己的规范）
date: 2014-01-02 20:29:00
categories: [JavaScript]
tags: []
---

解析Dom树:
{{}}结点：生成求值函数(ViewModel)和刷新函数(View)，立即进行一次求值（注册监听器到setter/getter）和刷新





解析流程：
domReady扫描Dom树：
         特定结点（带ms-属性/{{ }}结点）
        （1）解析属性
                     ms-important/ms-controller取ViewModel
                     ms-xxxx 绑定
        （2）解析子结点（传递vm链）：ms-/{{ }}的元素结点/文本结点



解析{{}}：生成求值器和绑定器，求值（触发getter：注册监听器），绑定（刷新界面）




内部函数说明：
scanTag：扫描元素结点

scanText：扫描文本结点

scanNodes：扫描子结点

          
          


