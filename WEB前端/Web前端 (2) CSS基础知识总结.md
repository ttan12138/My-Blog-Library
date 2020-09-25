#### CSS基



![image-20200801194338622](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200801194338622.png)



​	一个 HTML 文档可以显示不同的样式, CSS 指层叠样式表 (Cascading Style Sheets)，CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明。CSS声明总是以分号 ; 结束，声明组以大括号 {} 括起来。

##### 层叠次序

​	一般而言，所有的样式会根据下面的规则层叠于一个新的虚拟样式表中，其中数字 4 拥有最高的优先权(假设为1000)。

|                 样式规则                  | 假设权值 |
| :---------------------------------------: | :------: |
|         内联样式，如: style="..."         |   1000   |
|          ID选择器，如：#content           |   0100   |
|    类，伪类、属性选择器，如：.content     |   0010   |
|    类型选择器、伪元素选择器，如：div p    |   0001   |
| 通配符、子选择器、相邻选择器等。如：* > + |   0000   |
|                继承的样式                 |  无权值  |

​     按照上面假设的权重值可以很清晰的计算某属性的描述的权值，然后按照1,0,0,0 > 0,99,99,99的比较规则，从左往右逐个比较，前一等级相等才往后比。权重相同的情况下，后面的样式会覆盖掉前面的样式。有权值比没有权值要优先。

​	注：ie6及之后部分浏览器提供  !import，如 p{color:red !important;} ，使该样式提升优先级到最高。最好按照如下四条规则使用 !import:

1. Never 永远不要在全站范围的 css 上使用 !important;
2. Only 只在需要覆盖全站或外部 css（例如引用的 ExtJs 或者YUI ）的特定页面中使用 !important;
3. Never 永远不要在你的插件中使用 !important;
4. Always 要优化考虑使用样式规则的优先级来解决问题而不是 !important。

##### CSS选择器

|      |   选择器类别   |      示例      | 功能                                                         |
| :------------: | :------------: | :----------------------------------------------------------- | :----------------------------------------------------------: |
|   1   |   标签选择器   |       a        | 为特定的元素标签指定特定的样式。                             |
|    1    |    id选择器    |      #id       | 为标有特定 id 的 元素指定特定的样式。老版浏览器中，做派生选择器会被忽略 |
|    1    |    类选择器    |     .class     | 为标有class 的一类元素指定特定的样式。老版浏览器中，做派生选择器会被忽略 |
|   2   |   属性选择器   |    [width]     | 为拥有指定属性的元素设置样式，使不仅限于 class 和 id 属性    |
| 2 | 属性和值选择器 | [width=‘100%’] | 为拥有指定属性和值的元素设置样式。                           |
| 2-3 | 属性和值选择器 |      多值      | 完整value：~=(包含) 、\|=(a开头)；拼接字符串value：^=(a开头)、*=(包含)、$=(a结尾) |
|       2       |       *（通配符）       |       *        | 选择全部元素                                                 |
|   1   |   并集选择器   |     div,p      | 选择所有<div>元素和<p>元素                                   |
|   1   |   后代选择器   |     div p      | 选择<div>元素内的所有<p>元素 ，如div->p->p,其中p都能被选择 |
|       2       |       `> (子代选择器)`       |     div>p      | 选择所有父级是 <div> 元素的 <p> 元素，如div->p->p,只能选择第一个p |
|       2       |     `+ (兄弟选择器)`     |     div+p      | 选择所有紧接着<div>元素之后的<p>元素                         |
|     1     |     :link      |     a:link     | 选择 所有未访问链接                                          |
|    1    |    :visited    |   a:visited    | 选择 所有访问过的链接                                        |
|    1    |    :active     |    a:active    | 选择 活动链接                                                |
|     1     |     :hover     |    a:hover     | 选择 鼠标在链接上面时                                        |
|     2     |     :focus     |  input:focus   | 选择具有焦点的输入元素                                       |
|    2    |    :before     |    p:before    | 在每个<p>元素之前插入内容                                    |
|     2     |     :after     |    p:after     | 在每个<p>元素之后插入内容                                    |
|       3       |       ~        |      p~ul      | 选择p元素之后的每一个ul元素                                  |
| 1 | :first-letter/line | p:first-letter | 选择每一个<p>元素的第一个字母(-letter)、行(-line) |
| 2-3 | :first(last/only)-child | p:first-child | 指定只有当<p>元素是其父级的第(last最后，only唯一)一个子级的样式 |
| 3 | :first(last/only)-of-type | p:first-of-type | 每个p元素是其父级的第一个(last最后,only唯一)p元素 |
| 3 | :nth-child(n) | p:nth-child(2) | 选择每个p元素是其父级的第二个子元素 |
| 3 | nth-of-type(n) | p:nth-of-type(2) | 选择每个p元素是其父级的第二个p元素 |
| 3 | :nth-last-of-type(n) | p:nth-last-of-type(2) | 选择每个p元素的是其父级的第二个p元素，从最后一个子项计数 |
| 3 | :root | :root | 选择文档的根元素 |
| 3 | :empty | p:empty | 选择每个没有任何子级的p元素（包括文本节点） |
| 3 | :target | \#news:target | 选择当前活动的#news元素（包含该锚名称的点击的URL） |
| 3 | :enabled | input:enabled | 选择每一个已启用的输入元素 disabled:禁用、checked选中 |
| 3 | :not( ) | :not(p) | 选择每个并非p元素的元素 |
| 3 | ::selection | ::selection | 匹配元素中被用户选中或处于高亮状态的部分 |
| 3 | :out-of-range | :out-of-range | 匹配值在指定区间之外的input元素  in-range：内 |
| 3 | :read-write | :read-write | 用于匹配可读及可写的元素 |
| 3 | :read-only | :read-only | 用于匹配设置 "readonly"（只读） 属性的元素 |
| 3 | :optional | :optional | 用于匹配可选的输入元素 |
| 3 | :required | :required | 用于匹配设置了 "required" 属性的元素 |
| 3 | :valid | :valid | 用于匹配输入值为合法的元素 |
| 3 | :invalid | :invalid | 用于匹配输入值为非法的元素 |

