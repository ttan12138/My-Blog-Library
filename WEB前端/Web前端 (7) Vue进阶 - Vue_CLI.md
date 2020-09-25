# 1. Vue模块化

## 1.1 为什么要模块化

假设开发过程中有如下两个，由不同开发者开发的业务逻辑：

```js
// modul1.js
var name='小明'
var flag = true

function sum(num1, num2) {
  return num1 + num2
}

// modul2.js
var name = "小红"
var flag = false
```

如果分别基于上述两个模块再次进行开发，将不会出现任何问题。

```js
// modul3.js
if (flag) {
  console.log(sum(10, 20))
}
```

```html
<!--在index.html页面导入modul1.js和modul3.js-->
<script src="modul1.js" ></script>
<script src="modul3.js" ></script>
<!-- 打印结果： 30 -->
```

**但！是！ 当同时使用两个模块再开发时，将会出现不希望的问题：**

```html
<!-- 在index.html页面导入这些js文件 -->
<script src="modul1.js" ></script>
<script src="modul2.js" ></script>
<script src="modul3.js" ></script>
<!-- 不再打印 -->
```

此时，由于modul1和modul2中都有flag，导致modul3.js模块的逻辑变得不再清晰。这就是一种全局变量同名问题。

## 1.2 全局变量模块化

```javascript
// modul1.js
//模块对象
var moduleA = (function (param) {
  //导出对象
  var obj = {}
  var name = '小明'
  var flag = true

  function sum(num1, num2) {
    return num1 + num2
  }
  
  return obj
})()
```

```javascript
// modul3.js
//使用全局变量moduleA
if(moduleA.flag){
  console.log(moduleA.sum(10, 20))
}
```

如上操作最终打印结果并且为“30”，这是一种很好的解决全局变量同名问题的思路。

## 1.3 CommonJS的模块化

以下实现一种被广泛使用的js模块化规范：CommonJS，其核心思想是通过require方法来同步加载依赖的其他模块，通过module.exports导出需要暴露的接口。（需要nodeJS的依支持）

```javascript
// modul1.js
var name = '小明'
var flag = true

function sum(num1, num2) {
  return num1 + num2
}

// 导出对象 
module.exports = {
  flag : flag,  // ES6提供“对象键名与键值相同时的缩写”，即 flag,
  sum : sum     // 同理可缩写为 sum
}
```

```javascript
// 导入对象,nodejs语法,从modul1.js取出对象
var {flag,sum} = require("./modul1.js")

if(flag){
  console.log(sum(10, 20))
}
```

## 1.4 ES6的模块化

```html
  <script src="modul1.js" type="module"></script>
  <script src="modul2.js" type="module"></script>
  <script src="modul3.js" type="module"></script>
```

要实现模块化，首先在html中使用`type='module'`属性，此时各模块有明确的作用域。

### 1.4.1 导出

```javascript
// 单独导出
// 导出变量
export let name = '小明'
// 导出函数
export function show(value) {
  console.log(value);
}
// 导出类
export class Person{
  run(){
    console.log("run");
  }
}

// 统一导出/导出对象
var flag = true
var age = 22
function sum(num1, num2) {
  return num1 + num2
}

export {
  age, sum  
}
```

### 1.4.2 导入

> 使用`import {name,flag,sum} from './modul1.js'`导入多个需要的变量

```javascript
import {name,flag,sum,say,Person} from './modul1.js'

console.log(name)

if(flag){
  console.log(sum(10, 20))
}

say('hello')
const p = new Person();
p.run();

// 统一导入
// 在 modul3.js 中导入变量时候命名为 modulA ，如果要调用变量需要使用 modulA.变量
import * as modulA from './modul1.js'
console.log(modulA.flag);
console.log(modulA.name);
```

### 1.4.3 默认导出 export default

```js
// 导出
export default {
  flag,sum,age
}
```

```js
// 默认导入
import modulA from './modul1.js'
console.log(modulA.sum(10,110));
```

> 注意：默认导出会将所有需要导出的变量打包成一个对象，此时在`modul3.js`中导入变量时候命名为`modulA`，如果要调用变量需要使用`modulA.变量`。



# 2. 初识Webpack

## 2.1 初识webpack

### 2.1.1 什么是webpack

webpack是一个JavaScript应用的静态模块打包工具。它将一堆文件中的每个文件都作为一个模块，找出他们的依赖关系，将它们打包为可部署的静态资源。

![image-20200830153930515](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200830153930515.png)

从这句话中有两个要点，**模块**和**打包**需要关注。

> 模块化

webpack可以支持前端模块化的一些方案，例如AMD、CMD、CommonJS、ES6。可以处理模块之间的依赖关系。不仅仅是js文件可以模块化，图片、css、json文件等等都可以模块化。

> 打包

webpack可以将模块资源打包成一个或者多个包，并且在打包过程中可以处理资源，例如压缩图片，将scss转成css，ES6语法转成ES5语法，将TypeScript转成JavaScript等等操作。

### 2.1.2 webpack的安装

webpack依赖node环境。

**全局安装webpack**

```shell
// "-g" global
npm install webpack -g
// 指定版本安装
npm install webpack@3.6.0 -g
```

> 由于vue-cli2基于webpack3.6.0
> 如果要用vue-cli2的可以使用`npm install webpack@3.6.0 -g`

**局部安装**

```shell
npm install webpack --save-dev
```

- 此处指令在对应项目目录执行，就是局部安装
- 或者在package.json中定义scripts，其中包括了webpack命令，也是局部webpack

