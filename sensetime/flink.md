## 流处理架构

https://www.wtog.fun/2019/09/10/flink-vs-spark.html

* 核心

* Engine
  * Runtime --  运行时支持
    * FFI  -- ABI 多语言模块支持 函数计算
    * Docker ...
    * Wasm -- Wasm Runtime
    * Playground 这里可以直接集成官方的Playground (golang, rust 的都是开源的)
  * Manager -- 调度 管理模块
    * Job
    * Task  -- 
    * Plugin
      * Source 向内部系统输入数据源 -- 通信模块
      * Sink   向外部数据吐出数据   -- 通信模块
      * Function 业务逻辑           -- 业务模块
  * Extern 
  * Examples
  * Docs
