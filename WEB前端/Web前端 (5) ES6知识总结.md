可以说ECMAScript 是 JavaScript 语言的国际标准，JavaScript 是 ECMAScript 的实现。

#### let&const命令

​	ES6 新增了 let 命令，用来声明变量，使用它声明的变量只在所在的代码块中有效。

​	ES6 新增了 const 命令，用来声明常量，使用它声明的常量只在所在的代码块中有效。

| let声明变量  |        var声明变量         | const声明常量 |
| :----------: | :------------------------: | :-----------: |
| 没有变量提升 |         有变量提升         | 没有变量提升  |
|  块级作用域  | 无块级作用域，是函数作用域 | 有块级作用域  |
| 不能重复声明 |         可重复声明         | 不能重复声明  |
|  可重新赋值  |         可重新赋值         | 不能重新赋值  |

```js
console.log(a) //Cannot access 'a' before initialization at …………
console.log(b) //undefined
console.log(x) //Cannot access 'x' before initialization at …………

let a = 1
var b = 2
const x = 3 //const x;  Missing initializer in const declaration

;{
  let c = 1
  var d = 2
  const y = 3
}

console.log(c) //Error c is not defined
console.log(d) //2
console.log(y) //Error y is not defined

let a = 5    //Identifier 'a' has already been declared
var b = 10   //console.log(b) --> 10

a = 5  //console.log(a) --> 5
b = 10 //console.log(b) --> 10

x = 4    //TypeError: Assignment to constant variable 
const x = 4 //Identifier 'x' has already been declared
```

> 块级作用域可以代替匿名函数写法
>
> ```
> (function(){
>   var a = 1;
>   ………………
> }());
> ==>
> {
>   let a = 2;
>   ………………
> };
> ```

#### Symbol

​	ES6中引入的原始数据类型Symbol，表示独一无二的值。Symbol 是JavaScript 在ES6 中一种基本数据类型。

- Symbol() 函数返回的是 Symbol 类型的值，该类型具有静态方法和静态属性。

  ```js
  var sym1 = Symbol();
  var sym2 = Symbol('foo');
  var sym3 = Symbol('foo');
  ```

- 每一个 Symbol() 返回的值都是唯一的。一个Symbol 值能作为对象属性的标识符，这是改数据类型仅有的目的。

  ```js
  Symbol("abc") === Symbol("abc"); // false
  ```

- 不可以使用 new 操作符创建

  ```js
  var sym = new Symbol();  // TypeError报错
  ```

- 结合 Object() 函数，创建一个 Symbol 包装器对象

  ```js
  var sym = Symbol();
  typeof sym;    // "symbol“”
  var symobj = Object(sym);
  typeof symobj;  // "object"
  ```

- 全局共享 Symbol

  Symbol.for(key) 可以根据键值key从运行的symbol表中找对应symbol，找到即返回，找不到新建一个放到注册表中。

- 对象中查找symbol属性

  ```js
  var obj = {};
  var a = Symbol("a");
  var b = Symbol.for("b");
  obj[a] = "localSymbol";
  obj[b] = "globalSymbol";
  var objectSymbols = Object.getOwnPropertySymbols(obj);
  console.log(objectSymbols)         // [Symbol(a), Symbol(b)]
  ```

> symbol静态属性：
>
> - symbol.length -->0
>
> - symbol.iterator -->该方法为每一个对象定义了默认的迭代器。该迭代器可以被 for.. of 循环使用。
>
>   ```js
>   //自定义迭代
>   var myIterator = {};
>   theIterator[Symbol.iterator] = function* () {
>       yield 1;
>       yield 2;
>       yield 3;
>   };
>   [...theIterator] // [1, 2, 3]
>   //for……in迭代
>   var obj = {};
>   obj[Symbol("a")] = "a";
>   obj[Symbol.for("b")] = "b";
>   obj["c"] = "c";
>   obj.d = "d";
>   
>   for (var i in obj) {
>      console.log(i);// “c” "d"
>   }
>   ```
>
> - Symbol.replace
>    指定了当一个字符串替换所匹配字符串时所调用的方法。String.prototype.replace() 会调用此方法。
>    
> - Symbol.search
>    指定了一个搜索方法，这个方法接受用户输入的正则表达式，返回该正则表达式在字符串中匹配到的下标，String.prototype.search()会调用此方法。
>    
> - Symbol.split
>    指向 一个正则表达式的索引处分割字符串的方法。 String.prototype.split() 会调用此方法。
>    
> - 其他属性
>   
>    - Symbol.hasInstance
>      一个确定一个构造器对象识别的对象是否为它的实例的方法。使用 instanceof.
>    - Symbol.isConcatSpreadable
>      一个布尔值，表明一个对象是否应该flattened为它的数组元素。使用Array.prototype.concat().
>    - Symbol.unscopables
>      拥有和继承属性名的一个对象的值被排除在与环境绑定的相关对象外。
>    - Symbol.species
>      一个用于创建派生对象的构造器函数。
>    - Symbol.toPrimitive
>      一个将对象转化为基本数据类型的方法。
>    - Symbol.toStringTag
>      用于对象的默认描述的字符串值。使用Object.prototype.toString().

#### 解构赋值

- 左右两边结构必须一样；

- 右边必须是个东西，东西-->对象/数组；

- 声明和赋值不能分开(必须在一句话里完成)；			

  ```js
  注:let {a,b} = {70,10}
     //右边什么也不是！！
  let [a,b];
  [a,b] = [40,10];
    //声明和赋值是一句，不能分开！！！
  let [a,b,c] = [7,8,9];
  console.log(a,b,c); //7,8,9
  
  let [{a,b},[n1,n2,n3],x,y] = [{a:10,b:11},[10,9,8],true,'flase'];
  console.log(a,b,n1,n2,n3,x,y); //10 11 10 9 8 true "flase"
  
  let [json,arr5,x,y] = [{a:10,b:11},[10,9,8],true,'flase'];
  console.log(json,json.a,arr4,x,y); //{a: 10, b: 11} 10 [10, 9, 8] true "flase"
  ```


#### 代理模式-对象代理Proxy

​	所谓的代理者是指一个类别可以作为其它东西的接口。代理者可以作任何东西的接口：网络连接、内存中的大对象、文件或其它昂贵或无法复制的资源。

​	Proxy的主要作用就是可以对对象进行拦截，以及对数据读取、修改的过滤保护。Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

```js
let Person = {
    name:'es6',
    sex:'male',
    age:12,
    _prop:18
};
//var proxy = new Proxy(target, handler); //target拦截目标对象 handle定制拦截行为
let person = new Proxy(Person,{
  // get:获取某个属性时触发  target是代理的数据即Person，value是要操作的那个属性 可继承
  get(target,value){
    return target[value];
  },
  // set:当设置某个属性时触发 target是代理的数据即Person，key是要操作的属性，value是属性的值
  set(target,key,value){
    if(key !== 'sex')
      target[key] = value;
  },
  //has:判断对象是否具有某个属性时生效,典型的操作就是in运算符。target代理的数据,key是判断的属性
  has (target, key) {
    if(key[0] === '_'){  //设置_属性隐藏/不可扩展/不可配置
      return false;
    }
    return key in target;//for……in不会触发拦截
  },
  //deleteProperty:拦截删除 target是代理的数据即Person，key是要删除的属性
  deleteProperty: (target, key) => {
    if(key.startsWith('_')){
      console.log('私有属性进制删除哦~')                       
    } else {
      delete target[key]
      console.log(`删除${key}键成功~`)
    }
  },
  
});
console.table({
  name:person.name,
  sex:person.sex,
  age:person.age
}); 
```

