---
layout: post
title: 【MapReduce】
date: 2014-01-06 10:26:00
categories: [算法]
tags: []
---
MapReduce通过把对数据集的大规模操作分发给网络上的每个节点实现可靠性（Map）；每个节点会周期性地把完成的工作和状态的更新报告回来（Reduce）。大多数分布式运算可以抽象为MapReduce操作。Map是把输入Input分解成中间的KeyValue对，Reduce把KeyValue合成最终输出Output。这两个函数由程序员提供给系统，下层设施把Map和Reduce操作分布在集群上运行