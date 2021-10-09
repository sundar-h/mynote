# 手动实现一个异步运行时

`200行理解Rust阅读笔记`

1. 反应器(Executor)
2. 执行器(Reactor)
3. Future

反应器和执行器永远不需要直接相互通信
核心是通过Waker对象来协调
边界

一旦你理解了 Waker 的生命周期和所有权，你就会从用户的角度理解 Future 是如何工作的。下面是生命周期：
* Waker工作原理
* Future的移动

* 执行器创建一个 Waker
* 当一个 Future 在执行器中注册时，它会被赋予一个由执行器创建的 Waker 对象的克隆。由于这是一个共享对象（例如 Arc<T>），所有的克隆实际上都指向同一个底层对象。
* Future 克隆了 Waker，并将其传递给反应器，反应器将其储存起来，以便日后使用。

```rust
let none_leaf_fut = async {
    let leaf_fut = Reactor::new_io(...);
    let result = leaf_fut.await;
};
rt.block_on(none_leaf_fut);
```

``` rust
use futures::executor::block_on;

async fn hello_world() {
    println!("hello, world!");
}

fn main() {
    let future = hello_world(); // Nothing is printed
    block_on(future); // `future` is run and "hello, world!" is printed
}
```

* Future什么时候，真正开始执行
1. await: 每一个await代表一次状态变更.
2. block_on

看起来好像是 await的时候 传递给Executor, 执行Poll操作
await 移动Future到Executor

Future创建的时候 就被放到了执行器的Ready Queue

* Ready Queue --> Executor
* Waiting Queue

block_on 传递Future到Executor
wake() 转移把Future从Waiting Queue转移到Ready Queue，并唤醒Executor
Executor takes Future and Poll
