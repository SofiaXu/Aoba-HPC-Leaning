# 在 Rust 中实现插件系统方案分析与选择
## 备选方案概述

根据开发经验，笔者认为扩展方案可分为三种，脚本语言、IPC、互操作。各个方案又有各种可行实现：
1. 脚本语言
    - Lua
    - Javascript
2. IPC
    - gRPC
    - 管道
    - 共享内存
3. 互操作
    - FFI

本文将分析各种实现在 Rust 中的可行性

## 脚本语言
### Lua
Lua 是时下流行的脚本语言，上手简单，运行速度尚可，在游戏领域使用比较广泛（例如：文明系列、钢铁雄心等），其核心运行时也比较小，在 200-600 KiB 左右，在 rust 中也有对应的 Binding（例如 hlua、rlua）。

### Javascript
Javascript 是 Web 前端的脚本语言，近年来也有所发展，在 Node.js 的加持下，可以进行后端甚至桌面端的编程。一般所使用的 v8 引擎虽然比较大，但运行速度快且 Javascript 默认的库也比较丰富。在 rust 中的 Binding 不如 lua 丰富（例如 v8、nodejs）。

### 结论
使用脚本语言进行扩展只需要引用相关的运行时库近乎 0 开销，一般不会接触到操作系统或平台相关的操作，对跨平台兼容性好，同时在外置沙箱中运行，可以防止用户直接调用不相关方法。

示例代码
```rust
use hlua::Lua;

let mut lua = Lua::new();
lua.execute::<()>("a = 12 * 5").unwrap();
let a: i32 = lua.get("a").unwrap();
```

## IPC
### gRPC
gRPC 是谷歌开发的 RPC 框架。其特点是轻量、不需要注册中心、基于 HTTP/2 实现、跨平台支持好（可以根据接口定义进行代码生成）、报文紧凑等。目前多用于微服务。

### 管道
较为传统的 IPC 方式，可以在几乎任何操作系统上使用（Windows 和 Unix 的管道系统有差别），通常比 Socket 的通信方式开销少，速度较快。

### 共享内存
共享内存的方式存在于很多地方（例如 OpenMP、SQL Server），这种方式在单机上是几乎是没有任何开销的，但在进程间是不安全的，而且高度依赖操作系统。与 FFI 相比没有任何优势。机器间传输可以考虑使用 RDMA。

### 结论
本分类中 gRPC 看起来是开销最大的，但是不管是兼容性还是安全性都是最好的。管道虽然在进行包装后也可以进行 gRPC，但对跨操作系统的支持并不好。共享内存虽然是开销最小的选项，但是不管是兼容性（网络适配器、操作系统等）还是安全性（并不支持认证等）都是不高。

## 互操作
### FFI
通过接口调用其他语言的方法（例如 JNI、P/Invoke），这种方法经常应用于客户端应用，开销几乎为 0，不需要额外的内存和进程。但在热插拔等方面就不是很理想，可能需要在启动的时候加载入内存，或者 DI 相关组件等与插件系统兼容性不高，需要重写转接措施。另外对于使用虚拟机的运行时可能还需要运行其虚拟机才能继续执行代码。

## 结论
使用 gRPC/RESTful 或者说基于 Socket 的 Plugin System 可能是最佳的选择，不仅可以实现分布式部署和负载均衡，而且是语言无关的，可以在保证安全的情况下兼容各种语言。