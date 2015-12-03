title: CSS编码规范
date: 2015-11-28 18:04:06
tags: 规范
---
代码风格必须易于阅读、注释清晰

- 在多个选择器的规则集中，每个单独的选择器独占一行。
- 在规则集的左大括号前保留一个空格。
- 在声明块中，每个声明独占一行。
- 每个声明前保留一级缩进。
- 每个声明的冒号后保留一个空格。
- 对于声明块的最后一个声明，始终保留结束的分号。
- 规则集的右大括号保持与该规则集的第一个字符在同一列。
- 每个规则集之间保留一个空行。

```css
.selector-1,
.selector-2,
.selector-3[type="text"] {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    display: block;
    font-family: helvetica, arial, sans-serif;
    color: #333;
    background: #fff;
    background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
    padding: 10px;
}
```

### 1.2 声明顺序[非必须]
样式声明的顺序尽量遵守一个单一的原则。将相关的属性组合在一起，重要的属性往前放置。

相关的属性声明应该以下面的顺序分组处理：
- Positioning---位置
- Box model-----盒模型
- Typographic---排版
- Visual--------外观

参考规范代码：
```css
.selector {
    /* Positioning*/
    position: absolute;
    z-index: 10;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    /* Display & Box Model */
    display: inline-block;
    overflow: hidden;
    box-sizing: border-box;
    width: 100px;
    height: 100px;
    padding: 10px;
    border: 10px solid #333;
    margin: 10px;

    /* Other */
    background: #000;
    color: #fff;
    font-family: sans-serif;
    font-size: 16px;
    text-align: right;
}
```

### 1.3 例外及细微偏移原则的情况
对于大量仅包含单条声明的声明块，可以使用一种略微不同的单行格式。在这种情况下，在左大括号之后右大括号之前都应该保留一个空格。

```css
.selector-1 {width: 10%;}
.selector-2 {width: 20%;}
.selector-3 {width: 30%;}
```
对于以逗号分隔并且非常长的属性值，例如一堆渐变或者阴影的声明，可以放在多行中，这有助于提高可读性。

```css
.selector {
    box-shadow:
        1px 1px 1px #000,
        2px 2px 1px 1px #ccc inset;
    background-image:
        linear-gradient(#fff, #ccc),
        linear-gradient(#f3c, #4ec);
```

### 1.4 杂项
* 在十六进制值中，使用小写，如 `#aaa`。
* 单引号或双引号的选择保持一致。推荐使用双引号，如 `content:""`。
* 对于选择器中的属性值也加上引号，如`input[type="checkbox"]`。
* 如果可以的话，不要给0加上单位，如`margin: 0`。

### 1.5 预处理：格式方面额外的考虑

不同的CSS预处理有着不同的特性、功能以及语法。编码习惯应当根据使用的预处理程序进行扩展，以适应其特有的功能。

* 将嵌套深度限制在3级。对于超过3级的嵌套，给予重新评估，避免选择器深度太大。
* 减少嵌套规则的长度。当可读性受到影响时，将之打断。推荐避免出现多于20行的嵌套规则出现。

```less
.selector-1 {
    .child-selector{
        ...
    }
}
```

## [2.命名](#naming)
- 全小写 中划线-连接
- 不要使用下划线和驼峰命名CSS
- 命名的时候尽量采用清晰的、有意义的名字
- 对于html文件和css文件中的代码，尽量采用一致的命名规则

```css
/* 没有意义的命名  */
.s-scr {
    overflow: auto;
}
.cb {
    background: #000;
}

/* 比较有意义的命名方式 */
.is-scrollable {
    overflow: auto;
}
.column-body {
    background: #000;
}
```


## [3.实例](#example)
下面是含有上述约定的示例代码：

