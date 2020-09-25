## HTML

​		HTML 是 HyperTextMarkupLanguage, 超文本标记语言 的缩写。是一种标记语意的文档格式。

​	任何纯文本编辑器都能够编辑 html，比如记事本，editplus, notepad++, vscode等。

- DreamWeaver (Adobe公司的产品，过时了，不是一个好的代码编辑器）

- Sublime （**高效率的程序书写工具**）

- WebStorm （更高级的项目级别编程工具）

- VsCode （一个强大并拥有很多插件的编辑器）

- Hbuilder/~X   （配置方便，使用简单）

##  HTML语法

### HTML 骨架

  	英文输入状态下：输入 ! 按Tab就可以生成骨架

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>示例页面标题</title>
</head>
<body>
  <h6> 示例文章标题 </h6>
  <p> 示例文章段落 </p>
</body>
</html>
```

#### 文档声明头

​	任何一个标准的 HTML 页面，第一行一定是一个以<!DOCTYPE …… >开头的语句。<!DOCTYPE html>表示声明为HTML5文档，需要知道的事在HTML5中减去了因规范严格标准而制定的XHTML。

#### 字符集

​	字符集用 **meta** 标签定义，meta 表示 **元** 。元配置就是标识基本的配置项。

#### 引入样式和脚本（css || js）

- css

  - 行内样式  <p style={color:red}></p>

  - 内部样式 

  ```html
  <style type="text/css">
      p{
          color:red;
      }   
  </style>
  ```

  - 外部样式

  ```html
  <!--第一种-->
  <link rel="stylesheet" type="text/css" href=".css文件路径"/>
  <!--第二种-->
  <style> 
  @import url(".css文件路径")
  </style>
  ```

- js

```html
  <script type="text/javascript" src=".js文件路径">
      //无src属性，直接书写亦可
  </script>
```

### HTML语法特征

​			<body>    content    </body>       

​			开始标签  元素内容   结束标签

- HTML 元素以开始标签起始 
- HTML 元素以结束标签终止
- 元素的内容是开始标签与结束标签之间的内容
- 某些 HTML 元素具有空内容（empty content，也叫空元素。空元素在开始标签中进行关闭（以开始标签的结束而结束），常见的有**`<br/>，<hr/>，<img/>，<input/>，<meta/>，<link/>`**
- 大多数 HTML 元素可拥有属性
- HTML 标签对大小写不敏感：<P> 等同于 <p>。**标准W3C要求小写的 HTML 标签**。
- 即使您忘记了使用结束标签，大多数浏览器也会正确地显示 HTML，但是**不要忘记结束标签**

### HTML标签

以下只列出常用标签和新增html5标签，更多内容点击[这里](https://www.w3school.com.cn/tags/index.asp)。

#### 常见HTML标记

- 结构标记：html,head,body
- 头部标记：title,meta,link,style,base
- 标题标记：h1,h2,h3,h4,h5,h6【update 20161114】
- 文本修饰标记：font,b,i,u,strong
- 段落标记：p,hn,pre,marquee,br,hr,address,center,blockquote
- 列表标记：ul,ol,li,dl,dt,dd
- 超链接标记：a,map,area
- 图像及媒体元素标记：img,embed,object
- 表格标记：table,tr,td,th,tbody
- 表单标记：form,input,textarea,select,option,fieldset,legend,label
- 框架标记：frameset,frame,iframe
- 容器标记：div,spa

### HTML列表

- 无序列表 ul -> li

  属性：【type】disc(实心圆)是默认的样式，circle(空心圆)，square(实心方块)

- 有序列表 ol -> li

   属性：【type】10是从10开始数字样式，数字样式为默认样式，A/a(大小写英文字母样式)

- 自定义列表dl -> dt -> dd

  ​    自定义列表以 <dl> 标签开始。每个自定义列表项以 <dt> 开始。每个自定义列表项的定义以 <dd> 开始。

- 嵌套列表

- 默认样式：在无序列表中，默认第一层是disc,第二层是空心圆圈，第三层是实心方块，以后都是实心方块。

### HTML表格

```html
<table>
    <!--<colgroup>
          <col span="2" style="color:red;"/>
         </colgroup>
      注：html5中只支持span和html全局属性等属性-->
    <!--<caption>表格标题</caption>-->
    <!--<thead>页眉，含所有tr -> th</thead>-->
  <tr>
    <th>表头</th>
    <th>表头</th>
    <!--默认粗体居中-->
  </tr>
    <!--<tbody>主体，含所有tr -> td</tbody>-->
  <tr>
    <td>内容1</td>
    <td>内容2</td>
  </tr>
  <tr>
   <td>&nbsp;</td>
   <!--空内容td占位-->
   <td>内容4</td>
  </tr>
    <!--<tfoot>页脚，含最后一行用来总结（合计）的tr -> td</tfoot>-->
</table>

```

### HTML表单

表单三要素：form,input,submit三要素：form,input,submit

- form标签：点“提交”按钮，提交表单相关信息

- input标签：收集用户信息,取值有10种
    1.  text:文本域
    2. password:密码域
    3. file:文件域
    4. checkbox:复选框
    5. radio：单选按钮
    6. button:普通按钮
    7. submit:提交按钮
    8. reset:重置按钮
    9. hidden:隐藏域
    10. image:图像域（即图像按钮）
    
- textarea标签：多行文本域

    ```html
    <textarea cols ="30" rows="10"> </textarea>
    ```

    <textarea cols ="30" rows="10"> </textarea>

- label标签:为控件定义标签，通过for和控件的id绑定

    ```html
    <input type="radio" name="sex"  id="man"/>
    
    <label for="man">男</label>
    
     <input type="radio" name="sex"  id="woman"/>
    
    <label for="woman">女</label>
    ```

- select标签：配合子标签 <option> 用于下拉列表（不设置size）或列表项（设置size）。

     ```html
     <select>
          <option value="beijing">北京</option>
          <option value="shanghai">上海</option>
     </select>
     ```

     <select>
          <option value="beijing">北京</option>
          <option value="shanghai">上海</option>
     </select>

- button标签：按钮

    ```html
    <button>提交</button>
    ```
    
    <button>提交</button>

### HTML框架

- <frameset>在html5中已过期

- <frame/>在html5中已过期

- iframe标签： html5仍然可用

### 超链接target属性

- _blank:新窗口打开
- _parent:父窗口打开
- _self:自身窗口打开
- _top:顶层窗口打开

### 按行块分元素

- **块元素**：div,p,ul,ol,li,dl,dt,dd,form,hr,table,tr,td,h1-h6,filedset,caption
  - 以块的形式显示为一个矩形区域；
  - 块状元素独占一行，自上而下排列；
  - 块状元素可以定义自己的宽度和高度，以及盒模型中的margin,padding,border；
  - 块状元素可以作为一个容器包含其他的块状元素或内联元素。

- **内联元素（行内元素）**：a,strong,b,i,em,span,label,img,input
  - 内联元素在一行逐个进行显示；
  - 内联元素没有自己的形状，不能定义宽度和高度，它的宽高由内容来决定；
  - 内联元素设置与高度相关的一些属性（如margin-top,margin-bottom,padding-top,padding-bottom,line-height），显示无效或显示不准确；
  - 内联元素设置左右填充和外间距是可以的。

- **内联块状元素**：img,input,textarea
  - 既具有内联元素特点，也具有块状元素特点
  - 既可以定义容器的宽，高，margin，padding等，还可以和其他内联元素在一行显示

- **可变元素类型转换**： display:block|inline|inline-block|none|list-item;

	　　- block:将元素转换为块状元素（大部分块状元素的默认display属性值）
	　　- inline:将元素转换为内联元素（大部分内联元素的默认display属性值）
	　　- inline-block:将元素转换为内联块状元素（img,input等元素的默认display属性值）
	　　- list-item:将元素转换为列表类型（li的默认display属性值）
	　　- none:元素隐藏不可见

​         注：当元素设置了float属性后，相当于display:block。

### 新增HTML5元素


|                             标签                             | 描述                                               |
| :----------------------------------------------------------: | :------------------------------------------------- |
| [`<article>`](https://www.w3school.com.cn/tags/tag_article.asp) | 定义文章。                                         |
| [`<aside>`](https://www.w3school.com.cn/tags/tag_aside.asp)  | 定义页面内容之外的内容。                           |
| [`<audio>`](https://www.w3school.com.cn/tags/tag_audio.asp)  | 定义声音内容。                                     |
|   [`<bdi>`](https://www.w3school.com.cn/tags/tag_bdi.asp)    | 定义文本的文本方向，使其脱离其周围文本的方向设置。 |
| [`<canvas>`](https://www.w3school.com.cn/tags/tag_canvas.asp) | 定义图形。                                         |
| [`<command>`](https://www.w3school.com.cn/tags/tag_command.asp) | 定义命令按钮。                                     |
| [`<datalist>`](https://www.w3school.com.cn/tags/tag_datalist.asp) | 定义下拉列表。                                     |
| [`<details>`](https://www.w3school.com.cn/tags/tag_details.asp) | 定义元素的细节。                                   |
| [`<dialog>`](https://www.w3school.com.cn/tags/tag_dialog.asp) | 定义对话框或窗口。                                 |
| [`<embed>`](https://www.w3school.com.cn/tags/tag_embed.asp)  | 定义外部交互内容或插件。                           |
| [`<figcaption>`](https://www.w3school.com.cn/tags/tag_figcaption.asp) | 定义 figure 元素的标题。                           |
| [`<figure>`](https://www.w3school.com.cn/tags/tag_figure.asp) | 定义媒介内容的分组，以及它们的标题。               |
| [`<footer>`](https://www.w3school.com.cn/tags/tag_footer.asp) | 定义 section 或 page 的页脚。                      |
| [`<header>`](https://www.w3school.com.cn/tags/tag_header.asp) | 定义 section 或 page 的页眉。                      |
| [`<keygend>`](https://www.w3school.com.cn/tags/tag_keygen.asp) | 定义生成密钥。                                     |
|  [`<mark>`](https://www.w3school.com.cn/tags/tag_mark.asp)   | 定义有记号的文本。                                 |
| [`<meter>`](https://www.w3school.com.cn/tags/tag_meter.asp)  | 定义预定义范围内的度量。                           |
|   [`<nav>`](https://www.w3school.com.cn/tags/tag_nav.asp)    | 定义导航链接。                                     |
| [`<output>`](https://www.w3school.com.cn/tags/tag_output.asp) | 定义输出的一些类型。                               |
| [`<progress>`](https://www.w3school.com.cn/tags/tag_progress.asp) | 定义任何类型的任务的进度。                         |
|    [`<rp>`](https://www.w3school.com.cn/tags/tag_rp.asp)     | 定义若浏览器不支持 ruby 元素显示的内容。           |
|    [`<rt>`](https://www.w3school.com.cn/tags/tag_rt.asp)     | 定义 ruby 注释的解释。                             |
|  [`<ruby>`](https://www.w3school.com.cn/tags/tag_ruby.asp)   | 定义 ruby 注释。                                   |
| [`<section>`](https://www.w3school.com.cn/tags/tag_section.asp) | 定义 section。                                     |
| [`<source>`](https://www.w3school.com.cn/tags/tag_source.asp) | 定义媒介源。                                       |
| [`<summary>`](https://www.w3school.com.cn/tags/tag_summary.asp) | 为 <details> 元素定义可见的标题。                  |
|  [`<time>`](https://www.w3school.com.cn/tags/tag_time.asp)   | 定义日期/时间。                                    |
| [`<track>`](https://www.w3school.com.cn/tags/tag_track.asp)  | 定义用在媒体播放器中的文本轨道。                   |
| [`<video>`](https://www.w3school.com.cn/tags/tag_video.asp)  | 定义视频。                                         |
|   [`<wbr>`](https://www.w3school.com.cn/tags/tag_wbr.asp)    | 定义可能的换行符。                                 |

### HTTP 状态消息

当浏览器从 web 服务器请求服务时，可能会发生错误。

从而有可能会返回下面的一系列状态消息：

#### 1xx: 信息

| 消息:                   | 描述:                                                        |
| :---------------------- | :----------------------------------------------------------- |
| 100 Continue            | 服务器仅接收到部分请求，但是一旦服务器并没有拒绝该请求，客户端应该继续发送其余的请求。 |
| 101 Switching Protocols | 服务器转换协议：服务器将遵从客户的请求转换到另外一种协议。   |

#### 2xx: 成功

| 消息:                             | 描述:                                                        |
| :-------------------------------- | :----------------------------------------------------------- |
| 200 OK                            | 请求成功（其后是对GET和POST请求的应答文档。）                |
| 201 Created                       | 请求被创建完成，同时新的资源被创建。                         |
| 202 Accepted                      | 供处理的请求已被接受，但是处理未完成。                       |
| 203 Non-authoritative Information | 文档已经正常地返回，但一些应答头可能不正确，因为使用的是文档的拷贝。 |
| 204 No Content                    | 没有新文档。浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而Servlet可以确定用户文档足够新，这个状态代码是很有用的。 |
| 205 Reset Content                 | 没有新文档。但浏览器应该重置它所显示的内容。用来强制浏览器清除表单输入内容。 |
| 206 Partial Content               | 客户发送了一个带有Range头的GET请求，服务器完成了它。         |

#### 3xx: 重定向

| 消息:                  | 描述:                                                        |
| :--------------------- | :----------------------------------------------------------- |
| 300 Multiple Choices   | 多重选择。链接列表。用户可以选择某链接到达目的地。最多允许五个地址。 |
| 301 Moved Permanently  | 所请求的页面已经转移至新的url。                              |
| 302 Found              | 所请求的页面已经临时转移至新的url。                          |
| 303 See Other          | 所请求的页面可在别的url下被找到。                            |
| 304 Not Modified       | 未按预期修改文档。客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告诉客户，原来缓冲的文档还可以继续使用。 |
| 305 Use Proxy          | 客户请求的文档应该通过Location头所指明的代理服务器提取。     |
| 306 *Unused*           | 此代码被用于前一版本。目前已不再使用，但是代码依然被保留。   |
| 307 Temporary Redirect | 被请求的页面已经临时移至新的url。                            |

#### 4xx: 客户端错误

| 消息:                             | 描述:                                                        |
| :-------------------------------- | :----------------------------------------------------------- |
| 400 Bad Request                   | 服务器未能理解请求。                                         |
| 401 Unauthorized                  | 被请求的页面需要用户名和密码。                               |
| 402 Payment Required              | 此代码尚无法使用。                                           |
| 403 Forbidden                     | 对被请求页面的访问被禁止。                                   |
| 404 Not Found                     | 服务器无法找到被请求的页面。                                 |
| 405 Method Not Allowed            | 请求中指定的方法不被允许。                                   |
| 406 Not Acceptable                | 服务器生成的响应无法被客户端所接受。                         |
| 407 Proxy Authentication Required | 用户必须首先使用代理服务器进行验证，这样请求才会被处理。     |
| 408 Request Timeout               | 请求超出了服务器的等待时间。                                 |
| 409 Conflict                      | 由于冲突，请求无法被完成。                                   |
| 410 Gone                          | 被请求的页面不可用。                                         |
| 411 Length Required               | "Content-Length" 未被定义。如果无此内容，服务器不会接受请求。 |
| 412 Precondition Failed           | 请求中的前提条件被服务器评估为失败。                         |
| 413 Request Entity Too Large      | 由于所请求的实体的太大，服务器不会接受请求。                 |
| 414 Request-url Too Long          | 由于url太长，服务器不会接受请求。当post请求被转换为带有很长的查询信息的get请求时，就会发生这种情况。 |
| 415 Unsupported Media Type        | 由于媒介类型不被支持，服务器不会接受请求。                   |
| 416                               | 服务器不能满足客户在请求中指定的Range头。                    |
| 417 Expectation Failed            |                                                              |

#### 5xx: 服务器错误

| 消息:                          | 描述:                                              |
| :----------------------------- | :------------------------------------------------- |
| 500 Internal Server Error      | 请求未完成。服务器遇到不可预知的情况。             |
| 501 Not Implemented            | 请求未完成。服务器不支持所请求的功能。             |
| 502 Bad Gateway                | 请求未完成。服务器从上游服务器收到一个无效的响应。 |
| 503 Service Unavailable        | 请求未完成。服务器临时过载或当机。                 |
| 504 Gateway Timeout            | 网关超时。                                         |
| 505 HTTP Version Not Supported | 服务器不支持请求中指明的HTTP协议版本。             |