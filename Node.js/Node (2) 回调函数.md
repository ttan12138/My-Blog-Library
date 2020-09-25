## 异步编程

### 回调函数

不成立情况1:

```javascript
function add (x, y){
  console.log(1)
  setTimeout(function(){
    console.log(2)
    var ret = x + y
    return ret
  },1000)
  console.log(3)
}

console.log(add(10,20))

```

结果

```javascript
1
3
undefined
2  //等待1秒后打印

```

不成立情况2：

```javascript
function add (x, y){
  var ret
  console.log(1)
  setTimeout(function(){
    console.log(2)
    var ret = x + y
  },1000)
  console.log(3)
  return ret
}

console.log(add(10,20))

```

结果

```javascript
1
3
undefined
2  //等待1秒后打印

```

使用回调函数解决

回调函数：通过一个函数，获取函数内部的操作。（根据输入得到输出结果）

```javascript
var ret;
function add(x,y,callback){
    // callback就是回调函数
    // var x = 10;
    // var y = 20;
    // var callback = function(ret){console.log(ret);}
    console.log(1);
    setTimeout(function(){
        var ret = x + y;
        callback(ret);
    },1000);
    console.log(3);
}
add(10,20,function(ret){
    console.log(ret);
});

```

注意：

​	凡是需要得到一个函数内部异步操作的结果（setTimeout,readFile,writeFile,ajax,readdir）

​	这种情况必须通过   回调函数 (异步API都会伴随着一个回调函数)

ajax:

基于原生XMLHttpRequest封装get方法：

```javascript
var oReq = new XMLHttpRequest();
// 当请求加载成功要调用指定的函数
oReq.onload = function(){
    console.log(oReq.responseText);
}
oReq.open("GET", "请求路径",true);
oReq.send();

```

```javascript
function get(url,callback){
    var oReq = new XMLHttpRequest();
    // 当请求加载成功要调用指定的函数
    oReq.onload = function(){
        //console.log(oReq.responseText);
        callback(oReq.responseText);
    }
    oReq.open("GET", url,true);
    oReq.send();
}
get('data.json',function(data){
    console.log(data);
});

```

### Promise

参考文档：https://es6.ruanyifeng.com/#docs/promise

#### callback  hell（回调地狱）:

​		文件的读取无法判断执行顺序（文件的执行顺序是依据文件的大小来决定的）(异步api无法保证文件的执行顺序)

```javascript
var fs = require('fs');

fs.readFile('./data/a.text','utf8',function(err,data){
	if(err){
		// 1 读取失败直接打印输出读取失败
		return console.log('读取失败');
		// 2 抛出异常
		// 		阻止程序的执行
		// 		把错误信息打印到控制台
		throw err;
	}
	console.log(data);
});

fs.readFile('./data/b.text','utf8',function(err,data){
	if(err){
		// 1 读取失败直接打印输出读取失败
		return console.log('读取失败');
		// 2 抛出异常
		// 		阻止程序的执行
		// 		把错误信息打印到控制台
		throw err;
	}
	console.log(data);
});

```

通过回调嵌套的方式来保证顺序：

```javascript
var fs = require('fs');

fs.readFile('./data/a.text','utf8',function(err,data){
	if(err){
		// 1 读取失败直接打印输出读取失败
		return console.log('读取失败');
		// 2 抛出异常
		// 		阻止程序的执行
		// 		把错误信息打印到控制台
		throw err;
	}
	console.log(data);
	fs.readFile('./data/b.text','utf8',function(err,data){
		if(err){
			// 1 读取失败直接打印输出读取失败
			return console.log('读取失败');
			// 2 抛出异常
			// 		阻止程序的执行
			// 		把错误信息打印到控制台
			throw err;
		}
		console.log(data);
		fs.readFile('./data/a.text','utf8',function(err,data){
			if(err){
				// 1 读取失败直接打印输出读取失败
				return console.log('读取失败');
				// 2 抛出异常
				// 		阻止程序的执行
				// 		把错误信息打印到控制台
				throw err;
			}
			console.log(data);
		});
	});
});

```

#### Promise介绍

