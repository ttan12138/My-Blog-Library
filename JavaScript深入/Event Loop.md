# JS事件循环-Event Loop

`Event Loop`即事件循环，是指浏览器或者`node`的一种**解决javascript单线程**运行时不会阻塞的一种机制，也是**异步**的原理。

在**javascript**中，任务被分为两种，一种是宏任务（MacroTask）也叫Task，另一种叫做微任务（MicroTask）。

## 宏任务 - Task

宏任务包括`script`全部代码、`setTimeout`、`setInterval`、`setImmediate`、`I/O`、`UI Rendering`。

在浏览器和node中的支持如下：

![image-20200926142800628](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200926142800628.png)

## 微任务 - MicroTask

微任务包括`Process.nextTick`、`Promise`、`Object.observe(已废弃)`、`MutationObserver`。

在浏览器和node中的支持如下：

![image-20200926143154565](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200926143154565.png)

## 浏览器中的Event Loop

`JavaScript`有一个`main thread`主线程和`call-stack`调用栈（执行栈），所有任务都会放到调用栈等待主线程执行。

### JS调用栈

JS调用栈采用的是**后进先出**，当函数执行的时候，会被添加到栈的顶部，当函数执行完成后，就会从栈顶移出，直到栈内被清空。

> 本文 涉及 栈/队列 的数据结构的知识，建议系统的学习过数据结构后，再来看；之后文章将不会赘述数据结构相关知识。

### 同步任务和异步任务

`JavaScript`单线程任务被分为同步任务和异步任务：

- 同步任务会在调用栈中按照顺序等待主线程依次执行；
- 异步任务会在异步任务有结果后，将注册的回调函数放入任务队列中等待主线程空闲时被读取到栈内等待主线程执行。
- 任务队列`Task Queue`即队列，是一种先进先出的一种数据结构。

### 事件循环的进程模型

- 选择当前要执行的任务队列，选择任务队列中最先进入的任务，如果任务队列为空`null`，则执行跳转到微任务（`MicroTask`）的执行步骤。
- 将事件循环中的任务设置为已选择任务。
- 执行该任务。
- 将事件循环中当前运行任务设置为`null`。
- 将已运行完成的任务从任务队列中删除。
- Microtasks微任务步骤：进入microtask检查点。
- 更新界面渲染。
- 返回第一步。

#### Microtasks微任务步骤

- 设置microtask检查点标志为true。
- 当事件循环microtask执行不为空时：选择一个最先进入的microtask队列的microtask，将事件循环的microtask设置为已选择的microtask，运行microtask，将已经执行完成的microtask为null，移出microtask中的microtask。
- 清理IndexDB事务。
- 设置进入mircotask检查点的标志为false。

上述过程简单点说，就是下面这张图：

![image-20200926150238453](C:\Users\ttan12138\AppData\Roaming\Typora\typora-user-images\image-20200926150238453.png)

执行栈在执行完同步任务之后，检查微任务队列，不为空则执行完当前所有的微任务(执行顺序：先入先出)，若为空，执行宏任务栈顶的第一个宏任务，执行完成后，移出该宏任务，继续检查微任务队列……

### 例子

- ```js
  console.log('代码开始执行')
  
  setTimeout(function() {
    console.log('定时器开始')
  })
  
  new Promise(function(resolve){
    console.log('执行promise内部')
    resolve()
  }).then(function() {
    console.log('执行了promise-then')
  })
  
  console.log('代码执行结束')
  
  // 结果：代码开始执行 -- 执行Promise内部 -- 代码执行结束 -- 执行了promise-then -- 定时器开始
  /* 分析过程：
    首先，执行同步代码，‘代码开始执行 -- 执行Promise内部 -- 代码执行结束’;
    在执行同步代码过程中，遇到setTimeout,将其放到【宏任务队列】;
                      遇到promise的then方法,是微任务,将其放到【微任务队列】;
    同步代码执行完毕之后，执行【微任务队列】，打印“执行了promise-then”;
    【微任务队列】执行完毕后，执行【宏任务队列】的第一个宏任务setTimeout，打印“定时器开始了”；
    【微任务队列】为空，【宏任务队列】为空，至此js执行完成。
  */
  ```

  在此例中需要注意： **promise的构造函数是同步的，promise的链式调用是异步的。**