### 2.1.3 开始

新建一个文件夹，新建如下结构的目录：

#### **目录结构**

![image-20200829195935657](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829195935657.png)

如图所示在src(源码文件夹)、dist(最终发布的文件，都）。

**1.新建入口js文件`main.js`和`mathUtils.js`，`main.js`依赖`mathUtils.js`。**

```js
//1.新建mathUtils.js，用CommonJs规范导出
function add(num1,num2) {
  return num1+num2
}
function mul(num1,num2) {
  return num1*num2
}
module.exports = {
  add,mul
}

//2.新建入口js文件main.js 导入mathUtil.js文件，并调用
const {add,mul} = require("./mathUtils.js")

console.log(add(10,20))
console.log(mul(10,10))
```

**2.使用webpack命令打包js文件**

> - webpack3使用`webpack 需要打包文件目录 输出文件目录`
>
> - webpack4使用 `webpack 需要打包文件目录 -o 输出文件目录`

![image-20200829201210933](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829201210933.png)

如图显示打包成功，以及详细打包信息，查看dist文件夹下自动生成了一个`bundle.js`。

```javascript
// bundle.js
//2.新建入口js文件main.js 导入mathUtil.js文件，并调用
const {add,mul} = __webpack_require__(1)

console.log(add(10,20))
console.log(mul(10,10))

/***/ }),
/* 1 */
/***/ (function(module, exports) {

//1.新建mathUtils.js，用CommonJs规范导出
function add(num1,num2) {
  return num1+num2
}
function mul(num1,num2) {
  return num1*num2
}
module.exports = {
  add,mul
}

```

**3.新建一个index.html文件，导入bundle.js**

```html
<!-- 3.新建一个index.html文件 -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>webpack起步</title>
</head>
<body>
  <!-- 4.引用webpack打包后的js文件 -->
  <script src="./dist/bundle.js"></script>
</body>
</html>

<!-- 此时打印台输出
     30
     100
-->
```

**4.按照上面步骤，新建`info.js`和`main.js`测试ES6模块化打包**

```javascript
//info.js
export default {
  name: '小明',
  age: 24,
}
```

```javascript
//main.js
import info from './info.js'

console.log(info.name) 
console.log(info.age)
```

> 再次使用`webpack ./src/main.js ./dist/bundle.js`，重新打包
>
> 会得到期望输出：
>
> ​       小明     24

webpack可以帮我们打包js文件，只要指定入口文件（main.js）和输出的文件（bundle.js），不管是es6的模块化还是CommonJs的模块化，webpack都可以帮我们打包，还可以帮我们处理模块之间的依赖。

## 2.2 webpack的配置

### 2.2.1 基本配置

如果每次都用webpack命令自己写入口文件和出口文件会很麻烦，此时我们可以使用webpack的配置。

**1.在`webpack起步`目录下新建一个`webpack.config.js`**

```javascript
// webpack.config.js
// 1.导入node的path包获取绝对路径
const path = require('path')

// 2.配置webpack的入口和出口
module.exports = {
  entry: './src/main.js',// 入口文件
  output:{
    path: path.resolve(__dirname, 'dist'),
      // 动态获取打包后的文件路径__dirname,使用path.resolve拼接路径
    filename: 'bundle.js' // 打包后的文件名
  }
}
```

**2.在根目录执行`npm init`初始化node包，因为配置文件中用到了node的path包**

```shell
npm init 
```

初始化：(初始化时的配置选项可以直接回车使用默认)

![image-20200829203730369](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829203730369.png)

**3.使用webpack打包**

再次打包指令：`webkpack`

![image-20200829204007005](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829204007005.png)

这样入口和出口的配置已经配置完成了，只需要使用webpack命令就行了。

**4.使用自定义脚本（script）启动**

一般来是我们使用的是

```shell
npm run dev // 开发环境
npm run build // 生产环境
```

在package.json中的script中加上

```json
"build": "webpack"
```

![image-20200829204212857](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829204212857.png)

使用`npm run build`

```shell
npm run build
```

![image-20200829204251717](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829204251717.png)

### 2.2.2 全局安装和局部安装

webpack有全局安装和局部安装。

> 局部安装

**使用`npm run build`执行webpack会先从本地查找是否有webpack，如果没有会使用全局的。**

此时本地需要安装webapck

```shell
npm install webpack@3.6.0 --save-dev
```

package.json中自动加上开发时的依赖`devDependencies`

![image-20200829204508474](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829204508474.png)

再次使用`npm run build`，使用的是本地webpack版本。

## 2.3 webpack的loader

### 2.3.1 什么是loader

loader是webpack中一个非常核心的概念。

webpack可以将js、图片、css处理打包，但是对于webpack本身是不能处理css、图片、ES6转ES5等。

此时就需要webpack的扩展，使用对应的loader就可以。

![image-20200829205944833](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829205944833.png)

**loader使用**

> 步骤一：通过npm安装需要使用的loader

> 步骤二：通过webpack.config.js中的modules关键字下进行配置

大部分loader可以在webpack的官网找到对应的配置。

> 以下内容 只是简单介绍，具体请以[官网](https://webpack.js.org/guides/asset-management/#setup)的各loader配置为准

### 2.3.2 CSS文件处理	

**1.将除了入口文件（main.js）所有js文件放在js文件夹，新建一个css文件夹，新建一个normal.css文件**

```css
/* normal.css */
body{
  background-color: red;
}
```

**2.main.js导入依赖**

```javascript
// 依赖css文件
require('./css/normal.css')
```

此时如果直接进行打包`npm run build`。

![image-20200829205823870](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829205823870.png)

> 提示信息很清楚，打包到css文件时报错，提示我们可能需要一个loader来处理css文件。

**3.安装css-loader**

```shell
npm install --save-dev css-loader
```

**4.使用css-loader**

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,// 正则表达式匹配css文件
        // css-loader只负责css文件加载，不负责解析，要解析需要使用style-loader
        use: [{
          loader: 'css-loader'
        }]// 使用loader
      }
    ]
  }
}
```

> 执行`npm run build`，提示打包成功，但是背景色并没有变红色，是因为css-loader只负责加载css文件，不负责解析，如果要将样式解析到dom元素中需要使用style-loader。

**5.安装使用style-loader**

```shell
npm install --save-dev style-loader
```

```javascript
  module: {
    rules: [
      {
        test: /\.css$/,// 正则表达式匹配css文件
        // css-loader只负责css文件加载，不负责解析，要解析需要使用style-loader
        use: [{
          loader: 'style-loader'
        }, {
          loader: 'css-loader'
        }]// 使用loader
      }
    ]
  }
