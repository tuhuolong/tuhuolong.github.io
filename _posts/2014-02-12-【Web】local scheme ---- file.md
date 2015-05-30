---
layout: post
title: 【Web】local scheme ---- file:///
date: 2014-02-12 11:54:00
categories: [Web]
tags: []
---
	WebKit considers certain URL schemes to be "local". One of these is file:. Pages with non-local schemes aren't allowed to load resources from local schemes for security reasons. (E.g., it would be bad if http://www.example.com/ could use <iframe src=file:///etc/passwd> to read your passwords!)
	
