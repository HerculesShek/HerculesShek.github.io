---
layout: post
title: 前端 HTML CSS JS JQuery 学习指南
category: blog
tags: HTML CSS JS
description: 前端HTML CSS JS学习指南
---

## JQuery学习

### 安装&基本使用

需要下载jQuery的包，一般是下载最小包，最新版本从[这里][0]下载

获取一个元素的方式很方便，如 ```$("p")```是获取所有的p元素，例子：点击按钮显示或者隐藏所有的p元素

``` html
<head>
    <title>jquery demo</title>
    <script type="text/javascript" src="jquery-3.2.1.min.js"></script>
    <script type="text/javascript">
    $(document).ready(function() {
        $("button").click(function() {
            if ($("p").is(":visible") == true)
                $("p").hide();
            else
                $("p").show();
        })
    });
    </script>
</head>

<body>
    <h2>This is a heading</h2>
    <p>This is a paragraph.</p>
    <p>This is another paragraph.</p>
    <button type="button" style="height: 30px;">Click me and all p will hide </button>
</body>
```
基本语法是：```$(selector).action()``` 
示例：
``` javascript
$(this).hide()   //隐藏当前元素
$("p").hide()  // 隐藏所有段落
$(".test").hide() // 隐藏所有 class="test" 的所有元素
$("#test").hide() // 隐藏所有 id="test" 的元素
```
文档就绪函数:
``` javascript
$(document).ready(function(){
--- jQuery functions go here ----
});
```

这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码。
如果在文档没有完全加载之前就运行函数，操作可能失败。下面是两个具体的例子：
+ 试图隐藏一个不存在的元素
+ 获得未完全加载的图像的大小

这里的```document```是可以直接使用的，还有```window```

### JQuery选择器
参考[JQuery选择器参考手册][3]
注意：
由于 jQuery 是为处理 HTML 事件而特别设计的，那么当您遵循以下原则时，您的代码会更恰当且更易维护：
+ 把所有 jQuery 代码置于事件处理函数中
+ 把所有事件处理函数置于文档就绪事件处理器中
+ 把 jQuery 代码置于单独的 .js 文件中
+ 如果存在名称冲突，则重命名 jQuery 库

常见的事件有：ready click dbclick focus mouseover

### JQuery效果
显示 隐藏：
``` javascript
$(selector).hide(speed,callback);
$(selector).show(speed,callback);
$(selector).toggle(speed,callback);
```

淡入淡出：
```
$(selector).fadeIn(speed,callback);
$(selector).fadeOut(speed,callback);
$(selector).fadeToggle(speed,callback);
$(selector).fadeTo(speed,opacity,callback);
```

滑动：
```
$(selector).slideDown(speed,callback);
$(selector).slideUp(speed,callback);
$(selector).slideToggle(speed,callback);
```

动画：
```
$(selector).animate({params},speed,callback);
```

停止动画或者效果：
```
$(selector).stop(stopAll,goToEnd);
```

以上的callback都可以自行添加一个函数 即为回调函数

jQuery 方法链接：
```
$("#p1").css("color","red").slideUp(2000).slideDown(2000);
```

### JQuery HTML
获取DOM内容常用的3个方法:
```
text()
html()
val()
```
获取属性：
```
attr
```
设置内容和属性 方法同上 传入参数即可

text() html() val() 都有回调函数 attr也有回调函数

增加新的HTML内容的方法：
```
append // 在被选元素的结尾插入内容
prepend // 在被选元素的开头插入内容
after // 在被选元素之后插入内容 
before // 在被选元素之前插入内容
```
如：
``` javascript
function appendText()
{
var txt1="<p>Text.</p>";              // 以 HTML 创建新元素
var txt2=$("<p></p>").text("Text.");  // 以 jQuery 创建新元素
var txt3=document.createElement("p");
txt3.innerHTML="Text.";               // 通过 DOM 来创建文本
$("body").append(txt1,txt2,txt3);        // 追加新元素
}
```

JQuery删除：
```
remove()
empty()
```
JQuery CSS类
```
addClass()
removeClass()
toggleClass()
css()
```
**JQuery的css()方法** 
返回指定的属性的值：
```
css("propertyname");
```
设置属性的值：
```
css("propertyname","value");
$("p").css({"background-color":"yellow","font-size":"200%"});
```
**JQuery尺寸**
```
width()
height()
innerWidth()
innerHeight()
outerWidth()
outerHeight()
```

### JQuery遍历
**向上遍历**：

