# 默认允许的 lints

默认情况下，这些 lint 都设置为'allow'级别。因此，除非您使用标志或属性将它们设置为更高的 lint 级别，否则它们将不会显示。

## 匿名参数

此 lint 检测匿名参数。一些触发此 lint 的示例代码：

```rust
trait Foo {
    fn foo(usize);
}
```

当设置为'deny'时，这将产生：

```text
error: use of deprecated anonymous parameter
 --> src/lib.rs:5:11
  |
5 |     fn foo(usize);
  |           ^
  |
  = warning: this was previously accepted by the compiler but is being phased out; it will become a hard error in a future release!
  = note: for more information, see issue #41686 <https://github.com/rust-lang/rust/issues/41686>
```

这种语法大多是历史意外，可以很容易地解决：

```rust
trait Foo {
    fn foo(_: usize);
}
```

## 裸性状对象

这个棉绒暗示使用`dyn Trait`对于特质对象。一些触发此 lint 的示例代码：

```rust
#![feature(dyn_trait)]

trait Trait { }

fn takes_trait_object(_: Box<Trait>) {
}
```

当设置为'deny'时，这将产生：

```text
error: trait objects without an explicit `dyn` are deprecated
 --> src/lib.rs:7:30
  |
7 | fn takes_trait_object(_: Box<Trait>) {
  |                              ^^^^^ help: use `dyn`: `dyn Trait`
  |
```

要解决此问题，请按照帮助消息的建议执行操作：

```rust
#![feature(dyn_trait)]
#![deny(bare_trait_objects)]

trait Trait { }

fn takes_trait_object(_: Box<dyn Trait>) {
}
```

## 箱形指针

这使用了 Box 类型。一些触发此 lint 的示例代码：

```rust
struct Foo {
    x: Box<isize>,
}
```

当设置为'deny'时，这将产生：

```text
error: type uses owned (Box type) pointers: std::boxed::Box<isize>
 --> src/lib.rs:6:5
  |
6 |     x: Box<isize> //~ ERROR type uses owned
  |     ^^^^^^^^^^^^^
  |
```

这种棉绒主要是历史性的，并不是特别有用。`Box<T>`以前用于构建语言，以及进行堆分配的唯一方法。今天的 Rust 可以调用其他分配器等。

## 消隐率寿命在路径

此 lint 检测隐藏生命周期参数的使用。一些触发此 lint 的示例代码：

```rust
struct Foo<'a> {
    x: &'a u32
}

fn foo(x: &Foo) {
}
```

当设置为'deny'时，这将产生：

```text
error: hidden lifetime parameters are deprecated, try `Foo<'_>`
 --> src/lib.rs:5:12
  |
5 | fn foo(x: &Foo) {
  |            ^^^
  |
```

终生 elision 在这一生中消失了，但是这个被弃用了。

## 缺少拷贝的实现

这个 lint 检测到可能被遗忘的实现`Copy`。一些触发此 lint 的示例代码：

```rust
pub struct Foo {
    pub field: i32
}
```

当设置为'deny'时，这将产生：

```text
error: type could implement `Copy`; consider adding `impl Copy`
 --> src/main.rs:3:1
  |
3 | / pub struct Foo { //~ ERROR type could implement `Copy`; consider adding `impl Copy`
4 | |     pub field: i32
5 | | }
  | |_^
  |
```

您可以通过派生来修复 lint`Copy`。

这个 lint 被设置为'allow'，因为这个代码并不坏;特别是这样写一个类似的新类型是很常见的`Copy`类型不再`Copy`。

## 缺少调试的实现

此 lint 检测到缺少的实现`fmt::Debug`。一些触发此 lint 的示例代码：

```rust
pub struct Foo;
```

当设置为'deny'时，这将产生：

```text
error: type does not implement `fmt::Debug`; consider adding #[derive(Debug)] or a manual implementation
 --> src/main.rs:3:1
  |
3 | pub struct Foo;
  | ^^^^^^^^^^^^^^^
  |
```

您可以通过派生来修复 lint`Debug`。

## 失踪文档

此 lint 检测到公共项目的缺失文档。一些触发此 lint 的示例代码：

```rust
pub fn foo() {}
```

当设置为'deny'时，这将产生：

```text
error: missing documentation for crate
 --> src/main.rs:1:1
  |
1 | / #![deny(missing_docs)]
2 | |
3 | | pub fn foo() {}
4 | |
5 | | fn main() {}
  | |____________^
  |

error: missing documentation for a function
 --> src/main.rs:3:1
  |
3 | pub fn foo() {}
  | ^^^^^^^^^^^^
```

要修复 lint，请为所有项添加文档。

## 单次使用的，寿命

此 lint 检测仅使用一次的寿命。一些触发此 lint 的示例代码：

```rust
struct Foo<'x> {
    x: &'x u32
}
```

当设置为'deny'时，这将产生：

```text
error: lifetime name `'x` only used once
 --> src/main.rs:3:12
  |
