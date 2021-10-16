[Trait Object 还是 Virtual Method Table](https://juejin.cn/post/7011317906969460766)
[Trait 使用及原理分析](https://liujiacai.net/blog/2021/04/27/trait-usage/)
## trait object 

* 属于 Dynamically Sized Types (DST), 在编译期无法确定大小，只能通过指针来间接访问，常见的形式有 Box<dyn trait> dyn trait 等

使用trait的方式
* 动态派发:  动态分发使用 trait object 的方式实现 Box<&dyn trait> / &dyn trait
* 静态派发: 静态分发使用 trait bound 或者 impl trait 方式实现编译期单态化，根据类型参数生成对应的结构或者函数

## 静态派发
`在 Rust 中，泛型的实现采用的是单态化（monomorphization），会针对不同类型的调用者，在编译时生成不同版本的函数，所以泛型也被称为类型参数。好处是没有虚函数调用的开销，缺点是最终的二进制文件膨胀。在上面的例子中， print_greeting_static 会编译成下面这两个版本：`
```
print_greeting_static_cat(Cat);
print_greeting_static_dog(Dog);
```


## 动态派发
`
通过&或者Box智能指针来实现动态派发
不是所有函数的调用都能在编译期确定调用者类型，一个常见的场景是 GUI 编程中事件响应的 callback，一般来说一个事件可能对应多个 callback 函数，而这些 callback 函数都是在编译期不确定的，因此泛型在这里就不适用了，需要采用动态派发的方式：
&dync trait 对象 包含数据域和虚函数表域指针, 分别指向具体的类型和对应的trait函数表
`
```
trait ClickCallback {
    fn on_click(&self, x: i64, y: i64);
}

struct Button {
    listeners: Vec<Box<dyn ClickCallback>>,
}
```

## impl trait
`
在 Rust 1.26 版本中，引入了一种新的 trait 使用方式，即：impl trait，可以用在两个地方：函数参数与返回值。 该方式主要是简化复杂 trait 的使用，算是泛型的特例版，因为在使用 impl trait 的地方，也是静态派发，而且作为函数返回值时，数据类型只能有一种，这一点要尤为注意！
`
* 因为是编译器决定的类型，所以返回值必须一致

```
fn print_greeting_impl(g: impl Greeting) {
    println!("{}", g.greeting());
}
print_greeting_impl(Cat);
print_greeting_impl(Dog);

// 下面代码会编译报错
fn return_greeting_impl(i: i32) -> impl Greeting {
    if i > 10 {
        return Cat;
    }
    Dog
}

// | fn return_greeting_impl(i: i32) -> impl Greeting {
// |                                    ------------- expected because this return type...
// |     if i > 10 {
// |         return Cat;
// |                --- ...is found to be `Cat` here
// |     }
// |     Dog
// |     ^^^ expected struct `Cat`, found struct `Dog`
```


## impl trait VS impl dyn trait

```
trait Animal {
     fn walk(&self) {
         println!("walk");
     }
 }
 ​
 impl dyn Animal {
     fn talk() {
         println!("talk");
     }
 }
 ​
 struct Person;
 ​
 impl Animal for Person {}
 ​
 fn main() {
     let p = Person;
     p.walk();
     //p.talk(); //如果执行执行这条会报错
     Animal::talk();
 }
```
意味着 impl dyn Trait实际上是给Trait Object增加方法
```
 trait Animal {
     fn walk(&self) {
         println!("walk");
     }
 }
 ​
 impl dyn Animal {
     fn talk(&self) {
         println!("talk");
     }
 }
 ​
 struct Person;
 ​
 impl Animal for Person {}
 ​
 fn demo() -> Box<dyn Animal> {
     let p = Person;
     Box::new(p)
 }
 ​
 fn main() {
     let p = Person;
     p.walk();
 ​
     let p1 = demo();
     p1.walk();
     p1.talk();
 }
```

* 静态派发相较动态派发好处, 少了一次虚函数表的寻址过程
* 坏处是导致编译的二进制文件 膨胀
* [Rust静态分发和动态分发](https://zhuanlan.zhihu.com/p/163650432)

* [Exploring Dynamic Dispatch in Rust]( https://alschwalm.com/blog/static/2017/03/07/exploring-dynamic-dispatch-in-rust/)
* [理解C++虚函数表]()
