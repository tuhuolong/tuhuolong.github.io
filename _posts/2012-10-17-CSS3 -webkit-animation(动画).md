---
layout: post
title: CSS3 -webkit-animation(动画)
date: 2012-10-17 11:22:00
categories: [Tuhuolong, WebKit]
tags: [css, delay, animation, 测试, div]
---
@-webkit-keyframes name{

}

-webkit-animation : name  duration  iteration-count  direction  timing-function
  fill-mode  delay
或者
@-webkit-keyframes fontbulger {
        0% {
                font-size: 10px;
        }
        30% {
                font-size: 15px;
        }
        100% {
                font-size: 12px;
        }
}
#box {
        -webkit-animation-name:  fontbulger;
        -webkit-animation-duration: 1s;
        -webkit-animation-iteration-count:3;
        -webkit-animation-direction: alternate;
        -webkit-animation-timing-function: ease-out;
        -webkit-animation-fill-mode: both;
        -webkit-animation-delay: 2s;
}
    -webkit-animation:仍旧是一个复合属性，
-webkit-animation: name duration timing-function delay iteration_count direction;
包括以下几个属性
（1）  -webkit-animation-name    这个属性的使用必须结合@-webkit-keyframes一起使用
eg:  
@-webkit-keyframes fontchange{
        0%{font-size:10px;}
        30%{font-size:15px;}
        100%{font-siez:12px;}
}
百分比的意思就是duration的百分比，如果没有设置duration的话，则表示为无穷大
div{-webkit-animation-name:fontchange;}
（2）-webkit-animation-duration   表示动画持续的时间
（3）-webkit-animation-timing-function  表示动画使用的时间曲线
【语法】： -webkit-animation-timing-function: ease | linear | ease-in | ease-out | ease-in-out
（4）-webkit-animation-delay    表示开始动画之前的延时
【语法】 -webkit-animation-delay: delay_time;
（5）-webkit-animation-iteration-count  表示动画要重复几次
【语法】-webkit-animation-iteration-count: times_number;
（6） -webkit-animation-direction   表示动画的方向
【语法】-webkit-animation-direction: normal(默认)  | alternate
normal  方向始终向前
alternate 当重复次数为偶数时方向向前，奇数时方向相反
【另外】跟animation有关的其他属性
（1）-webkit-animation-fill-mode : none (默认)| backwards | forwards | both  设置动画开始之前和结束之后的行为（测试结
果不是很清晰）
（2）-webkit-animation-play-state: running（默认） | paused  设置动画的运行状态