​		为了解决以上编码方式带来的问题（回调地狱嵌套），所以在EcmaScript6新增了一个API:`Promise`。

- Promise：承诺，保证
- Promise本身不是异步的，但往往都是内部封装一个异步任务

Promise容器中存放了一共异步任务，该异步任务一开处于Pending状态（正在进行状态），执行完成后，会变成Resolved（成功）或者Rejected（失败）中的一种。 

基本语法：

```javascript
// 在ES6中新增了一个API Promise
// Promise是个构造函数


var fs = require('fs')

// 1.创建Promise容器
//  Promise容器一旦创建，就开始执行里面的代码
var p1 = new Promise(function (resolve, reject) {
  fs.readFile('./data/a.txt', 'utf8', function (err, data) {
    if (err) {
      // 失败了，承诺容器中的任务失败了
      // console.log(err);
			// 把容器的Pending状态变为rejected
      // 调用reject就相当于调用了then方法的第二个参数函数
      reject(err)
    } else {
      // 承诺容器中的任务成功了
      // console.log(data);
      // 把容器的Pending状态变为resolve
      // 也就是说这里调用的resolve方法实际上就是then方法传递的那个function
      resolve(data)
    }
  })
})

var p2 = new Promise(function (resolve, reject) {
  fs.readFile('./data/b.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})

var p3 = new Promise(function (resolve, reject) {
  fs.readFile('./data/c.txt', 'utf8', function (err, data) {
    if (err) {
      reject(err)
    } else {
      resolve(data)
    }
  })
})


// p1就是那个承诺
// 当p1成功了然后(then)做指定的操作
// then方法接收的fuction就是容器中的resolve函数
p1
  .then(function (data) {
    console.log(data)
    // 当 p1 读取成功的时候
    // 当前函数中 return 的结果就可以在后面的 then 中 function 接收到
    // 当你 return 123 后面就接收到 123
    //      return 'hello' 后面就接收到 'hello'
    //      没有 return 后面收到的就是 undefined
    // 上面那些 return 的数据没什么卵用
    // 真正有用的是：我们可以 return 一个 Promise 对象
    // 当 return 一个 Promise 对象的时候，后续的 then 中的 方法的第一个参数
    // 会作为 p2 的 resolve
    return p2
  }, function (err) {
    console.log('读取文件失败了', err)
  })
  .then(function (data) {
    console.log(data)
    return p3
  })
  .then(function (data) {
    console.log(data)
    console.log('end')
  })


```

图示理解promise

![1](C:/Users/ttan12138/Downloads/Study_Node.js-master/nodeJS学习笔记.assets/1.png)

封装Promise的`readFile`：

```javascript
var fs = require('fs');

function pReadFile(filePath) {
	return new Promise(function(resolve, reject) {
		fs.readFile(filePath, 'utf8', function(err, data) {
			if (err) {
				reject(err);
			} else {
				resolve(data);
			}
		});
	});
}

pReadFile('./a.txt')
	.then(function(data) {
		console.log(data);
		return pReadFile('./b.txt');
	})
	.then(function(data) {
		console.log(data);
		return pReadFile('./a.txt');
	})
	.then(function(data) {
		console.log(data);
	})


```

#### 应用场景

例如：在注册账号时，先要查询账号是否存在，再进行注册

mongoose所有的API都支持Promise：

```javascript
// 查询所有
User.find()
	.then(function(data){
        console.log(data)
    })

```

注册：

```javascript
User.findOne({username:'admin'},function(user){
    if(user){
        console.log('用户已存在')
    } else {
        new User({
             username:'aaa',
             password:'123',
             email:'fffff'
        }).save(function(){
            console.log('注册成功');
        })
    }
})

```

采用promise方式

```javascript
User.findOne({
    username:'admin'
	})
    .then(function(user){
        if(user){
            // 用户已经存在不能注册
            console.log('用户已存在');
        }
        else{
            // 用户不存在可以注册
            return new User({
                username:'aaa',
                password:'123',
                email:'fffff'
            }).save();
        }
    })
    .then(funciton(ret){
		console.log('注册成功');
    })

```

### Generator

async函数

### ES6

> 参考文档：https://es6.ruanyifeng.com/