```

> webpack使用多个loader是从右往左解析的，所以需要将css-loader放在style-loader右边，先加载后解析。

此时样式成加载解析到DOM元素上。

### 2.3.3 less文件处理

**1.在css文件夹中新增一个less文件**

```less
/* special.less */
@fontSize:50px; // 定义变量字体大小
@fontColor:orange; // 定义变量字体颜色
body{
  font-size: @fontSize;
  color: @fontColor;
}
```

**2.main.js中导入less文件模块**

```javascript
// 依赖less文件
require('./css/special.less')
// 向页面写入一些内容
document.writeln("hello,小明z!")
```

**3.安装使用less-loader**

```shell
npm install --save-dev less-loader less
```

在`webpack.config.js`中使用less-loader

```javascript
  module: {
    rules: [
      {
        test: /\.less$/,// 正则表达式匹配css文件
        // css-loader只负责css文件加载，不负责解析，要解析需要使用style-loader
        use: [{
          loader: 'style-loader'
        }, {
          loader: 'css-loader'
        }, {
          loader: 'less-loader'// less文件loader
        }]// 使用loader
      }
    ]
  }
```

**4.执行npm run build**

less文件就可以生效了，字体是orange，大小为50px。

### 2.3.4 图片文件的处理

> 准备工作：准备两张图片，图片大小为一张8KB以下（实际大小为5KB，名称为small.jpg），一张大于8KB（实际大小为10KB，名称为big.jpg），新建一个img文件夹将两张图片放入。

**1.修改normal.css样式，先使用小图片作为背景**

```css
body{
  /* background-color: red; */
  background: url("../img/small.jpg");
}
```

此时，如果直接使用npm run build 直接打包会报错，因为css文件中引用了图片url，此时需要使用**url-loader**。

**2.安装使用url-loader处理图片**

  url-loader像 file loader 一样工作，但如果文件小于限制，可以返回 [data URL](https://tools.ietf.org/html/rfc2397) 。

```shell
npm install --save-dev url-loader
```

配置

```javascript
{
        test: /\.(png|jpg|gif)$/,//匹配png/jpg/gif格式图片
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192//图片小于8KB时候将图片转成base64字符串，大于8KB需要使用file-loader
            }
          }
        ]
      }
```

**3.打包**

使用npm run build打包后，打开index.html。

> 小于`limit`大小的图片地址被编译成base64格式的字符串。

  此时，修改css文件，使用big.jpg做背景。

```css
body{
  /* background-color: red; */
  /* background: url("../img/small.jpg"); */
  background: url("../img/big.jpg");
}
```

再次打包，报错，提示未找到file-loader模块。

![image-20200829210541133](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829210541133.png)

**4. 因为大于`limit`的图片需要`file-loader`来打包，安装使用file-loader处理图片**

```shell
npm install --save-dev file-loader
```

不需要配置，因为url-loader超过limit的图片会直接使用file-loader。

再次打包，没有报错，打包成功，但是图片未显示。

![image-20200829210757758](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829210757758.png)

> 1.当加载的图片大小小于limit，使用base64将图片编译成字符串
>
> 2.当加载的图片大小大于limit，使用file-loader模块直接将big.jpg直接打包到dist文件家，文件名会使用hash值防止重复。
>
> 3.此时由于文件路径不对所以导致没有加载到图片

![image-20200829211019163](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829211019163.png)

**5.如何使用file-loader，指定路径**

修改output属性

```javascript
  output:{
    path: path.resolve(__dirname, 'dist'),//动态获取打包后的文件路径,path.resolve拼接路径
    filename: 'bundle.js',//打包后的文件名
    publicPath: 'dist/'
  },
```

此时打包，图片就可以正常显示了。

> 注意：一般来说，index.html最终也会打包到dist文件夹下，所以，并不需要配置publicPath，如何打包index.html请看webpack处理.vue文件。

> file-loader打包后，使用hash值做文件名太长，此时可以使用options的一些配置。

```js
options: {
	limit: 8192,
    // 图片小于8KB时候将图片转成base64字符串，大于8KB需要使用file-loader
	name: 'img/[name].[hash:8].[ext]'
    // img表示文件父目录，[name]表示文件名,[hash:8]表示将hash截取8位[ext]表示后缀
}
```

再次打包

![image-20200829211031600](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829211031600.png)

### 2.3.5 ES6语法处理

webpack打包时候ES6语法没有打包成ES5语法，如果需要将ES6打包成ES5语法，那么就需要使用babel。直接使用babel对应的loader就可以了。

安装

```shell
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```

配置

```js
      {
        test: /\.js$/,
        // 排除node模块的js和bower的js
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            // 如果要使用@babel/preset-env这里需要在根目录新建一个babel的文件
            // presets: ['@babel/preset-env']
            // 这里直接使用指定
            presets: ['es2015']
          }
        }
      }
