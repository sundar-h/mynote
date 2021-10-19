## 函数计算

* 线性处理流水线可以使用以下核心抽象在Akka Streams中表达

### Source
数据流入端
eg: mqtt --> Subscribe
数据源，产生数据

### Sink
数据流出端
eg: mqtt --> Publish
接收端, 消费数据

### 概念上可以借鉴 Akka 
`一个并行Actor Model` 异步通信模型


1. pipeline
2. plugin
3. wasm function


## pipeline
`这个类似 Actor Model 并发模型`
我的理解是一个多阶段任务流

参考Spark Flink storm 架构设计

我的理解和Actor Model不谋而合
* 任务拆分为阶段 (由使用者来编写任务流文件定义)
* 每个阶段 执行 只需要知道 自己的上游(UpStream)和下游(DownStream) [ps: Web IDE指定]
* 业务处理流程...
* 以消息为驱动 (Actor Model里面称为邮编地址)


单任务 作业 流

### 插件扩展机制 
1. Source: 从其它消息源接受数据
2. Sink:   将数据发送到外部系统; eg: MQTT, HTTP, 日志系统, 数据库
3. Function:  可重用业务逻辑

1. Rust原生支持--> 本身ABI并不未定, 注意版本 [abi_stable](https://github.com/rodrimati1992/abi_stable_crates/)
2. FFI 方式多语言支持
3. Wasm 支持

## wasm 不重要 算是 Plugin 的function的一个支持

https://github.com/lf-edge/ekuiper.git


## WebAssembly Runtime

* [WasmEde Runtime](https://wasmedge.org/)


## 其它讨论

### Rust does not have a stable ABI

* [rust-abi-wiki](https://github.com/slightknack/rust-abi-wiki)
* [A wiki about Rust's ABI and plans to stabilize it](https://slightknack.github.io/rust-abi-wiki/)

* [Rust does not have a stable ABI](https://people.gnome.org/~federico/blog/rust-stable-abi.html)


## 借鉴

### Golang
* [YoMo](https://www.secondstate.io/articles/yomo-wasmedge-real-time-data-streams/)
* [ekuiper](https://github.com/lf-edge/ekuiper/blob/master/docs/en_US/manager-ui/overview.md)


## Playground 借鉴

* About 里有对应的源码链接
[Golang Playground](https://play.golang.org/)
[Rust Playground](https://play.rust-lang.org/?version=nightly&mode=debug&edition=2021)
