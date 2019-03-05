# 命令行参数

这里是，`rustc`能做什么的命令行参数列表。

## `-h`/`--help`： 帮助下

该标志将打印出`rustc`的帮助信息。

## `--cfg`：配置编译环境

此标志可以打开或关闭各种`#[cfg]`设置。

该值可以是单个标识符，也可以是由`=`分隔两个标识符。

举些例子，`--cfg 'verbose'`要么`--cfg 'feature="serde"'`。分别对应`#[cfg(verbose)]`和`#[cfg(feature = "serde")]`。

## `-L`：将目录添加到库搜索路径

要查找外部包时，就传递给此标志的目录，rustc 会搜索。

## `-l`：将生成的包链接到一个原生库

此标志允许您在构建包时，指定一个特定原生库的链接。

## `--crate-type`：编译器要发出的包的类型列表

这指示`rustc`构建哪种箱子类型。

## `--crate-name`：指定正在构建的包的名称

这告诉`rustc`，您的箱子名称。

## `--emit`：发出除箱子以外的输出

这个标志可以打印出装配或 LLVM-IR 之类的东西，而不是生成一个箱子。

## `--print`：打印编译器信息

该标志打印出有，关编译器的各种信息。

## `-g`：包含调试信息

与`-C debuginfo=2`同义，更多看[这里](codegen-options/index.zh.html#debuginfo)。

## `-O`：优化您的代码

与`-C opt-level=2`同义，更多看[这里](codegen-options/index.zh.html#opt-level)。

## `-o`：输出的文件名

此标志控制，输出的文件名。

## `--out-dir`：用于写入输出的目录

输出包，被写入的目录。

## `--explain`：提供错误消息的详细说明

每个错误，`rustc`附带一个错误代码;这将打印出给定错误的更详细解释。

## `--test`：构建测试工具

编译这个箱子时，`rustc`会忽略你的`main`功能，而不是产生一个测试工具。

## `--target`：选择要构建的目标三元组

控制了哪个生产[目标](targets/index.zh.html)。

## `-W`：设置 lint 警告

该标志将设置，应将哪些 lint 设置为[警告水平](lints/levels.zh.html#warn)。

## `-A`：设置 lint 允许

该标志将设置，应将哪些 lint 设置为[允许水平](lints/levels.zh.html#allow)。

## `-D`：设置 lint 拒绝

该标志将设置，应将哪些 lint 设置为[拒绝等级](lints/levels.zh.html#deny)。

## `-F`：设置 lint 禁止

该标志将设置，应将哪些 lint 设置为[禁止等级](lints/levels.zh.html#forbid)。

## `--cap-lints`：设置最严格的 lint 级别

这个标志让你'盖'lints，更多，[看这里](lints/levels.zh.html#capping-lints)。

## `-C`/`--codegen`：代码生成选项

此标志将允许您,设置[codegen 选项](codegen-options/index.zh.html)。

## `-V`/`--version`：打印版本

此标志将打印出来`rustc`的版本。

## `-v`/`--verbose`：使用详细输出

该标志与其他标志组合使用时，会产生额外的输出。

## `--extern`：指定外部库的位置

此标志允许您传递外部箱子的位置和名称，并链接该箱到您正在构建的箱子中。

## `--sysroot`：覆盖系统根目录

“sysroot”就是`rustc`寻找 Rust安装附带的箱子;这个标志允许被覆盖。

## `--error-format`：控制错误的产生方式

此标志允许您控制错误的格式。

## `--color`：配置输出的着色

此标志可让您控制输出的颜色设置。
