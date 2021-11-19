## FFI 总结

```
语言之间二进制层面的交互, 统一使用C ABI
```



* 内存模型差异
* 内存访问
  * Golang
  * Rust

* 基本类型的映射

* 复杂类型 通过指针传递



## 总体原则

* 基准定义的地方，负责内存的创建和销毁

## C与Golang的内存模型差异

1. C语言的内存在分配之后就是稳定的，只要不是被人为提前释放，那么在Go语言空间可以放心大胆地使用

2. Go语言因为函数栈的动态伸缩可能导致栈中内存地址的移动(这是Go和C内存模型的最大差异) --> goroutinue的栈因为空间不足而发生了扩展，导致原来的Go语言内存被移动到了新的位置

* 因为1，**Golang访问C**内存是安全的(再C.free之前)

* 因为2, **C访问Golang**的内存是不安全的(内存指针内容可能已经移动, 指针失效)

* C 如何访问Golang内存的方式

  1. 传值的方式: 复制Golang的内存数据到新创建的C语言内存空间中。

     ```golang
     package main
     
     /*
     void printString(const char* s) {
         printf("%s", s);
     }
     */
     import "C"
     
     func printString(s string) {
         cs := C.CString(s)
         defer C.free(unsafe.Pointer(cs))
     
         C.printString(cs)
     }
     
     func main() {
         s := "hello"
         printString(s)
     }
     ```

     

  2. C直接使用Golang语言空间的内存地址: 

​		

```
```

