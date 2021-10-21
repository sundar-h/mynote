## Rust 隐式类型转换
[](https://juejin.cn/post/6999829181680844831)
[Rust 隐式类型转换（Coercions）](http://www.voycn.com/article/rust-yinshileixingzhuanhuancoercions)


## 隐士转换

&String -> &str

&Box<T> --> &T

&Vec<T> --> &[T]



sub-trait coercion
super-trait coercion

mut  * ref deref as_ref, as_mut



* 对象不可以使用 dyn trait的方法
* dyn trait对象可以使用对象的方法 和 dyn trait的方法
* super-trait coercion 可以使用 #![feature(trait_upcasting)] 来支持
  - [trait_upcasting](https://doc.rust-lang.org/nightly/unstable-book/language-features/trait-upcasting.html#trait_upcasting)