```js
var p = new Proxy(function () {}, {
    //construct:拦截new 
   //target：目标对象 args：构造函数的参数对象 newTarget：创造实例对象时，new命令作用的构造函数,如p()
  construct: function(target, args) {
    console.log('called: ' + args.join(', '));
    return { value: args[0] * 10 };  //construct必须返回对象 
  }
});
(new p(1)).value
```

```js
function fn(...n){
  console.log(n)
  return "i am fn";
}
let newfn = new Proxy(fn,{
  apply(fn){
    let res = fn.apply(this, arguments[2])
      // 可以获取函数返回值
      console.log( res )
      return 'i am in proxy'
  }
})
console.log(newfn(2,3))
```

| 方法                                                         | 描述                                          |
| ------------------------------------------------------------ | --------------------------------------------- |
| [handler.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/apply) | 拦截 Proxy 实例作为函数调用的操作             |
| [handler.construct()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/construct) | 拦截 Proxy 实例作为函数调用的操作             |
| [handler.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/defineProperty) | 拦截 Object.defineProperty() 的操作           |
| [handler.deleteProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/deleteProperty) | 拦截 Proxy 实例删除属性操作                   |
| [handler.get()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/get) | 拦截 读取属性的操作                           |
| [handler.set()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/set) | 拦截 属性赋值的操作                           |
| [handler.getOwnPropertyDescriptor()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/getOwnPropertyDescriptor) | 拦截 Object.getOwnPropertyDescriptor() 的操作 |
| [handler.getPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/getPrototypeOf) | 拦截 获取原型对象的操作                       |
| [handler.has()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/has) | 拦截 属性检索操作                             |
| [handler.isExtensible()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/isExtensible) | 拦截 Object.isExtensible()操作                |
| [handler.ownKeys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/ownKeys) | 拦截 Object.getOwnPropertyDescriptor() 的操作 |
| [handler.preventExtension()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/preventExtensions) | 拦截 Object().preventExtension() 操作         |
| [handler.setPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler/setPrototypeOf) | 拦截Object.setPrototypeOf()操作               |
| [Proxy.revocable()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/revocable) | 创建一个可取消的 Proxy 实例                   |

**Proxy中的this指向**

```js
//在 Proxy 代理的情况下，目标对象内部的this关键字会指向 Proxy 代理。
const target = {
  m: function () {
    console.log(this === proxy);
  }
};
const handler = {};
const proxy = new Proxy(target, handler);

target.m() // false
proxy.m() // true
```

#### Reflect

`Reflect`对象与`Proxy`对象一样，也是 ES6 为了操作对象而提供的新 API。把文档中所有的对象统一管理，所有对象里的所有属性和方法都能在reflect里边找到。

- **只要Proxy对象具有的代理方法，Reflect对象全部具有，以静态方法的形式存在。这些方法能够执行默认行为，无论Proxy怎么修改默认行为，总是可以通过Reflect对应的方法获取默认行为。**

- **新增的方法与现有一些方法功能重复，新增的方法会取代现有的方法**。

  ```js
  // 推荐使用reflect来操作对象，而不是直接操作对象，符合函数式变成的写法
  let obj2 = {
    name: 'abai',
    phone: '15000112222',
    makeName(){
      console.log('reflect实例123')
    },
  }
  
  console.log(Reflect.get(obj2,'name'),Reflect.has(obj2, 'sweet'))
  Reflect.set(obj2, 'sweet', true)
  console.log(Reflect)
  ```
  
  |方法描述|      |
  | ---- | ---- |
  |[handler.apply()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply) |通过指定的参数列表发起对目标函数的调用|
  |[handler.construct()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/construct) |此方法行为有点像 new 操作符构造函数，相当于运行 new target(...args)|
  |[handler.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/defineProperty) | 方法功能类似于Object.defineProperty()方法|
  |[handler.deleteProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/deleteProperty) | 功能类似于delete运算符|
  |[handler.get()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/get) | 从对象获取指定属性值|
  |[handler.set()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/set) | 设置指定对象的属性，比如为对象添加新属性或者修改原有属性的值|
  |[handler.getOwnPropertyDescriptor()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/getOwnPropertyDescriptor) | 功能类似于Object.getOwnPropertyDescriptor()|
  |[handler.getPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/getPrototypeOf) | 获取对象的原型对象|
  |[handler.has()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/has) | 获取对象的原型对象|
  |[handler.isExtensible()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/isExtensible) | 判断一个对象是否是可扩展的|
  |[handler.ownKeys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) | 返回一个数组，此数组中包含有参数对象自有属性名称|
  |[handler.preventExtension()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/preventExtensions) | 将对象设置为不可扩展|
  |[handler.setPrototypeOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/setPrototypeOf) | 设置指定对象的原型对象 |
  **实现观察者模式**
  
  ​    观察者模式（Observer mode）指的是函数自动观察数据对象，一旦对象有变化，函数就会自动执行。

```js
<input id="testinp" />
<div>实时更新的数据: <i  id="text"></i></div>

let oInput = document.getElementById('testinp');
let oText = document.getElementById('text');
let handler = {
  set: (target,key,val) => {
    if(key === 'text'){
      if(val !== oInput.value){oInput.value = val}
      oText.innerHTML = val
    }else{
      Reflect.set(target,key,val)
    }
  },
  get: (target, key) => {
    return Reflect.get(target, key)
  }
}

let textObj = new Proxy(oText, handler)

oInput.addEventListener('keyup',(e) => {
  textObj.text = e.target.value
})
setTimeout(() => {textObj.text = 'initValue'},1000)
```

**其他应用**

```jsx
//操作节点（切换两个不同的元素的属性或类名）
let view = new Proxy({
  selected: null
},
{
  set: function(obj, prop, newval) {
    let oldval = obj[prop];

    if (prop === 'selected') {
      if (oldval) {
        oldval.setAttribute('aria-selected', 'false');
      }
      if (newval) {
        newval.setAttribute('aria-selected', 'true');
      }
    }

    // The default behavior to store the value
    obj[prop] = newval;
  }
});

let i1 = view.selected = document.getElementById('item-1');
console.log(i1.getAttribute('aria-selected')); // 'true'

let i2 = view.selected = document.getElementById('item-2');
console.log(i1.getAttribute('aria-selected')); // 'false'
console.log(i2.getAttribute('aria-selected')); // 'true'
```