```

> 1.如果要使用@babel/preset-env这里需要在根目录新建一个babel的文件
>
> 2.exclude排除不需要打包的文件

## 2.4 webpack的vue

### 2.4.1 简单安装使用vue

如果需要使用vue，必须使用npm先安装vue。

```shell
npm install vue --save	
```

使用vue简单开发。

**1.在入口文件main.js导入已安装的vue，并在index.html声明要挂载的div。在main.js加入以下代码。**

```js
// 使用vue开发
import Vue from 'vue'

const app = new Vue({
  el: "#app",
  data: {
    message: "hello webpack and vue"
  }
})
```

修改index.html代码，添加

```html
  <div id="app">
    <h2>{{message}}</h2>
  </div>
```

**2.再次打包`npm run build`后打开index.html**

![image-20200829211159713](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829211159713.png)

发现message并没有正确显示，打开console发现vue报错。错误提示我们，正在使用`runtime-only`构建，不能将template模板编译。

> 1.`runtime-only`模式，代码中不可以有任何template，因为无法解析。
>
> 2.`runtime-complier`模式，代码中可以有template，因为complier可以用于编译template。

在webpack中配置，设置指定使用`runtime-complier`模式。

> webpack.config.js

```javascript
  resolve: {
    // alias:别名
    alias: {
        //指定vue使用vue.esm.js
      'vue$':'vue/dist/vue.esm.js'
    }
  }
```

**3.再次重新打包即可**

### 2.4.2 如何分步抽取实现vue模块

> 创建vue的template和el关系
>
> el表示挂载DOM的挂载点
>
> template里面的html将替换挂载点

一般我们使用vue会开发单页面富应用(single page application)，只有一个index.html，而且index.html都是简单结构。

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>webpack起步</title>
</head>
<body>
  <div id="app">
  </div>
  <script src="./dist/bundle.js"></script>
</body>
</html>
```

**1.第一次抽取，使用template替换`<div id="app"></div>`。**

```javascript
// 修改mian.js的vue相关代码
import Vue from 'vue'

new Vue({
  el: "#app",
  template:`
  <div>
    <h2>{{message}}</h2>
    <button @click='btnClick'>这是一个按钮</button>
    <h2>{{name}}</h2>
  </div>
  `,
  data: {
    message: "hello webpack and vue",
    name: '小明'
  },
  methods: {
    btnClick(){
      console.log("按钮被点击了")
    }
  },
})
```

使用template模板替换挂载的id为app的div元素，此时不需要修改html代码了，只需要写template。

再次打包，即可显示成功。

**2.第二次抽取，使用组件化思想替换template**

考虑第一次抽取，写在template中，main.js的vue代码太冗余。

```javascript
// 修改main.js的代码
// 1.定义一个组件
const App = {
  template: `
  <div>
    <h2>{{message}}</h2>
    <button @click='btnClick'>这是一个按钮</button>
    <h2>{{name}}</h2>
  </div>
  `,
  data() {
    return {
      message: "hello webpack and vue",
      name: '小明'
    }
  },
  methods: {
    btnClick(){
      console.log("按钮被点击了")
    }
  },
}
```

```javascript
// 修改main.js，vue实例中注册组件，并使用组件
new Vue({
  el: "#app",
  // 使用组件
  template: '<App/>',
  components: {
    // 注册局部组件
    App
  }
})
```

再次使用`npm run build`打包，打包成功，显示和使用template替换div一样。

**3.第三次抽取组件对象，封装到新的js文件，并使用模块化导入main.js**

![image-20200829211723157](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829211723157.png)

在package.json将vue-loader版本修改为13.0.0

```json
"vue-loader": "^13.0.0"
```

重新安装

```shell
npm install
```

再次打包，打包成功，就可以看见期望样式了。

**6.组件化开发**

我们使用app.vue分离了模板、行为、样式，但是不可能所有的模板和样式都在一个vue文件内，所以要用组件化。

在vue文件夹下新建一个Cpn.vue文件

```vue
<!-- Cpn.vue -->
<template>
  <div>
    <h2 class='title'>{{name}}</h2>
  </div>
</template>

<script type="text/ecmascript-6">
export default {
  name: "Cpn",
  data() {
      return {
        name: "组件名字是Cpn"
      };
    }
};
</script>

<style scoped>
.title {
  color: red;
}
</style>
```

```vue
<!-- 将Cpn.vue组件导入到App.vue -->
<template>
  <div>
    <h2 class='title'>{{message}}</h2>
    <button @click="btnClick">按钮</button>
    <h2>{{name}}</h2>
    <!-- 使用Cpn组件 -->
    <Cpn/>
  </div>
</template>

<script type="text/ecmascript-6">
// 导入Cpn组件
import Cpn from './Cpn.vue'
export default {
  name: "App",
  data() {
      return {
        message: "hello webpack",
        name: "小明"
      };
    },
    methods: {
      btnclick() {}
    },
    components: {
      Cpn // 注册Cpn组件
    }
};
</script>

<style scoped>
.title {
  color: green;
}
</style>
```

