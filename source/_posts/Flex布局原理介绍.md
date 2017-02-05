---
title: Flex布局原理介绍
categories: 样式
tags: [css_basic,flex]
date: 2017-01-03 05:31:26
updated: 2017-02-03 20:33:26
keywords: [flex, css, 弹性布局]
---
### 引言

CSS3中的 Flexible Box，或者叫flexbox，是用于排列元素的一种布局模式。

顾名思义，弹性布局中的元素是有伸展和收缩自身的能力的。 相比于原来的布局方式，如float、position，根据盒子模型，就可以计算出元素的展示尺寸（长宽非百分比），除非溢出，否则不依赖于父容器的大小。

而弹性布局中元素的大小是高度依赖父容器的大小的。因为，**它所具有的“伸缩性”，目标就是为了撑满父元素。**当然这是在任其“野蛮生长”的情况下，你也可以通过相关css属性控制其是否撑满、撑满什么轴。

弹性布局是一种全新的思维方式，让很多实现复杂的问题有了更好的理解方式（如垂直居中）。只需要给**直接**父容器设置为``display: flex;`` ，duang~ 子元素就默认具有了**可收缩性**。
Flex的语法规范也曾经有很多版本：  

* 最新版本是``display: flex;``
* 中间版本是``display: flexbox``;
* 最老版本``display: box``;

本篇文章侧重于flex难理解的点，适合于已经了解过[flex的api](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)的童鞋观看。（api其实就下图这么多，橙色是常用的）

![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/a493540fa5ba58ce63a43bc84e13a2cd.png)



#### 为何要引入主轴、交叉轴、轴线的概念

我们首先看一下，CSS布局的发展历程：

![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/a7081844485fdb0e22010e6f7143895a.png)

最开始使用的布局方式是table布局。还记得我在2010年大一的时候，使用的就是这种布局，那时候div+css还没有成为主流，至少还没覆盖到我的学校。

table布局的特点是非常好理解和上手，用过excel的都知道怎么组织代码：将网页划分为一个大的表格，将元素一个个填到表格中，如果表格不够大就合并单元格。但是这种布局的缺点是过于规整，只能适用于简单、规整的网页。且网站的布局严重依赖于html，很多冗余的布局html代码。

到了2011年大二，我换了一个校区，也换了个地方做网页，部门负责人跟我说：“现在大家都用div+css了，来给你上堂课。”  
我心里一懵，就这么不小心经历了“历史的变迁”。入门难了，新增了相对定位和绝对定位的概念，但是却能够应付复杂一点的网页了。

接着因为“文字环绕”的需求，出现了浮动（float）布局。虽然有很多副作用，但是人们更喜欢它的价值，如今使用的频率也很高，比如bootstrap的栅格系统。

CSS3新增了Flexbox。虽然前几种布局也可以实现垂直居中、等分、栅格等需求，但是往往需要脑袋转好几个弯，最后也不一定可行。而flex则使用了更直接明了的思路来实现这些布局。

总的来说，虽然布局方式变多了，**但并不是前者取代后者**。直到现在，考虑到浏览器兼容性等问题，一种布局都有自己独一无二的使用价值。


言归正传，翻开flex的入门教程，首先映入眼帘且比较难懂的就是主轴和交叉轴（对，就是下面这张图），这是前几种布局方式都没有的。

   

![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/d15ca4443cf1f99ad2b961c3de58dcc4.png)

前几种布局都可以按照人类书写的方式理解：“从左到右写，写不下就往下换行”。

但是flex特点是可以重新定义这种“书写方式”，你还可以从下到上写、从右到左写（见flex-direction属性），换行也可以从两个相反的方向换行（见flex-wrap属性）。所以引入了这个几个概念方便理解。

如果你只是简单的使用，其实只用得到默认值，个人认为可以不用过于深究主轴和交叉轴。



那么，到底这些轴的概念怎么就帮助我们理解了？

