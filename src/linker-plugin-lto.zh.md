# Linker-plugin-LTO

`-C linker-plugin-lto`标志 允许将 LTO 优化推迟到实际链接步骤，如果链接的所有目标文件都是由基于 LLVM 的工具链创建的，则相应地允许跨编程语言，执行过程优化。这里的主要示例是将 Rust 代码与 Clang 编译的 C/C++代码链接在一起。

## 用法

有两种主要情况，可以使用基于 LTO 的链接器插件：

- 编译一个 Rust`staticlib`，用作一个 C ABI 依赖项
- 编译一个 Rust 二进制文件，这里是`rustc`调用链接器

在这两种情况下，Rust 代码都必须使用`-C linker-plugin-lto`和 C/C++代码要用`-flto`或`-flto=thin`，以便将目标文件作为 LLVM bitcode 发出。

### Rust`staticlib`作为 C/C++程序中的依赖

在这种情况下，Rust 编译器只需要确保`staticlib`的对象文件格式正确。对于链接，必须使用具有 LLVM 的链接器插件（例如 LLD）。

直接用`rustc`：

```bash
# 编译 the Rust staticlib
rustc --crate-type=staticlib -Clinker-plugin-lto -Copt-level=2 ./lib.rs
# `-flto=thin`编译 C 代码
clang -c -O2 -flto=thin -o main.o ./main.c
# 链接 一切,确保 我们使用适当的链接器
clang -flto=thin -fuse-ld=lld -L . -l"name-of-your-rust-lib" -o main -O2 ./cmain.o
```

运用`cargo`：

```bash
# 编译 the Rust staticlib
RUSTFLAGS="-Clinker-plugin-lto" cargo build --release
# `-flto=thin`编译 C 代码
clang -c -O2 -flto=thin -o main.o ./main.c
# 链接 一切,确保 我们使用适当的链接器
clang -flto=thin -fuse-ld=lld -L . -l"name-of-your-rust-lib" -o main -O2 ./cmain.o
```

### C/C++代码作为 Rust 中的依赖项

在这种情况下，链接器将被`rustc`调用。我们必须再次确保使用适当的链接器。

直接用`rustc`：

```bash
# 编译 C code with `-flto`
clang ./clib.c -flto=thin -c -o ./clib.o -O2
# 拿C 代码，创建一个静态库
ar crus ./libxyz.a ./clib.o

# 附加参数，调用 `rustc`
rustc -Clinker-plugin-lto -L. -Copt-level=2 -Clinker=clang -Clink-arg=-fuse-ld=lld ./main.rs
```

直接用`cargo`：

```bash
# 编译 C code with `-flto`
clang ./clib.c -flto=thin -c -o ./clib.o -O2
# 拿C 代码，创建一个静态库
ar crus ./libxyz.a ./clib.o

# 通过 RUSTFLAGS，设链接的参数
RUSTFLAGS="-Clinker-plugin-lto -Clinker=clang -Clink-arg=-fuse-ld=lld" cargo build --release
```

### `rustc`明确指定要使用的链接器插件

如果想要使用 LLD 以外的链接器，则必须明确指定 LLVM 链接器插件。否则链接器无法读取目标文件。插件的路径作为参数传递给`-Clinker-plugin-lto`选项：

```bash
rustc -Clinker-plugin-lto="/path/to/LLVMgold.so" -L. -Copt-level=2 ./main.rs
```

## 工具链兼容性

为了使这种 LTO 工作，LLVM 链接器插件必须能够处理由`rustc`和`clang`两者生成的 LLVM bitcode。

使用一个`rustc`和`clang`可以获得最佳效果，它们基于完全相同的 LLVM 版本。可以使用`rustc -vV`，查看给定`rustc`版本的 LLVM。请注意，此处给出的版本号只是一个近似值，因为 Rust 有时会使用不稳定的 LLVM 修订版。但是，近似值通常是可靠的。

下表，显示了工具链版本的已知良好组合。

|           | Clang 7 | Clang 8 |
| --------- | ------- | ------- |
| Rust 1.34 | ✗       | ✓       |
| Rust 1.35 | ✗       | ✓（？） |

请注意，此功能的兼容性策略将来可能会更改。