再次打包，打开index.html，cpn组件的内容显示。

> 如果你在使用ES6语法导入模块时候想要简写的时候，例如这样省略`.vue`后缀
>
> ```vue
> import Cpn from './Cpn'
> ```
>
> 可以在`webpack.config.js`中配置：
>
> ```js
> resolve: {
>     // 导入模块简写省略指定后缀
>     extensions: ['.js', '.css', '.vue'],
>     // alias:别名
>     alias: {
>       // 指定vue使用vue.esm.js
>       'vue$':'vue/dist/vue.esm.js'
>     }
>   }
> ```

## 2.5 webpack的plugin

plugin插件用于扩展webpack的功能的扩展，例如打包时候优化，文件压缩。

**loader和plugin的区别**

loader主要用于转化某些类型的模块，是一个转化器。

plugin主要是对webpack的本身的扩展，是一个扩展器。

**plugin的使用过程**

步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要在安装)

步骤二：在**webpack.config.js**中的plugins中配置插件。

### 2.5.1 添加版权的Plugin

BannerPlugin插件是属于webpack自带的插件可以添加版权信息。

自带的插件无需安装，直接配置。

先获取webpack的对象，在配置BannerPlugin插件。

```javascript
// 获取webpack
const webpack = require('webpack')
// 配置plugins
module.exports = {
    ...
    plugins:[
        new webpack.BannerPlugin('最终解释权归zz所有')
      ]
}
```

![image-20200829212129080](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212129080.png)

打包后，查看bundle.js，结果如图所示：

![image-20200829212141568](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212141568.png)

多了一行我们自定义的版权声明注释。

### 2.5.2 打包html的plugin

之前我们的index.html文件都是存放在根目录下的。

在正式发布项目的时候发布的是dist文件夹的内容，但是dist文件夹是没有index.html文件的，那么打包就没有意义了。

所以我们需要将index.html也打包到dist文件夹中，这就需要使用**`HtmlWebpackPlugin`**插件了。

> **`HtmlWebpackPlugin`**：
>
> 自动生成一个index.html文件（指定模板）
>
> 将打包的js文件，自动同script标签插入到body中

首先需要安装**`HtmlWebpackPlugin`**插件

```shell
npm install html-webpack-plugin --save-dev	
```

使用插件，修改webpack.config.js文件中的plugins部分

```javascript
// 获取htmlWebpackPlugin对象
const htmlWbepackPlugin = require('html-webpack-plugin')
// 配置plugins
module.exports = {
    ...
    plugins:[
        new webpack.BannerPlugin('最终解释权归zz所有'),
        new htmlWbepackPlugin({
          template: 'index.html'
        })
      ]
}
```

> 1.template表示根据哪个模板来生成index.html
>
> 2.需要删除output中添加的publicPath属性，否则插入的script标签的src可能有误

再次打包，打开dist文件夹，多了一个index.html

![image-20200829212206678](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212206678.png)

自动加入了script引入了bundle.js。

### 2.5.3压缩打包代码插件

uglifyjs-webpack-plugin是第三方插件，如果是vuecli2需要指定版本1.1.1。

安装：

```shell
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev
```

配置plugin

```javascript
// 获取uglifyjs-webpack-plugin对象
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
// 配置plugins
module.exports = {
    ...
    plugins:[
        new webpack.BannerPlugin('最终解释权归zz所有'),
        new htmlWbepackPlugin({
          template: 'index.html'
        }),
    	new uglifyjsWebpackPlugin()
      ]
}
```

打包过后，打开bundle.js，发现已经压缩了，此时版权声明被删除了。

![image-20200829212228594](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212228594.png)

> webpack高版本自带了压缩插件。

## 2.6 webpack搭建本地服务器

webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用了express框架，可以实现热启动。

> 准备工作复制05-webpack的plugin文件夹到同级目录，并改名为06-webpack搭建本地服务器。

不过这是一个单独的模块，在webpack中使用之前需要先安装：

```shell
npm install --save-dev webpack-dev-server@2.9.1
```

devServe也是webpack中一个选项，选项本省可以设置一些属性：

- contentBase：为哪个文件夹提供本地服务，默认是根文件夹，这里我们需要改成./dist
- port：端口号
- inline：页面实时刷新
- historyApiFallback：在SPA（单页面富应用）页面中，依赖HTML5的history模式

修改webpack.config.js的文件配置

```javascript
// 配置webpack的入口和出口
module.exports = {
 ...
  devServer: {
    contentBase: './dist',//服务的文件夹
    port: 4000,
    inline: true//是否实时刷新
  }

}

```

配置package.json的script：

```json
"dev": "webpack-dev-server --open"
```

> --open表示直接打开浏览器

启动服务器

```shell
npm run dev
```

启动成功，自动打开浏览器，发现在本地指定端口启动了，此时你修改src文件内容，**会热修改。**

![image-20200829212415884](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212415884.png)

> 1.服务器启动在内存中。
>
> 2.开发调试时候最好不要使用压缩js文件的插件，不易调试。

## 2.7 webpack的配置文件分离

`webpack.config.js`文件中有些是开发时候需要配置，有些事生产环境发布编译需要的配置，比如搭建本地服务器的devServer配置就是开发时配置，接下来我们分析如何分离配置文件。

在根目录下新建一个`build`的文件夹，新建配置文件。

