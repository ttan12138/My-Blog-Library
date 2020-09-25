> Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

​	如上是官网对于Vue的介绍，初看理解并不会太深刻，所以这里不做赘述，会在之后文章中逐一学习。

>    因为想要对以前学习的Vue就行一次总结，并且查缺补漏，而且编写此总结时，vue3.x正处于beta阶段，所以以下的诸多内容为了学习方便，支持vue2.x版本。如有冲突或者需要其他版本内容，可以查看[官网](https://cn.vuejs.org/)

#### 初识Vue

##### 	Hello World

​	  首先，从运行一个“Hello World”开始。

```html
<!-- Step 2:  编写挂载点，锁定接下来Vue要操作的DOM对象。
	          vue的虚拟DOM会在这个对象上进行实际渲染。-->
<div id="app"></div>

<!-- 模板标签 -->
<template id="tp">
  <p>{{message}}</p>
</template>

<!--Step 1:  引用Vue -->
<!-- Vue CDN -> 提供Vue服务 -->
<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>
<!-- Vue-Router CDN -> 路由管理服务 -->
<script src="https://cdn.bootcss.com/vue-router/3.0.2/vue-router.js"></script>
<!-- Axios CDN -> axios服务 -->
<script src="https://cdn.bootcss.com/axios/0.18.0/axios.js"></script>

<script>
 //Step 3:  创建Vue实例对象，进行Vue操作   
new Vue({
  el: '#app',
  temlpate: '#tp',
  data: {
    message: 'Hello World',
  },
  computed: {
    isChange() {
      return this.message ? false : true 
    }  
  },
  methods: {
    showMessage() {
      return this.message
    }
  }
})
</script>
```

  以上代码就是简单的使用了vue显示“Hello World”。事实上，vue要执行的不止表面这么简单:

- **Step 1**:  引用Vue 

​		引用的Vue一般分为两个版本：开发版本/生产版本，晓名知意，开发版本支持调试和警告提示；生产版本则是正式上线时使用的（体积较小）。

> 常见的引用方式：[更多](https://vuejs.org/v2/guide/installation.html)
>
> ​	script标签引用： 准备好所需服务的vue库，如引用jQuery一样，可以使用本地库或者CDN库
>
> ​	npm安装引用：常在大型项目构建中使用，在后续vue-cli中会再做解释。安装指令：`npm install vue`

- **Step 2**:  编写挂载点，锁定接下来Vue要操作的DOM对象。
- **Step 3**:  创建Vue实例对象，进行相关操作。



**接下来进入正文，先来认识一下vue对象中都有哪些属性和操作吧**

#### Vue实例对象

##### 挂载Vue实例 -- el

​	格式：选择器/原生DOM绑定操作，

​	示例：`el: '#app',`

​				`el: '.app',`

​				`el: document.getElementById(app),`(此方式性能相对提高一点点)

​	作用：通过选择器将此Vue实例对象挂载到上面的DOM对象。

##### 数据属性 -- data

​	格式：对象，

​	示例：

```html
<div id="app">{{message}}</div>

<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>

<script>
new Vue({
  el: '#app',
  data: {
    message: 'Hello World',
    isChange: true,
    messageArray: []
  }
})
</script>
```

​	作用：包含此实例对象的所有数据，且这些数据都在vue的反应系统中，都能“反应”。

> [反应](https://vuejs.org/v2/guide/instance.html)：当data数据发生改变时，视图层（网页上设计此数据的内容）会发生相应改变。
>
> 注：可以使用Object.freeze()阻止反应，使反应系统不能对Object中的数据跟踪更改。

> **怎样使用data中数据？**
>
> - 在DOM文本节点中：使用双括号语法(大胡子语法)`{{表达式}}`,如{{ message }}
>
> - 在实例对象中：使用`this.`指向此实例对象(关于[this指向]()),如this.message
>
> - 在标签内联属性中：需要使用v-bind(关于指令v-bind),如v-bind:text='message'
>
> **大胡子表达式里可以写什么？**
>
> - `{{message + '!'}}`  --> 'Hello World'
> - `{{isChange ?  ‘succeed’ :  ‘failed’ }}` 三元表达式   --> 'succeed'
> - `{{showMessage()}}`  运行函数   --> 'Hellow World'
> -  `{{message.replace(/a-z/g,'')}}` 也可以写正则表达式    --> 'H W'

##### 指令 -- [v- ]([https://cn.vuejs.org/v2/api/#%E6%8C%87%E4%BB%A4](https://cn.vuejs.org/v2/api/#指令))

|             指令              |         作用          |
| :---------------------------: | :-------------------: |
|           `v-text`            |    相当于innerText    |
|           `v-html`            |    相当于innerHtml    |
| `v-if`、`v-else-if`、`v-else` |       条件渲染        |
|           `v-show`            |       条件渲染        |
|           `v-bind`            |       动态绑定        |
|            `v-on`             |       事件处理        |
|           `v-model`           | 表单输入绑定/双向绑定 |
|            `v-for`            |       列表渲染        |
|                               |                       |

示例：

```html
<style>
  /* 颜色样式：红、绿、蓝 */
  .color-red {
    color: red;
  }

  .color-blue {
    color: blue;
  }

  .color-green {
    color: green;
  }
</style>

<div id="app"></div>


<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>
<script>
  new Vue({
  el: '#app',
  template: `
    <div>
      <p>v-text: 将后面内容以string，渲染到vTextOrHtml中</p><p v-text='vTextOrHtml'></p>
	  <br/>

      <p>v-html: 将后面内容解析，然后渲染到vTextOrHtml中</p><p v-html='vTextOrHtml'></p>
	  <br/>

      <p>v-if -> v-else-if -> v-else ：后接内容结果为真时才渲染；</p>
      <p v-if='vIf == 1'>Hello v-If</p>
      <p v-else-if='vIf == 2'>Hello v-else-if</p>
      <p v-else>Hello v-else</p>
	  <br/>

      <p>v-show：后接内容结果为假，则此DOM节点的display转为none</p><p v-show='isTrue'></p>
      <br/>

      <pre>
		v-bind:×××    【xxx：为DOM对象的属性】
		--语法糖(简写)为  :xxx 
		:xxx 可以使xxx的值中使用data中的数据，进而实现动态绑定
		如:class = '{color-red: isred}'  
      </pre>
      <input v-bind:value="vBind" :class = '{color-red: isRed}' type="text"/>
	  <input :class = 'colorIs' type="text"/>
      <input v-bind:other1="other1" :other2="other2" :other3=" 'other3' " value="Hello :属性值" type="text"/><br/>
      <br/>
	  
	  <pre>
		v-on:×××    【xxx：为DOM对象的事件】
		--语法糖(简写)为  @xxx 
		@xxx 相当于onxxx  
      </pre>
      <p :class='colorIs'>v-on:click </p>
	  <button v-on:click="changeValue">点击变色</button>
      <button @click="changeValue">点击变色</button>
      <br/>

      <p>v-model 演示</p>
      <input v-model="vModel" type="text" />
      <p>{{ vModel }}</p>
      <br/>

      <p>v-for 演示</p>
      <ul v-for="(item, index) in vFor" :class="item.classStyle">
      	<li>{{index+1}}. {{item.name}} - {{item.age}}</li>
      </ul>
    </div>
      `,
  data() {
  	return {
  	  // v-text 及 v-html 使用数据
  	  vTextOrHtml: '<span style="color: red">我是红的</p>',
  	  
  	  // v-if 使用数据
  	  vIf: 2,
  	  
  	  // v-show 使用数据
  	  isTrue: false,
  	  
  	  // v-bind 使用数据
  	  vBind: "Hello v-bind",
  	  
  	  // v-bind 通过动态绑定 class 修改样式
  	  isRed: true,
      colorIs: 'color-green',
      colorHas: ['color-green', 'color-red', 'color-blue'],
      colorNum: 0,
        
     // v-bind 的 :属性 的使用形式
  	  other1: 'other1',
  	  other2: 'other2',
        
  	  // v-model 使用数据
  	  vModel: 'Hello v-model',
        
  	  // v-for 使用数据
  	  vFor: [{
  	  	name: '张三', // 姓名
  	  	age: 22, // 年龄
  	  	classStyle: "color-red" // 样式
  	  },{
  	  	name: '李四',
  	  	age: 23,
  	  	classStyle: "color-blue"
  	  },{
  	  	name: '王五',
  	  	age: 24,
  	  	classStyle: "color-green"
  	  }]
  	}
  },
  methods:{
      changeValue() {
        let index =  parseInt(Math.random()*this.colorHas.length)
        this.colorIs = this.colorHas[index]
      }
  }
})
</script>
```

- **v-if与v-show的不同：**

  `v-if` 是“真实的”条件渲染，会在触发期间正确销或重新创建。`v-if`是**懒惰的**：如果条件在初始渲染时为false，它将不会执行任何操作，在条件首次变为true之前不会渲染条件块。

  `v-show`则是使用基于CSS的切换，无论初始条件如何，始终会渲染元素。

  简而言之，**`v-if`的切换成本较高，而`v-show`的初始渲染成本较高**。如果您需要经常切换某些内容，则首选`v-show`；如果条件不太可能在运行时更改，则首选`v-if`。

  

- **v-if与v-for不可同时使用：**

  v-for比v-if更高的优先级，如下你不会得到一个由isRed决定的循环渲染p，而是index个由isRed决定的p标签。

  ```html
  <p v-for="item in vFor" v-if="isRed">{{item}}</p>
  ```




- v-on的修饰符**

  - `.stop`  阻止事件继续传播，如`@click.stop="doThis"`，

  - `.capture` 在捕获的过程监听，如`@click.capture="doThis"`，

    > .capture先执行父级的函数，再执行子级的触发函数(一般用法),
    >
    > 在vue中，当我们添加了事件修饰符capture后，才会变成**捕获监听器**，当元素发生冒泡时，先触发带有该修饰符的元素。若有多个该修饰符，则由外而内触发。

  - `.prevent` 拦截默认事件，如`@submit.prevent="onSubmit"`，使得提交不再重载

  - `.passive` 不拦截默认事件

    > 【浏览器只有等内核线程执行到事件监听器对应的JavaScript代码时，才能知道内部是否会调用preventDefault函数来阻止事件的默认行为，所以浏览器本身是没有办法对这种场景进行优化的。这种场景下，用户的手势事件无法快速产生，会导致页面无法快速执行滑动逻辑，从而让用户感觉到页面卡顿。】
    >
    > ​    passive这个修饰符会执行默认方法。如上述，每次事件产生，浏览器都会去查询是否有preventDefault阻止该次事件的默认动作。passive可以告诉浏览器，我们没用preventDefault阻止默认动作，不用查询了。
    >
    > ​    这里一般用在滚动监听，@scoll，@touchmove 。因为滚动监听过程中，移动每个像素都会产生一次事件，每次都使用内核线程查询prevent会使滑动卡顿。而通过passive将内核线程查询跳过，可以大大提升滑动的流畅度。
    >
    > **注：passive和prevent冲突，不能同时绑定在一个监听器上。**

  - `.self` 只有目标Dom是绑定了self的Dom时才触发。 [...](https://www.jianshu.com/p/ce33e745fb68)

  - `.once` 只触发一次

  - `.native`  一般来说自定义组件都是不支持对原生事件监听的，当接native原生事件时，也可以监听。

  - [按键修饰符]([https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6](https://cn.vuejs.org/v2/guide/events.html#按键修饰符))

  - [系统修饰符]([https://cn.vuejs.org/v2/guide/events.html#%E7%B3%BB%E7%BB%9F%E4%BF%AE%E9%A5%B0%E9%94%AE](https://cn.vuejs.org/v2/guide/events.html#系统修饰键))

    

- **v-bind:is** 与 **is**

  > 根据HTML规范，<table>、<ul>、<ol>、<select>等元素只能包含特定元素，当模板标签在使用有限制性的元素，在渲染时就会出现bug。如下例所示：
  >
  > ```html
  > <div id="root">        
  >   <table>            
  >     <tbody>            
  >       <row></row>            
  >       <row></row>            
  >       <row></row>            
  >     </tbody>        
  >   </table>    
  > </div>    
  > <script>        
  >   Vue.component('row',{            
  >     template: '<tr><td>this is a row</td></tr>'        
  >   })        
  >   var vm=new Vue({            
  >     el: "#root"        
  >   })    
  > </script>
  > ```
  >
  > 渲染结果如下： 
  >
  > ![image-20200827154932437](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200827154932437.png)
  >
  >  渲染完成后，tr元素放在了table元素的外部。因为tbody元素内部只能放tr标签，上例在t<able>内部写了<row>标签就会引发bug。引入is属性后上例中DOM部分可以这样写
  >
  > ```
  >  <table>
  >    <tbody>            
  >      <tr is="row"></tr>            
  >      <tr is="row"></tr>            
  >      <tr is="row"></tr>        
  >    </tbody>        
  >  </table>
  > ```
  >
  > 这样，便可以正确的渲染： 
  >
  > ![image-20200827154948733](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200827154948733.png)

  `is='row'`会直接在component中找row组件，并替换。

  `v-bind:is = 'name'`当组件data中存在`name: "row"`时，会去找row并进行替换。



- **v-model修饰符**
  - `.lazy ` 使输入框的值在“change”时同步而非“input”时
  - `.number` 使输入转为数值类型
  - `.trim `  过滤用户输入的首尾空白字符



- **v-bind与v-model的区别：**

  v-bind：将 Vue 中的数据同步到页面，即该值大部分用于前端向浏览器传固定数据。v-bind 可以给任何属性赋值，是从 Vue 到页面的单向数据流，即 Vue -> html。

  v-model：双向数据绑定，前端向浏览器传数据，用户操作浏览器的更改前端可以察觉到。v-model 只能给具有 value 属性的元素进行双向数据绑定（必须使用的是有 value 属性的元素），即 Vue -> html -> Vue



- **v-for的参数**

  使用整数：重复对应次数 n in 10

  使用数组：item in items  || (item, index) in items ||或者of替代in

  使用对象：value in object || (value, name) in object || (value, name, index) in object



##### 模板属性 -- template

 	格式：选择器/ES6模板``, 

​	 示例：

```html
<div id="app"></div>

<!-- 模板标签 -->
<template id="tp">
  <p>{{message}}</p>
</template>

<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>

<script>
new Vue({
  el: '#app',
  temlpate: '#tp',
  /*
  temlpate: `<p>{{message}}</p>`,
  */
  data: {
    message: 'Hello World',
  },
})
</script>
```

​	 作用：可以理解为Vue的虚拟DOM对象，运行时会被渲染到挂载点。

​	 注：template有且只有一个根节点。

##### 计算属性 -- computed

​	 格式：对象，

​	 使用： 当作data使用

​	 示例：

```html
<div id="app">{{message}}</div>

<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>

<script>  
new Vue({
  el: '#app',
  data: {
    message: 'Hello World',
  },
  computed: {
    isChange() {
      return this.message ? false : true 
    }  
  },
})
</script>
```

​	 作用： 包含所有需要较复杂操作而得到的数据。

> 注意：如果基于的属性值不变化，则由于缓存不调函数的优化机制，此方法内的操作不会执行。
>
> ```html
> <input type='text' v-model='num1'>
> <input type='text' v-model='num2'>
> <input type='text' v-model='num3'>
> 
> <p>{{sum}}</p>
> 
> computed: {
>   sum(){
> 	let addSum = parseInt(this.num1) + parseInt(this.num2)
> 	let allSum = addSum + parseInt(this.num3)
>   	return allSum
>   }
> }
> ```
>
> 如上只有当所有num发生变化时，sum才是正确结果。

##### 方法属性 -- methods

​	 格式：对象，

​	 示例：

```html
<div id="app">{{message}}</div>

<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>

<script>  
new Vue({
  el: '#app',
  data: {
    message: 'Hello World',
  },
  methods: {   
    showMessage() { 
        return this.message
    }
  }
})
</script>
```

​	 作用： 包含所有需要较复杂操作而得到的数据。

##### 过滤器 -- filters

作用：多用于进行数据格式化

使用：可以用在：双花括号插值和 `v-bind` 表达式。以‘|’指示，被添加在 JavaScript 表达式的尾部。

如`{{money | addDot}}` 、`v-bind:value="money | addDot"`

格式：同methods，默认将前面链式的结果作为第一个参数，其他参数接着作为第二、三…参数传入。

示例：

```html

<div id="app">{{money | addDot | addComma}}</div>
<!-- 可串联 -->

<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>

<script> 
    
// 全局过滤器
Vue.filter('addComma', function(money) {
  return (money / 1000 + "," + money % 1000);
})
    
new Vue({
  el: '#app',
  data: {
    money: 10000,
  },
  // 局部过滤器
  filters： {
  	addDot(money) {
  	  if (!money) return ''
      return  (money / 1000 + ".000");
  	}
  }
})
</script>
```

##### 侦听属性 -- watch

作用：监听数据发生变化，然后进行相应操作。

格式：同methods, key应与监听的数据名相同，允许两个参数，第一个是改变的值，第二个是改变前的值。

示例：

```html
<div id="app">
	<input type="text" v-model="money">
</div>

<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>

<script>  
new Vue({
  el: '#app',
  data: {
    money: '',
  },
  watch: {        
    // key: data中属性的属性名   
    money(newVal, oldVal) { 
      console.log(newVal, oldVal);
    }
  }
})
</script>
```

>  **watch、computed 与 methods 对比**
>
>  - `methods` 一般是根据点击之类的事件来触发的，例如用户通过 `@click="方法"` 来进行数据的改变。
>
>  - `computed` 强调计算，是根据 `data` 中的数据变化，而进行的操作。即 `this.任意数据` 改变了，那么，`computed` 就会进行改变；而如果 `this.任意数据` 不变，那么 `computed` 就会执行它的缓存策略，不会更新。`computed` 对绑定的值有依赖，如果每次操作的值不变化，则不进行计算，具有缓存特性。例如 `c = a + b`，`b` 是外界传来不断变化的，因为你只要显示 `c`，所以使用 `computed`。
>  -  `watch` 属性强调自身值的变化前后的动作，会侦听前后变化的状态，无论操作的值是否变化，都会执行定义的函数体，所以会有 data(newVal, oldVal)。如果需要完成 `c = a + b`，那么你需要 `watch` 数据 `a` 与 `b` 的变化，在这两者变化的时候，在方法中执行 `c = a + b`。
>
>  `watch` 在处理异步操作或者开销较大的操作上有优势。
>
>  - 执行异步操作不能串行返回结果，使用 `watch`；
>  - 开销较大的操作，避免堵塞主线程，使用 `watch`；
>  - 简单且串行返回的，使用 `computed`。

##### 组件属性 -- components

```js
<div id="app">
    <button-counter></button-counter>
</div>

<script src="https://cdn.bootcss.com/vue/2.5.21/vue.js"></script>

<script>  
// 以下是一个组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})

new Vue({
  el: '#app',
  components: {
      'button-counte': button-counter,
  }
})
</script>
```

至此，大部分常用的属性、方法都已经搞定，关于组件实例的属性将会在后续组件化知识中一一说明，其他更多请见[官网](https://cn.vuejs.org/)。



#### Vue组件化

在前面的文章中，使用了template作为vue实例的虚拟dom。但是，随着项目的不断扩大，如果全部代码都写在一个 `template` 中，不仅看起来冗余复杂，实际修改处理也会变得很麻烦。vue提供了一种解决方式就是对其进行划分，例如将一个页面划分为 `header`、`content`、`footer` 三部分。这样，当我们想要修改的时候只需要像有了目录一样直接就能找到想要修改的位置。

##### 组件基础

组件是可复用的Vue实例，且带有一个名字(如上button-counter)，组件具有 `new Vue`相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

在学习组件知识前，首先应该知道以下组件与vue的不同：

- **组件名大小写**

  定义组件名推荐以下两种方式

  - 使用 kebab-case(短横线分隔命名)

    ```js
    Vue.component('my-component-name', { /* ... */ })
    ```

  - 使用pascalCase(首字母大写命名)

    ```js
    Vue.component('MyComponentName', { /* ... */ })
    ```

  尽管有上述两种命名方式可以选择，但是在DOM对象中使用时最好使用 kebab-case，如

  ```html
  <my-component-name></my-component-name>
  ```

- **data必须是一个函数**，以使每个实例可以维护一份被返回对象的独立的拷贝，如果不加这条规则会使实例的数据相互影响。

```
data: function () {
  return {
    count: 0
  }
}
```

- **单个根元素**

  组件的template的根元素应该有且只有一个。

  ```html
  <!-- 错误：every component must have a single root element -->
  <template>
    <h3>{{ title }}</h3>
    <div v-html="content"></div>
  </template>
  
  <!-- 正确 -->
  <template>
    <div>
      <h3>{{ title }}</h3>
      <div v-html="content"></div>
    </div>
  </template>
  ```

##### 组件注册

- 全局注册

  ```js
  Vue.component('my-component-name', {
    // ... 选项 ...
  })
  ```

  全局注册的组件可以用在任何新创建的Vue根实例模板中。

- 局部注册

  局部注册的组件只能在注册过该组件的实例上使用

  ```js
  // 引入或创建组件实例
  var ComponentA = { /* ... */ }
  
  new Vue({
    el: '#app',
    // 使用components在新创建的Vue实例中注册
    components: {
    	// 应用组件名: 组件实例
      'component-a': ComponentA,
    }
  })
  ```

  > 嵌套组件
  >
  > ```js
  > var ComponentA = { /* ... */ }
  > 
  > var ComponentB = {
  >   components: {
  >     'component-a': ComponentA
  >   },
  >   // ...
  > }
  > // 或者通过 Babel 和 webpack 使用 ES2015 模块
  > //components/ComponentA.vue
  > export default {
  >   name: 'ComponentA'
  >   // ...
  > }
  > 
  > //components/ComponentB.vue
  > import ComponentA from './ComponentA.vue'
  > 
  > export default {
  >   name: 'ComponentB'
  >   components: {
  >     ComponentA
  >   },
  >   // ...
  > }
  > ```

- 模块系统自动化全局注册

  当使用了 webpack (或在内部使用了 webpack 的 [Vue CLI 3+](https://github.com/vuejs/vue-cli))，那么就可以使用 `require.context` 全局自动注册一些通用的基础组件。

  ```js
  import Vue from 'vue'
  import upperFirst from 'lodash/upperFirst'
  import camelCase from 'lodash/camelCase'
  
  const requireComponent = require.context(
    // 其组件目录的相对路径
    './components',
    // 是否查询其子目录
    false,
    // 匹配基础组件文件名的正则表达式
    /Base[A-Z]\w+\.(vue|js)$/
  )
  
  requireComponent.keys().forEach(fileName => {
    // 获取组件配置
    const componentConfig = requireComponent(fileName)
  
    // 获取组件的 PascalCase 命名
    const componentName = upperFirst(
      camelCase(
        // 获取和目录深度无关的文件名
        fileName
          .split('/')
          .pop()
          .replace(/\.\w+$/, '')
      )
    )
  
    // 全局注册组件
    Vue.component(
      componentName,
      // 如果这个组件选项是通过 `export default` 导出的，
      // 那么就会优先使用 `.default`，
      // 否则回退到使用模块的根。
      componentConfig.default || componentConfig
    )
  })
  ```

##### prop

Prop 是你可以在组件上注册的一些自定义 attribute，当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property。

- prop的格式

  ```js
  /*prop大小写：
    HTML中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法)的prop名需要使用其等价的kebab-case(短横线分隔命名)命名
    如  <component-a is-published="hello!"></component-a>
  */
  var componentA={
    // 格式一 不指定类型 props: ['key1', 'key2', 'key3', ...]
    props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
    // 格式二 指定默认和类型
    props: {
    /* prop验证：
    	attrName:{
    	  // 类型验证
    	  type: String/Number/Boolean/Array/Object/Date/Function/Symbol
    	  // 如果attrName只规定类型，则可写成attrName：typeName
    	  // `null` 和 `undefined` 会通过任何类型验证
    	  // 多可能类型 [String, Number, ...]
    	  // 可以自定义函数验证
    	  
    	  //规定默认值
    	  default: 'a'/1/...
    	  // 设置Array/Object/Function类型的默认值返回一个函数
    	  default() {
    	    return [...]/{...}/()
    	  }
    	  
    	  //是否必填
    	  required: true
  
  	  //自定义验证
  	  validator: function (value) {
          // 这个值必须匹配下列字符串中的一个
          return ['success', 'warning', 'danger'].indexOf(value) !== -1
        }
    	}
    */
      title: String,
      likes: Number,
      isPublished: Boolean,
      commentIds: Array,
      author: Object,
      callback: Function,
      contactsPromise: Promise // or any other constructor
    }
  }
  ```

- 静态/动态传值

  ```html
   <!-- 静态传值 传入字符串  ‘hello!’-->
   <component-a is-published="hello!"></component-a>、
  
   <!-- 动态传值 传入表达式  17 -->
   <component-a :is-published="8+9"></component-a>
  
   <!-- 动态传值 传入 hello   类型由hello和is-published决定-->
   <!-- 父组件中 data(){ hello: [...]/{...}/'...'/...,  } -->
   <component-a :is-published="hello"></component-a>
  
    <!-- 传入一个prop -->
    <!-- post: {id: 1,title: 'My Journey with Vue'} -->
    <component-a v-bind="post"></component-a>
    <!-- 等价于 -->
    <blog-post v-bind:id="post.id" v-bind:title="post.title" ></blog-post>
  ```

- 使用prop的注意

  - **单向数据流**：所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。

    > 这样会防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解。
    >
    > 额外的，每次父级组件发生变更时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

  - 转为本地prop

    - **这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用**。

      ```js
      props: ['initialCounter'],
      data: function () {
        return {
          // 定义本地数据接收prop
          counter: this.initialCounter
        }
      }
      ```

    - **这个 prop 以一种原始的值传入且需要进行转换。**

      ```js
      props: ['size'],
      computed: {
        normalizedSize: function () {
          return this.size.trim().toLowerCase()
        }
      }
      ```

  - 对象和数组prop的注意

    JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变变更这个对象或数组本身**将会**影响到父组件的状态。

  - prop验证时间

    prop 会在一个组件实例创建**之前**进行验证，所以实例的 property (如 `data`、`computed` 等) 在 `default` 或 `validator` 函数中是不可用的。

  - 非Prop的Attribute

    一个非 prop 的 attribute 是指传向一个组件，但是该组件并没有相应 prop 定义的 attribute。**这些 attribute 会被添加到这个组件的根元素上**。

    - 绝大多数attribute，从外部提供给组件的值会替换掉组件内部设置好的值。
    - `class` 和 `style`attribute，两边的 attribute值会被合并起来

    > **禁用 Attribute 继承’和‘$attrs’**
    >
    > ```
    > Vue.component('my-component', {
    >   inheritAttrs: false,
    >   //决定是否继承来自父组件的属性(默认继承)，不会影响 style 和 class 的绑定
    >   // ...
    > })
    > ```
    >
    > ##### vm.$attrs
    >
    > - 类型：{ [key: string]: string }
    >
    > - 作用：包含了父作用域中不作为 prop 被识别 (且获取) 的特性绑定 (`class` 和 `style` 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (`class`和 `style` 除外)。
    >
    > - 即父组件传递下来的，但是在你调用的那组件（this）里没注册使用的，需要继续传递下去的所有
    >
    >   ```html
    >   <son :message="message" :title="title" :age="age" :name="name"></son>
    >   <!-- 子组件接收四个数据，只用到title，剩下的传给孙组件 -->
    >   <grandson :message="message" :age="age" :name="name"></grandson>
    >   
    >   <!-- 使用 $attrs -->
    >   <grandson v-bind="this.$attrs"></grandson>
    >   
    >   ```

##### 自定义事件

- $emit

  作用：使子组件与父级组件进行沟通。

  格式：

  ```html
  // 子组件发送事件
  (this.)$emit('事件名')
  (this.)$emit('事件名', 参数)
  //注意：由于v-on会使Dom模板都转换成小写，有时会影响到v-on监听事件，所以应始终使用 kebab-case 的事件名。
  
  //父组件接收事件及内容，并作反应
  <component-a @事件名='反应'> </component-a>
  ```

  示例

  ```js
  <div id="app">
    <component-a @change-big='changeBig' @change-small='changeSmall' :text="text"></component-a>
  </div>
  
  <script>
   var componentA = {
    template: `
      <div>
        <button v-on:click='itemClick'>点我变大</button>
        <button v-on:click='itemClick2'>点我变小</button>
        <p>{{text}}</p>
      </div>
    `,
    props: ['text'],
    data(){
      return {
        smal: '小'
      }
    },
    methods: {
      itemClick(){
        this.$emit('change-big')
      },
      itemClick2(){
        this.$emit('change-small', this.smal)
      }
    }
  }      
  new Vue({
    el: '#app',
    components:{
      'component-a': componentA
    },
    data: {
      text: '中',
    },
    methods: {
      changeBig(){
        this.text = '大'
      },
      changeSmall(value){
        this.text = value
      }
    }
  })
  </script>
  ```

- 组件上使用v-model

  ```html
  <!-- 使用v-model时 -->
  <input v-model="searchText">
  <!-- 等价于 -->
  <input :value="searchText" @input="searchText = $event.target.value">
  
  <!-- 用在组件上 -->
  <!-- 子组件 -->
  <template>
    <input :value="value" @input="$emit('input', $event.target.value)">
  </template>
  <!-- 父组件
  	props: ['value'],
  -->
  <component-input v-model="searchText">
      
  <!>
  ```

  - **【vue 2.2.2+**】

  对于单选框、复选框等类型的输入控件可能会将 `value` attribute 用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。`model` 选项可以用来避免这样的冲突：

  ```js
  <base-checkbox v-model="lovingVue"></base-checkbox>
  
  Vue.component('base-checkbox', {
    model: {
      prop: 'checked',
      event: 'change'
    },
    props: {
      checked: Boolean
    },
    template: `
      <input
        type="checkbox"
        v-bind:checked="checked"
        v-on:change="$emit('change', $event.target.checked)"
      >
    `
  })
  ```

- 原生事件绑定到组件

  一般来说，`.native`或者子组件通过$emit线上发送事件都可以满足触发组件原生事件的需求，但是当触发的组件如`<input>`时，可能会发生一些意料之外的错误。

  ```html
  <!-- 有如下一样的一个重构的组件 base-input-->
  <label>
    {{ label }}
    <input
      v-bind="$attrs"
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  </label>
  <!-- 监听焦点onFocus时,不会有任何反应-->
  ```

  - **$listeners**

    作用：将事件的监听指向某个特定的元素

    如下示例中，子组件根元素是label不能实现对 focus的监听，使用$listeners可以实现将事件监听指向label->input 从而实现监听focus

    ```js
    <!-- 完全透明的包裹器 -->
    <child v-on:event-one="methodOne" v-on:focus="methodtwo" v-model="childText">
    
    Vue.component('child', {
      created(){
        console.log(this.$listeners)
        //{ 'event-one': f(), 'focus': f() }
      }
      computed: {
        inputListeners() {
          var vm = this
          // `Object.assign` 将所有的对象合并为一个新对象
          return Object.assign({},
            this.$listeners,  // 我们从父级添加所有的监听器
            // 然后我们添加自定义监听器，或覆写一些监听器的行为
            {
              // 这里确保组件配合 `v-model` 的工作
              input(event) {
                vm.$emit('input', event.target.value)
              }
            }
          )
        }
      },
      template: `
        <label>
          <input
            v-bind="$attrs"
            v-bind:value="value"
            v-on="inputListeners">
        </label>
      `
    })
    ```

- .sync【Vue 2.3.0+】

  对于双向绑定的prop，往往没有明显的数据来源标识，所以官方提供以下方式。

  > `update:myPropName`：
  >
  > ```js
  > this.$emit('update:myPropName', newTitle)
  > 
  > <text-document
  >   v-bind:title="doc.title" 
  >   v-on:update:title="doc.title = $event"
  > ></text-document>
  > <!-- 注： tilte是子组件prop -->
  > ```

为了方便起见，新增了`.sync` 修饰符的缩写：

```html
this.$emit('update:myPropName', newTitle)

<text-document v-bind:title.sync="doc.title"></text-document>
<!-- 带有.sync的v-bind 不支持后接表达式 -->

<!-- 用一个对象设置多个prop时，会把 doc 对象中的每一个 property (如 title) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 v-on 监听器。 -->
<text-document v-bind:title.sync="doc"></text-document>
<!-- 直接传入一个字面量对象并不被支持 -->
<text-document v-bind:title.sync="{title: doc.title}"></text-document>
```

##### 插槽 -- slot

作用：Vue提供的一套内容分发的API，`<slot>`作为承受分发内容的出口。

​            简而言之，就是传递DOM。

示例：

```js
// 简单插槽
// 父组件
<div id='app'>
  <son>
    <div>替换slot，我才显示</div>  
  </son>  
</div>

var son = {
  template:`
    <div>
      <slot>这里的内容不显示</slot>    
    </div>
  `,
  //....
}
```

如上就是一个简单的slot，父组件中使用了son组件，组件`<son></son>`中的内容会**传递到son**并**替换**`<slot></slot>`，显示 ‘替换slot，我才显示’ ，而因为显示了上述内容， ‘这里的内容不显示’ 将并不会显示。 

- 编译作用域

  如上示例中在父组件中使用的`<son></son>`的作用域是他的父组件`#app`的作用域。即son内的内容会先在父组件中编译好，再进行传递，传入son内slot再进行替换。

  ```html
  <!-- 以下是错误的，url是 son内数据 -->
  <son url="app">{{url}}</son>  
  <!-- 若父组件data中存在name属性，则正确 -->
  <son>{{name}}</son>
  ```

  **父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

- 后备内容

  如上 ‘这里的内容不显示’ 就是后备内容，当父组件使用son且son内不传入对应slot的内容时，后备内容就会显示。

  简而言之，就是为slot设置默认值。

  ```js
  <div id='app'>
    <son></son>  
  </div>
  
  var son = {
    template:`
      <div>
        <slot>这里的内容不显示</slot>    
      </div>
    `,
    //....
  }
  //此时在页面的内容是： 这里的内容不显示
  ```

- 具名插槽

  Q：当一个子组件有多个slot时，怎样进行替换呢？

  A：给slot加上一个特殊的attribute：name --- 具名插槽

  ```js
  // 具名插槽
  // 父组件
  <div id='app'>
    <son>
      <div v-slot='a'>替换a</div>  
      <div v-slot:b>替换b</div>
    </son>  
  </div>
  
  var son = {
    template:`
      <div>
        <slot name='a'>a</slot> 
  	  <slot name='b'>b</slot>
  	  <slot>c</slot>
      </div>
    `,
    //....
  }
  //此时页面的内容是： a b
  ```

  不具名的插槽会带一个隐含的名字default，父组件中没有指明的所有内容都会被作为default的内容。

  ```js
  // 不具名插槽与具名插槽
  // 父组件
  <div id='app'>
    <son>
      <div v-slot='a'>替换a</div>
      内容一
      <div v-slot:b>替换b</div>
      内容二
    </son>  
  </div>
  
  var son = {
    template:`
      <div>
        <slot name='a'>a</slot> 
        <slot>c</slot>
  	  <slot name='b'>b</slot>
  	  <slot>d</slot>
  	  <slot>e</slot>
      </div>
    `,
    //....
  }
  //此时页面的内容是： a 内容一 内容二 b 内容一 内容二 内容一 内容二
  ```

- 作用域插槽

  Q：如何在父组件中应用son时，能使用son内的数据？

  A：在slot上绑定一个attribute --- 作用域插槽

  ```js
  // 作用域插槽
  // 父组件
  <div id='app'>
    <son>
      <!-- 前文具名插槽说过，每个不具名的slot都有一个default的name -->
      <div v-slot:default='user2'>
        {{user2.user1.name}}    
  	</div>
    </son>  
  </div>
  
  var son = {
    template:`
      <div>
        <slot v-bind:user1="user"></slot> 
      </div>
    `,
    //....
  }
  // slot上的attribute被称为插槽prop。
  // user2是一个包含所有插槽prop的对象
  // 此时页面的内容是： 名字
  ```

  - 独占默认插槽的缩写语法

    - 当子组件中只有默认插槽时，子组件标签能当作插槽的template使用。

      ```html
      <son v-slot:default="user">
        {{ user2.user1.name }}
      </son>
      ```

    - v-slot不带参数意为指向不具名插槽，**当有具名插槽时，如此缩写可能会出现问题**。

      ```html
      <son v-slot="user2">
        {{ user2.user1.name }}
      </son>
      ```

  - 解构插槽

    作用域插槽的原理是将插槽内容传入到一个函数`function(user2){...}`,这代表v-slot的值可以是任何能作为函数参数的js表达式。

    - 当有多个插槽prop时，可以使用ES2015解构实现传入具体的插槽prop。

    ```html
    <son v-slot="{ user1 }">
      {{ user1.name }}
    </son>
    ```

    - prop 重命名

    ```html
    <son v-slot="{ user1: person }">
      {{ person.name }}
    </son>
    ```

    - prop是undefined时，定义默认prop

    ```
    <son v-slot="{ user1: {name: '名字'} }">
      {{ user1.name }}
    </son>
    ```

- 动态插槽名 【Vue 2.6.0+】

  使用动态指令指明插槽

  ```html
  <son v-slot:[name1]>
    {{ person.name }}
  </son>
  ```

- 具名插槽缩写 #  【Vue 2.6.0+】

  跟 `v-on` 和 `v-bind` 一样，`v-slot` 也有缩写，即把参数之前的所有内容 (`v-slot:`) 替换为字符 `#`。例如 `v-slot:header` 可以被重写为 `#header`。

  注：`v-slot:default`不能只缩写为`#`，可以缩写为`#default`，**没有参数的v-slot是不能所写的**。

##### 动态组件 & 异步组件

- 动态组件保持keep-alive

  `<keep-alive>`是Vue内置的用来保持不活跃组件实例的组件。`<keep-alive>`是抽象的组件(不会渲染Dom节点)。

  用法：

  - 使用`<keep-alive></keep-alive>`需要保持状态的组件包裹。被包裹的组件会被缓存，再次进行组件间切换的动作时，该组件将不会被销毁。

  - 同时，该组件及组件内所有嵌套组件会新增`activated`和`deactived`两个生命周期钩子函数。组件刚开始活跃会触发`activated()`，离开活跃状态会触发`deactived()`。

  - 提供三个可选props:

    - `include` - 逗号分隔字符串、正则表达式或一个数组。只有名称匹配的组件会被缓存。

    - `exclude` - 逗号分隔字符串、正则表达式或一个数组。任何名称匹配的组件都不会被缓存。

    - `max` - 数字。最多可以缓存多少组件实例。

  ```html
  <!-- 基本 -->
  <keep-alive>
    <component :is="view"></component>
    <router-view/>
    <!-- router-view后续内容vue-router会再次详解 -->
  </keep-alive>
  
  <!-- 多个条件判断的子组件 -->
  <keep-alive>
    <!-- 同时只能有一个子元素被渲染  v-for不允许在此 -->
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>
  
  <!-- Props -->
  <!--匹配首先检查组件自身的 name 选项，如果 name 选项不可用，则匹配它的局部注册名称 (父组件 components 选项的键值)。匿名组件不能被匹配。-->
  <!-- <keep-alive include="a,b"> -->
  <!-- <keep-alive :include="/a|b/"> -->
  <keep-alive :include="['a', 'b']">
    <component :is="view"></component>
  </keep-alive>
  
  <!--max 2.5.0+ -->
  <keep-alive :max="10">
    <component :is="view"></component>
  </keep-alive>
  ```

- 异步组件加载

  Vue允许以工厂函数的方式定义组件，这个工厂函数会异步解析组件的定义。Vue只会在这个组件需要渲染的时候才会触发这个工厂函数，且把结果缓存起来供未来重新渲染。
  
  ```js
  // 工厂函数定义组件
  Vue.component('async-son', function(resolve, reject){
    // 当组件被调用时，执行resolve()
      resolve({
      template: `<div>async-son</div>`
    })
    // 当组件被调用失败时，执行reject(reson)
      reject(reson)
  })
  
  // 官方推荐与webpack的code-splitting使用
  Vue.component('async', function(resolve){
    //require 将构建代码切割为多个包，再通过Ajax请求加载
    require(['./my-async-son'],resolve)
  })
  
  // Webpack 2 + ES2015
  // 全局
  Vue.component(
    'async-son',
    // 动态导入，返回Promise对象
    () => import('./my-async-son')
  )
  //局部
  components:{
    'async-son': () => import('./my-async-son')    
  }
  ```
  
  - 作用：使用异步组件加载，组件只有在使用时才会加载。
  
    ```js
    // 解决了组件之间循环引用的问题
    // A.vue
      <div><b></b></div>
    // B.vue
      <div><a></a></div>
    // 上述示例中，互相为父子代，当使用模块系统依赖/导入组件时，会遇到如下错误
    // “Failed to mount component: template or render function not defined.”
    // 解决方式： 以一个组件为点，异步加载另一个组件。
    ```
  
  - 处理加载状态 【Vue 2.3.0+】
  
    ```
    // 组件定义时，工厂函数也可以返回一个如下格式的对象：
    const AsyncComponent = () => ({
      // 需要加载的组件 (应该是一个 `Promise` 对象)
      component: import('./MyComponent.vue'),
      // 异步组件加载时使用的组件
      loading: LoadingComponent,
      // 加载失败时使用的组件
      error: ErrorComponent,
      // 展示加载时组件的延时时间。默认值是 200 (毫秒)
      delay: 200,
      // 如果提供了超时时间且组件加载也超时了，
      // 则使用加载失败时使用的组件。默认值是：`Infinity`
      timeout: 3000
    })
    ```
  
  
##### 边界

- 访问元素&组件

  - **$root**   访问根实例

    ```js
    new Vue({  // 注释为其所有子组件对应的访问方式
      data: {
        foo: 1  // foo:  读：this.$root.foo  写：this.$root.foo = 2
      },
      computed: {
        bar(){ /* ...*/ }   // bar():  访问：this.$root.bar
      },
      methods: {
        baz() { /*....*/ }  // baz():  调用：this.$root.baz()
      }
    })
    ```
  
  - **$parent**  访问父级组件实例
  
    ```js
    // 能够替代prop方式将数据传入子组件
  // 假设父组件数据同上Vue实例
    // 子组件访问方式类似于$root, 将$root替换为$parent即可
  
    // 注: 尽量不要使用$parent.$parent去找祖代元素
    ```
  
  - **$children**  访问子级组件实例
  
    ```js
    // 同$parent
    ```
  
  - **$refs**  访问子组件实例或子元素
  
    ```js
    // 当需要在js中直接访问一个子组件/子元素时，可以使用$ref
    // step1： 在子组件/子元素上 添加" ref='xxx' "
      <component-a ref="componentA"></component-a>
      
      // step2: 父组件使用 this.$refs.xxx.方法/数据 访问所需数据
      this.$refs.componentA.name
      this.$refs.componentA.showName()
      
      // 当现在父元素想要通过子组件的$refs访问孙级组件元素，
      // 只需在子组件封写方法 methodsA this.$refs.gSon.yyy 
      // 父组件调用 this.$refs.son.methodsA
      // 其他数据使用，以此类推
    ```
  
  - **$el** 当前实例/元素
  
    ```js
    // 获取元素节点 能打印整个元素
    this.$refs.temp.$el
    ```
  
  - 依赖注入
  
    增加两个新的实例选项：`provide` 和 `inject`，弥补了过于深层的子代不能使用$parent获取数据的遗憾
  
    ```
    // 父组件 通过provide指定想要提供给后代的数据/方法
    provide: function () {
      return {
        getMap: this.getMap
      }
    }
    
    // 子组件 通过inject接收指定想要的数据/方法
    inject: ['getMap']
    ```
  
    使用依赖注入后：
  
    - 祖先组件不需要知道哪些后代组件使用它提供的 property
    - 后代组件不需要知道被注入的 property 来自哪里
  
- 程序化的事件侦听器

  - 通过 `$on(eventName, eventHandler)` 侦听一个事件
  - 通过 `$once(eventName, eventHandler)` 一次性侦听一个事件
  - 通过 `$off(eventName, eventHandler)` 停止侦听一个事件

  ```js
  mounted: function () {
    // Pikaday 是一个第三方日期选择器的库
    this.picker = new Pikaday({
      field: this.$refs.input,
      format: 'YYYY-MM-DD'
    })
    // 事件手动控制
    this.$once('hook:beforeDestroy', function () {
      picker.destroy()
    })
  },
  
  // 在组件被销毁之前，
  // 也销毁这个日期选择器。
  /*
  beforeDestroy: function () {
    this.picker.destroy()
  }
  */
  ```

- 循环引用 ：全局注册组件不需要考虑此种情况，局部注册组件发生此种情况可查看上文 ‘异步组件’。

- 内联模板：

  给一个子组件`<son></son>`添加上特殊的attribute属性`inline-template`时, 这个子组件将会使用其里面的内容作为模板。

  ```html
  <div id="app">
    <son inline-template>
      <div>
        <p>{{message}}</p>
        <p>{{sonMessage}}</p>
      </div>
    </son>
  </div>
  <script>
    Vue.component('son',{
      data:function(){
        return {
          sonMessage:'sonMessage'
        }
      }
    });
    var app=new Vue({
      el:'#app',
      data:{
        message:'message'
      }
    });
  </script>
  
  <!-- 此时页面内容
    message
    sonMessage
  -->
  ```

- `type="text/x-template"`

  在Vue实例的DOM对象外用`<script type="text/x-template" id="template-example"></script>`定义模板。

- v-once创建静态组件

  对于全是静态资源的组件，在其模板根元素添加v-once使该内容只计算一次并缓存，一定程度会变快，但大部分情况没必要使用这种方式。


#### 实例的生命周期

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

![vue生命周期](D:\数据\blog\存档\vue生命周期.png)

##### 生命周期的钩子函数

- beforeCreate(数据装载之前) & created(数据装载之后)

  ```js
  new Vue({
    el: '#app',
    data: {
      name: 'app'
    },
    beforecreated(){
      console.log(this.name)  //  undefined
    },
    created(){
      console.log(this.name) //  app
    }
  })
  ```

  这两个钩子函数对于 `data` 来说，一个在 `data` 挂载前（beforeCreate）。

- created与beforeMount之间的生命周期

  检查el选项，template选项：
  
  ![image-20200829140813016](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200829140813016.png)
  
  - el选项
  
    - 无el选项 => 停止编译虚拟Dom，生命周期到created钩子函数结束，
  
      想要后续生命周期继续执行，可以手动执行vm.$mount(el)
  
  - template选项
  
    - 如果Vue实例对象中有template选项，则将其作为模板编译成render函数；
    - 如果没有template选项，则将外部HTML作为模板编译。
    - 优先级：[render](https://cn.vuejs.org/v2/api/#render)选项编译 --> template选项  --> 外部HTML
    - 如果以上都没有，则报错。
  
- beforeMount(载入之前) & mounted(载入之后)

  - beforeMount
  
    载入前（完成了data和el数据初始化），但是页面中的内容还是vue中的占位符，data中的message信息没有被挂在到Dom节点中，在这里可以在渲染前最后一次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取。
  
  - Mount
  
    载入后html已经渲染(ajax请求可以放在这个函数中)，把vue实例中的data里的message挂载到DOM节点中去
  
  >这里两个钩子函数间是载入数据。
  
  ```js
  new Vue({
    el: '#app',
    data: {
      name: 'app'
    },
    template: `<p>Vue起作用了</p>`,
    beforeMount: function() {
      // Vue 起作用，数据装载DOM之前
      console.log(document.body.innerHTML);
    },
    mounted: function() {
      // Vue 起作用，装载数据到DOM之后
      console.log(document.body.innerHTML);
    }
  })
  ```


![beforeMount&mounted](D:\数据\blog\存档\beforeMount&mounted.png)

- beforeUpdate(页面内容发生改变之前)& updated(页面内容发生改变之后)

  由前面官方提供的生命周期视图中可以看到，在页面挂载（mount）之后，只要是因修改数据而导致的重新渲染，beforeUpdate&updated就会被调用。
  
  如果待修改的数据没有载入模板中，不会调用这里两个钩子函数
  
  ```js
  new Vue({
    el: '#app',
    data: {
      name: 'app'
    },
    template: `<div>
      <p id="update">{{name}}</p>
      <button @click="name = 'ppa'">按钮</button>
    </div>`,
    // 基于数据的页面内容发生改变
    beforeUpdate: function() {
      console.log('beforeUpdate:', document.getElementById('update').innerHTML)
    },
    updated: function() {
      console.log('updated:', document.getElementById('update').innerHTML)
    }
  })
  ```
  
  ![beforeUpdate&updated](D:\数据\blog\存档\beforeUpdate&updated.png)
  
- beforeDestroy(所在实例被销毁之前)&destroyed(所在实例被销毁之后)

  - beforeDestroy
  
    销毁前执行（$destroy方法被调用的时候就会执行）,一般在这里善后:清除计时器、清除非指令绑定的事件等等…’)
  
  - destroyed
  
    销毁后 （Dom元素存在，只是不再受vue控制）,卸载watcher，事件监听，子组件
  
  ```js
  var son = {
    template: `<div>
          <slot></slot>
        <div>`,
    beforeDestroy: function() {
      // 实例销毁之前调用。在这一步，实例仍然完全可用。
        console.log('son组件还没有销毁')
    },
    destroyed: function() {
    // Vue 实例销毁后调用。
    // 调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
      console.log('son组件销毁了')
    }
  }
  new Vue({
    el: '#app',
    components: {
      'son': son
    },
    data: {
      isDestory: true
    },
    template: `<div>
             <p v-if="isDestory" id='destory'>
               <son>组件销毁前</son>
             </p>
             <p v-else>组件已销毁</p>
             <button @click="isDestory = false">按钮</button>
           </div>
           `,
  
  })
  ```
  
  ![beforeDestroy&destroyed](D:\数据\blog\存档\beforeDestroy&destroyed.png)
  
- activated(当前实例变活跃时) & deactivated（当前实例变不活跃时）

  **只有实例被`<keep-alive>`包裹时，才能使用。**

##### this.指向

  生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。但在其选项或回调函数，使用箭头函数( => ), 并不会找到this，会发生错误。

##### 常用

- beforecreate : 可以在这加个loading事件
- created ：在这结束loading，还做一些初始数据的获取，实现函数自-执行
- mounted ： 在这发起后端请求，拿回数据，配合路由钩子做一些事情
- beforeDestroy： 你确认删除XX吗？
- destroyed ：当前组件已被删除，清空相关内容

#### Vue可复用性

##### 混入

混入 (mixin) 是一种非常灵活的提供Vue 组件可复用性的功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。以下是官方例子，解释都比较好，所以直接使用一下，推荐[查看官网]([https://cn.vuejs.org/v2/guide/mixins.html#%E9%80%89%E9%A1%B9%E5%90%88%E5%B9%B6](https://cn.vuejs.org/v2/guide/mixins.html#选项合并))

```js
// 定义一个混入对象
var myMixin = {
  created: function() {
    this.hello()
  },
  methods: {
    hello() {
      console.log(this.text)
    }
  }
}

// 定义一个使用混入对象的组件
var component = {
  data() {
    return {
      text: 'hello from mixin!'
    }
  },
  mixins: [myMixin]
  //等同于
  /*
  created: function() {
    this.hello()
  },
  methods: {
    hello() {
      console.log(this.text)
    }
  }
  */
}
```

- 选项合并

  当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”，合并策略参考Vue.extend()。

```js
var mixin = {
  data() {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data() {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created() {
    console.log(this.$data)
    // 多以组件数据为优先
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

​        同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。

```js
var mixin = {
  created() {
    console.log('混入对象的钩子被调用')
  }
}

new Vue({
  mixins: [mixin],
  created() {
    console.log('组件钩子被调用')
  }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"
```

​        值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

```js
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})
// 
vm.conflicting() // => "from self"
```

- 全局混入

混入也可以进行全局注册。使用时格外小心！一旦使用全局混入，它将影响**每一个**之后创建的 Vue 实例。使用恰当时，这可以用来为自定义选项注入处理逻辑。

```js
// 为自定义的选项 'myOption' 注入一个处理器。
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"
```

- webpack + ES2015

```js
// 上述全局混入危险性较大，可以尝试下述方式封装一个mixin.js
// 此处导入后面 mixin对象 需要的内容，没有可省略
import {debounce} from "./utils" // 此处导入一个防抖函数
import BackTop from "components/content/backTop/BackTop" //此处导入一个BackTop组件

export const itemListenerMixin = { //向外传出一个名itemListenerMixin的mixin对象
  data() {
    // ...
  },

  mounted() {
    this.itemListener = () => {
      debounce(this.$refs.scroll.refresh, 500)
    }
    // ...
  }
}

export const backTopMixin = { //向外传出一个名backTopMixin的mixin对象
  components: {
    BackTop,
    // ...
  },
  data() {
    return {
      isShowBackTop: false
    }
  },

  methods: {
    backTopClick(){
      this.$refs.scroll.scrollTo(0,0,500)
    },
    // ...
  }
}

// 上内容，可以将开发过程中需要的可复用的内容封装在mixin.js,
// 在对应文件引入需要的mixin对象就可以使用了
// HomeBackTop.vue
import {itemListenerMixin, backTopMixin} from "common/mixin"

// ...
mixins: [itemListenerMixin,backTopMixin],
// ...
```

##### 单文件组件.vue
问题提出: 在一个.html或者.js文件中注册组件有以下几个明显的缺点:

- 全局定义强制要求每个component中的命名不重复
- 字符串模板缺乏高亮样式
- 不支持css意味着当HTML和JavaScript组件化时，css难以操作
- 没有构建步骤限制

解决: 新增.vue文件并且使用webpack或Browserify等构建工具可以解决以上问题。

```vue
// Hello.vue
<template>
  <div id='hello'>
      <p>{{message}}</p>
  </div>
</template>

<script>
  import * as test from './test'
  export default{
    name: 'Hello',
    data(){
      return {
        message: 'Hello World',
      }
    }
  }
</script>

<style scoped>
  #hello p {
    font-size: 36px;
    text-align: center;
  }
</style>
```

个人理解，.vue的作用其实就是把组件独立出来，在组件内部再进行template(组件DOM),script(组件逻辑),css(组件样式)划分，使组件内容更加耦合了，在开发过程中搭配使用会使得组件更加内聚和利于维护。

#### 结

至此，一些Vue的基础知识就差不多了，有没说明的地方都将在后续Vue总结中做铺垫知识展开。