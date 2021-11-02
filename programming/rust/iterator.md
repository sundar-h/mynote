
# 遍历器对象
* Es6 遍历器对象
* Rust 迭代器 

## ES6 Iterator

三个作用
任何数据结构只要实现了Iterator接口，就可以完成遍历操作
1. 为各种数据结构，提供一个统一的、渐变的访问接口
2. 使数据成员能够按照某种次序排列
3. 使用 for .. of循环消费


执行逻辑
（1）遇到yield语句，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield语句。

（3）如果没有再遇到新的yield语句，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

## Rust 迭代器
* 手动实现，需要提供next方法

```
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}
```

 再`for .. in `结构中使用, `for`结构会使用`.into_iterator()`方法将一些集合类型转换为迭代器.