- ```js
  async function fn1(){
    await fn3()
    console.log('fn1')
  }
  
  function fn2(){
    console.log('fn2')
  }
  
  function fn3(){
    console.log('fn3')
}
  
  fn1()
  fn2()
  // 结果：fn3 -- fn2 -- fn1
  ```
  
  在此例中需要注意： **`async`/`await`在底层转换为`promise`和`then`回调函数。也可以理解为promise的语法糖。所以上例中，`await`语句之后可以看作为一个`promise`对象的`then()`调用属于异步操作。**
  
- ```js
  console.log('script start')  // --1  --script start
  async function async1() {
    await async2()             // --3
    console.log('async1 end')  // --7  --async1 end
  }
  async function async2() {
    console.log('async2 end')  // --4 --async2 end
  }
  async1()                    // --2
  setTimeout(function() {
    console.log('setTimeout') // --10  --setTimeout
  }, 0)
  new Promise(resolve => {
    console.log('Promise')   // --5  --Promise
    resolve()
  }).then(function() {
    console.log('promise1')  // --8  --promise1
  }).then(function() {
    console.log('promise2')  // --9  --promise2
  })
  console.log('script end')  // --6   --script end
  ```

  以上例子需要注意：

  - 在谷歌老版本版本以下，先执行`promise1`和`promise2`，再执行`async1`。而在73版本，先执行`async1`再执行`promise1`和`promise2`。

  > (例子来源于知乎)
  >
  > ```
  > async function f() {
  >   await p
  >   console.log('ok')
  > }
  > ```
  >
  > 老版本：
  >
  > ```js
  > function f() {
  >   return RESOLVE(P).then(() => {console.log('OK')})
  >   /* RESOLVE为老版本的方法 可以看作是 promise.resolve,具体执行如下：
  >    如果p为promise, 则p.then()会被立即执行
  >    如果严格按照标准，await后的语句会先产生一个新的状态为resolve的promise p，这个过程是异步的，进入队列的是新promise的resolve过程，then不会立即调用，当队列执行到resolve过程时，才将then()加入队列，时序上变晚了。
  >    */
  > }
  > ```
  >
  > 73版本中：不考虑，直接从上撸到下

- ```js
  new Promise((resolve, reject) => {
    console.log( "promise1" )
    resolve()
  }).then( () => {
    console.log(1)
  }).then(() => {
    console.log(2)
  }).then(() => {
    console.log(3)
  })
  
  new Promise((resolve, reject) => {
    console.log("promise2")
    resolve()
  }).then(() => {
    console.log(4)
  }).then(() => {
    console.log(5)
  }).then(() => {
    console.log(6)
  })
  // 输出：promise1--promise1--1--4--2--5--3--6
  ```

  此例中需要注意：**then()回调不是一次性压入微任务队列的！**

- ```js
  async function t1() {
    console.log(1)
    console.log(2)
    await Promise.resolve().then(() => console.log('t1p'))
    console.log(3)
    console.log(4)
  }
  
  async function t2() {
    console.log(5)
    console.log(6)
    await Promise.resolve().then(() => console.log('t2p'))
    console.log(7)
    console.log(8)
  }
  
  t1()
  t2()
  
  console.log('end')
  // 输出： 1--2--5--6--end--t1p--t2p--3--4--7--8
  ```

- ```js
  async function t1() {
    console.log(1)
    console.log(2)
    await new Promise(resolve => {
      setTimeout(() => {
        console.log('t1p')
        resolve()
      }, 1000)
    })
    await console.log(3)
    console.log(4)
  }
  
  async function t2() {
    console.log(5)
    console.log(6)
    await Promise.resolve().then(() => console.log('t2p'))
    console.log(7)
    console.log(8)
  }
  
  t1()
  t2()
  
  console.log('end')
  // 输出：1--2--5--6--end--t2p--7--8--t1p--3--4
```
  
  

## Node.js中的Event Loop

`Node`的`Event loop`一共分为6个阶段，具体如下：

- `timers`: 执行`setTimeout`和`setInterval`中到期的`callback`。
- `pending callback`: 上一轮循环中少数的`callback`会放在这一阶段执行。
- `idle, prepare`: 仅在内部使用。
- `poll`: 最重要的阶段，执行`pending callback`，在适当的情况下回阻塞在这个阶段。
- `check`: 执行`setImmediate`(`setImmediate()`是将事件插入到事件队列尾部，主线程和事件队列的函数执行完成之后立即执行`setImmediate`指定的回调函数)的`callback`。
- `close callbacks`: 执行`close`事件的`callback`，例如`socket.on('close'[,fn])`或者`http.server.on('close, fn)`。

> 挖坑挖坑【[一次弄懂Event Loop](https://juejin.im/post/6844903764202094606?utm_source=gold_browser_extension#comment)】

## setImmediate()和setTimeout()的区别

`setImmediate`和`setTimeout()`都是定时器，使用方法也是大同小异，但根据它们被调用的时间会有不同的表现方式：

- `setImmediate()`设计用于在当前`poll`阶段完成后check阶段执行脚本 。
- `setTimeout()` 安排在经过最小（ms）后运行的脚本，在`timers`阶段执行。

```js
setTimeout(() => {
  console.log('timeout');
}, 0);
setImmediate(() => {
  console.log('immediate');
});
// 主线成调用因上下文而不同，受性能影响。结果不一致。
```

```js
// I/O 中调用始终首先执行立即回调 immediate => timeout
// I/O 读取文件后，进入poll阶段，先发现immediate,然后进入check阶段，执行immediate。最后进入了timers阶段，执行setTimeout
const fs = require('fs');
fs.readFile(__filename, () => {
  setTimeout(() => {
    console.log('timeout');
  }, 0);
  setImmediate(() => {
    console.log('immediate');
  });
});
```

> 阶段示意如下：
>
> ```js
> ┌───────────────────────────┐
> ┌─>│           timers          │
> │  └─────────────┬─────────────┘
> │  ┌─────────────┴─────────────┐
> │  │     pending callbacks     │
> │  └─────────────┬─────────────┘
> │  ┌─────────────┴─────────────┐
> │  │       idle, prepare       │
> │  └─────────────┬─────────────┘      ┌───────────────┐
> │  ┌─────────────┴─────────────┐      │   incoming:   │
> │  │           poll            │<─────┤  connections, │
> │  └─────────────┬─────────────┘      │   data, etc.  │
> │  ┌─────────────┴─────────────┐      └───────────────┘
> │  │           check           │
> │  └─────────────┬─────────────┘
> │  ┌─────────────┴─────────────┐
> └──┤      close callbacks      │
>    └───────────────────────────┘
> ```

## Process.nextTick()

`process.nextTick()`虽然它是异步API的一部分，但从技术上讲，它不是事件循环的一部分。

> `process.nextTick()`方法将 `callback` 添加到`next tick`队列。 一旦当前事件轮询队列的任务全部完成，在`next tick`队列中的所有`callbacks`会被依次调用。
>
> --》每个阶段完成后，如果存在`nextTick`队列，会清空队列中的所有回调函数，先于其他`microTask`执行。

```js
let bar;
setTimeout(() => {
  console.log('setTimeout');
}, 0)
setImmediate(() => {
  console.log('setImmediate');
})
function someAsyncApiCall(callback) {
  process.nextTick(callback);
}
someAsyncApiCall(() => {
  console.log('bar', bar); // 1
});
bar = 1;
// 输出：bar 1 -- setTimeout -- setImmediate
//      bar 1 -- setImmediate -- setTimeout
// 始终先执行process.nextTick(callback)
```