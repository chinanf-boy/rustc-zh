# 代码生成选项

所有这些选项都通过`-C`标志，传递给`rustc`，缩写“C = codegen”。通过运行`rustc -C help`，给出该版本的详细帮助列表。

## ar

此选项已弃用，不起任何作用。

## linker

此标志允许您控制，`rustc`调用哪个链接器来链接代码。

## link-arg=val

此标志允许您向调用的链接器，附加一个额外参数。

重要“温馨提示”；您可以多次传递此标志，以添加多个参数。

## link-args

此标志允许您向调用的链接器，附加多个额外参数。选项应以空格分隔。

## linker-flavor

此标志允许您通过`rustc`控制链接器设置。如果提供的一个链接器与`-C linker`标志(上面描述的)，然后根据提供的值推断链接器风格。如果没有提供链接器，则链接器风格可用来确定哪个链接器。每个`rustc`生成目标默认为某种链接器风格。

## link-dead-code

通常，链接器会删除死代码。此标志禁用此行为。

一个例子是，当试图构造代码覆盖率的衡量时，这个标志可能有用的。

## lto

此标志指示 LLVM 使用[链接时间优化](https://llvm.org/docs/LinkTimeOptimization.html)。

它需要`thin`和`fat`两个值中的一个，，'thin'的 LTO[是 LLVM 的新功能](http://blog.llvm.org/2016/06/thinlto-scalable-and-incremental-lto.html)，“fat”指 LTO 的经典版本。

## target-cpu

这指示`rustc`为特定的处理器，生成专用代码。

你可以`rustc --print target-cpus`，查看在此处传递的有效选项。此外，`native`可以传递，本主机的处理器。

## target-feature

单个目标将也支持不同的功能；此标志允许您控制启用或禁用功能。

要查看有效选项和使用示例，请运行`rustc --print target-features`.

## passes

此标志可用于向编译器，添加额外的 LLVM 传递参数(passes)。

该列表必须用空格分隔。

## llvm-args

此标志可用于将参数列表，直接传递给 LLVM。

该列表必须用空格分隔。

## save-temps

`rustc`将在编译期间生成临时文件；通常在完成工作后将其删除。此选项将使它们保留而不是删除。

## rpath

此选项允许您设置[`rpath`](https://en.wikipedia.org/wiki/Rpath)的值。

## overflow-checks

此标志允许您控制整数溢出的行为。此标志可以传递许多选项：

- 打开溢出检查：`y`，`yes`，或`on`.
- 关闭溢出检查：`n`，`no`或`off`.

## no-prepopulate-passes

传递参数管理器预先填充了传递列表；此标志会确保列表为空。

## no-vectorize-loops

默认情况下，`rustc`将尝试[向量化循环](https://llvm.org/docs/Vectorizers.html#the-loop-vectorizer)。 此标志将关闭该行为。

## no-vectorize-slp

默认情况下，`rustc`将尝试使用[superword-level 并发](https://llvm.org/docs/Vectorizers.html#the-slp-vectorizer). 此标志将关闭该行为。

## soft-float

此选项让`rustc`使用“软浮动”生成代码。默认情况下，许多硬件支持浮点数指令，因此生成的代码将利用这一点。“软浮动”在软件中模拟浮点数指令。

## prefer-dynamic

默认情况下，`rustc`倾向于静态链接依赖项。此选项将使其改用动态链接。

## no-integrated-as

LLVM 附带一个内部汇编程序；此选项将允许您使用外部汇编程序。

## no-redzone

此标志允许您禁用[红色地带](<https://en.wikipedia.org/wiki/Red_zone_(computing)>)。 此标志可以传递许多选项：

- 启用：`y`，`yes`或`on`.
- 禁用：`n`，`no`或`off`.

## relocation-model

此选项允许您选择要使用的重新定位模型。

要查找此标志的有效选项，请运行`rustc --print relocation-models`.

## code-model=val

此选项允许您选择要使用的代码模型。

要查找此标志的有效选项，请运行`rustc --print code-models`.

## metadata

此选项允许您控制用于符号管理的元数据。

## extra-filename

此选项允许您在每个输出文件名中，放置额外的数据。

## codegen-units

此标志允许您控制在执行代码生成时，使用的线程数。

增加并行性可能会加快编译时间，但也可能产生较慢的运行代码。

## remark

此标志允许您为这些优化过程打印备注。

传递参数列表应以空格分隔。

`all`：每个传递参数都要备注。

## no-stack-check

此选项已弃用，不起任何作用。

## debuginfo

此标志允许您控制调试信息：

- `0`：完全没有调试信息
- `1`：仅表格行
- `2`：完整的调试信息

## opt-level

此标志允许您控制优化级别。

- `0`：无优化，也打开`cfg(debug_assertions)`.
- `1`：基本优化
- `2`：一些优化
- `3`：所有优化
- `s`：优化二进制大小
- `z`：优化二进制大小，但也关闭向量化循环。

## debug-assertions

这个标志让你，控制`cfg(debug_assertions)`打开或关闭。

## inline-threshold

此选项允许您设置内联一个函数的阈值。

默认值为 225。

## panic

此选项允许您控制代码恐慌时，发生的情况。

- `abort`：紧急终止进程
- `unwind`：恐慌时打开堆栈

## incremental

此标志允许您启用强化编译。
