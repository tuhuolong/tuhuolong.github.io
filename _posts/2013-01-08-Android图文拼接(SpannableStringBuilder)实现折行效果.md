---
layout: post
title: Android图文拼接(SpannableStringBuilder)实现折行效果
date: 2013-01-08 20:38:00
categories: [Android]
tags: []
---
SpannableStringBuilder spanStringBuilder = new SpannableStringBuilder();

spanStringBuilder.append("tuhuolong");

ImageSpan imageSpan = new ImageSpan(context, resid);
spanStringBuilder.append("#");
spanStringBuilder.setSpan(imageSpan, spanStringBuilder.length()-1, spanStringBuilder.length(), Spannable.SPAN_INCLUSIVE_EXCLUSIVE);
TextView.setText(spanStringBuilder);
