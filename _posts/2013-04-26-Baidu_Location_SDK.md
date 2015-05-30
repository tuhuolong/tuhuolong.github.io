---
layout: post
title: Baidu_Location_SDK
date: 2013-04-26 14:44:00
categories: [Android]
tags: []
---
(1)启动注册GPS+WiFi+基站服务

GPS:
WiFi:SCAN_RESULTS_AVAILABLE_ACTION
基站:LISTEN_SIGNAL_STRENGTHS+LISTEN_CELL_LOCATION
                  (2)请求定位获取GPS+WiFi+基站信息
GPS:转换成BDLocation直接返回
网络定位:将所有定位信息上传到服务端处理，转换成BDLocation返回


