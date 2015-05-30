---
layout: post
title: WebView::drawExtras
date: 2012-07-18 14:21:00
categories: [WebKit, Android]
tags: []
---
WebView::drawExtras 绘制额外
        Find


        Selection (文字选择 下标+高亮文字)

                updateSelectionHandles()
        CursorRing (链接选择 边框)
                drawCursorPreamble(root) && m_ring.setup()
       
        (DrawExtra*)extra->draw


     

