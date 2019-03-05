# 生成目标

`rustc`默认情况下是跨平台编译器。这意味着，您可以用编译器来构建任何体系结构。*目标*列表是您可以构建的可能架构。

要查看可以设置的，生成目标的所有选项，请参阅[这里](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_target/spec/struct.Target.html)的文档。

要编译到特定目标，请使用`--target`标志：

```bash
$ rustc src/main.rs --target=wasm32-unknown-unknown
```