```javascript
// base.config.js（公共的配置）
// 1.导入node的path包获取绝对路径，需要使用npm init初始化node包
const path = require('path')
// 获取webpack
const webpack = require('webpack')
// 获取htmlWebpackPlugin对象
const htmlWbepackPlugin = require('html-webpack-plugin')

// 2.配置webpack的入口和出口
module.exports = {
  entry: './src/main.js',// 入口文件
  output:{
    path: path.resolve(__dirname, 'dist'),// 动态获取打包后的文件路径,path.resolve拼接路径
    filename: 'bundle.js',//打包后的文件名
    // publicPath: 'dist/'
  },
  module: {
    rules: [
      {
        test: /\.css$/,// 正则表达式匹配css文件
        // css-loader只负责css文件加载，不负责解析，要解析需要使用style-loader
        use: [{
          loader: 'style-loader'
        }, {
          loader: 'css-loader'
        }]// 使用loader
      },
      {
        test: /\.less$/,// 正则表达式匹配css文件
        // css-loader只负责css文件加载，不负责解析，要解析需要使用style-loader
        use: [{
          loader: 'style-loader'
        }, {
          loader: 'css-loader'
        }, {
          loader: 'less-loader'// less文件loader
        }]// 使用loader
      },
      {
        test: /\.(png|jpg|gif)$/,// 匹配png/jpg/gif格式图片
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,// 图片小于8KB时候将图片转成base64字符串，大于8KB需要使用file-loader
              name: 'img/[name].[hash:8].[ext]'// img表示文件父目录，[name]表示文件名,[hash:8]表示将hash截取8位[ext]表示后缀
            }
          }
        ]
      },
      {
        test: /\.js$/,
        // 排除node模块的js和bower的js
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            // 如果要使用@babel/preset-env这里需要在根目录新建一个babel的文件
            // presets: ['@babel/preset-env']
            // 这里直接使用指定
            presets: ['es2015']
          }
        }
      },
      {
        test: /\.vue$/,// 正则匹配.vue文件
        use: {
          loader: 'vue-loader'
        }
      }
    ]
  },
  resolve: {
    // alias:别名
    alias: {
      // 指定vue使用vue.esm.js
      'vue$':'vue/dist/vue.esm.js'
    }
  },
  plugins:[
    new webpack.BannerPlugin('最终解释权归zz所有'),
    new htmlWbepackPlugin({
      template: 'index.html'
    })
  ]
}
```

```javascript
// dev.config.js（开发时候需要的配置）
module.exports = {
  devServer: {
    contentBase: './dist',//服务的文件夹
    port: 4000,
    inline: true//是否实时刷新
  }
}
```

```javascript
// prod.config.js（构建发布时候需要的配置）
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  plugins:[
    new uglifyjsWebpackPlugin()
  ]
}
```

此时我们将`webpack.config.js`文件分成了三个部分，公共部分、开发部分、构建发布的部分。

> 1.如果此时是dev环境，我们只需要使用`base.config.js`+`dev.config.js`的内容
>
> 2.如果此时是生产发布构建的环境，我们只需要使用`base.config.js`+`prod.config.js`的内容

要将两个文件内容合并需要使用`webpack-merge`插件，安装`webpack-merge`。

```shell
npm isntall webpack-merge --save-dev
```

合并内容都是将`base.config.js`的内容合并到dev或者prod的文件中，修改`dev.config.js`和`prod.config.js`文件。

> 修改dev.config.js

```javascript
//导入webpack-merge对象
const webpackMerge = require('webpack-merge')
//导入base.config.js
const baseConfig = require('./base.config')

//使用webpackMerge将baseConfig和dev.config的内容合并
module.exports = webpackMerge(baseConfig, {
  devServer: {
    contentBase: './dist',//服务的文件夹
    port: 4000,
    inline: true//是否实时刷新
  }

})

```

> 修改prod.config.js

```javascript
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
//导入webpack-merge对象
const webpackMerge = require('webpack-merge')
//导入base.config.js
const baseConfig = require('./base.config')

//使用webpackMerge将baseConfig和prod.config的内容合并
module.exports = webpackMerge(baseConfig, {
  plugins:[
    new uglifyjsWebpackPlugin()
  ]
})
```

此时我们使用三个文件构成了配置文件，此时在不同环境使用不同的配置文件，但是webpack不知道我们新配置文件，此时我们需要在package.json中的script指定要使用的配置文件。

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config ./build/prod.config.js",
    "dev": "webpack-dev-server --open --config ./build/dev.config.js"
  }
```

此时使用`npm run build`打包文件，dist文件并不在根目录下，因为我们在`base.config.js`中配置的出口文件使用的是当前文件的路径，即打包的根路径是配置文件的当前路径，也就是build文件夹。

![image-20200830145208786](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200830145208786.png)

```javascript
  entry: './src/main.js',//入口文件
  output:{
    path: path.resolve(__dirname, 'dist'),//动态获取打包后的文件路径,path.resolve拼接路径
    filename: 'bundle.js',//打包后的文件名
    // publicPath: 'dist/'
  }
```

> 注意：__dirname是当前文件路径，path.resolve拼接路径，所以在当前路径下创建了一个dist文件夹。

此时修改output属性：

```javascript
output:{
    path: path.resolve(__dirname, '../dist'),//动态获取打包后的文件路径,path.resolve拼接路径
    filename: 'bundle.js',//打包后的文件名
    // publicPath: 'dist/'
  }
