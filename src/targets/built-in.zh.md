# 内置目标

`rustc`能够自动编译到多个目标，我们称之为“内置”目标，它们通常对应团队直接支持的目标。

要查看内置目标列表，您可以运行`rustc --print target-list`，或者看看[API 文档](https://doc.rust-lang.org/nightly/nightly-rustc/rustc_target/spec/index.html#modules)。每个模块为特定目标，都定义了一个构建器。
