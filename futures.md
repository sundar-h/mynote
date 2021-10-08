
# Withoutboats的 零成本异步I/O: 阅读笔记

[翻译地址](https://zhuanlan.zhihu.com/p/97574385?utm_source=wechat_session&utm_medium=social&utm_oi=38990447116288&utm_campaign=shareopn)
[原视频地址](https://www.youtube.com/watch?v=skos4B5x7qE)


## 引用

JavaScript 的 promises 是及早求值（eagerly evaluated）的。这意味着一旦创建了它，它就开始运行一个任务。而 Rust 的 Futures 则是惰性求值（lazily evaluated）的。它们需要被轮询一次才开始工作。

async 将被修饰的代码块(通常是fn函数), 装饰为一个Future状态机.

## async / .await之后的代码查看

## 状态机

### Future

`可以理解为一个堆上的Future结构体`

1. 计算逻辑(业务实际代码)
2. 返回状态(Poll::Ready()或Poll::Pending),

### Executor 执行器

`当Future准备好计算时轮询Future`
负责调度Future
执行Future的Poll方法, Poll返回Pending(遇到IO操作),移交Future给Reactor反应器.

### Reactor 反应器

`当I/O事件完成时唤醒Future`
负责处理所有耗时的 I/O, 暂停点, 暂停点需要回复，保存状态，这个状态是Ready()和Pending
todo: 这里和基于回调的Promise模型对比一下(回调地狱, 内存方面)
放应器事件(IO)准备就绪, 调用wake唤醒future，并移交future传给执行器Executor

## Future迁移过程

## buffer队列

### 基于Poll模型的状态机和基于回调模型的状态机对比

todo:

Rust: --> Poll

1. 整个Future生命期间只需要
2. 一次对内存分配,其大小就是你将这个状态机分配到堆中的大小,并且没有额外的开销。你不需要装箱、回调之类的东西，只有真正零成本的完美模型;

## 时序 流程

通过句柄使用 Future 的模型, 一下流程中的Future都是指堆上的Future句柄

1. Executor(执行器)轮询(Poll) Future, 直到Future需要执行I/O.
    1.1 Executor(执行器)将Future移交给反应器(Reactor)
2. Reactor(反应器)等待I/O事件
    2.1 I/O事件完成, Reactor(放映器)调用waker唤醒Future, 并将Future传递给Exector(执行器)
3. 重复1. 2 步骤, 直到Future返回Ready()
    3.1 Future已经完成，执行器释放句柄和真个Future
    3.2 调度完成

总结: 我们轮询 Future ，然后等待 I/O 将其唤醒，然后一次又一次地轮询和唤醒，直到最终整个过程完成为止。
三个阶段:
    轮询: 执行器轮询Future直到它返回Pending
    等待: 反应器记录Future正在监听某个事件
    唤醒: 事件发生,反应器唤醒Future使其能够再次被轮询

## 如何解放双手(不用手动实现这种模的状态机)

### 组合器(Combinator)

嵌套回调之类，可读性非常差

### async/.await

async: 将函转换为一个返回Future的函数
await:  是一种语法糖, 它会进入一个loop循环, 再循环中轮询Poll, 直到完成，跳出循环

async: `async`关键字将我们的代码块重写为一个状态机
await: 每一个await点，代表了一个状态变化

```rust
await!($future) => {
    loop {
        match $future.poll() {
            Poll::Pending => yield Poll::Pending,
            Poll::Ready(value) => break value,
        }
    }
}
```

## 期待

Generator(生成器): 类似于 Python 或 JavaScript 的生成器，除了拥有可以 return 的函数，还能使用可以 yield 的函数，这样你就可以在 yield 之后再次恢复执行；并且你可以将这些函数作为编写迭代器和流的方式，就像异步函数能够让你像编写普通函数那样编写 Future 一样。