##### CSS属性

可继承属性：color、 text-开头的、line-开头的、font-开头的字符颜色 color:red;

- **文字属性**

  - 文字大小 font-size: 14px;

  - 文字颜色 color：#ffff00;

  - 文本粗细 font-weight: normal;

    ​	 /*100:最细 || normal/400: 正常|| bold/700：粗体 ||900:最粗 */

  - 指定字体 font-famil： Serif； /* [更多 ](https://www.w3school.com.cn/css/css_font.asp)*/

  - 字体风格 font-style: italic;

  ​           /* normal: 正常|| italic：斜体  [更多 ](https://www.w3school.com.cn/tiy/t.asp?f=csse_font-style)  */

  - 缩进文本 text-indent: 5em；

       /*悬挂缩进，设置为负值，且设置等值padding-left 防止溢出显示 */

  - 字间隔 word-spacing：30px;

       /*正:增加 ||  0/noraml:正常 ||  负:减少 */

  - 装饰文本 text-decoration：none; 

  ​      /* none:取消如<a>的默认样式 || underline/overline: 下划线/上划线 || line-through：删除线   [更多 ](https://www.w3school.com.cn/cssref/pr_text_text-decoration.asp) */

  - 字符转换 text-transform：none；

    /* uppercase:全大写 ||  none:正常 ||  lowercase:全小写 || capitalize:单词首字母大写*/

  - 水平对齐 text-align: center;  

      /*left: 左对齐 || right：右对齐 ||  justify:两端对齐 */

  - 对齐设置justify后，可进一步设置text-justify：（仅IE5.5以上支持）

