---
layout: post
title: "ant external lib"
date: 2015-04-24 22:54:00 
comments: true
categories: []
tags: []
description: "ant external lib"
keywords: 
---


 
  
   custom_rules.xml
  
  <?xml version="1.0" encoding="UTF-8"?>;
<project name="custom_rules">;

     <path id="java.compiler.classpath.path">;
         <fileset dir="libs_ex" includes="*.jar" />;
     </path>;
     <property name="java.compiler.classpath" refid="java.compiler.classpath.path" />;

</project>; 
 
 
  $(function () {
                $('pre.prettyprint code').each(function () {
                    var lines = $(this).text().split('\n').length;
                    var $numbering = $('').addClass('pre-numbering').hide();
                    $(this).addClass('has-numbering').parent().append($numbering);
                    for (i = 1; i ').text(i));
                    };
                    $numbering.fadeIn(1700);
                });
            });
 


