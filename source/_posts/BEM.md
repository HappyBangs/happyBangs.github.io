---
title: BEM
date: 2016-08-30 12:17:00
categories: 样式
tags: [css规范, css_basic]
keywords: [css规范, css, BEM]
---
众所周知，统一的命名规范可以显著的提高开发速度，然而现在大多数项目中的css的代码，都不会约定命名规范来开发，这导致随着时间的增长，css代码越来越不可维护。  

BEM是Yandex团队提出的，通过一种严格方式来将 CSS Class划分成独立构成要素的一种命名方法。
> BEM is a highly useful, powerful and simple naming convention to make your front-end code easier to read and understand, easier to work with, easier to scale, more robust and explicit and a lot more strict.

BEM是Block，Element，Modifier的缩写。它的命名规则就是由这三部分组成。

* Block：块，独立的模块。如header，container，menu，checkbox，input。block可以相互嵌套，一个header是block,header里嵌套的搜索框是block,甚至一个icon一个logo也是block。
* Element：元素，block的一部分，不独立，是依赖于block的。  如 menu item,list item,header title。
* Modifier: 修饰符，表示block或者element的状态，使用它来改变元素或者块的外观或者行为。例如 disabled,checked,active等。  
 
官方示例中，块和元素之间使用 __ 分隔，元素/块和装饰符之间使用 -- 分隔，比较长的名称使用-分隔。例如：simple-form__submit--disabled(简单表单中禁用状态的提交按钮)。 但实际使用中可以自行定义间隔符类型。

一个前端页面，利用今天介绍的思想，可以转换为一个块和元素相互嵌套的对象模型，这种结构叫做 **BEM树**。  

例如，一个网站的菜单项，用BEM的方法分析网站的结构得到(json)：

```json
{
  block: 'menu',
  content: [
    { 
        elem: 'item', 
        content: 'Index' 
    },
    {
      elem: 'item',
      mods: { 'state' : 'current' },
      content: 'Products'
    },
    { 
        elem: 'item', 
        content: 'Contact' 
    }
  ]
}
```

转换为html代码：
```html
<ul class="menu">
    <li class="menu__item">Index</li>
    <li class="menu__item menu__item_state_current">Products</li>
    <li class="menu__item">Contact</li>
</ul>
``` 

优点：  

* 模块化。block(块)不会依赖页面中任何其他元素。
* 复用性强。名字很长，冲突都难。
* 结构清晰，易读。因为命名有一定的规则，所以会很容易的找到块所在的位置，元素的含义以及修饰符如何使用。
* 其他：基本不需要嵌套,层级少了，css性能提升了。

缺点：命名复杂，长度长。但可以使用Mixins自动生成名字（然而编写的时候还是很麻烦，很多人也是因为这个望而却步）。

虽然不会将BEM使用到项目中，但是BEM的设计思想还是很值得学习的。

使用BEM的网站：
![screenshot](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/7fcc9e9747b03d1d74fa8069ca2453a1)
