---
layout: post
title: ant external lib
date: 2015-04-24 22:54:00
categories: [Android]
tags: []
---
custom_rules.xml
	<?xml version="1.0" encoding="UTF-8"?>
	<projectname="custom_rules">
	
	     <pathid="java.compiler.classpath.path">
	         <filesetdir="libs_ex"includes="*.jar" />
	     </path>
	     <propertyname="java.compiler.classpath"refid="java.compiler.classpath.path" />
	
	</project> 