```

> 使用`../dist`，在当前目录的上级目录创建dist文件夹

# 3. Vue-CLI

## 3.1 vue-cli起步

### 3.1.1 什么是vue-cli

Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统，提供：

- 通过 `@vue/cli` 搭建交互式的项目脚手架。
- 通过 `@vue/cli` + `@vue/cli-service-global` 快速开始零配置原型开发。
- 一个运行时依赖 (`@vue/cli-service`)，该依赖：
  - 可升级；
  - 基于 webpack 构建，并带有合理的默认配置；
  - 可以通过项目内的配置文件进行配置；
  - 可以通过插件进行扩展。
- 一个丰富的官方插件集合，集成了前端生态中最好的工具。
- 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject。

### 3.1.2 **CLI是什么意思？**

- CLI是Command-Line Interface，即命令行界面，也叫脚手架。
- vue cli 是vue.js官方发布的一个vue.js项目的脚手架
- 使用vue-cli可以快速搭建vue开发环境和对应的webpack配置

### 3.1.3 vue cli使用

**vue cli使用前提node**

vue cli依赖nodejs环境，vue cli就是使用了webpack的模板。

安装vue脚手架，现在脚手架版本是vue cli4需要`node.js`v8.9版本以上

```shell
npm install -g @vue/cli
```

安装完成后使用命令查看版本是否正确：

```bash
vue --version
```

> 注意：安装cli失败

1. 以管理员使用cmd
2. 清空npm-cache缓存

```bash
npm clean cache -force
```

**拉取2.x模板（旧版本）**

 Vue CLI >= 3 和旧版使用了相同的 `vue` 命令，所以 Vue CLI 2 (`vue-cli`) 被覆盖了。如果你仍然需要使用旧版本的 `vue init` 功能，你可以全局安装一个桥接工具： 

```bash
npm install -g @vue/cli-init
# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同
vue init webpack my-project
```

**1.在根目录新建一个文件夹`vue-cli`，cd到此目录，新建一个vue-cli2的工程。**

```bash
cd vue-cli
// 全局安装桥接工具
npm install -g @vue/cli-init
// 新建一个vue-cli2项目
vue init webpack vue-cli2-test
```

> 注意：如果是创建vue-cli3的项目使用：

```bash
vue create vue-cli3-test
```

2.创建工程选项含义

![image-20200830151448428](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200830151448428.png)

- project name：项目名字（默认）
- project description：项目描述
- author：作者（会默认拉去git的配置）
- vue build：vue构建时候使用的模式
  - runtime+compiler：大多数人使用的，可以编译template模板
  - runtime-only：比compiler模式要少6kb，并且效率更高，直接使用render函数
- install vue-router：是否安装vue路由
- user eslint to lint your code：是否使用ES规范
- set up unit tests：是否使用unit测试
- setup e2e tests with nightwatch：是否使用end 2 end，点到点自动化测试
- Should we run `npm install` for you after the project has been created? (recommended)：使用npm还是yarn管理工具

等待创建工程成功。

> 注意：如果创建工程时候选择了使用ESLint规范，又不想使用了，需要在config文件夹下的index.js文件中找到useEslint，并改成false。

```javascript
    // Use Eslint Loader?
    // If true, your code will be linted during bundling and
    // linting errors and warnings will be shown in the console.
    useEslint: true,