| 值                                                           | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| auto                                                         | 浏览器决定齐行算法。                                         |
| none                                                         | 禁用齐行。                                                   |
| inter-word                                                   | 增加/减少单词间的间隔。                                      |
| [更多](https://www.runoob.com/cssref/css3-pr-text-justify.html) | [更多](https://www.runoob.com/cssref/css3-pr-text-justify.html) |

​			文本最后一行不被对齐，可如下在最后一行添加一行空行解决：

```css
.justify_list:after {
    width: 100%;
    height: 0;
    margin: 0;
    display: inline-block;
    overflow: hidden;
    content: '';
  }
```

- **背景属性**

  | 属性                                                         | 描述                                                         | CSS  |
  | :----------------------------------------------------------- | :----------------------------------------------------------- | :--- |
  | [background](https://www.w3school.com.cn/cssref/pr_background.asp) | 背景综合属性，如`background: #ff0000 url() no-repeat 10px 10px center;` | 1    |
  | [background-attachment](https://www.w3school.com.cn/cssref/pr_background-attachment.asp) | 背景图像固定(fixed)或随着页面滚动(scroll)。                  | 1    |
  | [background-color](https://www.w3school.com.cn/cssref/pr_background-color.asp) | 背景颜色。                                                   | 1    |
  | [background-image](https://www.w3school.com.cn/cssref/pr_background-image.asp) | 背景图像。                                                   | 1    |
  | [background-position](https://www.w3school.com.cn/cssref/pr_background-position.asp) | 设置背景图像的开始位置。                                     | 1    |
  | [background-repeat](https://www.w3school.com.cn/cssref/pr_background-repeat.asp) | 设置是否及如何重复背景图像。                                 | 1    |
  | [background-clip](https://www.w3school.com.cn/cssref/pr_background-clip.asp) | 规定背景的绘制区域。                                         | 3    |
  | [background-origin](https://www.w3school.com.cn/cssref/pr_background-origin.asp) | 规定背景图片的定位区域。                                     | 3    |
  | [background-size](https://www.w3school.com.cn/cssref/pr_background-size.asp) | 规定背景图片的尺寸。                                         | 3    |

  - **CSS精灵图(css sprite)**

    ![image-20200802161008687](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200802161008687.png)

    ​        将一个页面涉及到的所有零星背景图像都集中到一张大图中去，用css 的背景定位来显示需要显示的图片部分,**一个图片的大小是减少的**。

    ```
    background-position: 描述左右上下的词;
    描述左右的词儿： left、 center、right
    描述上下的词儿： top 、center、bottom
    
    .home{
      width:84px; 
      height: 47px;   /*需要图标的宽高  即显示的宽高*/
      background-image: url(sprite.png);  /*图片路径*/
      background-position: -425px -250px; /*图标从图片左上角开始的x,y的负值*/
    }
    ```

- **定位属性**

    | 属性                                                         | 描述                                                   | CSS  |
    | :----------------------------------------------------------- | :----------------------------------------------------- | :--- |
    | [bottom](https://www.w3school.com.cn/cssref/pr_pos_bottom.asp) | 设置定位元素下外边距边界与其包含块下边界之间的偏移。   | 2    |
    | [clear](https://www.w3school.com.cn/cssref/pr_class_clear.asp) | 规定元素的哪一侧不允许其他浮动元素。                   | 1    |
    | [clip](https://www.w3school.com.cn/cssref/pr_pos_clip.asp)   | 剪裁绝对定位元素。                                     | 2    |
    | [cursor](https://www.w3school.com.cn/cssref/pr_class_cursor.asp) | 规定要显示的光标的类型（形状）。                       | 2    |
    | [display](https://www.w3school.com.cn/cssref/pr_class_display.asp) | 规定元素应该生成的框的类型。                           | 1    |
    | [float](https://www.w3school.com.cn/cssref/pr_class_float.asp) | 规定框是否应该浮动。                                   | 1    |
    | [left](https://www.w3school.com.cn/cssref/pr_pos_left.asp)   | 设置定位元素左外边距边界与其包含块左边界之间的偏移。   | 2    |
    | [overflow](https://www.w3school.com.cn/cssref/pr_pos_overflow.asp) | 规定当内容溢出元素框时发生的事情。                     | 2    |
    | [position](https://www.w3school.com.cn/cssref/pr_class_position.asp) | 规定元素的定位类型。                                   | 2    |
    | [right](https://www.w3school.com.cn/cssref/pr_pos_right.asp) | 设置定位元素右外边距边界与其包含块右边界之间的偏移。   | 2    |
    | [top](https://www.w3school.com.cn/cssref/pr_pos_top.asp)     | 设置定位元素的上外边距边界与其包含块上边界之间的偏移。 | 2    |
    | [vertical-align](https://www.w3school.com.cn/cssref/pr_pos_vertical-align.asp) | 设置元素的垂直对齐方式。                               | 1    |
    | [visibility](https://www.w3school.com.cn/cssref/pr_class_visibility.asp) | 规定元素是否可见。                                     | 2    |
    | [z-index](https://www.w3school.com.cn/cssref/pr_pos_z-index.asp) | 设置元素的堆叠顺序。                                   | 2    |

- **2D/3D 转换属性（Transform）**

    | 属性                                                         | 描述                                                         | CSS  |
    | :----------------------------------------------------------- | :----------------------------------------------------------- | :--- |
    | [transform](https://www.w3school.com.cn/cssref/pr_transform.asp) | 向元素应用 2D 或 3D 转换。[更多](https://www.w3school.com.cn/cssref/pr_transform.asp) | 3    |
    | [其他](https://www.w3school.com.cn/css3/css3_3dtransform.asp) | [其他](https://www.w3school.com.cn/css3/css3_3dtransform.asp) | 3    |
    
- **过渡属性（Transition）**

    | 属性                                                         | 描述                                                         | CSS  |
    | :----------------------------------------------------------- | :----------------------------------------------------------- | :--- |
    | [transition](https://www.w3school.com.cn/cssref/pr_transition.asp) | 简写属性，用于在一个属性中设置四个过渡属性。                 | 3    |
    | [更多](https://www.w3school.com.cn/cssref/pr_transition.asp) | [更多](https://www.w3school.com.cn/cssref/pr_transition.asp) | 3    |
    

```css
div{
  width:100px;
  transition: width 2s; /*过渡属性名称 过渡时间 过渡效果(linear匀速) 过渡开始等待时间*/
  -moz-transition: width 2s; /* Firefox 4 */
  -webkit-transition: width 2s; /* Safari 和 Chrome */
  -o-transition: width 2s; /* Opera */
}
```

​		更多属性点击[这里](https://www.w3school.com.cn/cssref/index.asp#flexbox)

##### CSS布局

- **盒模型：**

​		一个盒子中主要的属性就5个：width、height、padding、border、margin。

   ![image-20200801215840570](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200801215840570.png)

​      真实占有宽度= 左border + 左padding + width + 右padding + 右border
`padding(margin) : 30px 20px 40px 100px; /*（上、右、下、左）*/`

`   border:10px solid red; /* 边框宽度(width)、类型(style)、颜色(color)*/`

​     **善于使用父级的padding，而不是子级的margin**

​     **margin可以处理兄弟元素的关系**

​	注：

​		1.IE6中不支持小于12px的盒子，看起来会变大，解决方法：使字号小于盒高

​		2.IE6双倍margin_bug：当出现连续浮动元素时，margin与浮动方向相同，队首双倍margin，解决方法：反向margin

​		3.IE6 3px_bug：如子代右浮动且margin-right：10px，右侧会出现13px的间距，解决方法：反向margin

- **标准文档流**

1. 空白折叠现象：HTML中所有的文字之间，如果有空格、换行、tab都将被折叠为一个空格显示。

2. 高矮不齐，底边对齐

3. 自动换行，一行写不满，换行写

   标准文档流--margin塌陷问题：竖直方向margin不叠加，会以较大为准

   

- **脱离标准流**

  **1.浮动**

  ​      -- 浮动的元素脱离标准流。（能够并排了，并且能够设置宽高了。无论它原来是个div还是个span）。
  ​      -- 浮动的元素互相靠帖。
  ​      -- 浮动的元素有“字围”效果。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title></title>
  <style text="text/css">
    ul{
      float:left;
      margin:0px;
      padding:0px;
    }
    ul li{
      color:white;
      float:left;
      background-color:#FF923D;
    }
  </style>
</head>
<body>
  <div>
    <ul>
      <li>TEST1|</li>
      <li>TEST2|</li>
      <li>TEST3|</li>
      <li>TEST4</li>
    </ul>
  </div>
  <div>
    <ul>
      <li>TEST-01|</li>
      <li>TEST-02|</li>
      <li>TEST-03|</li>
      <li>TEST-04</li>
    </ul>
  </div>
</body>
</html>
```

![image-20200801222201121](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200801222201121.png)

​	**2.清除浮动**

​	①给浮动元素的祖先元素添加高度

```css
  div{
    height:40px;
  }
```

![image-20200801221811461](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200801221811461.png)

​	②给最后一个浮动元素添加style：clear:both;  **会使margin失效**

​	③**隔墙法** 添加一个块级元素

```css
#heightdiv{
      clear:both;
      height:10px;
    }
```

```html
<div>
    <ul>
      <li>TEST1|</li>
      <li>TEST2|</li>
      <li>TEST3|</li>
      <li>TEST4</li>
    </ul>
  </div>
  <div id="heightdiv"></div>
  <div>
    <ul>
      <li>TEST-01|</li>
      <li>TEST-02|</li>
      <li>TEST-03|</li>
      <li>TEST-04</li>
    </ul>
  </div>
```



![image-20200801222132924](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200801222132924.png)

​	④**内墙法**

```css
.heightdiv{
      height:40px;
    }
```

```html
<div>
    <ul>
      <li>TEST1|</li>
      <li>TEST2|</li>
      <li>TEST3|</li>
      <li>TEST4</li>
    </ul>
    <div class="heightdiv"></div>
  </div>

  <div>
    <ul>
      <li>TEST-01|</li>
      <li>TEST-02|</li>
      <li>TEST-03|</li>
      <li>TEST-04</li>
    </ul>
    <div class="heightdiv"></div>
  </div>
```

![image-20200801222556533](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200801222556533.png)

​	⑤**overflow : hidden;**（IE6不支持，可追加_zoom:1;）

​     	本意就是清除溢出到盒子外面的文字。但是，前端开发工程师又发现了，它能做偏方。

一个父亲不能被自己浮动的儿子，撑出高度。但是，只要给父亲加上overflow:hidden; 那么，父亲就能被儿子撑出高了。这是一个偏方。

 ```css
   div{
   overflow:hidden;
   }
 ```

   

![image-20200801222747813](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200801222747813.png)

​	3.**定位**

​		①相对定位（relative）：元素**相对自己原来的位置**，进行位置调整，**在原来位置占有空间**（在文件流）。

​	<img src="C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200802164847852.png" alt="image-20200802164847852" style="zoom: 50%;" />

​		②绝对定位（position:absolute）：脱离文件流，不遵守标准流性质，不分行块，以最近的(绝对/相对)定位的盒子做参考，且不需考虑参考盒子的padding。

```html
<div id="box1">
<!-- box1 ==> 绝对/相对定位 一般来说“子绝父相” -->
  <div id="box2">
  <!-- box2 ==> 无定位 -->
    <div id="box3"></div>
    <!-- box3 ==> 绝对定位，将以box1为参考，因为box2没有定位，box1就是最近的父辈元素 -->
  </div>
</div>
```

​		③固定定位（position: fixed）：以浏览器窗口为参考定位，页面滚动不影响其位置。  IE6 不兼容。

![image-20200802173812022](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200802173812022.png)



-  **居中**

    - **盒子居中**

    处于标准流的盒子，width设置明确时，margin：0 auto；可以使盒子居中。

  - **定位居中**
  
    ①绝对盒子居中：因脱离标准流，margin：auto；失效
  
    ```html
    <style>
      * {
        margin: 0 auto;
        padding: 0;
      }
      #re_po {
        width: 100%;
        height: 100px;
        background-color: #000000;
        position: relative;
      }
      #ab_po {
        width: 100px;
        height: 50px;
        background-color: #FFFF00;
        position: absolute;
        left: 50%;
        top: 50%;
        margin-left: -50px;
        margin-top: -25px;
        /* transform: translate(-50%, -50%); */ /*进阶-未知宽高*/
        /*margin：auto；       
          left: 0;
     	  top: 0;
          right: 0;
          bottom: 0;*/      /*进阶-位置宽高*/
      }
    </style>
    
    <div id="re_po">
      <div id="ab_po"></div>
    </div>
    ```
  
    ​	![image-20200802171859713](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200802171859713.png)
  
     ②固定定位居中：同absolute。
  
    - **文字垂直居中**
  
      ```html
      <style>
        p {
          height: 60px;
          line-height: 60px;  /*line-height与height相等*/
          background-color: skyblue;
          color: #FFFA90;
        }
      </style>
      
      <p>单行文字垂直居中</p>
      <!--多行文字垂直居中，设置盒子padding-->
      <!--严格保证文字居中：行高-字号优先偶数设置-->
      ```
  
      

- **多维**
  - **z-index**
    - z-index 值表示谁更接近用户，即谁压着谁。数值大的压住数值小的。
    - 只有定位了的元素，才能有 z-index 值。浮动不能用。
    - z-index 值没有单位，默认的 z-index 值是 0，可以设置为正整数。
    - 如果页面元素都没有设置 z-index 值或者 z-index 值一样，那么写在 HTML 后面的元素在上面且能压住别的元素。定位了的元素，永远能压住没有定位的元素

![image-20200802175733833](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200802175733833.png)

![image-20200802175611897](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200802175611897.png)