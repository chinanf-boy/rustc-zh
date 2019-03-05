# 自定义目标

如果您想为尚未支持的目标构建`rustc`，您可以使用“自定义目标规范”来定义目标。这些目标规范文件是 JSON。要查看主机目标的 JSON，您可以运行：

```bash
$ rustc +nightly -Z unstable-options --print target-spec-json
```

要查看其他目标，请添加`--target`标志：

```bash
$ rustc +nightly -Z unstable-options --target=wasm32-unknown-unknown --print target-spec-json
```

要使用自定义目标，请参阅[`xargo`](https://github.com/japaric/xargo)。