3 | struct Foo<'x> {
  |            ^^
  |
```

## 琐碎，铸件

这种棉绒可以检测到可以移除的琐碎石膏。一些触发此 lint 的示例代码：

```rust
let x: &u32 = &42;
let _ = x as *const u32;
```

当设置为'deny'时，这将产生：

```text
error: trivial cast: `&u32` as `*const u32`. Cast can be replaced by coercion, this might require type ascription or a temporary variable
 --> src/main.rs:5:13
  |
5 |     let _ = x as *const u32;
  |             ^^^^^^^^^^^^^^^
  |
note: lint level defined here
 --> src/main.rs:1:9
  |
1 | #![deny(trivial_casts)]
  |         ^^^^^^^^^^^^^
```

## 平凡的数字，铸件

此 lint 检测可以删除的数字类型的简单转换。一些触发此 lint 的示例代码：

```rust
let x = 42i32 as i32;
```

当设置为'deny'时，这将产生：

```text
error: trivial numeric cast: `i32` as `i32`. Cast can be replaced by coercion, this might require type ascription or a temporary variable
 --> src/main.rs:4:13
  |
4 |     let x = 42i32 as i32;
  |             ^^^^^^^^^^^^
  |
```

## 不可达，酒馆

这个棉绒触发了`pub`从箱子根无法到达的物品。一些触发此 lint 的示例代码：

```rust
mod foo {
    pub mod bar {

    }
}
```

当设置为'deny'时，这将产生：

```text
error: unreachable `pub` item
 --> src/main.rs:4:5
  |
4 |     pub mod bar {
  |     ---^^^^^^^^
  |     |
  |     help: consider restricting its visibility: `pub(crate)`
  |
```

## 不安全的代码

这种棉绒可以使用`unsafe`码。一些触发此 lint 的示例代码：

```rust
fn main() {
    unsafe {

    }
}
```

当设置为'deny'时，这将产生：

```text
error: usage of an `unsafe` block
 --> src/main.rs:4:5
  |
4 | /     unsafe {
5 | |
6 | |     }
  | |_____^
  |
```

## 不稳定的特性

此 lint 已弃用，不再使用。

## 未使用的-的 extern-包装箱

这种棉绒可以防止`extern crate`从未使用过的物品。一些触发此 lint 的示例代码：

```rust,ignore
extern crate semver;
```

当设置为'deny'时，这将产生：

```text
error: unused extern crate
 --> src/main.rs:3:1
  |
3 | extern crate semver;
  | ^^^^^^^^^^^^^^^^^^^^
  |
```

## 未使用的进口，括号

此 lint 捕获导入项目周围不必要的括号。一些触发此 lint 的示例代码：

```rust
use test::{A};

pub mod test {
    pub struct A;
}
# fn main() {}
```

当设置为'deny'时，这将产生：

```text
error: braces around A is unnecessary
 --> src/main.rs:3:1
  |
3 | use test::{A};
  | ^^^^^^^^^^^^^^
  |
```

要解决这个问题，`use test::A;`

## 未使用的，资格

此 lint 检测到不必要的限定名称。一些触发此 lint 的示例代码：

```rust
mod foo {
    pub fn bar() {}
}

fn main() {
    use foo::bar;
    foo::bar();
}
```

当设置为'deny'时，这将产生：

```text
error: unnecessary qualification
 --> src/main.rs:9:5
  |
9 |     foo::bar();
  |     ^^^^^^^^
  |
```

你可以打电话`bar()`直接，没有`foo::`。

## 未使用的，结果

此 lint 检查语句中表达式的未使用结果。一些触发此 lint 的示例代码：

```rust,no_run
fn foo<T>() -> T { panic!() }

fn main() {
    foo::<usize>();
}
```

当设置为'deny'时，这将产生：

```text
error: unused result
 --> src/main.rs:6:5
  |
6 |     foo::<usize>();
  |     ^^^^^^^^^^^^^^^
  |
```

## 变体大小的差异

此 lint 检测具有各种变体大小的枚举。一些触发此 lint 的示例代码：

```rust
enum En {
    V0(u8),
    VBig([u8; 1024]),
}
```

当设置为'deny'时，这将产生：

```text
error: enum variant is more than three times larger (1024 bytes) than the next largest
 --> src/main.rs:5:5
  |
5 |     VBig([u8; 1024]),   //~ ERROR variant is more than three times larger
  |     ^^^^^^^^^^^^^^^^
  |
```
