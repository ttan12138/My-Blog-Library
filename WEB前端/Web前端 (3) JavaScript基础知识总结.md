JavaScript基础知识



​	[JavaScrip](https://zh.wikipedia.org/wiki/JavaScript)对于网页的作用有提供网页特效、用户交互、表单验证、控制结构和样式。

​	JavaScript 是互联网上最流行的脚本语言，这门语言可用于 HTML 和 web，更可广泛用于服务器、PC、笔记本电脑、平板电脑和智能手机等设备。

​	JavaScript是一种[脚本语言](https://zh.wikipedia.org/wiki/脚本语言), 主要目的是动态的控制web标准中的结构和样式（结构、样式、行为) 。

##### 数据类型

 JS数据类型可以分为两大类：

1. 基本数据类型
   - 数字 number   1213
   
   - 字符串 string   "asd"
   
     > 字符串常用方法：
     >
     > ①length：字符串长度。
     >
     > ②indexOf(str, index)：文本首次出现str的索引 ,检索起始位置参数index可省略。
     >
     > ③lastIndexOf(str,index) : 文本最后一次出现str的索引 ,检索起始位置参数index可省略。
     >
     > ④search(str) : 文本首次出现str的索引，str可为正则表达式。
     >
     > ⑤slice / substring(ins1,ins2): ins1(包括开始位置)起始位置; ins2(不包括结束位置)不省略,返回ins1~ins2之间的字符串；省略则截取ins1以后的字符串。
     >
     > ⑥replace(str1, str2): 返回str2替换掉str1后的新字符串，若不区分大小写，应正则 /i
     >
     > ⑦substr(ins1,lenth): 第二参数为长度，若省略同⑤的省略。
     >
     > ⑧concat(str1, str2): 连接1和2,效果同“+”。
     >
     > ⑨trim():删除空格（低版本不支持，可用replace）
     >
     > ⑩split("？"): 以？将字符串分割成数组，“”表示分成单个字符
   
   - 布尔值 boolean  true/false
   
   - 特殊数据类型  null/undefined
   
2. 复合数据类型
   - 变量(数值可以改变的)
       ①声明变量 var  
       	var age=18;
       		--也可同时赋值 ==>初始化
       ②赋值
       	age=20
       ③同时定义多个变量 变量之间用逗号隔开
       	var name="ssss",age=18,zex="男"；
       
       > 变量命名规范
       >
       > - 不允许以数字开头
       > - 不允许使用关键字和保留关键字
       > - 由数字、字母、下划线和美元符号组成
       > - 尽可能的使用[小驼峰命名法](https://m.html.cn/qa/css3/13040.html)或者下划线命名
       
   - 标识符
       ①数字、字母、下划线和美元符号组成
       ②不能以数字开头
       ③标识符区分大小写
       ④标识符必须见名生意。
>  - 判断变量/常量数据类型
>     【格式】 typeof 变量/常量(JS是弱语言 变量赋值成什么类型就是什么类型)
>

>   ```js
>   var age = 18; //->Number
>   alert(typeof age); 
>   age="xxxx";   //-> String
>   alert(typeof age);
>   ```
>

##### 类型转换

1. 任何类型的数据和字符串类型数据相加操作，其他类形会自动转换成字符串类型。字符串拼接。此时+叫拼接符。
   
```js
	var temp ="你"+"好";
var temp2 = "你"+12;
alert(temp);
alert(temp2);
alert(typeof temp2); 
alert ("h"+undefined);
alert ("h"+undefined); 输出hundefined
```

	> 查看语法错误
	>   ① 火狐：fireBug 附加组件
	>   ② 谷歌  chome控制台

2. 任何数据除了和字符串进行相加运算外，会先将数据转换为字符串再进行计算。
	 ①与NaN做算术运算的结果始终都是NaN，包括NaN与NaN的结果=NaN.
	②字符串如果是纯数字字符串 转换成数字，否则转换成NaN
	
	```js
	var temp=1-"2";
	alert(temp); //打印结果为-1
	
	var temp=1-"2a";//2a转换为NaN
	alert(temp);  //打印结果为NaN
	```

>
>	判断NAN的方法： isNAN()函数
>	代码规范：
>	1.注意层级缩进  tab = 四个空格
>	2.; , 后面都跟一个空格 运算符 = + 前后都应该加空格
>	3.每一条语句后面都必须加; （分号）
>

3. 任何其他数据类型除了和字符串进行相加操作，与数字类型做算术运算时，其他数据类型都会自动的转换成数字。
   	其中true-->1，false-->0，null-->0，undefinrd -->NaN

##### 强数字数据类型转换函数
1. toString() 将任意类型转换为字符串

2. Boolean()将别的类型转换成布尔值

   ```js
   var temp = Boolean(0); //-->false
   var temp = Boolean(1); //-->true	
   var temp = Boolean(""); //-->false
   var temp = Boolean("hello"); //-->true
   
   var temp = Boolean(null); //-->false
   var temp = Boolean(undefined); //-->false
   ```

3. Number()将别的数据类型转换为数字
    ①布尔值 true-->1 false-->0
      	②字符串 传数字字符串-->对应数字，否则NaN
      	③特殊数据类型 null-->0   undefined-->NaN

4. parseInt() 取整，兼容Number的功能

   ```js
   var temp = Number("20a"); //-->NaN
   var temp = parseInt("20a"); //-->20
   var temp = paseInt(3.14); //-->3
   ```

5. parseFloat() 取浮点数，可以将别的数据转换为带小数的数字

   ```js
   var temp = parseFloat("3.14"); //-->3.14
   var temp = 1 / 0; //--> Infinity （无穷大）
   var temp = -1 / 0; //--> -Infinity （无穷小）
   ```

##### 运算符

1. 算术运算符（+,-,*,/,%,++,--）

   ①a++;  先取a的值，在进行+1，a--同

   ```js
   var a = 5;
   alert(a++); //-->5
   alert(a);   //-->6
   ```

   ②++a; 先进行+1，再取a的值，--a同

   ```js
   var a = 5;
   alert(++a);  //-->6
   alert(a);  //-->6
   ```

2. 关系(比较)运算符 (>,<,>=,<=,==,===,!=,!==)
   ①如果两个操作数都是数值，则数值比较
   ②如果两个操作数都是字符串，则比较两个字符串对应的字符编码值(ASCII码表 )。逐位比较，直到能比较出大小。
   ③两个操作数中有一个是数值，则将另一个转换成数值，再进行比较。

   > 关系运算符 =  和 != 
   > ①一个操作数为布尔值，则比较前将其转换为数值.
   > ②如果其中一个为字符串，则比较之前将其转换为数值.
   > ③如果其中一个为NaN，则== 返回 false ,!= 返回 true.NaN自身不相等
   > ④在 === 和 !== ,如果值和类型都相等，才返回true 否则返回false	
   
4. 逻辑运算符 &&,|| 的短路操作
   
   ```js
   alert(5 < 3 && alert(a))；//-->有结果且为false
   alert(5 > 3 || alert(a))；//-->有结果且为ture
   ```
   
5.  ！逻辑非运算: 事先将表达式转换为布尔值再取反
   ①空字符串 -->true 
   ②非空字符串 --> false
   ③操作数是任意非0的数值（包括Infinity） -->false
   ④操作数是0，返回true
   ⑤操作数是NaN,返回值是true
   ⑥操作数是undefined 返回值是true
   
5. [位运算]([https://baike.baidu.com/item/%E4%BD%8D%E8%BF%90%E7%AE%97/6888804?fr=aladdin](https://baike.baidu.com/item/位运算/6888804?fr=aladdin)) <<,>>,&,|,^

6. 三目（三元）运算符 return a > b ? a : b ;

##### 函数

1. 声明

   ```js
   //自定义
   function 函数名(参数列表) {
     //函数体 (可执行的语句)
   }
   //直接量声明
   var fun1 = function(参数列表) {
       //函数体
   }
   //关键字声明
   var fun2 = new Function(参数列表,
   {
       //函数体
   })
   /*参数列表：此处参数称形参，可以在函数体中直接使用；
   		多个参数用逗号发分开；
   		可以省略，即不引入参数声明；
   		可以设置默认值，如（a=2,b）。*/
   ```

2. 调用

   ```js
   函数名(参数值列表);
   /*参数值列表：此处参数称实参，可以在函数体中直接使用；尽量按照对应声明格式调用*/
   ```

3. 带返回值声明和调用

   ```js
   function 函数名(参数){
   	return 值;
   }
    var result = 函数名;
   ```


4. 函数体内变量声明提升：

   函数体内，声明的变量会提升到函数体最顶端，但不会提升赋值。

   ```js
   function fun(){
     console.log(num);
     var num = 20;
   }
   ==>
   function fun(){
     var num;
     console.log(num);
     num = 20;
   }
   //当出现如下情况时候
   var a = 10;
   f1();   //输出： undefined 9
   function f1(){
       //实际这里有var a;
    var b = 9;
    console.log(a);
    console.log(b);
    var a = '123';
   }
   ```

5. 函数对象方法：

   call()和apply()，这两个方法都是函数对象的方法，需要通过函数对象来调用, 主要设置函数体内this导向。

   - 当对函数调用时，call()和apply()都会调用函数执行
- 在调用call()和apply()可以将一个对象指定为第一个参数，此时这个对象将会成为函数执行时的this

​    apply：用另一个对象替换当前对象。例如：B.apply(A, arguments);即A对象应用B对象的方法。

​    call：用另一个对象替换当前对象。例如：B.call(A, args1,args2);即A对象调用B对象的方法。

​	具体参照下述this中例子。

##### 作用域

作用域简单来说就是一个变量的作用范围。在JS中作用域分成两种：

1. 全局作用域：直接在script标签中编写的代码都运行在全局作用域中

   ①在打开页面时创建，在页面关闭时销毁。

   ②全局作用域中有一个全局对象window，window对象由浏览器提供，可以在页面中直接使用，它代表的是整个的浏览器的窗口。

   ③在全局作用域中创建的变量都会作为window对象的属性保存，在全局作用域中创建的函数都会作为window对象的方法保存

   ④在全局作用域中创建的变量和函数可以在页面的任意位置访问。在函数作用域中也可以访问到全局作用域的变量。

   ⑤尽量不要在全局中创建变量	

2.函数作用域：是函数执行时创建的作用域。

​	①每次调用函数都会创建一个新的函数作用域。

​	②在函数执行时创建，在函数执行结束时销毁。

​	③在函数作用域中创建的变量，不能在全局中访问。

​	④当在函数作用域中使用一个变量时，它会先在自身作用域中寻找，如果找到了则直接使用，如果没有找到则到上一级作用域中寻找，一直找。

- 变量的声明提前
	- 在全局作用域中，使用var关键字声明的变量会在所有的代码执行之前被声明，但是不会赋值。
		所以我们可以在变量声明前使用变量。但是不使用var关键字声明的变量不会被声明提前。
	- 在函数作用域中，也具有该特性，使用var关键字声明的变量会在函数所有的代码执行前被声明，
		如果没有使用var关键字声明变量，则变量会变成全局变量
	
- 函数的声明提前
	- 在全局作用域中，使用函数声明创建的函数（function fun(){}）,会在所有的代码执行之前被创建，
		也就是我们可以在函数声明前去调用函数，但是使用函数表达式(var fun = function(){})创建的函数没有该特性
	- 在函数作用域中，使用函数声明创建的函数，会在所有的函数中的代码执行之前就被创建好了。

##### 递归

​	递归的使用规则：
​		1.首先找到临界值，无需计算获得的值
​		2.找出这一次与上一次的关系
​		3.假设当前函数已经可以使用了，调用自身计算上一次的运行结果，再写出这次的运行结果。		

```js
function sum(n){
  if(n==1){
    return 1;
  }
  return sum(n-1)+n;
}
alert(sum(100));
```

​		[斐波那契数列](https://zh.wikipedia.org/wiki/斐波那契数列)：function(n) = function(n-3) + function(n-1);

##### 数组

1. 创建数组

   ```js
   var arr02 = new Array(10);//创建长度为10的数组
   var arr = new Array(1, true, "hello");
   var arr03 = Array(1, true, "hello");
   var arr04 = [1, true, "hello"];
   ```

2. 通过下标来访问

3. 遍历

   ```js
   var arr = [];
   
   for(var i = 0; i < 10; i++){
     arr[i] = i * i;
   }
   
   for(var 变量 in 数组){
       console.log(变量);
   }
   ```

4. 常用方法

   - 栈：先进后出
     ①push()
         格式:数组.push(元素);
         功能：给数组末尾添加数组
         参数：添加元素、参数的个数随意
         res返回值：添加完元素之后的长度
         //如下 var res = arr.push(11, 22, 33, 44);
         //alert(res);					

     ②pop()
         格式：数组.pop();
         功能：移除数组末尾的最后一个元素
         返回值：移除的尾元素
         //如下 var res = arr.push();
         // alert(res);			

	- 队列：一头进，另一头出
           先进先出
       ①后进入方法 push()
       ②出去方法 shift()
           格式：数组.shift();
           功能：从数组的头部取下一个元素
           返回值：取下的头元素		
       ③前进入方法 unshift()
           格式：数组.unshift(元素)；
           功能：从数组头部插入元素，个数随意
           返回值：插入之后的长度同上res
       
       > concat():见上字符串方法
       >
       > join("?"): 把数组按照 ？转换为字符串，“”不做插入转换 
       >
       > split():见上字符串方法

##### 对象

1. 创建对象

​	① 使用对象字面量

```js
var person = {name: "小明", age: 18, showName: function(){console.log(name)},…}
// 空行和折行并不重要，所以，也可以这么写：
var person = {
  name: "小明", 
  age: 18, 
  showName: function(){
    console.log(name)
  },…
}
```

​	② 使用关键字new，简易性、可读性和执行速度均不是很好

```js
var person = new Object();
person.name = "小明";
person.age = 18;
person.showName = show;
function  show(){...}
/* new创建过程
	1. 创建一个新对象
	2. 将新对象作为函数执行上下文(this)
	3. 执⾏构造函数中的代码，为新对象添加属性和⽅法
	4. 返回这个新对象
*/
```

​	 ③ JavaScript中对象是引用数据类型且易变，如var x = Person; 此时改变x会同时改变Person。

2. 访问/添加属性

   ① 对象.属性名 = 属性值;

   ② 对象["属性名"] = 属性值;

   ③ 对象[表达式] = 属性值;  （表达式结果为属性名)

   注：如果读取一个对象中没有的属性，不会报错，会返回undefined。

3.  删除对象中的属性 delete 对象.属性名/对象[“属性名”]

   delete不会删除被继承的属性，但如果删除原型对象中的属性，将会影响所有从原型继承的对象。

4.  遍历对象中的属性：for…in… 每个属性都会执行一次

5. this（上下文对象）	

   每次调用函数时，解析器都会将一个上下文对象作为隐含的参数传递进函数。使用this来引用上下文对象，根据函数的调用形式不同，this的值也不同。

   > this的不同的情况：
   >  ① 以函数的形式调用时，this是window
   >  ② 以方法的形式调用时，this就是调用方法的对象
   >  ③ 以构造函数的形式调用时，this就是新创建的对象
   >  ④ 使用call和apply调用时，this是指定的那个对象
   >
   > ```js
   > //call&apply
   > function fun(a,b) {
   >   console.log("a = "+a);
   >   console.log("b = "+b);
   >   console.log(this);
   > }
   > 			
   > var obj = {
   >   name: "obj",
   >   sayName:function(){
   >     console.log(this);
   >   }
   > };
   > 
   > //fun.call(obj,2,3);  //->obj对象
   > //fun.apply(obj,[2,3]); //->obj对象
   > 
   > var obj2 = {
   >   name: "obj2"
   > };
   > 
   > //fun.apply(); //->Window
   > //fun.call(); //->Window
   > //fun(); //->Window
   > 
   > fun.call(obj);  //->obj对象
   > fun.apply(obj);  //->obj对象
   > fun();  //->Window
   > 			
   > obj.sayName.apply(obj2); //->obj2对象
   > ```
   >
   > 

6.  构造函数是专门用来创建对象的函数

   ① 一个构造函数我们也可以称为一个类

   ② 通过一个构造函数创建的对象，我们称该对象时这个构造函数的实例

   ③ 通过同一个构造函数创建的对象，我们称为一类对象

   ④ 构造函数就是一个普通的函数，只是他的调用方式不同，
   		如果直接调用，它就是一个普通函数
   		如果使用new来调用，则它就是一个构造函数

   ```js
   function Person(first, last, age, eye) {
       this.firstName = first;
       this.lastName = last;
       this.age = age;
       this.eyeColor = eye;
   }
   ```

- instanceof 用来检查一个对象是否是一个类的实例
  - 语法：对象 instanceof 构造函数
    - 如果该对象时构造函数的实例，则返回true，否则返回false
    - Object是所有对象的祖先，所以任何对象和Object做instanceof都会返回true

7. 原型（prototype）

    创建一个函数以后，解析器都会默认在函数中添加一个数prototype
     prototype属性指向的是一个对象，这个对象我们称为原型对象。

   - 当函数作为构造函数使用，它所创建的对象中都会有一个隐含的属性执行该原型对象。
   	这个隐含的属性可以通过对象`.__proto__`来访问。
   - 原型对象就相当于一个公共的区域，凡是通过同一个构造函数创建的对象他们通常都可以访问到相同的原型对象。
   	我们可以将对象中共有的属性和方法统一添加到原型对象中，
   		这样我们只需要添加一次，就可以使所有的对象都可以使用。
   - 当我们去访问对象的一个属性或调用对象的一个方法时，它会先自身中寻找，
   	  如果在自身中找到了，则直接使用。
   	  如果没有找到，则去原型对象中寻找，如果找到了则使用，
   	  如果没有找到，则去原型的原型中寻找，依此类推。直到找到Object的原型为止，Object的原型的原型为null，
   	  如果依然没有找到则返回undefined
   - hasOwnProperty()
   	- 这个方法可以用来检查对象自身中是否含有某个属性
   	- 语法：对象.hasOwnProperty("属性名")

##### Date对象

- 在JS中使用Date对象来表示一个时间

```js
//创建一个Date对象
//如果直接使用构造函数创建一个Date对象，则会封装为当前代码执行的时间
var d1 = new Date();
		
//创建一个指定的时间对象
//需要在构造函数中传递一个表示时间的字符串作为参数
//日期的格式  月份/日/年 时:分:秒
var d2 = new Date("2/18/2011 11:10:30");
		
//对象方法：
//getDate()	- 获取当前日期对象是几日
var date = d2.getDate();

//getDay()  - 获取当前日期对象时周几, 返回一个0-6的值, 0->周日, 1->周一
var day = d2.getDay();
		
//getMonth() - 获取当前时间对象的月份, 返回一个0-11的值 0->1月, 1->2月, 11->12月
var month = d2.getMonth();
		
//getFullYear() - 获取当前日期对象的年份
var year = d2.getFullYear();
		
//getTime()  - 获取当前日期对象的时间戳
			//时间戳，指的是从格林威治标准时间的1970年1月1日，0时0分0秒到当前日期所花费的毫秒数（1秒 = 			   1000毫秒）, 计算机底层在保存时间时使用都是时间戳
var time = d2.getTime();

//利用时间戳来测试代码的执行的性能
//获取当前的时间戳
var start = Date.now();		
for(var i=0 ; i<100 ; i++){
  console.log(i);
}
var end = Date.now();
console.log("执行了："+(end - start)+"毫秒");
```

##### Math对象

 Math和其他的对象不同，它不是一个构造函数，它属于一个工具类不用创建对象，它里边封装了数学运算相关的属性和方法，如Math.PI 表示的圆周率。

```js
//对象方法：
//abs()  可以用来计算一个数的绝对值 Math.abs(-1)
		
//Math.ceil() - 可以对一个数进行向上取整，小数位只有有值就自动进1；Math.ceil(1.1)
//Math.floor()  - 可以对一个数进行向下取整，小数部分会被舍掉；Math.floor(1.99)
//Math.round()  - 可以对一个数进行四舍五入取整；Math.round(1.4)

/*  Math.random()	- 可以用来生成一个0-1之间的随机数
		  		- 生成一个0-x之间的随机数 Math.round(Math.random()*x)
				- 生成一个x-y之间的随机数 Math.round(Math.random()*(y-x)+x)
*/

//max() 可以获取多个数中的最大值 Math.max(10,45,30,100)
//min() 可以获取多个数中的最小值 Math.min(10,45,30,100)

//Math.pow(x,y)  返回x的y次幂 Math.pow(12,3)
//Math.sqrt()  用于对一个数进行开方运算
```
##### 正则表达式

 *  正则表达式用于定义一些字符串的规则，
 *  计算机可以根据正则表达式，来检查一个字符串是否符合规则，
 *  获取将字符串中符合规则的内容提取出来

1. 创建正则表达式

   ①构造函数创建：var 变量 = new RegExp("正则表达式","匹配模式");

   ​	 在构造函数中可以传递一个匹配模式作为第二个参数，i->忽略大小写, g->全局匹配模式。
   ​		如：var reg = new RegExp("a", "i");  这个正则表达式可以不区分大小写检查一个字符串中是否含有a

   ②使用字面量来创建正则表达式：var 变量 = /正则表达式/匹配模式；

   ​	    如：var reg = /a/i;

      * 使用字面量的方式创建更加简单

      * 使用构造函数创建更加灵活

        > 正则表达式：
        >
        > ​	|  -> 表示或者
        >
        > ​	[]  -> 里面内容表示或者 
        >
        > ​			 如[ab] == a|b; [a-z] == 任意小写字母; [A-Z] == 任意大写字母; 
        >
        > ​				 [A-z] == 任意字母; [0-9]==任意数字
        >
        > ​	`[^ ]`  -> 除了, 如 `/[^0-9]/ == 除了数字；/[^ab]/ == 除了a和b`

        

2. 正则表达式的方法

    test()   - 使用这个方法可以用来检查一个字符串是否符合正则表达式的规则，符合则返回true

##### JSON (JavaScript Object Notation JS对象表示法)

​    -> JS中的对象只有JS自己认识，其他的语言都不认识

​    -> JSON就是一个特殊格式的字符串，这个字符串可以被任意的语言所识别，

​         并且可以转换为任意语言中的对象，JSON在开发中主要用来数据的交互

1.  格式

      JSON和JS对象的格式一样，只不过JSON字符串中的属性名必须加双引号

      如 var arr1 = '[1,2,3,"hello",true]';
           var obj2 = '{"arr":[1,2,3]}';
   ​	   var arr2 ='[{"name":"小明","age":18,"gender":"男"},{"name":"小红","age":18,"gender":"女"}]';

2. JSON分类：

   ①对象 {}  ②数组 []

3. JSON中允许的值：

   字符串, 数值, 布尔值, null, 对象, 数组

4. JSON-->JS对象  (IE7及以下浏览器不支持)

   JSON.parse()： 返回将JSON字符串转换后的JS对象

5. JS对象-->JSON  (IE7及以下浏览器不支持)

   JSON.stringify()：返回将一个JS对象转换后的JSON字符串

6.  eval() ；可以执行JSON字符串，返回执行结果；安全性和执行效率很差，尽量不使用，

##### DOM对象

dom是打通html和css和js的工具，dom->document
	元素节点：<div></div>
	属性节点：title = “属性节点”
	文本节点：测试节点

![image-20200804150324012](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200804150324012.png)

1. 访问(获取)节点

```js
/*获取节点
  id         document.getElementById()
  className  document.getElementsByClassName()
  tagName    node.getElementsByTagName()
  name       document,getElementsByName() ->一般值文本框输入标签
*/

//封装$()函数
function $(vArg){
  switch(vArg[0]){
    case "#":   //id
      return document.getElementById(vArg.substring(1));
      break;
    case ".":
      return elementByClassName(document, vArg.substring(1));
      break;
    default:
      var str = vArg.substring(0, 5);
      if(str == "name="){
        return document.getElementsByName(vArg.substring(5));
      } else{
        return dcument.getElementsByTagName(vArg);
      } 
      break;
  }
}

//由于getElementsByClass()低版本浏览器不支持，所以可以封装一个getClass函数解决
function getClass(classN) {//创建函数 传入形参
  if(!document.getElementsByClassName) {//判断document.getElementsByClassName方法是否支持
    var list = document.getElementsByTagName("*");//先取得所有的dom标签元素
    //console.log(list.length)
    var temp = [];//创建临时数组
    for(var i = 0; i < list.length; i++) {//循环每一个dom元素
      if(list[i].className == classN) {//判断当前这个元素的class名称是否等于ClassN
        temp.push(list[i])//如果等于，将该元素添加到数组中去
      }
     }
     return temp;//；返回给函数
  }else{
    return document.getElementsByClassName(classN); 
  }
}
```

2. DOM节点的访问关系

   - 父节点

     node.prentNode: 返回调用者的父节点

   - 兄弟节点

     node.nextSibling: 返回调用者的下一个节点(包含空文档/换行节点，IE678指下一个元素节点)，

     node.nextElementSibling: 下一个元素节点，

     node.previousSibling: 前一个节点(包含空文档/换行节点，IE678前一个元素节点)，

     node.previousElementSibling: 前一个元素节点。

   - 子节点

     - 单个子节点

       prent.firstChild: 第一个子节点(包括空文档/换行节点，IE678~~)

       prent.firstElementChild: 火狐谷歌IE9指第一个元素节点

        prent.lastChild: 最后一个子节点(同firstChild)

       prent.lastElementChild: 最后一个元素节点

     - 全部子节点

       prent.childNodes: 标准属性,返回调用者子元素集合(HTML节点、属性/文本节点)，返回对象数组nodeType如下：

       | nodeType值 | nodeValue(类型) |
       | :--------: | :-------------: |
       |     1      |    元素节点     |
       |     2      |    属性节点     |
       |     3      |    文本节点     |

       prent.children: 非标准，返回调用者所有的HTML节点(包括注释节点)。

3. 操作节点方法

   - 创建节点：document.creatElement(标签名)

     如 var oDiv = document.creatElement("div");

   - 插入节点：

     - node.apendChild()：node后面追加节点

     - node.insertBefore(插入的节点，参照节点)

   - 删除节点: prent.removeChild

   - 替换节点: parent.replaceChild(新节点，旧节点)

     ```html
     <ul id="list">
       <li id="sr">环境设计系</li>
       <li id="tm">土木工程系</li>
     </ul>
     
     <script>
       var para = document.creatElement("li");
       var text = document.creatTextNode("计算机系");
       para.appendChild(text);
       
       var element = document.getElementById("list");
       var child1 = document.getElementById("sr");
       element.insertBefore(para, child1);
         
       var child2 = document.getElementById("tm");
       element.replaceChild(para, child2);
     </script>
     ```

   - 克隆节点： node.cloneNode(true/false) true->克隆整个node；false->只克隆本节点。  

4. 访问元素属性

```js
/*访问元素属性
  tagName 获取元素节点的标签名
  innerHTML(innerText) 获取和设置元素标签间的内容(可渲染)/(不可渲染)
  style.css属性 获取和设置行内样式
  属性名 获取和设置属性值*/

//访问实时最终显示样式（只读）  obj元素对象

function Displaystyle(obj,name){
  if(window.getComputedStyle){
    //getComputedStyle();  IE9+及大部分浏览器支持；(obj,伪元素(一般设为null))
    return getComputedStyle(obj,null)[name];
  }else{
    //currentStyle;  IE8及以下不支持，且仅IE浏览器支持；
    return obj.currentStyle[name];
  }
}

function getStyle(elem, attr){
    //简单点
  return elem.currentStyle ? elem.currentStyle[attr] : getComputedStyle(elem)[attr];
}

window.onload = function(){
  var oBtn = document.getElementById("btn");
  // console.log(oBtn);
  // console.log(oBtn.tagName);  ->DIV
  // console.log(oBtn.innerHTML = "<h1>变大了</h1>");
  // console.log(oBtn.id);
  // console.log(oBtn.title);
  // console.log(oBtn.className);
  // oBtn.className = "ss";
  // console.log(oBtn.style.width);
  // console.log(oBtn.style.backgroundColor);
```



###### 设置节点属性方法

​	getAttribute(name);
​	setAttribute(name, value);
​	removeArribute(name);

​    [更多](https://www.w3school.com.cn/jsref/dom_obj_all.asp)

##### 事件

> 常用事件集合
> 	onclick   ->鼠标单击事件
> 	onMouseOver ->鼠标经过事件
> 	onMouseOut  ->鼠标移出事件
> 	onSelect   ->文本框选中事件
> 	onFocus    ->光标聚集事件
> 	onBlur   ->移开光标事件
> 	onKeyup  ->按下并释放键盘上的一个键时触发
> 	onLoad    ->网页加载事件
> 	onUnload  ->关闭网页事件

1. 事件处理

```js
function demo(){
  alert("单击事件");
}
function show(){
  alert("第二个事件");
}
```

​		DOM2级事件：浏览器通用

```html
<input type="button" value="按钮"  id="btn" /> 
```

```js
var oBtn = document.getElementById("btn");
oBtn.addEventListener("click",demo);
//多个函数
oBtn.addEventListener("click",show);

oBtn.romoveEventListener("click");  //移出全部事件监听
```

​		html事件处理：在标签上加事件，在js中书写对应函数，不利于修改

```html
<!--绑定单函数-->
<input type="button" value="按钮" id="btn" onclick="demo()" />
<!--绑定多个函数-->
<input type="button" value="按钮" id="btn" onclick="demo();show()">
<!-- 以下是错误的方式，只显示demo-->
<input type="button" value="按钮" id="btn" onclick="demo()" onclick="show()">
```

​		DOM0级事件：容易被覆盖

```js
var oBtn = document.getElementById("btn");
oBtn.onclick = demo;
//多个事件
oBtn.onclick = show;
```

	另，IE浏览器可使用：
	  组件.attachEvent("事件"，函数名，布尔值)
	  组件.detachEvent(~)
- 兼容问题解决

> ```js
> var oBtn = document.getElementById("btn1");
> //判断浏览器支持哪一种方法
> if (oBtn.addEventListener){
>   oBtn.addEventListener("click",demo);
> }else if(oBtn.attachEvent){
>   oBtn.attachEvent("onclick",demo);
> }else{
>   oBtn.onclick = demo();
> }
> 
> function demo(){
>   alert("支持支持");
> }
> ```

2. 事件对象
       在触发DOM事件时会产生一个对象
       事件对象event:
             type  							获取事件类型(click、mouse等)
             target  						 获取事件目标 谁触发了该事件([object HTMLInputElement])
             stopPropagation()      阻止事件冒泡
             preventDefault()    	 阻止事件默认行为

   - 事件冒泡：当子元素绑定了事件，在在执行过程中向外传播使祖、父代元素也能响应事件的一种情况。

     ```js
     <div id="div2" click="show2">
       <div id="div1" click="show1"> </div>
     </div>
     show1()->打印1
     show2()->打印2
     输出：点击div1，时先显示1，然后会显示2 
     只要html结构上有包含关系就会出现上述情况，而不是显示中的包含或不包含
     ```

     事件冒泡如果不加以处理，有时会出现诸多问题。

     阻止事件冒泡：(为了方便，大可针对以下确定哪种阻止方式加一个判断)

     - 标准W3C方式: event.stopPropagation()
     - 非标准IE方式: event.cancelBubble=true

   - 事件默认：当一个事件发生时，浏览器有时会执行自己的默认做的事情，针对这些，如果我们不需要他影响用户对我们界面的操作，如<a>标签的默认跳转等，可以取消事件默认(event.preventDefault())。

   > ​    异常捕获(catch)：

   > ```js
   > // 捕获   输出   ReferenceError: str is not defined
   > function demo(){
   >   try{
   >     alert(str);
   >   }catch(err){
   >     alert(err);
   >   }
   > }
   > demo();
   > ```



   

   

   