```jsx
//对象多重继承
var obj1 = {
  name: "obj-1",
  foo() {
    console.log( "obj1.foo:", this.name );
  }
},
obj2 = {
  name: "obj-2",
  foo() {
    console.log( "obj2.foo:", this.name );
  },
  bar() {
    console.log( "obj2.bar:", this.name );
  }
},
handlers = {
  get(target,key,context) {
    if (Reflect.has( target, key )) {//有对应属性则转发
       return Reflect.get(target, key, context);
    }else {//没有则遍历父对象列表
      //Symbol.for("[[Prototype]]")表示要继承的多个父对象
      for (var P of target[Symbol.for( "[[Prototype]]" )]) {
        if (Reflect.has( P, key )) {
          return Reflect.get(P, key, context)
        }
      }
    }
  }
},
obj3 = new Proxy({
  name: "obj-3",
  baz() {
    this.foo();
    this.bar();
  }
},handlers);

obj3[Symbol.for("[[Prototype]]")] = [obj1, obj2];
obj3.baz();
//obj1.foo:obj-3
//obj2.bar:obj-3
```

#### 箭头函数

- 只有一个参数 () 可以省略

- 只有一个return {return} 可以省略

  ```js
  var a1 = document.getElementById("a1")
  let show1 = (a, b) => {
    return a + b;
  }
  show1(3, 4); 
  a1.innerHTML = show1(3, 4)
  			
  let arr = [3, 4, 9, 7, 19]
  
  arr.sort(function(n1, n2){
    return n1 - n2;
  })
  // arr.sort((n1, n2) => n1 - n2)
  a1.innerHTML = arr.sort((n1, n2) => n1 - n2)
  ```

#### 扩展运算符

- 收集剩余数组
  Rest Parameter 必须是最后一个
- 展开数组 
  展开后的效果，跟直接把数组放到那里效果相同					

```js
//注：不允许赋值
//默认参数：
function add(a, b = 4, c = 5){}

let openArr = (a, b, ...args) => args; //不能在...args后面接参数
var a2 = document.getElementById("a2");
a2.innerHTML = openArr(2, 4, 6, 7, 8, 9);  //6,7,8,9
			
let show2 = (a, b, c) => a + " " + b + " " + c; 
			
// a2.innerHTML = show2(4, 5, 6);
let arr2 = [4, 5, 6];
// a2.innerHTML = show2(...arr2);
let arr3 = [7, 8, 9];
			 
let arr4 = [...arr2, ...arr3]; 
a2.innerHTML = arr4;  //[4,5,6,7,8,9]
```
#### 数组方法

- map   		映射		一个对一个

​       [10,15,20,25]  ["001","002"，"003"]
​       [A, B, C, D]   [{leavl:"ss",name:"q"},  {leavl:"sss",name:"w"}, {leavl:"sp",name:"e"}]

```js
//使用：
arr.map(function(item){
  alert(item);
  // 对数组进行操作
});
联合箭头函数：
arr.map = item => {alert(item);};							
```

- reduce		汇总		一堆出一个

  算总数   [10,12,54] => 76
  算给平均分   [12,15,18] => 15
```js
//使用：
arr.reduce(function(tmp,item,index){
  // add: return tmp+item;
  // avg:
  if(item == index){
    return (tmp+item) / (index+1);
  }else{
    return tmp+item;
  }
});
/* [10,12,15,18]
tmp  	item 	index 	sum
10   	12   	1   	22
22   	15   	2   	37
37   	18   	3       55
*/
```
- filter		过滤器			留一部分消一部分
``` js
//使用：
let result = arr.filter(item=>{
  return false;
}); //Array[0]
						
let result = arr.filter(item=>item >= 15?true:false); //15,18
```

- forEach		循环(迭代)  
``` js
arr.forEach((item,index)=>item+index);
```

#### Map和Set

​    ES6 中引入了4种新的数据结构: 集合（Set）、弱集合（WeakSet）、映射（Map）、弱映射（WeakMap）

- Set: Set 对象是值的集合，可以按照插入的顺序迭代它的元素。Set 中的元素只会出现一次，即 Set 中的元素是唯一的。

  语法: new Set([ iterable ]); // iterable:可迭代对象，所有元素添加到新的Set。

  > 由于 Set 中的值总是唯一的，所以需要判断两个值是否相等。在对象的扩展章节中，我们讲到可以通过Object.is() 来判断是否严格相等，在 Set 中，-0 和 +0 是两个不同的值，NaN 和 undefined 是可以被存储在 Set 中的，因为 NaN 在ES6中是严格相等的。

  属性：

  - length 属性：0
  - Set.prototype: 表示Set构造器的原型，允许向所有Set对象添加新的属性。
  - Set.prototype.constructor: 返回实例构造函数，默认是Set。
  - Set.prototype.size: 返回Set对象的值的个数（ NaN不算值）。

  方法：

  - Set.prototype.add(value): 对象尾部添加元素。

  - Set.prototype.clear(): 清除所有元素。

  - Set.prototype.has(value): 判断值是否存在于Set中。

  - Set.prototype.delete(value): 删除Set中元素。

  - 遍历方法：

    forEach(function) ：对Set每个元素执行一遍function。

    keys()/values(): 通过.next()对每个元素的名/值进行遍历。

    entries(): 通过.next()对每个元素的名+值进行遍历。

  应用：

  Array与Set相互转换：

  ```js
  var myArray = ["value1", "value2", "value2"]
  //使用构造器将Array转换为Set
  var mySet = new Set(myArray) //["value1", "value2"]
  myset.has("value1") //true
  ```

  数组去重：

  ```js
  //用数组静态方法Array.from
  let array1 = Array.from(new Set([1,1,1,2,3,2,4]))  //1,2,3,4
  let array2 = [...new Set([1,1,1,2,3,2,4])]  //1,2,3,4
  ```

  字符串转Set

  ```js
  var text = "abc"
  var stringSet = new Set(text) //set(3)["a","b","c"]
  ```

  实现并集、交集、差集

  ```js
  let a = new Set([1,2,3])
  let b = new Set([2,3,4])
  //并集
  let unionSect = new Set([...a,...b]);
  //交集
  let interSect = new Set([...a].filter(x=>b.has(x)))
  //差集
  let differSect = new Set([...a].filter(x=>!b.has(x)))
  ```

- WeakSet

  - 可以new创建的，与Set结构类似
  - 与Set的区别
    - 成员只能是对象,不能是其他任何类型；
    - 兑现弱引用,垃圾回收机制不考虑WeakSet对对象的引用；
    - 没有size属性,不能遍历。
  - 方法：add，delete， has

