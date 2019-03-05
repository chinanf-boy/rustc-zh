# Lint

在软件中，“lint”是用于帮助改进源代码的工具。Rust 编译器包含许多 lints，当它编译你的代码时，它也会运行 lints。根据您配置的方式，这些 lint 可能会产生警告，错误或根本没有任何内容。

这是一个小例子：

```bash
$ cat main.rs
fn main() {
    let x = 5;
}
$ rustc main.rs
warning: unused variable: `x`
 --> main.rs:2:9
  |
2 |     let x = 5;
  |         ^
  |
  = note: #[warn(unused_variables)] on by default
  = note: to avoid this warning, consider using `_x` instead
```

这是`unused_variables`lint，它告诉你，你已经引入了一个你不在代码中使用的变量。注意`warning: unused variable:`x``不是*错误*，所以这不是错误，但它可能是一个 bug，所以你得到一个警告。
