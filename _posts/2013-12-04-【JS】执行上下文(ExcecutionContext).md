---
layout: post
title: 【JS】执行上下文(ExcecutionContext)
date: 2013-12-04 19:59:00
categories: [JavaScript]
tags: []
---
	ExecutionContext = {
	    VariableObject: {
	        arguments: {
	            0: 22,
	            length: 1
	        },
	        arg1: 22,
	        c: pointer to function c()
	        a: undefined,
	        b: undefined
	    },
	    ScopeChain: { ... },
	    this: { ... }   调用对象
	}