- Map

  Map对象保存键值对。任何值都可以作为一个键或一个值。

  使用方法：new Map([iterable]) //iterable可以是数组或其他可迭代对象，或两个元素的数组

  属性：

  - length属性：0；
  - Map.prototype.constructor: 创建实例的原型，默认是Map函数；
  - Map.prototype.size: 返回对象键值对的数量。

  方法：

  - Map.prototype.set(key,value)：返回设置完对应键值的Map对象；
  - Map.prototype.clear(): 移除所有键值对；
  - Map.prototype.has("键名")：是否包含键对应的值；
  - Map.prototype.delete(key): 移除任何键相关联的值，删除成功返回true；
  - Map.prototype.get(key): 返回键对应的值；没有键返回undefined。
  - 遍历方法：
    - keys()/values()/entries(): 按插入顺序返回键名/键值/键值对；
    - forEach(callback[,thisArg]): 为每一键值对执行一次callback函数，thisArg每次回调的this指向(可选)。

  应用：

  Map与Array相互转换

  ```js
  let myMap = new Map()
  myMap.add("key","value")
  myMap.add(1, "value")
  [...myMap];  //["key","value"][1,"value"]
  //Array转Map
  cosnt arr = new Map([["key","value"],[1,"value"]) //Map{"key"=>"value",1=>"value"}
  ```

  Map与Object相互转换

  ```js
  //Map转object
  function mapToObject(myMap){
    let obj = Object.creat(null);
    for(let [k,v] of myMap){
     obj[k] = v
    }
    return obj
  }
  var myMap = new Map()
  myMap.set("key","value")
  myMap.set(1,"value")
  
  mapToObject(myMap) //Object {1: "value", key: "value"}
  
  //Object转Map
  function objectToMap(obj){
    let myMap = new Map()
    for(let k of Object.keys(obj)){
      myMap.set(k,obj[k])
    }
    return myMap
  }
  objectToMap({1: "value", key: "value"}) //Map{"1"=>"value","key"=>"value"}
  ```

  Map与JSON相互转换

  ```js
  //Map转JSON 
  //Map键名为字符串
  function mapTOJson(myMap){
    return JSON.stringify(mapToObject(myMap)) //{"1": "value", "key": "value"}
  }
  var myMap = new Map()
  myMap.set("key","value")
  myMap.set(1,"value") 
  mapToJson(myMap)
  //Map键名为非字符串
  function mapToArrayJson(map){
    return JSON.stringify([...map]) //[["key","value"][1,"value"]]
  }
  mapToArrayJson(myMap)
  
  //JSON转Map
  // JSON对象
  function jsonToStrMap(jsonStr) {
    return objToStrMap(JSON.parse(jsonStr));
  }
  jsonToStrMap({"1": "value", "key": "value"}); //MapMap{"1"=>"value","key"=>"value"}
  
  // 整个JSON 是数组
  function jsonToMap(jsronStr) {
    return new Map(JSON.parse(jsronStr)); 
  }
  jsonToMap([["key","value"],[1,"value"]]); // Map {"1"=>"value","key"=>"value"}
  ```

- WeakMap

  - 与Map结构类似，用于生成键值对的集合。

  - 与Map的区别

    - 只接受对象作为键值(null除外)，用于生成键值对的集合
    - 键名所指向的对象不计入垃圾回收机制
    - 没有size, 不能遍历，不支持clear()

  - 应用

    ```js
    //以DOM节点作为键名的场景应用
    let myElement = document.getElementById('logo')
    let myWeakmap = new WeakMap()
    
    myWeakmap.set(myElement,{timeClicked:0})
    myElement.addEventListener('click',function(){
      let logoData = myWeakmap.get(myElement)
      logoData.timeClick++
    },false)
    
    //部署私有属性
    const _counter = new WeakMap();
    const _action = new WeakMap();
    
    class Countdown {
      constructor(counter, action) {
        _counter.set(this, counter);
        _action.set(this, action);
      }
      dec() {
        let counter = _counter.get(this);
        if (counter < 1) return;
        counter--;
        _counter.set(this, counter);
        if (counter === 0) {
          _action.get(this)();
        }
      }
    }
    
    const c = new Countdown(2, () => console.log('DONE'));
    
    c.dec()
    ```

    

#### 字符串

- 新增的方法
  - startsWith(str，index)：返回布尔值，str是否在源字符串的头部；
  
  - endsWith(str，index)：返回布尔值，str是否在源字符串的尾部；
  
  - includes(str，index): 返回布尔值，表示是否找到了str。
  
    > 以上方法index可选，表示搜索开始位置。ends针对前index个字符。
    >
    > ```js
    > let str = "https://www.baidu.com";
    > if(str.startsWith("http://")){
    >   console.log("普通网址");
    > }else if(str.startsWith("https://")){
    >   console.log("加密网址");
    > }else if(str.startsWith("git://")){
    >   console.log("git地址");
    > }else{
    >   console.log("其他")；
    > }
    > ```
  
  - repeat(num)：返回将字符串重复num次返回。
  
  - padStart(targetLength,str)
  
  - padEnd(targetLength,str)
  
    > targetLength目标字符串长度；str用str补齐
    >
    > ```js
    > 'a'.padStart(3, 'bb')  --> bba
    > 'a'.padEnd(7, 'bb')   -->abbbbbb
    > ```
  
- 对Unicode 的支持：codePointAt()、fromCodePoint()

  ​	在ES5中，常使用charAt() 来表示字符存储位置，用charCodeAt() 来表示对应位置字符Unicode 的编码。在JavaScript 内部，字符以UTF-16 的形式存储，每个字符固定为2个字节，对于那些需要4个字节存储的字符并不支持。因此，ES6使用 codePointAt() 方法来支持存储4字节的字符：

  ```jsx
  var s = '猿';
  console.log(s.codePointAt(0)); // 29503
  ```

  ​    在 ES5 中，从码点返回对应的字符的方法是 fromCharCode()，这个并不能返回字符为32位的utf-16 的字符。ES6 中使用 String.fromCodePoint() 代替：

  ```jsx
  console.log(String.fromCodePoint(29503)); // 猿
  ```
- 字符串模板 --> 字符串连接

  ```js
  let a = 12;
  let sr = `
     <p>${a}</p>
  `;
  ```
  
- 字符串的遍历接口： for..of...

  ```rust
  for(let str of 'asd') {
     console.log(str);
  }
  // a
  // s
  // d
  ```

#### 对象

- 属性简写

  ```js
  var age = 18
  var person = {
    age,    //等同于age:age,
    name: '小明'  
  }
  ```

- 方法简写

  ```js
  var person = {
   showName()  { 
   }
   //等同于 showName:function(){}
  }
  ```

- 属性名表达式

  ```js
  var result = {
    "type": 'score',  //result["type"]
    [value]: 80,		//result[value]
    ['showValue'](){  //result.showValue()
    }
     //result.showValue.name == showValue;
  }
  ```

- 其他方法

  - Object.is() 判断两数值是否严格相等

    ```js
    Object.is(+0, -0);  //false
    Object.is(NaN, NaN); //true
    ```

  - Object.assign() 将源对象sourceN所有可枚举属性复制到目标对象target。

    ```js
    Object.assign(target, source1, source2, ..., sourceN)；
    //后来的被覆盖，不是对象的被转成对象，null/undefined不能放置在目标位置上。
    //字符串会以数组形式复制到对象，其他值无效果
    let a1 = 'yuan';
    let a2 = true;
    let a3 = 11;
    let a4 = NaN;
    Object.assign({}, a1, a2, a3, a4); // {0: "y", 1: "u", 2: "a", 3: "n"}
    
    //只是浅复制：尽量规避同名属性替换问题.
    ```

  - Object.keys()：返回对象自身的所有可枚举属性的键名数组。

  - for...in 循环：只遍历对象自身的和继承的可枚举性。

#### 面向对象

面向对象：
					1.多了个class关键字，构造器和类分开
					2.class中直接放方法