```css
/* ==========================================================================
   Grid layout
   ========================================================================== */

/**
 * Column layout with horizontal scroll.
 *
 * This creates a single row of full-height, non-wrapping columns that can
 * be browsed horizontally within their parent.
 *
 * Example HTML:
 *
 * <div class="grid">
 *     <div class="cell cell-3"></div>
 *     <div class="cell cell-3"></div>
 *     <div class="cell cell-3"></div>
 * </div>
 */

/**
 * Grid container
 *
 * Must only contain `.cell` children.
 *
 * 1. Remove inter-cell whitespace
 * 2. Prevent inline-block cells wrapping
 */

.grid {
    height: 100%;
    font-size: 0; /* 1 */
    white-space: nowrap; /* 2 */
}

/**
 * Grid cells
 *
 * No explicit width by default. Extend with `.cell-n` classes.
 *
 * 1. Set the inter-cell spacing
 * 2. Reset white-space inherited from `.grid`
 * 3. Reset font-size inherited from `.grid`
 */

.cell {
    position: relative;
    display: inline-block;
    overflow: hidden;
    box-sizing: border-box;
    height: 100%;
    padding: 0 10px; /* 1 */
    border: 2px solid #333;
    vertical-align: top;
    white-space: normal; /* 2 */
    font-size: 16px; /* 3 */
}

/* Cell states */

.cell.is-animating {
    background-color: #fffdec;
}

/* Cell dimensions
   ========================================================================== */

.cell-1 { width: 10%; }
.cell-2 { width: 20%; }
.cell-3 { width: 30%; }
.cell-4 { width: 40%; }
.cell-5 { width: 50%; }

/* Cell modifiers
   ========================================================================== */

.cell--detail,
.cell--important {
    border-width: 4px;
}
```

## [4.代码组织](#organization)

对于css代码库来说，如何组织代码文件是很重要的，尤其对于那些很大的代码库显得更加重要。这里介绍几个组织代码的建议：

* 逻辑上对不同的代码进行分离。
* 不同的组件(component)的css尽量用不同的css文件（可以在build阶段用工具合并到一起）
* 如果用到了预处理器（比如less），把一些公共的样式代码抽象成变量，例如颜色，字体等

## [5.最佳实践](#practice)

### 5.1 css选择器
1) 不同的标签类型尽可能不用相同的css类名；
2) 尽可能不用标签选择器，用css类名和id；
3) 在不确定唯一的标签上，少用id选择器。
```css
/*bad*/
ul#menus{
}
div.info{
}
/*good*/
.main-menus{
}
.info{
}
```


### 5.2 属性名称和值的定义精简
css的某些属性定义可以分拆为各个独立项，比如padding，border, margin, background, font等，虽然分拆定义的可读性会好一些，但合并后也不会对可读性带来影响，反而代码更简洁；
此外对属性值为0的单位可以省略，url值两端的引号可以省略。

```css
    /*bad*/
    border-top-style: none;
    font-family: palatino, georgia, serif;
    font-size: 100%;
    line-height: 1.6;
    padding-bottom: 2em;
    padding-left: 1em;
    padding-right: 1em;
    padding-top: 0;
    margin: 0.8em;
    background: #00FF00 url('bgimage.gif') no-repeat fixed top;
    /*good*/
    border-top: 0;
    font: 100%/1.6 palatino, georgia, serif;
    padding: 0 1em 2em;
    margin: 0.8em;
    background: #00FF00 url(bgimage.gif) no-repeat fixed top;
```

### 5.3 重设(reset)
全局重设可以确保一个网站在所有浏览器保持统一的外观。

```css
/*bad*/
*{
    margin: 0;
    padding: 0;
}
p{
  margin: 0;
  padding: 0;
}
/*good*/
body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,
form,fieldset,input,textarea,p,blockquote,th,td {
    padding: 0;
    margin: 0;
}
table {
    border-collapse: collapse;
    border-spacing: 0;
}
fieldset,img {
    border: 0;
}
address,caption,cite,code,dfn,em,strong,th,var {
    font-weight: normal;
    font-style: normal;
}
ol,ul {
    list-style: none;
}
caption,th {
    text-align: left;
}
h1,h2,h3,h4,h5,h6 {
    font-weight: normal;
    font-size: 100%;
}
q:before,q:after {
    content:”;
}
abbr,acronym {
    border: 0;
}
```

### 5.4 避免写兼容某个浏览器的css代码[仅作参考，非强制]
- 尽量少用CSS Hack，遇到浏览器兼容问题，首先考虑的是能否换一种方案解决
- 如果没有合适的解决方案，可以单独写一个兼容性的文件，作为补丁加到特定的浏览器上。

```css
/*bad*/
.bb{
height:32px;
  background-color:#f1ee18;/*所有识别*/
  .background-color:#00deff9; /*IE6、7、8识别*/
  +background-color:#a200ff;/*IE6、7识别*/
  _background-color:#1e0bd1;/*IE6识别*/
}
```
```html
<!--good-->
<!--[if IE 6]>
<link rel="stylesheet" type="text/css" href="css/ie6.css" />
<![endif]-->
<!--[if IE 7]>
<link rel="stylesheet" type="text/css" href="css/ie7.css" />
<![endif]-->
```

