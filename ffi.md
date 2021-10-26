# Langurage FFI Resource

## 问题
* host和guest language 的内存管理问题
* 类型系统的转换 ABI?

## Rust

[The Rustonomicon](https://doc.rust-lang.org/nomicon/ffi.html)
[Rust死灵书 - Rust高级与非安全程序设计](https://www.bookstack.cn/read/rustonomicon_zh-CN/src-11.FFI.md#C%E4%BB%A3%E7%A0%81%E5%88%B0Rust%E5%87%BD%E6%95%B0%E7%9A%84%E5%9B%9E%E8%B0%83)


[Plugins in Rust: Getting Started](https://nullderef.com/blog/plugin-start/)
[A simple plugin interface for the Rust FFI](http://kmdouglass.github.io/posts/a-simple-plugin-interface-for-the-rust-ffi/)
[Plugins in Rust](https://adventures.michaelfbryan.com/posts/plugins-in-rust/)
[dynamic-loading--plugins](https://michael-f-bryan.github.io/rust-ffi-guide/dynamic_loading.html)

## Golang
[cgo](https://pkg.go.dev/cmd/cgo#hdr-Go_references_to_C)
[cgo Wiki](https://github.com/golang/go/wiki/cgo#function-variables)
[Calling C code from go](https://karthikkaranth.me/blog/calling-c-code-from-go/)
[The official Cgo documentation](https://pkg.go.dev/cmd/cgo)
[More details](https://eli.thegreenplace.net/2019/passing-callbacks-and-pointers-to-cgo/)
"" https://dev.to/mattn/call-go-function-from-c-function-1n3



## [Rust CString ](https://doc.rust-lang.org/std/ffi/struct.CString.html)

### 类比
CString --> &CStr
String --> &str
CString 一个有所有权，CStr 一个没有
CString.as_ptr() --> *const c_char 只读 read only

CString::as_bytes() 返回没有C 字符串结束符的\0的bytes
以*const u8 为参数的时候，使用


## Rust Call C Or C++
[Calling a tiny C code in Rust](https://liufuyang.github.io/2020/02/02/call-c-in-rust.html)
