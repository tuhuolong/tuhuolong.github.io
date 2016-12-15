---
layout: post
title: "Position(Static, Absolute, Relative, Fixed)"
date: 2012-10-20 14:03:00 
comments: true
categories: [attributes]
tags: [attributes]
description: "Position(Static, Absolute, Relative, Fixed)"
keywords: attributes
---


 
  
   
    Static
   
   :
   
    正常页面流
   
   （结点）
   
    流
   
  
 
 
  
   
    Relative
   
   : 相对于
   
    自己
   
   ，在Static结点的上面（z-index）
  
 
 
  
   
    Absolute
   
   : 相对于第一个非Static
   
    父结点
   
   ，不属于页面流（结点流）（不会影响其他结点）
  
 
 
  
   
    Fixed
   
   : 相对于
   
    窗口
   
   (Viewport)
  
 
 
  
   
   
  
 
 
 
 
  
   
    
    
   
  
 
 
  
   
    
     Static
    
    . This is the default for everysingle page element. Different elements don't have different default values forpositioning, they all start out as static. Static
 doesn't mean much, it justmeans that the element will flow into the page as it normally would. The onlyreason you would ever set an element to position: static is toforcefully-remove some positioning that got applied to an element outside ofyour control. This
 is fairly rare, as positioning doesn't cascade.
   
  
 
 
  
   
    
     Relative
    
    . This type of positioning isprobably the most confusing and misused. What it really means is "relativeto itself". If you set position: relative; on an element
 but noother positioning attributes (top, left, bottom or right), it will no effect onit's positioning at all, it will be exactly as it would be if you left it asposition: static; But if you DO give it some other positioning attribute,say, top: 10px;, it will
 shift it's position 10 pixels DOWN from where itwould NORMALLY be. I'm sure you can imagine, the ability to shift an elementaround based on it's regular position is pretty useful. I find myself usingthis to line up form elements many times that have a tendency
 to not want toline up how I want them to.
   
  
 
 
  
   
    There are two other things that happen when you set position: relative; on an element that you should be aware of. One is that it introduces the ability to use z-index on that element,
 which doesn't really work with statically positioned elements. Even if you don't set a z-index value, this element will now appear on top of any other statically positioned element. You can't fight it by setting a higher z-index value on a statically positioned
 element. The other thing that happens is it limits the scope of absolutely positioned child elements. Any element that is a child of the relatively positioned element can be absolutely positioned within that block. This brings up some powerful opportunities
 whichI
    
     talkabout here
    
    .
   
  
 
 
  
   
    
     Absolute
    
    . This is a very powerful type of positioning that allows you to literally place any page element exactly where you want it. You use the positioning attributes
 top, left bottom and right toset the location. Remember that these values will be relative to the next parent element with relative (or absolute) positioning. If there is no suchparent, it will default all the way back up to the <html>; element itself meaning
 it will be placed relatively to the page itself.
   
  
 
 
  
   
    The trade-off, and most important thing to remember, about absolute positioning is that these elements are removed from the flow of elements on the page. An element with this type of
 positioning is not affected by other elements and it doesn't affect other elements. This is aserious thing to consider every time you use absolute positioning. It's over use or improper use can limit the flexibility of your site.
   
  
 
 
  
   
    
     Fixed
    
    . This type of positioning is fairlyrare but certainly has its uses. A fixed position element is positioned relativeto the viewport, or the browser window itself.
 The viewport doesn't changewhen the window is scrolled, so a fixed positioned element will stay rightwhere it is when the page is scrolled, creating an effect a bit like the oldschool "frames" days. Take a look at
    
     this
 site
    
    (update: deadlink, sorry), where the left sidebar is fixed. This site is a perfect examplefor since it exhibits both good and bad traits of fixed positioning. The goodis that it keeps the navigation present at all times on the page and it createsand
 interested effect on the page. The bad is that there are some usabilityconcerns. On my smallish laptop, the content in the sidebar is cut off andthere is no way from me to scroll down to see the rest of that content. Also ifI scroll all the way down to the
 footer, it overlaps the footer content notallowing me to see all of that. Cool effect, can be useful, but needs to bethoroughly tested.
   
  
 
 
 


