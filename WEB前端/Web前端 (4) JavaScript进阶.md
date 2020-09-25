#### 原型与原型链

##### 函数的prototype属性

  * 每个函数都有一个prototype属性, 它默认指向一个Object空对象(即称为: 原型对象)

  * 原型对象中有一个属性constructor, 它指向函数对象

    ```js
    function fn() {}
    
    console.log(Date.prototype, typeof Date.prototype)  =>Object
    console.log(fn.prototype, typeof fn.prototype)  =>Object
    console.log(Date.prototype.constructor===Date)  =>true
    console.log(fn.prototype.constructor===fn)  =>true
    ```
##### 给原型对象添加属性(一般都是方法)

  * 作用: 函数的所有实例对象自动拥有原型中的属性(方法)

  ```js
  function F() {}
  
  F.prototype.age = 12 //添加属性
  F.prototype.setAge = function (age) { // 添加方法
    this.age = age
  }
  // 创建函数的实例对象
  var f = new F()
  console.log(f.age)
  f.setAge(23)
  console.log(f.age)
  ```

##### 显式原型与隐式原型

- 每个函数function都有一个`prototype`属性(属性值是对象)，即显式原型；每个实例对象都有一个`__proto__`，可称为隐式原型

- 函数的`prototype`属性: 在定义函数时自动添加的，属性值是一个对象，默认值是一个空Object对象
- 对象的`__proto__`属性: 创建对象时自动添加的, 属性值是当前实例所属类的`prototype`原型
- 在`prototype`原型对象中，默认存在一个`constructor`属性，属性值就是当前函数本身

- 内存结构

![image-20200805193737685](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200805193737685.png)

  * ES6之前，程序员能直接操作显式原型, 但不能直接操作隐式原型

##### 原型链 (隐式原型链)

  * 访问一个对象的属性时，

    ① 先在自身属性中查找，找到返回

    ② 如果没有, 再沿着__proto__这条链向上查找, 找到返回

    ③ 如果最终没找到, 返回undefined

- 构造函数/原型/实体对象的关系

![原型链分析](D:\数据\HBuilderProjects\尚硅谷前端资料\02进阶\js高级\源码_课件\JSAdvance\prepare\02_函数高级\01_原型与原型链\原型链分析.png)

![image-20200805210312092](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200805210312092.png)

##### instanceof 运算符

用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。

表达式: A instanceof B

如果B函数的显式原型对象在A对象的原型链上, 返回true, 否则返回false

```js
function Foo() {  }
var f1 = new Foo();
console.log(f1 instanceof Foo);
console.log(f1 instanceof Object);

console.log(Object instanceof Function)
console.log(Object instanceof Object)
console.log(Function instanceof Object)
console.log(Function instanceof Function)
function Foo() {}
console.log(Object instanceof  Foo);
```



#### 执行上下文与执行上下文栈

##### 执行上下文

1. 全局执行上下文

  * 在执行全局代码前将window确定为全局执行上下文
  * 对全局数据进行预处理
    * var定义的全局变量==>undefined, 添加为window的属性
    * function声明的全局函数==>赋值(fun), 添加为window的方法
    * this==>赋值(window)
  * 开始执行全局代码
2. 函数执行上下文

  * 在调用函数, 准备执行函数体之前, 创建对应的函数执行上下文对象
  * 对局部数据进行预处理
    * 形参变量==>赋值(实参)==>添加为执行上下文的属性
    * arguments==>赋值(实参列表), 添加为执行上下文的属性
    * var定义的局部变量==>undefined, 添加为执行上下文的属性
    * function声明的函数 ==>赋值(fun), 添加为执行上下文的方法
    * this==>赋值(调用函数的对象)
  * 开始执行函数体代码

##### 执行上下文栈(JavaScript 的调用栈)

- 在全局代码执行前，JavaScript 引擎会将执行上下文压入栈空间中，通常把这种用来管理执行上下文的栈称为执行上下文栈，又称调用栈。
- 在全局执行上下文(window)确定后, 将其添加到栈中(压栈)
- 在函数执行上下文创建后, 将其添加到栈中(压栈)
- 在当前函数执行完后,将栈顶的对象移除(出栈)
- 当所有的代码执行完后, 栈中只剩下window

