## 异步编程

[JS文档](http://caibaojian.com/es6/generator.html)

async/await 依赖生成器 将异步转换为同步
生成器Generator依赖迭代器 生成器返回迭代器对象


## vs ES6


## ES6 Generator 异步

特性:

1. 
生成器返回一个迭代器对象(Iterator Object)
2. 遇到`yield` 暂停执行, 返回`yield`之后的值 (value, false)
3. 遇到return, 返回 (return_value,true)

1. 状态机
2. 遍历器对象生成函数

返回一个指向内部状态的指针对象 --> [Iterator遍历器对象](./iterator.md)

`yield`: 暂停执行和恢复执行

Generator返回一个遍历器对象(Iterator)
只有调用了`next()`方法，才会执行(Lazy Evaluation 惰性求值)