首先你可以先去温习一下[flex的api](http://codepen.io/enxaneta/full/adLPwv/)。

为了帮助理解，以类比的方式类比人类书写的方式，浏览器“书写”的方向为主轴方向，浏览器“换行的方向”为交叉轴方向，每行就是一条轴线。

更正式一点的说法是，浏览器的布局方式是：

**沿着主轴的方向依次排列，如果要换行，则沿着交叉轴的方向进行换行，每行代表一条轴线。**

细心的同学应该发现了，“依次”其实也不是“依次”，我们可以使用子元素的order属性对元素进行重新排序。由此可见，flex给子元素提供了很大的灵活性，但本篇文章只使用order的默认值。

主轴、交叉轴可以帮助我们理解这些概念：

*  重新定义浏览器“书写方式”。

    ``flex-direction`` 改变主轴方向；

    ``flex-wrap`` 改变交叉轴方向

    如下图，主轴和交叉轴的排列组合有4*2 =8 种。

    ![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/239feda71c5e515840a570c26c012c2b.png)

    比如，可以像写对联一样，从上到下竖着书写，从右到左换行。

    （2017新年快乐~）

    ![Bitmap.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/c8a2a5bf0b0a5192cc1571f2179a4f53.png)

   ```css
    .container {
      flex-direction: column;
      flex-wrap: wrap-reverse;
    }
   ```
    为了方便表达，本篇文章都使用默认的轴方向。

*  轴上的元素的排列方式(justify-content, align-items)。  
   * justify-content 定义了元素在主轴轴上如何对齐；   
   * align-items 定义了元素在交叉轴上如何对齐。   

   因为轴都有方向，因此也有轴起点和轴终点：  

   * flex-start: 对齐轴起点;    
   * flex-end: 对齐轴终点;  
   * center：在轴线上居中;  
   * space-between：两端不带间距的轴线两端对齐;  
   * space-around: 两端带间距的轴线两端对齐, 且每个子元素之间的间距相同（假设为x）。两端元素离父元素间距为（x/2）。 **注意这个间距既不是margin也不是padding，盒子模型来计算展示方式已经不适用了。**  
   * baseline：交叉轴特有，基线对齐。   
   * stretch：交叉轴特有，有占满整个主轴高度的意向。当设置了子元素高度为非auto时不生效。 

*  多根轴线时，轴线之间的排列方式(align-content)。  
   align-content的参照轴是交叉轴。其属性也和上面的justify-content、align-items大同小异：flex-start、flex-end、center、space-between、space-around、stretch。不多做解释。


### 元素宽度如何伸缩

能决定元素展示宽度的属性有：``flex-shrink，flex-grow，flex-basis，width，min-width``

因为flex为前三个属性的缩写方式，所以常用写法是``flex-shrink，flex-grow，flex-basis``统一用``flex``设置。

常见的flex设置：

| 序号   | 样式          | flex-grow | flex-shrink | flex-basis |
| ---- | ----------- | --------- | ----------- | ---------- |
| ①    | flex默认值     | 0         | 1           | auto       |
| ②    | flex: 1;    | 1         | 1           | 0%         |
| ③    | flex: auto; | 1         | 1           | auto       |
| ④    | flex: none; | 0         | 0           | auto       |



那么，flex-grow和flex-shrink的值会对元素造成什么影响呢？

如下图所示，当元素允许缩小时，最终展示的效果会是正好撑满主轴。

![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/b7150d5b04ac74a7f26ad44e9c5d6cd8.png)

在父容器中有三个元素A1,A2,A3,他们都有一个**初始宽度**（比如设置了width且flex-basis不为0%）。初始宽度在下一小节会详细讲。

如果按照初始宽度放入普通父容器中，那么他们会溢出**x**个像素（见初始尺寸行）。

当父元素``display: flex;``， 

且A1``flex-shrink:1``，A2``flex-shrink:1``，A3``flex-shrink:1``时，

A1、A2、A3都具有可收缩的特性。



flex-shrink的值表示需要收缩的宽度占总溢出宽度的比例，因此展示尺寸这么算：

1. 将x平均分为(1+1+2) = 4份，每份宽度为**i = x/4**

2. A1的展示尺寸为：A1默认宽度 - i × 1；

   A2的展示尺寸为：A1默认宽度 - i × 1；

   A3的展示尺寸为：A1默认宽度 - i × 2；

还是很好理解的吧。同理，当元素不够撑满父元素时，需要伸展的宽度也是按照这种方式计算的。只是比例基数变成了剩余空间的宽度。

如果你希望元素不能伸缩，那么需要设置相应的属性为0。

如: ``flex-shrink: 0``。



### flex-basis和width的关系

flex-basis 用于计算上一小节中元素的“初始宽度”。

* flex-basis为auto时, 初始宽度为元素内容大小或者设置的宽度值（盒子模型中的占用宽度）。
* flex-basis为像素值时，初始宽度为flex-basis的值。
* flex-basis为百分比时，初始宽度为占父容器的比例。
* flex-basis为0或0%时，初始宽度为0。

虽然flex-basis的优先级大于width，但是**最后计算的展示尺寸受限于min-width或者max-width**。

比如，元素A算出来的展示宽度为100px，但是它有个css属性``min-width: 200px;``, 那么最后的展示宽度仍然为200px。但是计算的初始尺寸仍然是由flex-basis决定。  


### 兼容性

从[caniuse](http://caniuse.com/#search=flex%20box)上可以查到，通过加上各种前缀，flex可以兼容到ie10以及以上。

16年年初在实际使用过程中，发现UC的支持性很不好。这次又重新试了一次，新版的UC也能很好的支持flex了。看来flex正在慢慢占领移动端。

### 几个案例

通过上面几小节的描述，可以发现flex用了一种全新的思路来布局。
列出几个常见的案例，以下案例的代码统一在[我的codepen](http://codepen.io/cheer/pen/dOmpXv)可查看。

① 垂直居中  
![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/e03f10a5f35fe203fb877eaa4b33b993.png) 
```css
.container {
    display: inline-flex;
    align-items: center;
    justify-content: center;
}
```

② 一侧固定，一侧自适应  
![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/157c8233a8290a330e20900974a8c92c.png) 
```scss
.container {
    display: flex;
    .sidebar {
        width: 100px;
    }
    .content {
        flex: 1;
    }
}
```

③ 多列等高
![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/8bc2041ba11386c873fe8f401a56244b.png)
```css
.container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-content: stretch;
}
```

### 总结
flex布局是围绕父元素和轴来进行布局的。这种全新的思路巧妙地只需要简单几行代码就可以实现曾经头疼的效果，其思路的建立过程非常值得借鉴。