```js
//1. 进入全局执行上下文
var a = 10
var bar = function (x) {
  var b = 5
  foo(x + b)              //3. 进入foo执行上下文
}
var foo = function (y) {
  var c = 5
    console.log(a + c + y)
}
bar(10)                    //2. 进入bar函数执行上下文
```

```js
//1. 依次输出什么?
//2. 整个过程中产生了几个执行上下文?
console.log('global begin: '+ i)
var i = 1
foo(1);
function foo(i) {
  if (i == 4) {
    return;
  }
  console.log('foo() begin:' + i);
  foo(i + 1);
  console.log('foo() end:' + i);
}
console.log('global end: ' + i)
```

#### 作用域与作用域链

##### 作用域

  * 就是一块"地盘", 一个代码段所在的区域
  * 它是静态的(相对于上下文对象), 在编写代码时就确定了

  * 分为全局作用域函数作用域，没有块作用域(ES6有)

  * 隔离变量，不同作用域下同名变量不会有冲突

##### 作用域链

  * 多个上下级关系的作用域形成的链, 它的方向是从下向上的(从内到外)
  * 查找变量时就是沿着作用域链来查找的

##### 按作用域链查找一个变量的查找规则

①在当前作用域下的执行上下文中查找对应的属性, 如果有直接返回, 否则进入2，

②在上一级作用域的执行上下文中查找对应的属性, 如果有直接返回, 否则进入3，

③再次执行2的相同操作, 直到全局作用域, 如果还找不到就抛出找不到的异常。

##### 与执行上下文的区别和联系

-> 区别1

  * 全局作用域之外，每个函数都会创建自己的作用域，作用域在函数定义时就已经确定了。而不是在函数调用时
  * 全局执行上下文环境是在全局作用域确定之后, js代码马上执行之前创建
  * 函数执行上下文环境是在调用函数时, 函数体代码执行之前创建

-> 区别2

  * 作用域是静态的, 只要函数定义好了就一直存在, 且不会再变化
  * 上下文环境是动态的, 调用函数时创建, 函数调用结束时上下文环境就会被释放

-> 联系

  * 上下文环境(对象)是从属于所在的作用域

  * 全局上下文环境==>全局作用域

  * 函数上下文环境==>对应的函数使用域

#### 闭包

1. 闭包的产生

  * **函数A执行时，导致其内部的函数B被返回到外部并保存时**

  * 产生条件

    - 函数嵌套

    - 内部函数引用了外部函数的数据(变量/函数)

2. 常见闭包

   ①将函数作为另一个函数的返回值

   ```js
   function fn1() {
     var a = 2
   
     function fn2() {
       a++			//引用外部函数变量，产生闭包
       console.log(a)  //产生
     }
   
     return fn2
   }
   var f = fn1() 
   f() // 3
   f() // 4
   f = null  //及时释放-->死亡
   
   //应用闭包--封装自定义模块1
   function coolModule() {
     //私有的数据
     var msg = 'atguigu'
     var names = ['I', 'Love', 'you']
   
     //私有的操作数据的函数
     function doSomething() {
       console.log(msg.toUpperCase())
     }
     function doOtherthing() {
       console.log(names.join(' '))
     }
   
     //向外暴露包含多个方法的对象
     return {
       doSomething: doSomething,
       doOtherthing: doOtherthing
     }
   }
   ```

   ②将函数作为实参传递给另一个函数调用

   ```js
   function showMsgDelay(msg, time) {
     setTimeout(function () {
       console.log(msg)
     }, time)
   }
   showMsgDelay('hello', 1000)
   
   //应用闭包--封装自定义模块2
   (function (window) {
     //私有的数据
     var msg = 'atguigu'
     var names = ['I', 'Love', 'you']
     //操作数据的函数
     function a() {
       console.log(msg.toUpperCase())
     }
     function b() {
       console.log(names.join(' '))
     }
   
     window.coolModule2 =  {
       doSomething: a,
       doOtherthing: b
     }
   })(window)
   ```

3. 闭包的作用

   - 使用函数内部的变量在函数执行完后, 仍然存活在内存中(延长了局部变量的生命周期)

   - 让函数外部可以操作(读写)到函数内部的数据(变量/函数)

4. 闭包的生命周期

   - 产生：在嵌套内部函数定义执行完。
   - 死亡：在嵌套内部函数成为垃圾对象时。
   - 如上2-①