```

## 3.2 vue-cli2的目录结构

创建完成后，目录如图所示：

![image-20200830151633177](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200830151633177.png)

其中build和config都是配置相关的文件。

### 3.2.1 build和config

![image-20200829212711270](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212711270.png)

如图所示，build中将webpack的配置文件做了分离：

- `webpack.base.conf.js`（公共配置）
- `webpack.dev.conf.js`（开发环境）
- `webpack.prod.conf.js`（生产环境）

我们使用的脚本命令配置在`package.json`中。

![image-20200829212721938](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212721938.png)

打包构建：

```bash
npm run build
```

如果搭建了本地服务器`webpack-dev-server`，本地开发环境：

```bash
npm run dev
```

此时`npm run build`打包命令相当于使用node 执行build文件夹下面的build.js文件。

> build.js

![image-20200829212738661](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212738661.png)

1. 检查dist文件夹是否已经存在，存在先删除
2. 如果没有err，就使用webpack的配置打包dist文件夹

在生产环境，即使用build打包时候，使用的是`webpack.prod.conf.js`配置文件。

![image-20200829212751961](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212751961.png)

源码中，显然使用了`webpack-merge`插件来合并prod配置文件和公共的配置文件，合并成一个配置文件并打包，而`webpack.dev.conf.js`也是如此操作，在开发环境使用的是dev的配置文件。

config文件夹中是build的配置文件中所需的一些变量、对象，在`webpack.base.conf.js`中引入了`index.js`。

```javascript
const config = require('../config')
```

### 3.2.2 src和static

src源码目录，就是我们需要写业务代码的地方。

static是放静态资源的地方，static文件夹下的资源会原封不动的打包复制到dist文件夹下。

### 3.2.3 其他相关文件

#### 3.2.3.1 .babelrc文件

.babelrc是ES代码相关转化配置。

```json
{
  "presets": [
    ["env", {
      "modules": false,
      "targets": {
        "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
      }
    }],
    "stage-2"
  ],
  "plugins": ["transform-vue-jsx", "transform-runtime"]
}
```

1. browsers表示需要适配的浏览器，份额大于1%，最后两个版本，不需要适配ie8及以下版本
2. babel需要的插件

#### 3.2.3.2  .editorconfig文件

.editorconfig是编码配置文件。也可作为组内代码规范。

```properties
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
```

一般是配置编码，代码缩进2空格，是否清除空格等。

#### 2.3.3 .eslintignore文件

.eslintignore文件忽略一些不规范的代码。

```
/build/
/config/
/dist/
/*.js
```

忽略build、config、dist文件夹和js文件。

#### 3.2.3.4 .gitignore文件

.gitignore是git忽略文件，git提交忽略的文件。

#### 3.2.3.5 .postcssrc.js文件

css转化是配置的一些。

#### 3.2.3.6 index.html文件

index.html文件是使用`html-webpack-plugin`插件打包的index.html模板。

#### 3.2.3.7 package.json和package-lock.json

1. package.json(包管理,记录大概安装的版本)
2. package-lock.json(记录真实安装版本)

## 3.3 runtime-compiler和runtime-only区别

新建两个vuecli2项目：

```bash
//新建一个以runtime-compiler模式
vue init webpack runtime-compiler
//新建一个以runtime-only模式
vue init webpack runtime-only
```

两个项目的main.js区别

> runtime-compiler

```javascript
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
})
```

> runtime-only

```javascript
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  render: h => h(App)
})
```

**compiler编译解析template过程**

![image-20200829212819516](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212819516.png)

`vm.options.template`解析成`ast(abstract syntax tree)`抽象语法树，抽象语法树编译成`vm.options.render(functions)`render函数。render函数最终将template解析的ast渲染成虚拟DOM（`virtual dom`），最终虚拟dom映射到ui上。

**runtime-compiler**
template会被解析 => ast(抽象语法树) => 然后编译成render函数 => 渲染成虚拟DOM（vdom）=> 真实dom(UI)
**runtime-only**
render => vdom => UI

相比起来，runtime-only的显著特点：1.性能更高，2.需要代码量更少

> render函数

```javascript
render:function(createElement){
  //1.createElement('标签',{标签属性},[''])
  return createElement('h2',
    {class:'box'},
    ['Hello World',createElement('button',['按钮'])])
  //2.传入组件对象
  //return createElement(cpn)
}
```

h就是一个传入的createElement函数，.vue文件的template是由vue-template-compiler解析。

将runtime-compiler的main.js修改

```javascript
new Vue({
  el: '#app',
  // components: { App },
  // template: '<App/>'
  //1.createElement('标签',{标签属性},[''])
  render(createElement){
    return createElement('h2',
    {class:'box'},
    ['hello vue', createElement('button',['按钮'])])
  }
})
```

并把config里面的inedx.js的`useEslint: true`改成false，即关掉eslint规范，打包项目`npm run dev`，打开浏览器。

![image-20200829212834629](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212834629.png)

再修改main.js

```javascript
new Vue({
  el: '#app',
  // components: { App },
  // template: '<App/>'
  //1.createElement('标签',{标签属性},[''])
  render(createElement){
    // return createElement('h2',
    // {class:'box'},
    // ['hello vue', createElement('button',['按钮'])])
    //2.传入组件
    return createElement(App)
  }
```

再次打包，发现App组件被渲染了。

![image-20200829212844866](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212844866.png)

## 3.4 vue-cli3/vue-cli4

### 3.4.1 vue-cli3起步

**vue-cli3与2版本区别**

- vue-cli3基于webpack4打造，vue-cli2是基于webpack3
- vue-cli3的设计原则是"0配置"，移除了配置文件，build和config等
- vue-cli3提供`vue ui`的命令，提供了可视化配置
- 移除了static文件夹，新增了public文件夹，并将index.html移入了public文件夹

**创建vue-cli3项目**

```bash
vue create vue-cli3-test
```

**目录结构：**

![image-20200830153045940](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200830153045940.png)

- public 类似 static文件夹，里面的资源会原封不动的打包
- src源码文件夹

使用`npm run serve`运行服务器，打开浏览器输入http://localhost:8080/

![image-20200830153403807](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200830153403807.png)

打开src下的main.js

```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```

`Vue.config.productionTip = false`构建信息是否显示

如果vue实例有el选项，vue内部会自动给你执行`$mount('#app')`，如果没有需要自己执行。

### 3.4.2 vue-cli3的配置

在创建vue-cli3项目的时候可以使用`vue ui`命令进入图形化界面创建项目，可以以可视化的方式创建项目，并配置项。

![image-20200830153503498](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200830153503498.png)

vue-cli3配置被隐藏起来了，可以在`node_modules`文件夹中找到`@vue`模块，打开其中的`cli-service`文件夹下的`webpack.config.js`文件。

![image-20200829212932035](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212932035.png)

再次打开当前目录下的`lib`文件夹，发现配置文件`service.js`，并导入了许多模块，来自与lib下面的config、util等模块

![image-20200829212956603](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829212956603.png)

**如何要自定义配置文件**

在项目根目录下新建一个`vue.config.js`配置文件，必须为`vue.config.js`，vue-cli3会自动扫描此文件，在此文件中修改配置文件。

```javascript
//在module.exports中修改配置
module.exports = {
  // 如下是增加文件路径 assets === src/assets
  configureWebpack: {
    resolve: {
      alias:{
        'assets': '@/assets',
        'common': '@/common',
        'components': '@/components',
        'network': '@/network',
        'views': '@/views',
      }
    }
  }
 // ...
}
```

### vue-cli4 

Vue CLI4 和 Vue CLI3区别不大。