```js
//-----new nethods:
// 【1】声明/构造/使用
class User {
  constructor(name,pass){
    this.name = name;
    this.pass = pass;
  }
				
  showName(){
    console.log(this.name);
  }
				
  showPass(){
    console.log(this.pass)
  }
}
			
var u2 = new User('red','4321');
u2.showName();
u2.showPass();
			
// 【2】继承
class VipUser extends User{			
  constructor(name,pass,level){
    super(name,pass);
    this.level = level;
  }
				
  /* constructor(...arg1){
    super(...arg1);
  } */
  showLevel(){
    console.log(this.level);
  }
}
var v2 = new VipUser('black','54321','ss');
v2.showName();
v2.showPass();
v2.showLevel();

//----old methods :
//【1】声明/构造/使用
function Users(name, pass){
  this.name = name;
  this.pass = pass;
}
				
Users.prototype.showName=function(){
  console.log(this.name);
}
				
Users.prototype.showPass=function(){
  console.log(this.pass);
}
				
var u1 = new Users('blue','1234');
u1.showName();
u1.showPass();
// 【2】继承
function VipUsers(name,pass,level){
  Users.call(this,name,pass);
  this.level = level;
}
				
VipUsers.prototype = new Users();
				
VipUsers.prototype.constructor = VipUsers;
				
VipUsers.prototype.showLevel = function (){
  console.log(this.level);
}
var v1 = new VipUsers('green','12345','ss')
v1.showName()
v1.showPass()
v1.showLevel()
```

#### JSON对象
- JSON.stringify

- JSON.parse

- 简写 名字一样  名字跟值一样 留一个
  						方法 show(){~~~}

  标准写法：
  	1.只能用双引号
  	2.所有名字都必须用引号包起来

```js
let json = {a:12,b:15};
let str = 'http://www.baidu.com/user?data='+encodeURIComponent(JSON.stringify(json)) ;
console.log(str);
```

#### 迭代器(Iterator)与for...of

- 迭代器

  ​     迭代器是带有特殊接口的对象。含有一个next()方法，调用返回一个包含两个属性的对象，分别是value和done，**value表示当前位置的值**，**done表示是否迭代完**，当为true的时候，调用next就无效了。迭代器可以统一处理所有集合数据的方法迭代器是一个接口，只要该数据结构暴露了一个iterator的接口，就可以完成迭代，Iterator接口主要供for...of消费。

  - 默认Iterator接口：部署在数据结构的Symbol.iterable属性，即该数据结构只要有可遍历(iterable)的数据，就说是可遍历的。
  - 调用了iterator的场合：解构赋值、扩展运算符、Generator 函数中的 yield* 表达式、for...of、Array.from、Map()、Set()、Promise.all()、Promise.race()

- 遍历器 for...of（可以配合continue,return,break使用，几乎可遍历所有数据）

  **遍历器是一种线性处理，对于任何非线性的数据结构，部署遍历器接口就等于部署一种线性变换**。

  > 支持for...of的原生数据结构
  >
  > - Array
  >
  > - Map
  >
  > - Set
  >
  > - String
  >
  > - **TypedArray（一种通用的固定长度缓冲区类型，允许读取缓冲区中的二进制数据）**
  >
  > - 函数中的 arguments 对象
  >
  > - NodeList 对象
  >
  > - 对象的遍历需要开发者指定。
  >
  >   ```js
  >   //对象遍历
  >   function Obj(value) {
  >     this.value = value;
  >     this.next = null;
  >   }
  >   Obj.prototype[Symbol.iterator] = function() {
  >     var iterator = {
  >       next: next
  >     };
  >     var current = this;
  >     function next() {
  >       if (current) {
  >         var value = current.value;
  >         current = current.next;
  >         return {
  >           done: false,
  >           value: value
  >         };
  >       }else {
  >         return {
  >           done: true
  >         };
  >       }
  >     }
  >     return iterator;
  >   }
  >   var one = new Obj(1);
  >   var two = new Obj(2);
  >   var three = new Obj(3);
  >   one.next = two;
  >   two.next = three;
  >   for (var i of one) {
  >     console.log(i);
  >   }
  >   // 1
  >   // 2
  >   // 3
  >   ```

- 应用迭代器

  - 斐波那契数列
    定义一个数组迭代器，每次都迭代输出一个新的值，直到斐波那契数列有两个运行前提，第一个前提是初始化的前两个数字为0，1,第二个前提是将来的每一个值都是前两个值的和。

  ```js
  var it = { [Symbol.iterator]() {
      return this
    },
    n1: 0,
    n2: 1,
    next() {
      let temp1 = this.n1,
      temp2 = this.n2;
      [this.n1, this.n2] = [temp2, temp1 + temp2]
      return {
        value: temp1,
        done: false
      }
    }
  }
  
  for (var i = 0; i < 20; i++) {
    console.log(it.next())
  }
  ```

  - 任务队列迭代器
    定义一个任务队列，该队列初始化时为空，将待处理的任务传递后，传入数据进行处理。第一次传递的数据只会被任务１处理，第二次传递的只会被任务２处理… 

  ```js
  var Task = {
    actions: [],
    [Symbol.iterator]() {
      var steps = this.actions.slice();
      return { [Symbol.iterator]() {
        return this;
        },
        next(...args) {
          if (steps.length > 0) {
            let res = steps.shift()(...args);
            return {
              value: res,
              done: false
            }
           } else {
             return {
               done: true
             }
           }
        }
      }
    }
  }
  
  Task.actions.push(function task1(...args) {
    console.log("任务一：相乘") 
    return args.reduce(function(x, y) {
      return x * y
    })
  },function task2(...args) {
    console.log("任务二：相加") return args.reduce(function(x, y) {
      return x + y
    }) * 2
  },function task3(...args) {
    console.log("任务三：相减") return args.reduce(function(x, y) {
      return x - y
    })
  });
  
  var it = Task[Symbol.iterator]();
  console.log(it.next(10, 100, 2));
  console.log(it.next(20, 50, 100)) console.log(it.next(10, 2, 1))
   /*
  任务一：相乘 {
    "value": 2000,
    "done": false
  }任务二：相加 {
    "value": 340,
    "done": false
  }任务三：相减 {
    "value": 7,
    "done": false
  }*/
  ```

  - 延迟执行
     假设我们有一个数据表，我们想按大小顺序依次的获取数据，但是我们又不想提前给他排序，有可能我们根本就不去使用它，所以我们可以在第一次使用的时候再排序，做到延迟执行代码：

  ```js
  var table = {
    "d": 1,
    "b": 4,
    "c": 12,
    "a": 12
  }
  table[Symbol.iterator] = function() {
    var _this = this;
    var keys = null;
    var index = 0;
    return {
      next: function() {
        if (keys === null) {
          keys = Object.keys(_this).sort();
        }
        return {
          value: keys[index],
          done: index++>keys.length
        };
      }
    }
  }
  
  for (var a of table) {
    console.log(a)
  } 
  // a b c d
  ```

#### Promise

Promise对象用于**异步操作**，它表示一个尚未完成且预计在未来完成的异步操作。

-->**异步操作可以一起执行多个任务，函数调用后不会立即返回执行的结果。**

-->**异步任务总是在当前脚本执行完同步任务后才执行任务**。

 --> **回调地狱**

```js
reqest('1.html','',function(data1){
  console.log("第一次请求成功，返回了"，data1)
  request('2.html',data1,function(data2){
    console.log('第二次请求成功，返回了'，data2)
    request('3.html',data2,function(data3){
      console.log('第三次请求成功，返回了',data3)
      ……………………
    },function(error3){
      console.log('第三次请求失败，失败信息', error3)
    })
  },function(error2){
    console.log("第二次请求失败, 失败信息", error2)
  })
},function(error1){
  console.log('第一次请求失败，失败信息', error1)
})
```

