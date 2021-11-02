## 
* Runtime
* GC
* coroutine
* 类型系统



## 协程 Coroutine

* Golang: Stackfull 有堆栈的
* Rust: Stackless 无堆栈的协程 即: 生成器 Generator


1. 使用 async/await 关键字很容易将普通的 Rust 代码转换为无栈协程（甚至可以使用宏来完成）
2. 不需要上下文切换和保存/恢复 CPU 状态
3. 无需处理动态栈分配
4. 非常节省内存
5. 允许跨阻塞点借用



## 类型系统

* Golang 类型严格，没有隐式类型转换
* Rust 有自动类型转换  ~~ 不太喜欢 (包括Into From 和 泛型约束 as_ref, as_mut)



## 工具生态

## 性能VS调试

* pprof --> golang
* pprof-rs --> rust
