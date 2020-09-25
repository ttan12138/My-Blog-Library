# jQuery

![jQuery文档结构图](D:\数据\HBuilderProjects\尚硅谷前端资料\02进阶\jquery\源码_课件\课件\jQuery文档结构图.png)

## 1. 了解jQuery

### 1.1 是什么: What?

- 一个JS函数库: write less, do more
- 封装简化DOM操作(CRUD) / Ajax

### 1.2 为什么用它: why?

- 强大选择器: 方便快速查找DOM元素
- 隐式遍历(迭代): 一次操作多个元素
- 读写合一: 读数据/写数据用的是一个函数
- 链式调用: 可以通过.不断调用jQuery对象的方法
- 事件处理
- DOM操作(CUD)
- 样式操作
- 动画
- 浏览器兼容

### 1.3 如何使用: How?

- 引入jQuery库

  本地引入与CDN远程引入  /  测试版与生产版(压缩版)

- 使用jQuery

  使用jQuery函数: $/jQuery

  使用jQuery对象: $xxx(执行$()得到的)

## 2. jQuery的2把利器

### 2.1 jQuery函数: $/jQuery

jQuery向外暴露的就是jQuery函数, 可以直接使用。

**当成一般函数使用: $(param)**

- param是function: 相当于window.onload = function(文档加载完成的监听)
- param是选择器字符串: 查找所有匹配的DOM元素, 返回包含所有DOM元素的jQuery对象
- param是DOM元素: 将DOM元素对象包装为jQuery对象返回 $(this)
- param是标签字符串: 创建标签DOM元素对象并包装为jQuery对象返回

**当成对象使用: $.xxx**

- `$.each(obj/arr, function(key, value){})`： 隐式遍历数组
- `$.trim(str)`： 去除`str`两端的空格

### 2.2 jQuery对象

- 执行$()返回的就是jQuery对象，包含所有匹配的n个DOM元素的伪数组对象
- 基本行为:
  - `length/size()`:  得到dom元素的个数
  - `[index]`:  得到指定下标对应的dom元素
  - `each(function(index, domEle){})`:  遍历所有dom元素
  - `index()`:  得到当前dom元素在所有兄弟中的下标
- 与DOM对象
  - DOM对象与jQuery对象的语法完全不一样、方法几乎不一样，如`innerHTML()`与`html()`等；
  - DOM --> jQuery:   $(DOM对象)
  - jQuery --> DOM:   jQuery对象[i] ||  jQuery对象.get(i)

## 3. 选择器

​     有特定语法规则(css选择器)的字符串,用来查找某个/些DOM元素: $(selector), 部分可以参考css的选择器语法理解。

### 3.1 分类

- 基本

  |                 选择器                  |              作用              |
  | :-------------------------------------: | :----------------------------: |
  |                  `#id`                  |            id选择器            |
  |                `element`                |           元素选择器           |
  |                `.class`                 |           属性选择器           |
  |                   `*`                   |            任意标签            |
  | `selector1,selector2,selector3（并集）` | 取多个选择器的并集(组合选择器) |
  |  `selector1selector2selector3（交集）`  | 取多个选择器的交集(相交选择器) |

- 层次：  找子孙后代, 兄弟元素

   |        选择器         |                  作用                  |
   | :-------------------: | :------------------------------------: |
   | `ancestor descendant` |  在给定的祖先元素下匹配所有的后代元素  |
   |    `parent>child`     |    在给定的父元素下匹配所有的子元素    |
   |      `prev+next`      | 匹配所有紧接在 prev 元素后的 next 元素 |
   |    `prev~siblings`    | 匹配 prev 元素之后的所有 siblings 元素 |

- 过滤： 在原有匹配元素中筛选出其中一些

  `:first`、`:last`、`:eq(index)`、`:lt`、`:gt`、`:odd`、`:even`、`:not(selector)`、`:hidden`、`:visible`、`[attrName]`、`[attrName=value]`

- 表单

  `:input`、`:text`、`:checkbox`、`:radio`、`:checked（选中的）`

### 3.2 具体使用