以上代码出现多层嵌套，而且只是简单地打印了一下请求数据和失败数据，看起来既冗余又不利于编程和维护。

--> **promise.then链式回调**

```js
sendRequest('1.html','').then(function(data1){
  console.log('第一次请求成功，返回了', data1)
}).then(function(data2){
  console.log('第二次请求成功，返回了', data2)
}).then(function(data3){
  console.log('第三次请求成功，返回了', data3)
}).catch(function(error){
  //用catch捕捉前面的错误
  console.log('请求失败', error)；
})
```

- Promise基本用法

  Promise三种状态：

  - Pending --> 进行中,初始状态
  - Fulfilled  --> 操作成功
  - Rejected --> 操作失败

  状态由Pending改变为Fulfilled或者Rejected之后，会**凝固**，不会再发生变化。状态改变时，then绑定的函数会被调用。

- Promise基本方法

  - .then()

    > 语法：Promise.prototype.then(onFulfilled, onRejected)
    >
    > ​     Promise对象状态发生改变时根据改变后状态选择回调函数onFulfilled/onRejected执行，执行后返回新的Promise对象。如上then链式回调中`.then.then`，上一个then结束的Promise发生变化下一个then重新选择执行……。

  - .catch()

    > 语法：Promise.prototype.catch(onRejected)
    >
    > ​     捕获操作失败时发生的错误; catch能捕捉链式回调中前面所有then方法抛出的错误，捕获catch发生的错误需要在后面接一个catch。
    >
    > ```js
    > //其他捕获写法
    > var promise = new Promise(function(resolve, reject){
    > throw new Error('错误')
    > })
    > 
    > var promise = new Promise(function(resolve,reject){
    > reject(new Error('错误'));
    > })
    > ```
    >
    > ```js
    > //状态凝固，当Promise先请求成功了，状态不会变为rejected，不会被catch
    > var promise = new Promise(function(resolve, reject) {
    > resolve();
    > throw 'error';
    > });
    > 
    > promise.catch(function(e) {
    > console.log(e);      //This is never called
    > });
    > 
    > 
    > //在回调函数前抛异常
    > var p1 = { 
    > then: function(resolve) {
    >  throw new Error("error");
    >  resolve("Resolved");
    > }
    > };
    > 
    > var p2 = Promise.resolve(p1);
    > p2.then(function(value) {
    > //not called
    > }, function(error) {
    > console.log(error);       // => Error: error
    > });
    > 
    > //在回调函数后抛异常
    > var p3 = { 
    > then: function(resolve) {
    >  resolve("Resolved");
    >  throw new Error("error");
    > }
    > };
    > 
    > var p4 = Promise.resolve(p3);
    > p4.then(function(value) {
    > console.log(value);     // => Resolved
    > }, function(error) {
    > //not called
    > });
    > ```
    >
    > ​     如果没有使用 catch 方法指定处理错误的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应（chrome 会抛错，Safari 和 Firefox 不抛错），这是 Promise 的一个缺点。

  - .all()

    > 语法：Promise.all(iterable)
    >
    > ​    Promise.all 方法接受一个数组（或具有 Iterator 接口的对象）作为参数，数组中的对象（[p1, p2, p3）均为 Promise 实例（如果不是一个 Promise，该项会被用 Promise.resolve 转换为一个 Promise），实例是同时开始、并行执行的，它的状态由这个上 Promise 实例决定。
    >
    > ```js
    > var p1 = new promise(function(resolve,reject){
    >     setTimeout(resolve, 3000, 'first')
    > })
    > var p2 = new promise(function(resolve,reject){
    >     resolve('second')
    > })
    > var p3 = new promise(function(resolve,reject){
    >     setTimeout(resolve, 1000, 'third')
    > })
    > //当三个实例的状态都变为Fulfilled，p的状态菜户变为 Fulfilled，并将三个Promise返回的结果，按照参数的顺序（而不是resolved的顺序）存入数组，传给p的回调函数。
    > Promise.all([p1, p2, p3]).then(function(values){
    >     console.log(values)
    > })
    > 
    > var p4 = new promise(function(resolve,reject){
    >     setTimeout(resolve, 3000, 'one')
    > })
    > var p5 = new promise(function(resolve,reject){
    >     reject('two')
    > })
    > var p6 = new promise(function(resolve,reject){
    >     setTimeout(reject, 1000, 'three')
    > })
    > //- 当三个实例中有一个装填为 rejected，p 的状态也会变为 rejected，并把第一个 reject 的 Promise 的返回值，传给 p 的回调函数.
    > Promise.all([p4, p6, p5]).then(function(data){
    >     console.log('resolve', data)
    > },function(error){
    >     console.log('reject',error) //==>reject two
    > })
    > ```

  - .race()

    > 语法：Promise.race(iterable)
    >
    > ​     参数参考all()方法，Promise.race()将多个Promise实例打包为一个实例，参数中任一实例状态发生变化，总实例状态跟着改变，并将第一个改变状态的参数实例的返回值传给p的回调函数。

  - .reaolve()

    > 语法：Promise.resolve(value)
    >
    > ​			Promise.resolve(promise)
    >
    > ​			Promise.resolve(thenable)
    >
    > 让Promise对象立即进入Fulfilled状态，返回新的Promise对象
    >
    > ```js
    > Promise.resolve('Success');//可以将带then方法的对象转换为Promise对象，支持后接.then
    > //等同于
    > new Promise(function (resolve) {
    >     resolve('Success');
    > });
    > ```

  - .reject()

    > 同上resolve
    >
    > 状态转为rejected

- 其他方法

  - .done()

    > 语法：asyncFunc().then(f1).catch(r1).then(f2).done();
    >
    > 总是处于回调链尾端，保证抛出任何可能出现的错误。

  - .finally()

    > 可以接受一个普通回调函数做参数，不管怎样都必须执行。

- 注意

  - Promise一旦创建，无法中途取消；
  - 如果不设置回调函数，内部抛错不会反应到外部；
  - 处于pending状态不能确定现在进行到哪里。
  - 构造器中书写的逻辑，即使发生意外输出，rejected状态绝大多数情况能正常返回。在没有明确throw时，最好使用reject。

- 自测： https://zhuanlan.zhihu.com/p/30797777

#### Generator函数

Generator即生成器，生成器对象由Generator 函数返回。

> 语法： 
>
> ```js
> function* gen() {
>   yield 1;
>   yield 2;
>   yield 3;
> }
> var g = gen(); //"Generator{ }"
> ```

- 生成器函数特征：

  - `*`  在function关键字与函数名之间(空格一般没有特殊要求，等多的是如上语法所示)；

  - 函数内部使用了yield表达式，用于定义Generator函数中的状态；

  - 调用Generator函数时，不会立即执行，返回的是Iterator对象，通过调用next()方法依次遍历每个状态;

    ```js
    function *gen () {
      yield 1
      yield 2
      return 3
    }
    
    const g = gen()   // Iterator对象
    //每次调用next()返回value-done ,可参看iterator中next
    //value没有返回值或者为undefined,说明函数运行结束
    g.next() // {value: 1, done: false}
    g.next() // {value: 2, done: false}
    g.next() // {value: 3, done: true}
    g.next() // {value: undefined, done: true}
    ```

- 使用Generator

  - yield 表达式：定义内部状态或暂停执行

    - yield表达式用在另一个表达式中需加上（），做函数参数和语句时可以不使用（）

      ```js
      function *gen () {
        console.log('hello' + yield) // ×
        console.log('hello' + (yield)) // √
        console.log('hello' + yield '凯斯') // ×
        console.log('hello' + (yield '凯斯'))  //√
        foo(yield 1)  //√
        const param = yield 2 // √
      }
      ```

  - yield* 表达式: 在一个Generator函数调用另一个Generator函数时使用

    - 如下代码不能实现

      ```js
      function *foo () {
        yield 1
      }
      function *gen () {
        yield foo()
        yield 2
      }
      const g = gen()
      g.next()   // {value: Generator, done: false}
      g.next()   // {value: 2, done: false}
      g.next()   // {value: undefined, done: true}
      ```

    - yield* 可以看做是for...of

      ```js
      function *foo () {
        yield 1
      }
      function *gen () {
        yield* foo()
        yield 2
      }
      /*等同于
      function* gen(){
        for(let item of foo()){
          yield item
        }
        yield 2
      }
      */
      
      const g = gen()
      g.next()   // {value: 1, done: false}
      g.next()   // {value: 2, done: false}
      g.next()   // {value: undefined, done: true}
      ```

    - yield* 可以遍历具有Iterator接口的数据类型

    - yield* 取出嵌套数组中的成员

      ```js
      // 普通方法
      const arr = [1, [[2, 3], 4]]
      const str = arr.toString().replace(/,/g, '') //1234
      for (let item of str) {
        console.log(+item)      // 1, 2, 3, 4
      }
      
      // 使用yield*表达式
      function *gen (arr) {
        if (Array.isArray(arr)) {
          for (let i = 0; i < arr.length; i++) {
            yield* gen(arr[i])
          }
        } else {
          yield arr
        }
      }
      const g = gen([1, [[2, 3], 4]])
      for (let item of g) {
        console.log(item)       // 1, 2, 3, 4
      }
      ```

  - next作用

    - next的作用是分段执行Generator函数
    - 每次调用next从函数头/上一次停止的地方开始，直到遇见下一个yield或return语句停止，返回包含value和done的对象。

  - next执行逻辑

    - ①遇到yield表达式，暂停执行后面的操作，并将紧跟在yield表达式后面的那个表达式的值，作为返回的对象的value属性值。
    - ②下一次调用next方法时，再继续往下执行，遇到yield表达式返回①
    - ③如果没有再遇到新的yield表达式，继续向下运行，直到遇到return语句为止，将return语句后面表达式的值，作为返回的对象的value属性值。
    - ④如果该函数没有return语句，则返回的对象的value属性值为undefined。

  - next方法参数
    - yield语句本身没有返回值，返回的总是上一个yield句的值，总会返回undefined
    - 当next传入参数时，参数会被当做上一个yield语句的返回值
  - **yield语句表达式向函数外传递数据，next()向函数内传递值**

- 遍历Generator对象

  任何一个对象的Symbol.iterator属性都指向默认的遍历器对象生成函数，Generator函数也是遍历器对象生成函数，所以将Generator函数赋值给Symbol.iterator属性，使得对象具有Iterator接口，从而可以被遍历。

  ```js
  const person = {
    name: '小明',
    height: 180
  }
  function* gen () {
    const arr = Object.keys(this)
    for (let item of arr) {
      yield [item, this[item]]
    }
  }
  person[Symbol.iterator] = gen
  for (let [key, value] of person) {
    console.log(key, value)   // name 小明 , height 180
  }
  //省去next遍历
  function* gen2(){
      yield 1
      yield 2
      yield 3
      return 4
  }
  for(let item of gen2()){
    console.log(item) //1 2 3 4 
  }
  ```

- 相关应用

  - 协程 ：指多个线程相互协作，完成异步任务

    ES6中实现协程，其最大特点是可以交出函数的执行权

    整个Generator函数是一个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地方只需都用yield语句注明即可。

    ```js
    function* gen() {
      var y = yield x + 2;
      return y;
    }
    var g = gen(1);
    g.next(); // { value: 3, done: false };
    g.next(); // { value: undefined; done: true }
    ```

  - Thunk函数：自动执行Generator函数的方法

    起源于“求值策略”中的“传名调用”，即函数的参数应在执行时求值。**将参数放到一个临时函数之中，再将这个临时函数传入函数体。这个临时函数就叫做 Thunk 函数。**

    ```
    //ES5 版本转换器
    var Thunk = function(fn) {
      return function () {
        var args = Array.prototype.slice.call(arguments);
        return function (callback) {
          args.push(callback);
          return fn.apply(this, arg);
        }
      }
    };
    
    // ES6 版本转换器
    const Thunk = function(fn) {
      return function(...args) {
        return function(callback) {
          return fn.call(this, ...args, callback);
        }
      }
    }
    ```

    ```js
    //以下使用了node.js模块fs读文件方法，做多参数函数实例
    fs.readFile(fileName, callback);
    
    // Thunk版本的readFile（单参数版本）
    var readFileThunk = Thunk(fs.readFile);
    readFileThunk(fileName)(callback);
    ```

    **自动执行Generator函数**:yield后面接的异步操作必须是Thunk函数

    ```js
    function run(fn) {
      var gen = fn();
      function next(err, data) {
        var result = gen.next(data);
        if(result.done) return;
        result.value(next);
      }
      next();
    }
    function* g() {
      ...
    }
    
    run(g);
    ```

  - Co模块：将Thunk函数与Promise对象包装成为一个模块

    前提：yield后必须是Thunk函数或Promise对象

    - 并发操作放在数组里

    ```js
    co(function* () {
      var res = yield [
        Promise.resolve(1),
        Promise.resolve(2)
      ];
      console.log(res);
    }).catch(onerror);
    ```

    - 并发操作写在对象里

    ```js
    co(function* () {
      var res = yield {
        1: Promise.resolve(1),
        2: Promise.resolve(2)
      };
      console.log(res);
    }).catch(onerror);
    ```

    - 处理多个 generator 异步操作

    ```js
    co(function* () {
      var valuse = [n1, n2, n3];
      yield values.map(somethingAsync);
    });
    
    function* somethingAsync(x) {
      // do something async
      return y;
    }
    //允许并发 3 个 somethingAsync 异步操作
    ```

#### async函数

- async函数的优势

  - 内置执行器: async 函数自带执行器，不需要像 Generator 函数需要 co 模块实现自动流程管理。像调用普通函数一样：asyncReadFile() ，只要一行;
  - 更好的语义性：async 和 await 比起星号和 yield，语义更清楚。async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果;
  - 更广的适用性: co 模块约定，yield 命令后面只能是 Thunk 函数 或 Promise对象，而 async 函数的 await 命令后面，可以是 Promise 对象和原始类型的值(原始值相当于同步操作)；
  - 返回值是Promise：可用then指定下一步操作。

  - **可以认为：async 函数是 Generator 函数的语法糖，是由多个异步操作包装成的一个 Promise 对象，而 await 命令就是内部 then 命令的语法糖。**

- 用法：

  ```js
  async function name([param[, param[, ... param]]]) { statements }
  ```

  ```js
  //使用场景
  // 函数声明
  async function foo() {};
  
  // 函数表达式
  const = foo = async function() {};
  
  // 对象的方法
  let obj = { async foo() {} };
  obj.foo().then(...);
  
  // calss 的方法
  class Storage {
    constructor() {
      this.cachePromise = caches.open('yuan');
    }
  
    async getYuan(name) {
      const cache = await this.cachePromise;
      return cache.match(`/yuan/${ name } .png`);
    }
  }
  
  const storage = new Storage();
  storage.getYuan('monkey').then(...);
  ```

- 返回Promise对象

  async函数内部return语句返回的值可以作为回调函数的参数。

  ```js
  async function helloAsync(){
    return "helloAsync";
  }
    
  console.log(helloAsync())  // Promise {<resolved>: "helloAsync"}
   
  helloAsync().then(v=>{
    console.log(v);         // helloAsync
  })
  ```

- Promise对象的状态变化

  只有async 函数内部的 await 命令执行完，才会执行 then 方法指定的回调函数。

- await命令

  - await 操作符用于等待一个 Promise 对象, 它只能在异步函数 async function 内部使用

    > await针对所跟不同表达式的处理方式：
    >
    > - Promise 对象：await 会暂停执行，等待 Promise 对象 resolve，然后恢复 async 函数的执行并返回解析值。
    > - await 命令后的 Promise 对象如果有一个变为 reject 状态，则整个 async 函数都会中断， reject 的参数会被 catch 方法的回调函数接收到。
    > - 非 Promise 对象：直接返回对应的值，即自动转为一个resolve的Promise对象。

- 注意点

  - await 关键字仅在 async function 中有效。如果在 async function 函数体外使用 await ，你只会得到一个语法错误。

  - await 命令后面的 Promise 对象运行的结果可能是 reject 的状态，所以最好把 await 命令放在 try...catch 代码块里，或者在await 命令后加一个 catch 方法：

    ```js
    // await 命令写在 try...catch 
    async function f() {
      try {
        await somePromise();
      }
      catch(err) {
        console.log(err);
      }
    }
    
    // 在 await 后加 catch
    async function f() {
      await somePromise()
      .catch (function (err) {
        console.log(err)
      });
    }
    ```

  - 多个 await 后面的异步操作如果不存在继发关系，最好让他们同时出发

    ```js
    let foo = await getFoo();
    let bar = await getBar();
    ```

  - 多个请求并发执行，可以使用 Promise.all 方法

    ```js
    async function dbFunc(db) {
      let docs = [{}, {}, {}];
      let promises = doc.map((doc) = > db.post(doc));
    
      let results = await Promise.all(promises);
      console.log(results);
    }
    ```

- 异步应用对比：Promise、Generator、async

  以 setTimeout 来模拟异步操作：

  ```jsx
function foo (obj) {
    return new Promise((resolve, reject) => {
      window.setTimeout(() => {
        let data = {
          height: 180
        }
        data = Object.assign({}, obj, data)
        resolve(data)
      }, 1000)
    })
  }
  function bar (obj) {
    return new Promise((resolve, reject) => {
      window.setTimeout(() => {
        let data = {
          talk () {
            console.log(this.name, this.height);
          }
        }
        data = Object.assign({}, obj, data)
        resolve(data)
      }, 1500)
    })
  }
  ```
  
  两个函数都返回了 Promise 对象，使用 Object.assign() 方法合并传递过来的参数。

  ```js
  // Promise 实现异步：
//此方法的缺点：语义不好，不够直观，使用多个 then 方法，有可能出现回调地狱。
  function main () {
  return new Promise((resolve, reject) => {
      const data = {
        name: 'keith'
      }
      resolve(data)
    })
  }
  main().then(data => {
    foo(data).then(res => {
      bar(res).then(data => {
        return data.talk()   // keith 180
      })
    })
  })
  ```
  
  ```js
  // Generator 函数实现异步：
//Generator 函数相较于 Promise 实现异步操作，优点在于：异步过程同步化，避免回调地狱的出现。缺点在于：需要借助run 函数或者 co 模块实现流程的自动管理。
  function *gen () {
    const data = {
    name: 'keith'
    }
  const fooData = yield foo(data)
    const barData = yield bar(fooData)
    return barData.talk()
  }
  function run (gen) {
    const g = gen()
    const next = data => {
      let result = g.next(data)
      if (result.done) return result.value
      result.value.then(data => {
        next(data)
      })
    }
    next()
  }
  run(gen)
  ```
  
  ```js
  // async 函数实现异步：
  //async 函数优点在于：实现了执行器的内置，代码量减少，且异步过程同步化，语义化更强。
  async function main () {
    const data = {
    name: 'keith'
    }
    const fooData = await foo(data)
  const barData = await bar(fooData)
    return barData
}
  main().then(data => {
    data.talk()
  })
  ```
  
- 其他应用

  ```jsx
  //从豆瓣API上获取数据：
  var fetchDoubanApi = function() {
      return new Promise((resolve, reject) = >{
          var xhr = new XMLHttpRequest();
          xhr.onreadystatechange = function() {
              if (xhr.readyState === 4) {
                  if (xhr.status >= 200 && xhr.status < 300) {
                      var response;
                      try {
                          response = JSON.parse(xhr.responseText);
                      } catch(e) {
                          reject(e);
                      }
                      if (response) {
                          resolve(response, xhr.status, xhr);
                      }
                  } else {
                      reject(xhr);
                  }
              }
          };
          xhr.open('GET', 'https://api.douban.com/v2/user/aisk', true);
          xhr.setRequestHeader("Content-Type", "text/plain");
          xhr.send(data);
      });
  };
  (async function() {
      try {
          let result = await fetchDoubanApi();
          console.log(result);
      } catch(e) {
          console.log(e);
      }
  })();
  ```

  ```jsx
  //根据电影名称，下载对应的海报**
  import fs from 'fs';
  import path from 'path';
  import request from 'request';
  var movieDir = __dirname + '/movies',
  exts = ['.mkv', '.avi', '.mp4', '.rm', '.rmvb', '.wmv'];
  // 读取文件列表
  var readFiles = function() {
      return new Promise(function(resolve, reject) {
          fs.readdir(movieDir,
          function(err, files) {
              resolve(files.filter((v) = >exts.includes(path.parse(v).ext)));
          });
      });
  };
  // 获取海报
  var getPoster = function(movieName) {
      let url = `https: //api.douban.com/v2/movie/search?q=${encodeURI(movieName)}`;
      return new Promise(function(resolve, reject) {
          request({
              url: url,
              json: true
          },
          function(error, response, body) {
              if (error) return reject(error);
              resolve(body.subjects[0].images.large);
          })
      });
  };
  // 保存海报
  var savePoster = function(movieName, url) {
      request.get(url).pipe(fs.createWriteStream(path.join(movieDir, movieName + '.jpg')));
  }; 
  (async() = >{
      let files = await readFiles();
      // await只能使用在原生语法
      for (var file of files) {
          let name = path.parse(file).name;
          console.log(`正在获取【$ {
              name
          }】的海报`);
          savePoster(name, await getPoster(name));
      }
      console.log('=== 获取海报完成 ===');
  })();
  ```