```
parent()
parents()
parentsUntil()
```
**后代**：
```
children()
find()
```
**同胞**：
```
siblings()
next()
nextAll()
nextUntil()
prev()
prevAll()
prevUntil()
```
**过滤**：
```
first()
last()
eq()
filter() 
not()
```

### JQuery AJAX
jQuery load()方法是简单但强大的AJAX方法。load()方法从服务器加载数据，并把返回的数据放入被选元素中。语法：
```
$(selector).load(URL,data,callback);
```
get和post方法：
```
$.get(URL,callback);
$.post(URL,data,callback);
```

### JQuery杂项
jQuery 使用 $ 符号作为 jQuery 的简写。
noConflict() 方法会释放会 $ 标识符的控制，这样其他脚本就可以使用它了。
```
$.noConflict();
jQuery(document).ready(function(){
  jQuery("button").click(function(){
    jQuery("p").text("jQuery 仍在运行！");
  });
});
```

## 一些问题：
### margin和padding
关于如何布局的问题 首先是 margin和padding等问题，margin是用来设置**“外边距”**的<br/>
margin始终是透明的。
margin通过使用单独的属性，可以对上、右、下、左的外边距进行设置。即：margin-top、margin-right、margin-bottom、margin-left。<br/>
这里必须注意[垂直外边距合并问题][4]<br/>
更进一步，看[这篇文章][5] 里面提到：
>
这个问题发生的原因是根据规范，一个盒子如果没有上补白(padding-top)和上边框(border-top)，那么这个盒子的上边距会和其内部文档流中的第一个子元素的上边距重叠。<br/>
再说了白点就是：父元素的第一个子元素的上边距margin-top如果碰不到有效的border或者padding.就会不断一层一层的找自己“领导”(父元素，祖先元素)的麻烦。只要给领导设置个有效的 border或者padding就可以有效的管制这个目无领导的margin防止它越级，假传圣旨，把自己的margin当领导的margin执行。<br/>
对于垂直外边距合并的解决方案上面已经解释了，为父元素例子中的middle元素增加一个border-top或者padding-top即可解决这个问题。
>

padding是留白或者补白，是边框和正文之间的距离

何时应当使用margin：
- 需要在border外侧添加空白时。
- 空白处不需要背景（色）时。
- 上下相连的两个盒子之间的空白，需要相互抵消时。如15px + 20px的margin，将得到20px的空白。

何时应当时用padding：
- 需要在border内测添加空白时。
- 空白处需要背景（色）时。
- 上下相连的两个盒子之间的空白，希望等于两者之和时。如15px + 20px的padding，将得到35px的空白。

关于margin和padding 此[文章][6]介绍的不错

负margin原来[如此有用][7] 总结为：
>
在margin属性中一共有两类参考线，top和left的参考线属于一类，right和bottom的参考线属于另一类。top和left是以外元素为参考，right和bottom是以元素本身为参考。margin的位移方向是指margin数值为正值时候的情形，如果是负值则位移方向相反。

### 父元素高度固定后覆盖问题
当父元素的高度固定后，如果子元素的高度之和大于父元素，那么就会溢出，并且可能会覆盖该父元素下发元素的内容

**<span class="todo-span">TODO 后续在这里补充例子</span>**

### 浮动
关于浮动 单独写了一篇[CSS浮动][8]
### CSS的position
参考文章:
+ [CSS之Position详解][9] 
+ [CSS中position属性( absolute \| relative \| static \| fixed )详解][10]
+ [CSS - position属性详解][11]


## HTML

[0]: http://jquery.com/download/ "JQuery Download"
[1]: https://developers.google.com/protocol-buffers/docs/downloads "protobuf-download"
[2]: https://developers.google.com/protocol-buffers/docs/cpptutorial "C++ Toturial"
[3]: http://www.w3school.com.cn/jquery/jquery_ref_selectors.asp "JQuery选择器参考手册"
[4]: http://www.w3school.com.cn/css/css_margin_collapsing.asp "垂直外边距合并问题"
[5]: http://www.hicss.net/do-not-tell-me-you-understand-margin/
[6]: http://www.hicss.net/use-margin-or-padding/
[7]: http://www.hicss.net/i-know-you-do-not-know-the-negative-margin/
[8]: /blog/2017/07/07/css-float "CSS浮动"
[9]: http://www.cnblogs.com/Zigzag/archive/2009/02/19/position.html
[10]: http://blog.csdn.net/chen_zw/article/details/8741365
[11]: http://www.jianshu.com/p/a2fbc435bbdd







