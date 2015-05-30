---
layout: post
title: WebKit Layout 数据结构
date: 2012-07-03 11:08:00
categories: [WebKit]
tags: [layout, webkit, 数据结构, primitive, css, table]
---

**一、Render树的构成**
在我们编写网页及使用JS的时候，大概都知道DOM树及其主要构成，了解到DOM树的构建其实质是对一个html或xml文件的内容采取树结构的方式来组织及描述，不同的标签及其在文档中的位置决定了其在整颗DOM树的地位及属性，针对具体DOM树的构成及不同树节点的描述，可以参考有关DOM的相关标准等，以后有机会我们也会单独来了解。

也许对于Render树大家就不那么了解了，简单的说来，它是对DOM树更进一步的描述，其描述的内容主要与布局渲染等CSS相关属性如left、top、width、height、color、font等有关，因为不同的DOM树结点可能会有不同的布局渲染属性，甚至布局时会按照标准动态生成一些匿名节点，所以为了更加方便的描述布局及渲染，WebKit内核又生成一颗Render树来描述DOM树的布局渲染等特性，当然DOM树与Render树不是一一对应，但可以相互关联，下面分别描述其主要节点：

**1、基类RenderObject**
RenderObject作为所有Render树节点的基类，完全类似与DOM树中的Node基类，它是构成Render树的基础，作用非比寻常，其中包含了构成Render树所可能涉及到的一些基本属性及方法，内容相当多，其主要数据成员及方法分别如下：
RenderObject主要数据成员![](http://blog.chinaunix.net/photo/10867_100616210055.jpg)图一
**其中成员m_parent、m_previous、m_next为构建Render树设置好关联基础；****m_Node则为DOM树中对应的节点；
m_style成员则描述该节点对应的各种CSS基本属性数据，下面会单独介绍；
至于其他的诸如m_positioned、m_isText、m_inline、m_floating、m_replaced等则描述其特性，就像CSS标准对不同元素的属性分类定义一样，从字面上我们就可以从上一节[WebKit网页布局实现之基本概念及标准篇](http://blog.csdn.net/lq0622/article/details/6803967)中可以找到它们这么定义的踪影。

成员m_needsPositionedMovementLayout、m_normalChildNeedsLayout、m_posChildNeedsLayout、m_needsLayout等主要用来描述该RenderObject是否确实需要重新布局；
当一个新的RenderObject对象插入到Render树的时候，它会设置其m_needsLayout属性为true，同时会根据该RenderObject对象在祖先RenderObject看来是一个positioned(拥有positiong:absolute或fixed属性)状态的孩子，如是则将相应祖先RenderObject对象的属性m_posChildNeedsLayout设置为true;

如果是一个in-flow(positon:static或relative)状态的孩子，则将相应祖先RenderObject对象的属性m_normalChildNeedsLayout设置为true;

主要方法：
//与是否需要layout相关
bool needsLayout() const { return m_needsLayout || m_normalChildNeedsLayout ||m_posChildNeedsLayout; }
bool selfNeedsLayout() const { return m_needsLayout; }
bool posChildNeedsLayout() const { return m_posChildNeedsLayout; }
bool normalChildNeedsLayout() const { return m_normalChildNeedsLayout; }

//与基本属性相关
bool isFloating() const { return m_floating; }
bool isPositioned() const { return m_positioned; } // absolute or fixed positioning
bool isRelPositioned() const { return m_relPositioned; } // relative positioning
bool isText() const { return m_isText; }
bool isInline() const { return m_inline; } // inline object
bool isCompact() const { return style()->display() == COMPACT; } // compact object
bool isRunIn() const { return style()->display() == RUN_IN; } // run-in object
bool isDragging() const { return m_isDragging; }
bool isReplaced() const { return m_replaced; } // a "replaced" element (see CSS)

//与外部DOM关联相关
RenderView* view() const;
// don't even think about making this method virtual!
Node* element() const { return m_isAnonymous ? 0 : m_node; }
Document* document() const { return m_node->document(); }
void setNode(Node* node) { m_node = node; }
Node* node() const { return m_node; }

// RenderObject tree manipulation
//////////////////////////////////////////
virtual bool canHaveChildren() const;
virtual bool isChildAllowed(RenderObject*, RenderStyle*) const { return true; }
virtual void addChild(RenderObject* newChild, RenderObject* beforeChild = 0);
virtual void removeChild(RenderObject*);
virtual bool createsAnonymousWrapper() const { return false; }

// raw tree manipulation
virtual RenderObject* removeChildNode(RenderObject*, bool fullRemove = true);
virtual void appendChildNode(RenderObject*, bool fullAppend = true);
virtual void insertChildNode(RenderObject* child, RenderObject* before, bool fullInsert = true);
// Designed for speed. Don't waste time doing a bunch of work like layer updating and repainting when we know that our
// change in parentage is not going to affect anything.
virtual void moveChildNode(RenderObject*);

virtual void paint(PaintInfo&, int tx, int ty);

/*
* This function should cause the Element to calculate its
* width and height and the layout of its content
*
* when the Element calls setNeedsLayout(false), layout() is no
* longer called during relayouts, as long as there is no
* style sheet change. When that occurs, m_needsLayout will be
* set to true and the Element receives layout() calls
* again.
*/
virtual void layout() = 0;

其中很多方法如paint()、layout()等是虚拟的，不同的子类可以重载它；

其中方法container() 、containingBlock()、paint()、layout()很值得大家深入研究；

总的说来RenderObject基类定义一些通用属性、方法，以便维护、布局、渲染Render树。

2、子类RenderBox
RenderBox代表描述CSS标准中的Box Model，它继承自RenderObject;****RenderBox主要数据成员****![](http://blog.chinaunix.net/photo/10867_100616210256.jpg)**图二
****其主要重载了部分继承而来的方法。
**********3、子类RenderContainer**在新的 WebKit 中，不再存在。****
4、子类RenderFlow
RenderFlow主要用来描述CSS标准中提到的能进行inline-flow、block-flow相关处理的Render树结点，它继承自RenderContainer；******RenderFlow主要数据成员********![](http://blog.chinaunix.net/photo/10867_100616210646.jpg)**********图四
其主要方法包括在flow的过程中创建、关联匿名对象等；

**5、子类RenderBlock**
RenderBlock代表CSS标准中的block-level元素，它继承自RenderFlow；
RenderBlock主要数据成员![](http://blog.chinaunix.net/photo/10867_100616210809.jpg)****图五
********************它维护了一组由它定位的positioned树节点，以及有关overflow方面的设置；********************其主要重载了RenderObject继承下来的layout、paint等方法；
********************因为html中的body、div、p等标签对应RenderBlock类对象，其在Render树具有非常重要的地位，其layout、paint等方法的实现，往往是WebKit整个布局、渲染处理的发起中心，内容比较多并且复杂，以后有机会详解。**********6、子类RenderInline
RenderInline代表inline-level元素，其继承自RenderFlow，主要重载了RenderObject关于inline-flow方面处理的方法，提供了splitFlow、splitInlines等处理自动换行的方法。

7、子类RenderText
RenderText代表对html中Text node对应的Render树节点，它直接继承自RenderObject；
**RenderText主要数据成员![](http://blog.chinaunix.net/photo/10867_100616210945.jpg)****图六
****************************************它提供关于处理文字方面如显示文字、行高计算、整个Text node对应的宽度等；它没有重载layout方法，因为它自身的定位往往由RenderBlock、RenderInline父对象来处理；
********************************************8、子类RenderImage****************************************RenderImage代表html中img标签对应的树节点，它继承自RenderBox；******************************************RenderImage继承关系及主要数据成员********************************************![](http://blog.chinaunix.net/photo/10867_100616211224.jpg)**********************************图七
**********************************************************************************其主要提供关于图片显示、大小设置等方面的处理，其中paintReplaced方法将其图片显示出来；**
**************************************************************************************9、子类RenderView************************************************************************************RenderView对应整个html文档对象的树节点，可看成是Render树的根，它继承自RenderBlock；**RenderView主要数据成员![](http://blog.chinaunix.net/photo/10867_100616211354.jpg)****图八
************************************************************************************其中m_frameview成员对应整个文档对应的FrameView，而m_widgets则包括了该文档可能包含的plugin插件等对应的Render树节点；
************************************************************************************RenderView对象作为Render树的根，它往往随着Document对象的创建而创建，它的layout、paint方法的发起往往是整颗Render树布局、渲染处理的开始；其中也包含了对选择处理。************************************************************************************10、其他
整个Render树中涉及的树节点类型，还有很多如RenderButton、RenderTable、RenderMedia等；并且各个类的方法及数据成员非常多，这里只是初步列出主要的类及其主要方法，特别是可能涉及到布局、渲染方方面的方法，以便我们能从中大致WebKit布局、渲染所涉及的基本内容及方法。

二、CSS属性的描述
1、RenderStyle类
RenderObject对象的m_style成员为RenderStyle类对象，它往往用来描述一个RenderObject所可能涉及的CSS属性数据(如left、top、align、color、font等等)，其数据成员往往对应于CSS中定义的所有属性项，内容非常的庞杂，简单的说来就是将CSS标准中的所有属性按照一定分类定义到一个数据结构中。

2、RenderStyle类主要方法
为了获取、设置CSS属性所对应的值，RenderStyle类提供了所有的获取、设置CSS属性的方法如：
void setDisplay(EDisplay v) { noninherited_flags._effectiveDisplay = v; }
void setOriginalDisplay(EDisplay v) { noninherited_flags._originalDisplay = v; }
void setPosition(EPosition v) { noninherited_flags._position = v; }
void setFloating(EFloat v) { noninherited_flags._floating = v; }

void setLeft(Length v) { SET_VAR(surround,offset.left,v) }
void setRight(Length v) { SET_VAR(surround,offset.right,v) }
void setTop(Length v) { SET_VAR(surround,offset.top,v) }
void setBottom(Length v){ SET_VAR(surround,offset.bottom,v) }

void setWidth(Length v) { SET_VAR(box,width,v) }
void setHeight(Length v) { SET_VAR(box,height,v) }
等等。。。。

三、RenderObject及子类对象的生成
1、CSSParser
CSSParser类顾名思义，主要用来解析文本中各种CSS属性，并且有效的组织在一个RenderStyle对象中。
其主要方法parseValue、applyProperty的部分代码示例如下：
bool CSSParser::parseValue(int propId, bool important)
{
.....................................................
case CSSPropertyFloat: 
// left | right | none | inherit + center for buggy CSS
if (id == CSSValueLeft || id == CSSValueRight ||
id == CSSValueNone || id == CSSValueCenter)
valid_primitive = true;
break;

case CSSPropertyClear: // none | left | right | both | inherit
if (id == CSSValueNone || id == CSSValueLeft ||
id == CSSValueRight|| id == CSSValueBoth)
valid_primitive = true;
break;

case CSSPropertyWebkitBoxAlign:
if (id == CSSValueStretch || id == CSSValueStart || id == CSSValueEnd ||
id == CSSValueCenter || id == CSSValueBaseline)
valid_primitive = true;
break;
.....................................................
case CSSPropertyWebkitBoxPack:
if (id == CSSValueStart || id == CSSValueEnd ||
id == CSSValueCenter || id == CSSValueJustify)
valid_primitive = true;
break; 
....................................................
}

void CSSStyleSelector::applyProperty(int id, CSSValue *value)
{
case CSSPropertyOpacity:
HANDLE_INHERIT_AND_INITIAL(opacity, Opacity)
if (!primitiveValue || primitiveValue->primitiveType() != CSSPrimitiveValue::CSS_NUMBER)
return; // Error case.
// Clamp opacity to the range 0-1
m_style->setOpacity(min(1.0f, max(0.0f, primitiveValue->getFloatValue())));
return;
case CSSPropertyWebkitBoxAlign:
{
HANDLE_INHERIT_AND_INITIAL(boxAlign, BoxAlign)
if (!primitiveValue)
return;
EBoxAlignment boxAlignment = *primitiveValue;
if (boxAlignment != BJUSTIFY)
m_style->setBoxAlign(boxAlignment);
return;
}
...................................................
}

2、CSSStyleSelector类
CSSStyleSelector类其作用是基于所有用户的stylesheets集合为一个给定的DOM Element创建出其对应的RenderStyle对象。其主要功能由方法RenderStyle* styleForElement(Element*, RenderStyle* parentstyle=0, bool allowSharing = true, bool resolveForRootDefault = false);来实现。

3、构建Render树
在构建DOM树的过程中，Dom Element对象创建完后，往往通过attach方法来创建RenderObject对象，进而构建Render树。
其基本实现流程如下：void Element::attach()=>createRendererIfNeeded()=>createRenderer；

RenderObject* Element::createRenderer(RenderArena* arena, RenderStyle* style)
{
if (document()->documentElement() == this && style->display() == NONE) {
// Ignore display: none on root elements. Force a display of block in that case.
RenderBlock* result = new (arena) RenderBlock(this);
if (result)
result->setAnimatableStyle(style);
return result;
}
return RenderObject::createObject(this, style);
}

RenderObject* RenderObject::createObject(Node* node, RenderStyle* style)
{
Document* doc = node->document();
RenderArena* arena = doc->renderArena();

const ContentData* contentData = style->contentData();
if (contentData && !contentData->m_next && contentData->m_type == CONTENT_OBJECT && doc != node) {
RenderImageGeneratedContent* image = new (arena) RenderImageGeneratedContent(node);
image->setStyle(style);
if (StyleImage* styleImage = contentData->m_content.m_image)
image->setStyleImage(styleImage);
return image;
}

RenderObject* o = 0;

switch (style->display()) {//往往在CSSStyleSelector::styleForElement或CSSStyleSelector::adjustRenderStyle时//调用setDisplay()以确定其display属性。
case NONE:
break;
case INLINE:
o = new (arena) RenderInline(node);
break;
case BLOCK:
o = new (arena) RenderBlock(node);
break;
case INLINE_BLOCK:
o = new (arena) RenderBlock(node);
break;
case LIST_ITEM:
o = new (arena) RenderListItem(node);
break;
case RUN_IN:
case COMPACT:
o = new (arena) RenderBlock(node);
break;
case TABLE:
case INLINE_TABLE:
o = new (arena) RenderTable(node);
break;
case TABLE_ROW_GROUP:
case TABLE_HEADER_GROUP:
case TABLE_FOOTER_GROUP:
o = new (arena) RenderTableSection(node);
break;
case TABLE_ROW:
o = new (arena) RenderTableRow(node);
break;
case TABLE_COLUMN_GROUP:
case TABLE_COLUMN:
o = new (arena) RenderTableCol(node);
break;
case TABLE_CELL:
o = new (arena) RenderTableCell(node);
break;
case TABLE_CAPTION:
o = new (arena) RenderBlock(node);
break;
case BOX:
case INLINE_BOX:
o = new (arena) RenderFlexibleBox(node);
break;
}
return o;
}
这样就不同的DOM树节点结合不同的显示属性，创建出不同的RenderObject子类对象，进而形成一个Render树。

四、总结
其实WebKit涉及网页布局方面的数据结构远不止这些，其中有的也比较复杂，这里只是列出自己认为较为重要的一小部分，希望能对了解WebKit的网页布局渲染有一定的基础性作用