5. 关于闭包的几点问题

   - 函数执行完后, 函数内部声明的局部变量是否还存在?

     如上2-①中，由于f引用内部函数 --> 内部函数以及闭包都没有成为垃圾对象

   - 在函数外部能直接访问函数内部的局部变量吗?

     如上2-①中，调用f()时间接操作了函数内部的局部变量。

   - 函数执行完，闭包局部变量没有释放，内存占用时间变长，容易内存泄漏，应及时释放。

6. 体会闭包

   ```
   //代码片段一
     var name = "The Window";
     var object = {
       name: "My Object",
       getNameFunc: function () {
         return function () {
           return this.name;
         };
       }
     };
     console.log(object.getNameFunc()());  //The Window
   
   //代码片段二
     var name2 = "The Window";
     var object2 = {
       name2: "My Object",
       getNameFunc: function () {
         var that = this;
         return function () {
           return that.name2;
         };
       }
     };
     console.log(object2.getNameFunc()());//My Object
     
   //代码片段三
     function fun(n, o) {
       console.log(o)
       return {
         fun: function (m) {
           return fun(m, n)
         }
       }
     }
     var a = fun(0)
     a.fun(1)
     a.fun(2)
     a.fun(3) //undefined,0,0,0
   
     var b = fun(0).fun(1).fun(2).fun(3) //undefined,0,1,2
   
     var c = fun(0).fun(1)
     c.fun(2)
     c.fun(3) //undefined,0,1,1
   
   ```

   

#### 对象创建与继承

##### 对象的创建

- Object构造函数模式
    * 套路: 先创建空Object对象, 再动态添加属性/方法

    * 适用场景: 起始时不确定对象内部数据

    * 问题: 语句太多

      ```js
      var p = new Object()
      p = {}
      p.name = 'Tom'
      p.age = 12
      p.setName = function (name) {
        this.name = name
      }
      p.setaAge = function (age) {
        this.age = age
      }
      
      console.log(p)
      ```

      

- 对象字面量模式
    * 套路: 使用{}创建对象, 同时指定属性/方法

    * 适用场景: 起始时对象内部数据是确定的

    * 问题: 如果创建多个对象, 有重复代码

      ```js
      var p = {
        name: 'Tom',
        age: 23,
        setName: function (name) {
          this.name = name
        }
      }
      console.log(p.name, p.age)
      p.setName('JACK')
      console.log(p.name, p.age)
      
      var p2 = {
        name: 'BOB',
        age: 24,
        setName: function (name) {
        this.name = name
        }
      }
      ```

- 工厂模式
  - 套路: 通过工厂函数动态创建对象并返回

  - 适用场景: 需要创建多个对象

  - 问题: 对象没有一个具体的类型, 都是Object类型

    ```js
    function createPerson(name, age) {
      var p = {
       name: name,
       age: age,
       setName: function (name) {
         this.name = name
       }
      }
      return p
    }
    
    var p1 = createPerson('Tom', 12)
    var p2 = createPerson('JAck', 13)
    console.log(p1)
    console.log(p2)
    ```

- 自定义构造函数模式
    * 套路: 自定义构造函数, 通过new创建对象

    * 适用场景: 需要创建多个类型确定的对象

    * 问题: 每个对象都有相同的数据, 浪费内存

      ```js
      function Person(name, age) {
        this.name = name
        this.age = age
        this.setName = function (name) {
          this.name = name
        }
      }
      
      var p1 = new Person('Tom', 12)
      var p2 = new Person('Tom2', 13)
      console.log(p1, p1 instanceof Person)
      ```

- 构造函数+原型的组合模式
    * 套路: 自定义构造函数, 属性在函数中初始化, 方法添加到原型上

    * 适用场景: 需要创建多个类型确定的对象

      ```js
      function Person (name, age) {
        this.name = name
        this.age = age
      }
      Person.prototype.setName = function (name) {
        this.name = name
      }
      var p1 = new Person('Tom', 12)
      var p2 = new Person('JAck', 23)
      p1.setName('TOM3')
      console.log(p1)
      
      Person.prototype.setAge = function (age) {
        this.age = age
      }
      p1.setAge(23)
      console.log(p1.age)
      
      Person.prototype = {}
      p1.setAge(34)
      console.log(p1)
      var p3 = new Person('BOB', 12)
      p3.setAge(12)
      ```

##### 对象的继承