- 普通选择器
  - 标签选择器：$("p")
  - 类选择器：$(".a1")
  - id选择器：$("#b1")
  - 并(或)集选择器：$(".class, #id")
  - 交集选择器：$("p.a1")  $(".a1#b1") {以.||#分割的}
  - 全局选择器：$(" ")
- 层次选择器 (取后面的元素)
  - 相邻选择器：$("选择器1+选择器2")  选择器1之后的一个选择器2元素 
  - 同辈选择器：$("p~a1")
  - 后代选择器：$("p a1")
  - 子代选择器：$("p>.a1")
- 属性选择器
  - $("[style]")
  - $("[style=ss]") || $("[style="ss"]")
  - $("[style!=ss]")
  - $("[style$=s]")以s结尾
  - $("[style^=s]")以s开头的元素
  - $("[style =s]")包含s的元素
- 过滤器选择器
  - $("p:first") ||  $("p").first()
  - $("p:last")  ||  $("p").last()
  - $("p:even")
  - $("p:odd")
  - $("p:eq(index)") 
  - $("p:gt(index)")
  - $("p:lt(index)")
  - $("p:not(选择器)")
  - $("p:header")
  - $("input:focus")
- 可见性选择器
  - :visible
  - :hidden

## 4. 属性/文本

​    常用操作标签的属性, 标签体文本方法如下：

- attr(name) / attr(name, value): 读写非布尔值的标签属性

- prop(name) / prop(name, value): 读写布尔值的标签属性

- removeAttr(name)/removeProp(name): 删除属性

- addClass(classValue): 添加class；多个Class，空格分隔

- removeClass(classValue): 移除指定class；无参移除全部

- val() / val(value): 读写标签的value

- html() / html(htmlString): 读写标签体文本

```js
//1. 读取第一个div的title属性
console.log($('div:first').attr('title')) // one

//2. 给所有的div设置name属性(value为atguigu)
$('div').attr('name', 'atguigu')

//3. 移除所有div的title属性
$('div').removeAttr('title')

//4. 给所有的div设置class='guiguClass'
$('div').attr('class', 'guiguClass')

//5. 给所有的div添加class='abc'
$('div').addClass('abc')

//6. 移除所有div的guiguClass的class
$('div').removeClass('guiguClass')

//7. 得到最后一个li的标签体文本
console.log($('li:last').html())

//8. 设置第一个li的标签体为"<h1>mmmmmmmmm</h1>"
$('li:first').html('<h1>mmmmmmmmm</h1>')

//9. 得到输入框中的value值
console.log($(':text').val()) // 读取

//10. 将输入框的值设置为atguigu
$(':text').val('atguigu') // 设置      读写合一
//11. 点击'全选'按钮实现全选
 // attr(): 操作属性值为非布尔值的属性
 // prop(): 专门操作属性值为布尔值的属性
var $checkboxs = $(':checkbox')
$('button:first').click(function () {
  $checkboxs.prop('checked', true)
})

//12. 点击'全不选'按钮实现全不选
$('button:last').click(function () {
  $checkboxs.prop('checked', false)
})
```

  > 补充：操作DOM
  > 【1】创建 $(js document对象)
  > 【2】插入 
  > 	内插：
  > 		B从后面插入到A
  > 			A.append(B) 
  > 			B.appendTo(A)
  > 		B从前面插入到A
  > 			A.prepend(B)
  > 			B.prependTo(A)
  > 	外插：
  > 	    B插入到A后面
  > 			A.after(B)
  > 			B.insertAfter(A)
  > 		B插入到A前面
  > 			A.before()
  > 			B.insertBefor

## 5. 事件模块

### 5.1 绑定事件

原生JS：addEventListener("事件",function(){})

```js
// 单事件
$(选择器).bind("click",function(){})
$(选择器).bind({"click":function(){},"submit":function(){}})
$(选择器).eventName(function(){})
$(选择器).on('eventName', function(){})

// 复合事件
$(选择器).hover(inFunction(){}, onFunction(){}):规定当鼠标指针悬停在被选元素上时要运行的两个函数
$(选择器).toggle(f1,f2,f3…): 切换事件，1.9以上支持
```

> 常用事件：
>
> 鼠标事件：click:单击事件、mouseover:鼠标悬浮、mouseout:鼠标离开
> 键盘事件：(不可存在太多 容易失效)keydown:按键 从上往下按的过程、keypress:按键按到底、keyup:松开按键
>        event.keycode == ? 控制哪个按键按下
> 表单事件：focus()、blur()

### 5.2 解绑事件

```js
$(选择器).off('eventName')
$(选择器).unbind('eventName')
```

### 5.3 编码

```js
$(选择器).delegate(selector, 'eventName', function(event){}) // 回调函数中的this是子元素
$(选择器).undelegate('eventName')
```

### 5.4 事件坐标

```js
event.offsetX: 原点是当前元素左上角
event.clientX: 原点是窗口左上角
event.pageX: 原点是页面左上角
```

### 5.5 事件相关

> 停止事件冒泡: event.stopPropagation()
>
> 阻止事件的默认行为: event.preventDefault()
>

## 6. CSS模块

### 6.1 style样式

​    css(styleName): 根据样式名得到对应的值

​    css(styleName, value): 设置一个样式

​    css({多个样式对}): 设置多个样式

   位置坐标

​    offset(): 读/写当前元素坐标(原点是页面左上角)

​    position(): 读当前元素坐标(原点是父元素左上角)

​    scrollTop()/scrollLeft(): 读/写元素/页面的滚动条坐标

   尺寸

​    width()/height(): width/height

​    innerWidth()/innerHeight(): width + padding

​    outerWidth()/outerHeight(): width + padding + border

## 7. 筛选模块

### 7.1 过滤

​    在jQuery对象内部的元素中找出部分匹配的元素, 并封装成新的jQuery对象返回

> ​    `first()`
>
> ​    `last()`
>
> ​    `eq(index)`
>
> ​    `filter(selector)`: 对当前元素提要求
>
> ​    `not(selector)`: 对当前元素提要求, 并取反
>
> ​    `has(selector)`: 对子孙元素提要求
>

### 7.2 查找

​    查找jQuery对象内部的元素的子孙/兄弟/父母元素, 并封装成新的jQuery对象返回

> `children(selector)`: 子标签中找
>
> `find(selector)`: 后代标签中找
>
> `preAll(selector)`: 前面所有的兄弟标签
>
> `nextAll(selector)`:后面所有的兄弟标签
>
> `siblings(selector)`: 前后所有的兄弟标签
>
> `parent(selector)`: 父标签

```js
var $lis = $('ul>li')
//1. ul下li标签第一个
$lis.first().css('background', 'red')
$lis[0].style.background = 'red'

//2. ul下li标签的最后一个
$lis.last().css('background', 'red')

//3. ul下li标签的第二个
$lis.eq(1).css('background', 'red')

//4. ul下li标签中title属性为hello的
$lis.filter('[title=hello]').css('background', 'red')

//5. ul下li标签中title属性不为hello的
$lis.not('[title=hello]').css('background', 'red')
$lis.filter('[title!=hello]').filter('[title]').css('background', 'red')

//6. ul下li标签中有span子标签的
$lis.has('span').css('background', 'red')

var $ul = $('ul')
//7. ul标签的第2个span子标签
$ul.children('span:eq(1)').css('background', 'red')

//8. ul标签的第2个span后代标签
$ul.find('span:eq(1)').css('background', 'red')

//9. ul标签的父标签
$ul.parent().css('background', 'red')

//10. id为cc的li标签的前面的所有li标签
var $li = $('#cc')
$li.prevAll('li').css('background', 'red')

//11. id为cc的li标签的所有兄弟li标签
$li.siblings('li').css('background', 'red')
```

## 8. 文档处理(CUD)模块

### 8.1 添加/替换元素

> `append(content)`:  向当前匹配的所有元素内部的最后插入指定内容
>
> `prepend(content)`: 向当前匹配的所有元素内部的最前面插入指定内容
>
> `before(content)`: 将指定内容插入到当前所有匹配元素的前面
>
> `after(content)`: 将指定内容插入到当前所有匹配元素的后面替换节点
>
> `replaceWith(content)`: 用指定内容替换所有匹配的标签删除节点

### 8.2 删除元素

>
> `empty()`: 删除所有匹配元素的子元素, 掏空(自己还在)
>
> `remove()`: 删除所有匹配的元素, 将自己及内部的孩子都删除

## 9. 动画效果

### 9.1 淡入淡出

不断改变元素的透明度(opacity)来实现的

> fadeIn(): 带动画的显示
>
> fadeOut(): 带动画隐藏
>
> fadeToggle(): 带动画切换显示/隐藏

### 9.2 滑动动画

不断改变元素的高度实现

> slideDown(): 带动画的展开
>
> slideUp(): 带动画的收缩
>
> slideToggle(): 带动画的切换展开/收缩

### 9.3 显示隐藏

默认没有动画, 动画(opacity/height/width)

> show(): (不)带动画的显示
>
> hide(): (不)带动画的隐藏
>
> toggle(): (不)带动画的切换显示/隐藏

### 9.4  jQuery动画本质

在指定时间内不断改变元素样式值来实现的

> animate(): 自定义动画效果的动画
>
> stop(): 停止动画

## 10. 其他

### 10.1 插件机制

- 扩展jQuery函数对象的方法

```js
$.extend({
  xxx: fuction () {} // this是$
})

$.xxx()
```

- 扩展jQuery对象的方法

```js
$.fn.extend({
  xxx: function(){} // this是jQuery对象
})

$obj.xxx()
```
