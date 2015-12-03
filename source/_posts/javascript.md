title: Javascript编码规范
date: 2015-11-28 17:54:06
tags: 规范
---
为规范前端开发代码，提高代码质量，特制定此文档，其中**声明**，**安全**和**分号**这三节是必须执行的，组件类必须遵循**注释规范**。

## [1.声明](#declaration)

- 变量声明必须加var关键字，严格控制作用域；
- 建议使用驼峰式命名变量和函数，如：`functionNamesLikeThis, variableNamesLikeThis, ClassNamesLikeThis,namespaceNamesLikeThis`；
- 私有成员变量和方法命名以下划线开头，如：`var _this`；
- 常量定义单词全部大写，以下划线连接，但不要用const关键字来声明，如：`SOME_CONSTANTS`；
- 函数参数大于3个时，应以对象形式作为参数集传递；
- 禁止在代码块中声明函数，错误的范例：`if (true) {function foo() {}}`；
- 禁止用new来实例化**基本类型**，错误的范例：`var x = new Boolean(false)`；
- 直接定义数组或对象，而不使用new关键字声明，错误的范例：`var a = new Array();var o = new Object()`；
- 使用单引号来定义字符串；
- 文件名必须全部用小写，文件名分隔符用**中划线**连接，版本连接符用**实心点**，合并文件的文件名连接符用**下划线**，如：passport-core.min.js和reset-1.0_utils-1.0.css；
- 函数/方法名以动词开头，比如`function doSomething(){}`；
- 构造函数/类名尽量是名词属性，首字母大写，后面以驼峰方式。比如`function Person(){}`；

## [2.安全](#safe)

- 审查用户输入，如：从URL获取参数，使用跳转页面的referer，用于eval或DOM操作的用户数据；
- 禁止通过在iframe使用script进行跨域回调；
- 警惕jquery xss，禁止这样的写法：$(window.location.hash)；
- 禁止引用站外资源；

## [3.分号](#semicolon)

**函数表达式**（有别于函数声明，如：function x(){}）必须用分号结束，下面是错误的范例，第三行漏掉分号，后面的函数被当作参数传递给myMethod，导致解析错误：

```javascript
    var myMethod = function() {
      return 42;
    }
    (function() {
      // 一个匿名函数，在这里会被错误解析当作参数调用导致报错
    })();
```

## [4.异常](#exception)

由于无法完美的控制都不出现异常，所以尽量在可能但不确定出现异常的地方（大量运算，AJAX请求，数组操作或DOM操作等）用try-catch(e)来抛出异常，这样有利于规模较大的项目中排查错误。使用自定义异常抛出错误信息，更有利于错误信息阅读，且更加通用，因为不同浏览器抛出的信息不一样，有时候可能很难定位到问题所在位置，特别是IE下可能遇到的如下报错信息：

```javascript
    Stack overflow at line: 0
    或：
    未知的运行时错误
```

## [5.this关键字](#this_key)

在单体对象里面，使用self替换this
```javascript
    var demo = {
        init: function(){
            var self = this;

            self.name = "demo";
        }
    };
```

## [6.标准化代码编写](#model)

为获取最大化的可移植性和兼容性，代码中应使用标准中支持的方式来书写代码，虽然浏览器支持某些语法但不在规范中，你无法保证你将来不会遇到兼容其他浏览器的情况，如果这个浏览器完全或部分特性只按规范实现。

```javascript
    //错误的范例
    var char = 'hello world'[3];
    var myForm = document.myForm;

    //正确的写法
    var char = 'hello world'.charAt(3)
    var myForm = document.forms[0];或者var myForm = document.getElementsByTagName('form')[0];
```


## [7.面向对象编程](#oop)

- 类中的成员变量使用构造函数来初始化；
- 除非是必须移除类的成员，否则析构函数中对成员的销毁应通过将其设置为null，而不是用delete，因为重新赋值方式性能比用delete好；
- 避免通过prototype方式污染内置对象原型链；

## [8.作用域相关](#scope)

- 不使用with关键字，容易造成作用域混乱；
- this仅用于类成员函数或对象中；
- 通用全局函数，特别是通用组件代码应将业务逻辑放入闭包中，并通过“命名空间”将其引入；
- 若函数中使用到全局变量，则访问全局变量时应使用window来引入，如：

```javascript
    var somevar = 10；
    function getvar(){
        var num = window.somevar + 1;
        //...
        return num;
    }
```


## [9.格式化代码](#format)

没有强制要求代码需格式化成什么样子，但尽量使代码具有较高的可读性。下面给出一些建议的代码风格：

