# Rust 中依赖注入的类型
## 前言
依赖注入是当下面向对象设计中重要的设计模式之一，但在 Rust 中因为各种限制可能无法实现一个与 Java 或 C# 中的注入容器相媲美的容器，参考了语言特征和 GitHub 上现有类库，总结出下面两种类型的依赖注入
## 基于特性对象（dyn trait）的依赖注入
### 特点
- 容器支持形式多，可以用常见的控制反转（IoC）容器，也可以使用目前不常使用的服务定位器（Service/Resource Locator，常见于 Servlet、WPF（.NET Framework）、ASPX 等，不过目前该形式不建议使用）
- 可以运行时执行，也可以编译时执行，但大量使用特性对象可能会降低效率（全单例或许会减少损耗，因为创建对象仅在启动时，典型例子 Spring Boot）
- Rust 本身缺乏反射机制，可能有些操作需要奇技淫巧才能完成。
### 简单示例
```rust
struct UserController {
    user_repository: Box<dyn UserRepository>,
}

impl UserController {
    fn get_users(&self) -> Vec<User> {
        self.user_repository.get_users()
    }
}
```
### Rust 的典型实现
- [coi](https://crates.io/crates/coi)
- [shaku](https://crates.io/crates/shaku)
## 基于泛型（特性模板）的依赖注入
### 特点
- 容器（或许有）实现复杂，需要在编译时识别依赖树，大型项目会导致编译时间非常长，且报错可能找不到（例如 Android 上的 Dagger2/hilt，其中比较严重的坑就是如果对他的容器不熟悉很容易陷入找不到依赖的问题，甚至是直接报错）
- 不涉及反射
- 编译时执行生成或依赖树由开发人员自行编写
### 简单示例
```rust
struct UserController<T> where T: UserRepository {
    user_repository: T,
}

impl<T: UserRepository> UserController<T> {
    fn get_sers(&self) -> Vec<User> {
        self.user_repository.get_users()
    }
}
```
### Rust 的典型实现
无
## 结论
在性能要求不是很高或完全满足的情况下应该使用本文所述第一种的容器，当然使用全单例的启动时加载也不失为一种优雅的方法。在对性能要求苛刻或希望尽可能减少调用的情况下，使用第二种更好，但不推荐自行编写，实现比较麻烦。