# 什么是 rustc？

欢迎来到“rustc 书”！`rustc`是 Rust 编程语言的编译器，由项目组开发提供。编译器将您的源代码和生产二进制代码，变成一个或可执行文件。

大多数 Rust 程序员都不会直接调用`rustc`，而是通过[Cargo](../cargo/index.html)来完成，虽然这一切都只是调用`rustc`流程！如果你想看看 Cargo 如何调用`rustc`， 您可以

```bash
$ cargo build --verbose
```

它会打印出每个`rustc`调用。本书可以帮助您了解每个选项的作用。此外，虽然大多数 Rustaceans 使用 Cargo，但并非所有人都这样做：有时，他们将`rustc`整合进其他构建系统。本书会提供您需要执行此操作的所有选项的指南。

## 基本用法

假设你在文件中有一个小小的 hello world 程序，`hello.rs`：

```rust
fn main() {
    println!("Hello, world!");
}
```

要将此源代码转换为可执行文件，您可以使用`rustc`：

```bash
$ rustc hello.rs
$ ./hello # 在 *NIX
$ .\hello.exe # 在 Windows
```

注意，通常我们只在使用`rustc`时，传递*crate 根文件*，不是(我们希望编译的)每个文件。例如，如果我们有一个`main.rs`，看起来像这样：

```rust,ignore
mod foo;

fn main() {
    foo::hello();
}
```

还有一个`foo.rs`，如下：

```rust,ignore
pub fn hello() {
    println!("Hello, world!");
}
```

要编译它，我们将运行此命令：

```bash
$ rustc main.rs
```

没必要告诉`rustc`关于`foo.rs`文件; 该`mod`语句为`rusts`提供所需的一切。这与您使用 C 编译器的方式不同，在 C 编译器中，您要在每个文件上调用编译器，然后将所有内容链接在一起。换句话说，*crate*就是一个编译单位，而不是一个特定的模块组。