### 5.5 块元素和行内元素的区别，避免写无用的css代码
- 块级元素显示会独占一行，display默认样式是block，而行内元素是inline
- 默认情况下以下的css样式对行内元素是无效的：width height margin-top margin-bottom padding-top padding-bottom，所以避免给行内元素定义这些无用的样式。
```css
/*bad*/
span{
  height: 100px;
  margin-top: 20px;
  margin-bottom: 10px;
  padding:15px 0;
  width:200px;
}
/*good*/
div{
  height: 100px;
  margin-top: 20px;
  margin-bottom: 10px;
  padding:15px 0;
  width:200px;
}
```

### 5.6 CSS选择器权重
- css的选择器是有权重的，当有多个样式时，权重高的样式会起作用；
- 如果权重相同，则最后定义的样式会起作用，但尽量避免类型的重复定义；
- 为了代码的重用性，应尽可能定义小的权重，这和不推荐使用id来定义样式有同样的考虑。
```css
/*权重为1*/
div{
}
/*权重为10*/
.class1{
}
/*权重为100*/
#id1{
}
/*权重为100+1=101*/
#id1 div{
}
/*权重为10+1=11*/
.class1 div{
}
/*权重为10+10+1=21*/
.class1 .class2 div{
}
```
```html
<div id="id" class="class_1 class_2"></div>
<!--bad-->
<style>
  #id{
    font-size: 12px;
  }
  .class_1{
    font-size: 14px;
  }
  .class_1 .class_2{
    font-size: 16px;
  }
</style>
<!--good-->
<style>
  .class_1{
    font-size: 14px;
  }
</style>
```


### 5.7 多组合，少继承
把css定义中的固定部分和可变部分分开定义，使得代码达到最大程度的重用，这样的结果是增加了元素上添加的css类个数，但是提高了代码的维护性和可读性。
```css
.btn {
  display: inline-block;
  *display: inline;
  padding: 4px 10px 4px;
  margin-bottom: 0;
  *margin-left: .3em;
  font-size: 13px;
  line-height: 18px;
  *line-height: 20px;
  color: #333333;
  ...
  *zoom: 1;
  -webkit-box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
     -moz-box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
          box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
}
/*在此基础上定义各种按钮的特定样式*/

.btn.disabled,
.btn[disabled] {
  cursor: default;
  background-color: #e6e6e6;
  background-image: none;
  opacity: 0.65;
  filter: alpha(opacity=65);
  -webkit-box-shadow: none;
     -moz-box-shadow: none;
          box-shadow: none;
}

.btn-large {
  padding: 9px 14px;
  font-size: 15px;
  line-height: normal;
  -webkit-border-radius: 5px;
     -moz-border-radius: 5px;
          border-radius: 5px;
}

.btn-small {
  padding: 5px 9px;
  font-size: 11px;
  line-height: 16px;
}
.btn-mini {
  padding: 2px 6px;
  font-size: 11px;
  line-height: 14px;
}
```

### 5.8 媒体查询位置

尽量将媒体查询的位置靠近他们相关的规则。不要将他们一起放到一个独立的样式文件中，或者丢在文档的最底部。这样做只会让大家以后更容易忘记他们。示例：

```css
    .element { ... }
    .element-avatar { ... }
    .element-selected { ... }

    @media (min-width: 480px) {
      .element { ...}
      .element-avatar { ... }
      .element-selected { ... }
    }
```

### 5.9 不要使用@import加载CSS文件

```css
/*-----bad----*/
@import "skin.css";
```

### 5.10 编码
为了避免 HTML 和 CSS 文件编码不同时造成中文解析乱码，造成的不必要的麻烦，CSS 文件头部统一加上文件对应的编码，例如文件编码为 UTF-8 时：
```css
@charset "utf-8";
```

@charset前面不能有任何字符，@charset必须出现在 CSS 文件的最开始。

## [6.参考文档](#reference)
- [dyygtfx/FeCodeGuide](https://github.com/dyygtfx/FeCodeGuide)
- [tsq/front-end-principles](https://github.com/tsq/front-end-principles/)
- [imweb/code-guide](https://github.com/imweb/code-guide/blob/master/md/css.md)