- 原型链继承

  - 套路：

    ①定义父类型构造函数

    ②给父类型的原型添加方法

    ③定义子类型的构造函数

    ④创建父类型的对象赋值给子类型的原型

    ⑤将子类型原型的构造属性设置为子类型

    ⑥给子类型原型添加方法

    ⑦创建子类型的对象: 可以调用父类型的方法

    - 关键：子类型的原型为父类型的一个实例对象

         ```js
         function Supper() { //父类型
           this.superProp = 'The super prop'
         }
         //原型的数据所有的实例对象都可见
         Supper.prototype.showSupperProp = function () {
           console.log(this.superProp)
         }
         
         function Sub() { //子类型
           this.subProp = 'The sub prop'
         }
         
         // 子类的原型为父类的实例
         Sub.prototype = new Supper()
         // 修正Sub.prototype.constructor为Sub本身
         Sub.prototype.constructor = Sub
         
         Sub.prototype.showSubProp = function () {
           console.log(this.subProp)
         }
         
         // 创建子类型的实例
         var sub = new Sub()
         // 调用父类型的方法
         sub.showSubProp()
         // 调用子类型的方法
         sub.showSupperProp()
         ```

- 借用构造函数继承(假的)
  - 套路:

    ①定义父类型构造函数

    ②定义子类型构造函数

    ③在子类型构造函数中调用父类型构造

  - 关键: 在子类型构造函数中通用super()调用父类型构造函数

    ```js
    function Person(name, age) {
      this.name = name
      this.age = age
    }
    
    function Student(name, age, price) {
      Person.call(this, name, age)   // this.Person(name, age)
      this.price = price
    }
    
    var s = new Student('Tom', 20, 12000)
    console.log(s.name, s.age, s.price)
    ```

- 原型链+借用构造函数的组合继承
  - 套路

    ①利用原型链实现对父类型对象的方法继承

    ②利用call()借用父类型构建函数初始化相同属性

    ```js
    function Person(name, age) {
      this.name = name
      this.age = age
    }
    Person.prototype.setName = function (name) {
      this.name = name
    }
    
    function Student(name, age, price) {
      Person.call(this, name, age) //得到父类型的属性
      this.price = price
    }
    Student.prototype = new Person()  //得到父类型的方法
    Student.prototype.constructor = Student
    Student.prototype.setPrice = function (price) {
      this.price = price
    }
    
      var s = new Student('Tom', 12, 10000)
    s.setPrice(11000)
    s.setName('Bob')
    console.log(s)
    console.log(s.constructor)
    ```

    

#### 线程机制与事件机制

##### 进程与线程

- 概念

  进程：程序的一次执行, 它占有一片独有的内存空间

​       线程： CPU的基本调度单位, 是程序执行的一个完整流程

  * 特点

    - 一个进程中一般至少有一个运行的线程: 主线程

    - 一个进程中也可以同时运行多个线程, 我们会说程序是多线程运行的

    - 一个进程内的数据可以供其中的多个线程直接共享

    - 多个进程之间的数据是不能直接共享的
- 浏览器运行是单进程还是多进程?是单线程还是多线程？
  - 有的是单进程 firefox/老版IE
  - 有的是多进程 chrome/新版IE
  - 都是多线程运行的

- 如何查看浏览器是否是多进程运行的呢?  任务管理器==>进程



##### 浏览器内核

- 什么是浏览器内核?

  - 支持浏览器运行的最核心的程序

- 不同的浏览器，内核可能不一样

  - Chrome, Safari: webkit

  - firefox: Gecko

  - IE: Trident

  - 360,搜狗等国内浏览器: Trident + webkit

- 内核由很多模块组成
  - html,css文档解析模块 : 负责页面文本的解析
  - dom/css模块 : 负责dom/css在内存中的相关处理
  - 布局和渲染模块 : 负责页面的布局和效果的绘制
  - 布局和渲染模块 : 负责页面的布局和效果的绘制
  - 定时器模块 : 负责定时器的管理
  - 网络请求模块 : 负责服务器请求(常规/Ajax)
  - 事件响应模块 : 负责事件的管理
##### JS是单线程的

- 如何证明js执行是单线程的?

  - setTimeout()的回调函数是在主线程执行的

  - 定时器回调函数只有在运行栈中的代码全部执行完后才有可能执行

