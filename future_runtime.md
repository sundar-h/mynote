# 手动实现一个异步运行时

`200行理解Rust阅读笔记`

1. 反应器(Executor)
2. 执行器(Reactor)
3. Future

反应器和执行器永远不需要直接相互通信
核心是通过Waker对象来协调
边界

一旦你理解了 Waker 的生命周期和所有权，你就会从用户的角度理解 Future 是如何工作的。下面是生命周期：

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
ksjf
k, j, o, dd 不起作用