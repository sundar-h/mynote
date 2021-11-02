# 记录Rust设计模式的应用


## 函数式编程模式
* combinator pattern 解析器组合子
* 类似JavaScript中的Promise.then()的链式使用
* Haskell 的函数式编程
* 中间类似的generator/yield模式 --> .next() 或者Iterator概念
[用 Rust 学习解析器组合子 (combinator)](https://rustmagazine.github.io/rust_magazine_2021/chapter_6/parser-combinator.html)
```
iterator.map.filter....
```