- 为什么js要用单线程模式, 而不用多线程模式?

  - JavaScript的单线程，与它的用途有关。

  - 作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。

  - 这决定了它只能是单线程，否则会带来很复杂的同步问题

- js引擎执行代码的基本流程

  - 先执行初始化代码: 包含一些特别的代码(设置定时器、绑定监听、发送ajax请求)

  - 后在某个时刻执行回调代码：处理回调代码

```js
setTimeout(function () {
  console.log('timeout 3')
}, 3000)

setTimeout(function () {
  console.log('timeout 2')
  alert('2222')
}, 2000)

alert('提示...')
console.log('alert之后')
```

- 事件循环模型

  ![事件循环模型](D:\数据\HBuilderProjects\尚硅谷前端资料\02进阶\js高级\源码_课件\JSAdvance\prepare\04_线程机制与事件机制\事件循环模型.png)

  - 执行初始化代码, 将事件回调函数交给对应**模块管理**

  - 当事件发生时, 管理模块会将回调函数及其数据添加到**回调列队**中
  
  - 只有当初始化代码执行完后(可能要一定时间), 才会遍历读取回调队列中的回调函数执行

```js
function fn1() {console.log('fn1()');}
fn1();

document.getElementById('btn').onclick = function () {
  console.log('处理点击事件')
};

setTimeout(function () {
  console.log('到点了')
}, 2000);

function fn2() { console.log('fn2()');}
fn2();
/* 输出
  		  fn1()
  		  fn2()
 		  到点了
点击后 --> 处理点击事件  
*/
```

> setInterval()与setTimeout()
>
> * setInterval的回调函数将尝试每隔100ms执行一次, 只要回调代码执行的时间不超过100ms就能定时
> * setTimeout的方式: 如果回调代码执行需要xms, 下次回调的时间就会被延迟xms

Web Workers

1. H5规范提供了js分线程的实现, 取名为: Web Workers

   ![H5 Web Workers(多线程)](D:\数据\HBuilderProjects\尚硅谷前端资料\02进阶\js高级\源码_课件\JSAdvance\prepare\04_线程机制与事件机制\H5 Web Workers(多线程).png)

2. 相关API
  * Worker: 构造函数, 加载分线程执行的js文件
  * Worker.prototype.onmessage: 用于接收另一个线程的回调函数
  * Worker.prototype.postMessage: 向另一个线程发送消息
3. 不足
  * worker内代码不能操作DOM(更新UI)
  * 不能跨域加载JS
  * 不是每个浏览器都支持这个新特性

4. 应用
- 计算得到斐波那契数列中第n个数的值

  - 在主线程计算: 当位数较大时, 会阻塞主线程, 导致界面卡死

  - 在分线程计算: 不会阻塞主线程

#### 其他

##### 分号问题

1. js一条语句的后面可以不加分号
2. 是否加分号是编码风格问题, 没有应该不应该，只有你自己喜欢不喜欢
3. 在下面2种情况下不加分号会有问题
  * 小括号开头的前一条语句
  * 中方括号开头的前一条语句
4. 解决办法: 在行首加分号
5. 强有力的例子: vue.js库
6. 知乎热议: https://www.zhihu.com/question/20298345

```js
// 情形一: 小括号开头的前一条语句
var a = 3
;(function () {
})
/*
错误理解: 将3看成是函数调用
var a = 3(function () {
})
*/

// 情形二: 中方括号开头的前一条语句
var b = a
;[1, 3, 5].forEach(function (item) {
  console.log(item)
})
/*
错误理解:
a = b[5].forEach(function(e){
  console.log(e)
})
*/
```

##### 内存泄漏与内存溢出

1. 内存溢出
  * 一种程序运行出现的错误
  * 当程序运行需要的内存超过了剩余的内存时, 就出抛出内存溢出的错误
2. 内存泄露
  * 占用的内存没有及时释放
  * 内存泄露积累多了就容易导致内存溢出
  * 常见的内存泄露:
    * 意外的全局变量
    * 没有及时清理的计时器或回调函数
    * 闭包

```js
// 1. 内存溢出
var obj = {}
for (var i = 0; i < 100000; i++) {
  obj[i] = new Array(10000000)
}
console.log('------')

// 2. 内存泄露
// 意外的全局变量
function fn () {
  a = [] //不小心没有var定义
}
fn()
// 没有及时清理的计时器
setInterval(function  () {
  console.log('----')
}, 1000)
```