```javascript
    // 花括号起始位置与语句开始在同一行
    if (something) {
      // ...
    } else {
      // ...
    }

    // 数组和对象书写
    var arr = [1, 2, 3];  // [之后，]之前没有空格
    var obj = {a: 1, b: 2, c: 3};  // {之后，}之前没有空格

    // 多行数组和对象书写
    var a = [
      'huangrongrong',
      'chenying',
      'small grey grey',
      'shenshunlin'
    ];

    var pos = {
      top: 10,
      right: 20,
      bottom: 15,
      left: 12
    };

    // 将业务逻辑相关的代码写一起，不相关的以空行分隔
    doSomethingTo(x);
    doSomethingElseTo(x);
    andThen(x);

    nowDoSomethingWith(y);

    andNowWith(z);

    var x = a ? b : c;  // 一行能写完

    // 写不完可以换行加缩进
    var y = a ?
        longButSimpleOperandB : longButSimpleOperandC;

    // 或者这样
    var z = a ?
            moreComplicatedB :
            moreComplicatedC;

```

## [10.类型转换](#type)

对于代码中需要进行==逻辑判断的变量，建议进行强制类型转换或使用===代替。

下列值在布尔表达式中结果为false：

- null
- undefined
- '' //空字符串
- 0 //数字

而下面的为true：

- '0' //字符串
- [] //空数组
- {} //空对象

还有一些难以区分的表达式，以下表达式结果全为true：
```javascript
    Boolean('0') == true
    '0' != true
    0 != null
    0 == []
    0 == false

    Boolean(null) == false
    null != true
    null != false

    Boolean(undefined) == false
    undefined != true
    undefined != false

    Boolean([]) == true
    [] != true
    [] == false

    Boolean({}) == true
    {} != true
    {} != false
```

## [11.注释规范](#comments)

文档注释遵循YUIDoc规范（[http://yui.github.io/yuidoc/syntax/](http://yui.github.io/yuidoc/syntax/)），所有的文件、类、方法和属性都应该用合适的标记和类型进行注释。

- YUIDoc要求文档中至少要有一个class和constructor标记来定义类和构造函数；
- YUIDoc中注释块必须以/**（至少两个星号）开头；
- 方法、参数和返回值等必须有注释说明；
- 单行注释使用//；
- 对于代码中特殊用途的变量、存在临界值、函数中使用的hack、使用了某种算法或思路等需要进行注释描述；
- 建议加入开发者个人信息及最后修改时间注释，虽然这些信息不会被YUIDoc解析显示；

下面给出代码注释实例作为参考，更多标记和类型定义请参考YUIDoc官方文档。

```javascript
    /**
    这是一个UELib类

    @class UELib
    @constructor

    @author xxx <xxx@gmail.com>
    @modified someone
    @date 2013-06-25
    **/
    function UELib = function(){};

    /**
    My method description.  Like other pieces of your comment blocks,
    this can span multiple lines.

    @method css
    @param prop {String} set/get a css property
    @param [value] {String} A value you want to set to a css property
    @example
       ele.css('width', '100%');
    @return {Boolean} Returns true on success
    **/
    UELib.prototype.css = function(prop,value){

    };
```

## [12.杂项](#others)

- 尽量使用优雅的模版写法，避免多行字符串用\加换行的方式或使用+运算连接字符串这种难维护的编码方式；
- 禁止使用IE条件编译@cc_on；

## [13.赋值](#assign)

给变量赋值时，如果右侧是含有比较语句的表达式，需要用圆括号包裹。
```javascript
//好的方法
var flag = (i < count);
//不好的写法：遗漏括号
var flag = i < count;
```

## [14.三元操作符](#ternary-operator)

三元操作符应当仅仅用在条件赋值语句中，而不要作为if语句的替代品。
```js
//好的写法
var value = condition ? value1 : value2;

//不好的写法
condition ? doSomething():do somethingElse();
```

## [15.语句](#statement)

(1)简单语句
    每一行包含一条语句，以分号`;`结束
(2)返回语句
    当返回一个值的时候不应当使用圆括号包裹，除非在某些情况下这么做可以让返回值更容易理解。
(3)复合语句
- 复合语句是大括号括起来的语句列表。
- 括起来的语句应当较复合语句多缩进一个层级
- 开始的大括号应当在复合语句所在的行尾，结束的大括号应当独占一行且同复合语句的开始保持同样的缩进。
- 当语句是控制结构的一部分时，诸如if或者for语句，所有的语句都需要大括号括起来，也包括单个语句。
- 像if一样的语句开始的关键字，其后应该紧跟一个空格，起始大括号应当在空格之后。

```js
if (condition) {
    statements
} else if (condition) {
    statements
} else {
    statements
}
```

```js
for (test1; condition; update) {
    statements
}
for (variable in object) {
    statements
}
```

Switch下的每个case都应该保持一个缩进，除第一个之外包括default在内的每个case都应该在之前保持一个空行
```javascript
switch (value) {
    case 1:
    /* 抛出错误 */

    case 2:
        doSomething();
        break;

    case 3:
        return true;

    default:
        throw new Error(this should not happen);
}
```

## [16.参考文档](#reference)
- [hiwanz/javascript-style-reference](https://github.com/hiwanz/javascript-style-reference)

- [http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)

- [http://www.w3.org/TR/DOM-Level-2-HTML/html.html](http://www.w3.org/TR/DOM-Level-2-HTML/html.html)
