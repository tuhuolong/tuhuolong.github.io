---
layout: post
title: isValidResourceType
date: 2015-05-22 17:30:00
categories: [Android]
tags: []
---
	bool isValidResourceType(const String8& type)
	{
	    return type == "anim" || type == "animator" || type == "interpolator"
	        || type == "transition"
	        || type == "drawable" || type == "layout"
	        || type == "values" || type == "xml" || type == "raw"
	        || type == "color" || type == "menu" || type == "mipmap";
	}
