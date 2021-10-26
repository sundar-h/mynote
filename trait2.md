## 类型系统

转换

## 外部模块添加方法

[Rust: Extending a Type](https://cmcenroe.me/2016/08/22/rust-extending-type.html)
futures 的StreamExt等方法扩展



## []? 的其它用法 类型转换](https://rustwiki.org/zh-CN/rust-by-example/error/multiple_error_types/reenter_question_mark.html)

? 是 “要么 unwrap 要么 return Err(From::from(err))”。From::from 是 不同类型间的转换工具，也就是说，如果在错误能够转换成返回类型的地方使用 ?，它就 会自动转换成返回类型。
