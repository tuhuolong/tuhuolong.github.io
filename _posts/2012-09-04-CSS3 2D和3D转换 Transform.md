---
layout: post
title: CSS3 2D和3D转换 Transform
date: 2012-09-04 12:57:00
categories: [浏览器, WebKit]
tags: [css, matrix, safari, firefox, opera, chrome]
---
Transform 适应于对任一DOM元素的2D或3D转换,这些转换效果有:旋转(rotate)、拉伸(scale)、平移(move)、倾斜(skew)等，利用Transform和javascript可以制作一些简单的动画，可以为web应用带来些意想不到的效果。
目前浏览器并不是完全支持所有的Transform ，IE9、Firefox 和Opera 仅支持2D transforms ，相应的CSS定义为：-ms-transform 、-moz-transform 和-o-transform ，而Safari 和Chrome 都支持2D和3D transforms ，相应的CSS定义为：-webkit-transform 。
下面给出一个transform定义实例：
`div``{``transform:rotate(``7``deg);``-ms-transform:rotate(``7``deg);
``/*
 IE 9 */``-moz-transform:rotate(``7``deg);
``/*
 Firefox */``-webkit-transform:rotate(``7``deg);
``/*
 Safari and Chrome */``-o-transform:rotate(``7``deg);
``/*
 Opera */``}``<font
 color=``"#c0504d"``>``1.1` `transform</font>``语法:transform:
``none``|<i>transform-functions</i>;``说明:`Value
Description
none
定义这不需要转换
matrix(n,n,n,n,n,n)
使用有6个值的矩阵(matrix)来定义一个 2D 转换
matrix3d
(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)
使用有4*4的矩阵(matrix)来定义一个 3D 转换
translate(x,y)
定义一个2D 转换,x:水平位移,y:垂直位移,个人觉得和margin-left(left) 和margin-top(top)类似.
translate3d(x,y,z)
定义一个3D 转换,x:水平位移,y:垂直位移,z:Z方向的位移
translateX(x)
定义一个水平方向2D 转换
translateY(y)
定义一个垂直方向2D 转换
translateZ(z)
定义一个Z方向3D 转换
scale(x,y)
定义一个2D 拉伸, x:水平拉伸,y:垂直拉伸
scale3d(x,y,z)
义一个3D 拉伸, x:水平拉伸,y:垂直拉伸,z:Z方向的拉伸
scaleX(x)
定义一个水平方向拉伸
scaleY(y)
定义一个垂直方向拉伸
scaleZ(z)
定义一个Z方向的拉伸
rotate(angle)
定义一个 2D 旋转, angle :旋转角度(deg)
rotate3d(x,y,z,angle)
定义一个 3D 旋转,
rotateX(angle)
定义一个X轴3D旋转
rotateY(angle)
定义一个Y轴3D旋转
rotateZ(angle)
定义一个Z轴3D旋转
skew(x-angle,y-angle)
定义一个X轴和Y轴的2D 倾斜
skewX(angle)
定义一个X轴的2D 倾斜
skewY(angle)
定义一个Y轴的2D 倾斜
perspective(n)
定义一个3D转换效果和透视图
1.2 transform-origin
定义元素旋转的位置，配合transform使用。
语法：transform-origin: x-axis y-axis z-axis;
说明：
Property Value
Description
x-axis
[]()[定义]()X轴的转换位置. 可能的值有:
- left
- center
- right
- length
- %

y-axis
定义Y轴的转换位置. 可能的值有:
- top
- center
- bottom
- length
- %

z-axis
定义Z轴的转换位置. 可能的值有:
- length

1.3 transform-style
对于支持3D转换的浏览器，指定元素内的子元素是否进入其3D环境中。
语法：transform-style: flat|preserve-3d;
说明：
Property Value
Description
flat
子元素不参与3D转换
preserve-3d
子元素将显示在3D环境中
实例:
