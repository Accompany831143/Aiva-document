<!--
 * @Date: 2021-01-12 16:15:57
 * @LastEditors: Aiva
 * @LastEditTime: 2021-01-12 16:16:18
 * @FilePath: \Gitbook\JavaScript\eventloop.md
-->
# JavaScript 事件循环，宏任务和微任务

## 事件循环 Event Loop

程序中设置两个线程：一个负责程序本身的运行，称为"主线程"；另一个负责主线程与其他进程（主要是各种I/O操作）的通信，被称为"Event Loop线程"（可以译为"消息线程"）。

所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

### 事件循环具体过程就是：

- 同步任务进入主线程，异步任务进入Event Table并注册函数。
- 当异步任务完成时，Event Table会将这个函数移入Event Queue。
- 主线程内的任务执行完毕执行栈为空，会去Event Queue读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的Event Loop(事件循环)。

```javascript
console.log('Hi');
setTimeout(function() {
  console.log('callback');
}, 0);
console.log('Bye');

//Hi,Bye,callback
```

## 宏任务与微任务

在JavaScript中，任务被分为两种，一种宏任务（MacroTask），一种叫微任务（MicroTask）。

### MacroTask（宏任务）

**宿主环境提供的（浏览器和node）**

- `script`全部代码、`setTimeout`、`setInterval`。

### MicroTask（微任务）

**语言标准提供的**

- `Promise`、`await`

### 宏任务与微任务执行顺序：

- 执行栈在执行完同步任务后，查看执行栈是否为空，如果执行栈为空，就会去检查微任务队列是否为空，如果为空的话，就执行宏任务，否则就一次性执行完所有微任务。
- 每次单个宏任务执行完毕后，检查微任务队列是否为空，如果不为空的话，会按照先入先出的规则全部执行完微任务后，设置微任务队列为`null`，然后再执行宏任务，如此循环。

**总结：同步—>微任务—>宏任务**

常见异步任务类型

- settimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行
- promise本身是同步的立即执行函数(同步)
- promise.then里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行；
- async函数表示函数里面可能会有异步方法，await后面跟一个表达式，async方法执行时，遇到await会立即执行表达式，然后把await表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行

```javascript
new Promise(function(resolve){
    console.log('1');
    resolve();
}).then(function(){
    console.log('2')
});

setTimeout(function(){
    console.log('3')
});

console.log('4');
//1,4,2,3
```

[原文链接](https://www.cnblogs.com/zhoujingguoguo/p/11414864.html)