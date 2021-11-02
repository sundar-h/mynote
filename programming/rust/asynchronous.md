## 异步模型

### 演进过程:
* Iterator next
* Generator 异步的、惰性求值
* Async/Await

Future 的方法 FutureExt


[基础概念](https://youjiali1995.github.io/rust/async/)
**异步编程的语法目标，就是怎样让它更像同步编程。**

* 回调 
* 事件(时间错过了,再监听不会得到结果)
* Promise/ Features (错过了，再监听，结果还是一样);--> 解决回调地狱.
链式调用


## Rust 的Promise VS JavaScript Promise

## JavaScript 异步编程基础概念
* Promise
* Generator/ yield ; .next() --> 使用场景: 实现Iterator 迭代器模式 对比 Stream 流
* async/ await; async 函数就是 Generator 函数的语法糖。 await 作用于Promise对象;

* 定时器都是异步编程的
* 所有的事件绑定都是异步编程的
* Ajax读取数据都是异步编程的，我们一般设置为异步编程
* 回调函数都是异步编程的

* Promise是一个对象.
* 它代表了某个未来才会知道结果的事件（通常是一个异步操作）的结果
* 状态机 (pending: 未完成, fulfilled: 已成功, rejected:已失败)
* Promise 对象的状态改变，只有两种可能：
    1. 从 pending（未完成）变为 fulfilled（已成功）
    2. 从 pending（未完成）变为 rejected（已失败）
*  一旦状态改变，就不会再变，任何时候都可以得到这个结果
* 其中.then()和.catch()方法体是异步执行的(pending--->fulfilled, pending--->rejected)

解决问题:
* 解决回调地狱的问题(层层嵌套-->变为-->链式调用)
* 支持多个并发请求,获取并发请求返回的数据。

缺点:
* 无法取消,一旦建立就立刻执行,无法中途取消
* 如果不设置回调函数，Promise内部抛出的错误,不会反映到外部
* 当处于pending状态时，无法得知目前进展到哪一个阶段(刚刚开始还是即将完成)

```
console.log(1)

let promise = new Promise(function (resolve, reject) {
  console.log(2)
  resolve(3)
  console.log(4)
})

// then的第二个函数是可选的，非必须。
promise.then(function (value) {
  console.log(value)
})

console.log(5)

// 1
// 2
// 4
// 5
// 3
```
1. Promise 新建后会立马执行其中代码
2. 调用 resolve 或 reject 并不会终结 Promise 的参数函数的执行
3. 在当前脚本所有同步任务执行完后才会执行 then 方法指定的回调函数
resolve/reject只是改变状态机状态

### then()的链式调用
* then方法会隐性返回一个新的promise实例
* 总是推荐使用catch()方法代替then方法中的第二个回调

```
new Promise((resolve, reject)=> {
  resolve(1)
})
  .then(val => { // 状态为成功时执行
    console.log(val)
    return 2
  })
  .then(value => { // 状态为成功时执行
    console.log(value)
  })
  
// 1
// 2

// 在 then 方法中可以返回一个 Promise 对象（即有异步操作），这时后一个 then 方法的回调函数，会等待返回的 Promise 对象的状态发生变化，才会被调用。
function getNum (num) {
  let promise = new Promise((resolve, reject)=> {
    setTimeout(() => {
      resolve(num)
    }, 1000)
  })
  return promise
}

getNum(1)
  .then(val => {
    console.log(val)
    return getNum(2) // 返回Promise对象
  })
  .then(val => {
    console.log(val)
  })
  
// 1
// 2
```

### JavaScript Promise 特性
1. 如果 Promise 状态已经变成resolved，再抛出错误是无效的。
2. then 方法指定的回调函数，如果运行中出错，也会被catch方法捕捉到。
3. Promise 对象抛出的错误，总是会被下一个catch语句捕获。
4. Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。
```
let promise = new Promise((resolve, reject)=> { // 解析一
  resolve('Promise实例正确')
  throw new Error('Promise实例出错');
})

promise
  .then(val => { // 解析二
    console.log(val)
    throw new Error('then方法内出错')
    return '链式调用then'
  })
  .then(val => { // 解析三
    console.log(val)
  })
  .catch(err => { // 解析四
    console.log(err)
    throw new Error('catch方法内出错')
  })
  .catch(err => { // 解析五
    console.log(err)
    throw new Error('最后的catch方法出错')
  })

setTimeout(() => { // 解析六
  console.log('promise后调用')
}, 0)

// Promise实例正确
// Error: then方法内出错
// Error: catch方法内出错
// Uncaught (in promise) Error: 最后的catch方法出错
// promise后调用
```
[ES6中的Promise详解](https://chenminzhe.com/2020/04/05/ES6%E4%B8%ADPromise%E8%AF%A6%E8%A7%A3/)
### then方法的规则
* then方法下一次的输入需要上一次的输出
* 如果一个promise执行完后 返回的还是一个promise，会把这个promise 的执行结果，传递给下一次then中
* 如果then中返回的不是Promise对象而是一个普通值，则会将这个结果作为下次then的成功的结果
* 如果当前then中失败了 会走下一个then的失败
* 如果返回的是undefined 不管当前是成功还是失败 都会走下一次的成功
* catch是错误没有处理的情况下才会走
* then中不写方法则值会穿透，传入下一个then中

[es6入门4--promise详解](https://www.cnblogs.com/echolun/p/10693470.html)
[async 函数的含义和用法](https://www.ruanyifeng.com/blog/2015/05/async.html)

## async / await 基于Generator的语法糖, 返回一个Promise对象 
```async-await是promise和generator的语法糖```
* async 关键字函数的调用结果是立刻返回一个promise对象
* async 关键字将函数转换为 promise
* 调用任何返回Promise的函数时使用 await
* await 它可以放在任何异步的，基于 promise 的函数之前。它会暂停代码在该行上，直到 promise 完成，然后返回结果值。在暂停的同时，其他正在等待执行的代码就有机会执行了。
* await 只能在异步函数内部工作。
* 异步函数内部执行的时候，遇到await就会先返回，等到出发的异步操作完成，再接着执行函数体后面的语句

```
// 指定50毫秒以后，输出"hello world"。
function timeout(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value)
}

asyncPrint('hello world', 50);
```

* 除了到处都是 .then() 代码块！
使用async/await 重构代码


```
async function myFetch() {
  let response = await fetch('coffee.jpg');
  return await response.blob();
}

myFetch().then((blob) => {
  let objectURL = URL.createObjectURL(blob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
});
```

async/await 的添加错误处理
1. try...catch
```
async function myFetch() {
  try {
    let response = await fetch('coffee.jpg');
    let myBlob = await response.blob();

    let objectURL = URL.createObjectURL(myBlob);
    let image = document.createElement('img');
    image.src = objectURL;
    document.body.appendChild(image);
  } catch(e) {
    console.log(e);
  }
}

myFetch();
```
2. 
```
async function myFetch() {
  let response = await fetch('coffee.jpg');
  return await response.blob();
}

myFetch().then((blob) => {
  let objectURL = URL.createObjectURL(blob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
})
.catch((e) =>
  console.log(e)
);
```
[async和await:让异步编程更简单](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous/Async_await)



## Rust Features
* 类似JavaScript的async/await.
### 区别
* Java script的承诺是立即执行(early evaluated)的。 这意味着一旦它被创建，它就开始运行一个任务。 
* 与此相反,Rust的Futures是延迟执行(lazy evaluated)。 除非轮询(poll)一次,否则什么事都不会发生。


Rust中的异步实现基于轮询,每个异步任务分成三个阶段:
1. 轮询阶段(The Poll phase). 一个Future被轮询后,会开始执行,直到被阻塞. 我们经常把轮询一个Future这部分称之为执行器(executor)
2. 等待阶段. 事件源(通常称为reactor)注册等待一个事件发生，并确保当该事件准备好时唤醒相应的Future
3. 唤醒阶段. 事件发生,相应的Future被唤醒。 现在轮到执行器(executor),就是第一步中的那个执行器，调度Future再次被轮询，并向前走一步，直到它完成或达到一个阻塞点，不能再向前走, 如此往复,直到最终完成.

[Rust异步浅谈](https://mp.weixin.qq.com/s/C7hewOZ9BQ0zKRoeYYfF7A)
[Rust异步浅谈](https://leaxoy.github.io/2020/03/rust-async-runtime/)

底层传输，我们可以将其抽象成 Reader / Writer（async 下是 Stream / Sink），上层的消息，我们将其抽象成 Message。接受和发送的数据，可以看做是一个 Frame，里面有不同的格式，于是，我们可以有这样的设计：

### 异步的最基本概念
* Feature: 代表一次性的异步值; 
* Stream:  并发异步版本的版本的Iterator(Iterator是同步的) 代表可重复的异步值; 用来抽象源源不断的数据源; 用来抽象 Websocket Connection 读取端,在Websokcet中; 服务端源源不断的接受客户端的值并处理，直至客户端断开连接;更进一步的抽象，MQ中的Consumer, Tcp中接收方，都可以看作是一个Stream;
* Sink:    代表一次或多次的异步值; Sink可以来抽象网络连接的写入端，消息队列中的 Producer

#### 补充: 典型系统中的各种流 [Rust流(Streams)](https://zhuanlan.zhihu.com/p/70247995)
* Source:   可以生成数据的流
* Sink:     可以消费数据的流
* Throught: 消费数据,对其进行操作然后生成新数据的流
* Duplex:   流可以产生数据,也可以独立消费数据

|            |        |      |         |        |
| ---------- | ------ | ---- | ------- | ------ |
|            | Source | Sink | Through | Duplex |
| AsyncRead  | Yes    | No   | Yes     | Yes    |
| AsyncWrite | No     | Yes  | No      | Yes    |
| Stream     | Yes    | No   | Yes     | No     |




在Sink的上层，我们可以封装 send 以及 send_all 等方法，用来抽象对应的 Future 与 Stream.


* Feature VS Stream(可重复的异步值的Stream: 用来抽象源源不断的数据源: Websocket Connection, MQ中的Consumer, Tcp中的业务数据包)
* Executor
* Reactor
* Waker

Runtime 由两部分组成，Executor和Reactor。
Executor为执行器:就绪的任务，没有任何阻塞的等待，循环执行一系列就绪的Future，当Future返回pending的时候，会将Future转移到Reactor上等待进一步的唤醒。

Reactor为反应器(唤醒器)，轮询并唤醒挂在的事件，并执行对应的wake方法，通常来说，wake会将Future的状态变更为就绪，同时将Future放到Executor的队列中等待执行。

Reactor作为反应器，上面同时挂在了成千上万个待唤醒的事件， 这里使用了mio统一封装了操作系统的多路复用API。在Linux中使用的是Epoll，在Mac中使用的则是Kqueue，具体的实现在此不多说。

```
pub enum Poll<T> {
    Ready(T),
    Pending,
}

pub trait Future {
    type Output;

    fn poll(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output>;
}
```

```
pub trait Stream {
    type Item;

    fn poll_next(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Option<Self::Item>>;
}
```

Stream对应了同步原语中的Iterator的概念，想一下，是不是连签名都是如此的相像呢。

```
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}
```

```
pub trait Sink<Item> {
    type Error;

    fn poll_ready(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Result<(), Self::Error>>;
    fn start_send(self: Pin<&mut Self>, item: Item) -> Result<(), Self::Error>;
    fn poll_flush(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Result<(), Self::Error>>;
    fn poll_close(self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Result<(), Self::Error>>;
}
```


## Rust异步需要注意的地方

1 不要在任何异步函数中执行任何阻塞操作，不仅仅是thread::sleep, 还有标准库的Tcp/Udp, 以及sync中的channel, Mutex, RWLock 都不应该继续使用，除非你知道你在干什么！替换为async-std 与 futures中实现的版本。
2. 如非必要，不要自己尝试去实现Future，自己实现的没有触发wake操作的话，将永远不会唤醒，取而代之，用已经实现好的Future进行组合。
3. 使用async/await代替所有需要异步等待的点，这将会极大的简化你的代码。


## std::future futures::future::Future
Rust 的Future，有两个不同的来源：
* 首先是std::future::Future，来自 Rust 的标准库。
* 第二个是futures::future::Future，来自futures-rs 箱子。
而定义在futures-rs箱子中的 Future，是该类型的原始实现。（因为）要启用async/await语法，Future 的主要 trait 被转移到 Rust 的标准库中，成为了std::future::Future。从某种意义上说std::future::Future就可以看作是futures::future::Future。
