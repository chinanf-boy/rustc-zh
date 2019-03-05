# Lint 等级

在`rustc`，lints 分为四个*等级*：

1.  允许(allow)
2.  警告(warn)
3.  拒绝(deny)
4.  禁止(forbid)

每个 lint 都有一个默认级别（在本章后面的 lint 列表中进行了解释），并且编译器具有默认警告级别。首先，让我们解释这些级别的含义，然后再讨论配置。

## 允许

存在这些 lint，但默认情况下不执行任何操作。例如，请考虑以下源代码：

```rust
pub fn foo() {}
```

编译此文件不会产生警告：

```bash
$ rustc lib.rs --crate-type=lib
$
```

但是这段代码违反了`missing_docs`lint(缺乏说明文档)。

这些 lint 主要由配置手动打开，我们将在本节后面讨论。

## 警告

如果您违反“警告”等级的 lint，将发出警告。例如，这段代码与`unused_variable`lint 相违背：

```rust
pub fn foo() {
    let x = 5;
}
```

这将产生此警告：

```bash
$ rustc lib.rs --crate-type=lib
warning: unused variable: `x`
 --> lib.rs:2:9
  |
2 |     let x = 5;
  |         ^
  |
  = note: #[warn(unused_variables)] on by default
  = note: to avoid this warning, consider using `_x` instead
```

## 拒绝

如果您违反“拒绝”等级的 lint，将发出错误。例如，此代码违背`exceeding_bitshifts`lint。

```rust,ignore
fn main() {
    100u8 << 10;
}
```

```bash
$ rustc main.rs
error: bitshift exceeds the type's number of bits
 --> main.rs:2:13
  |
2 |     100u8 << 10;
  |     ^^^^^^^^^^^
  |
  = note: #[deny(exceeding_bitshifts)] on by default
```

lint 的错误和常规的传统错误有什么区别？Lints 可以通过级别进行配置，因此像“允许”lints 的方式，能让默认情况下“拒绝”的警告不再出现。同样，您若希望设置，让一个默认情况下`warn`lint 能产生错误。那 lint 等级能帮到你。

## 禁止

'禁止'是一种比'拒绝'更强的特殊 lint 等级。它与'deny'相同，因为此级别的 lint 会产生错误，但与'deny'级别不同，'禁止'级别不能被低于一个错误的信息所覆盖。但是，lint 等级仍可能受到`--cap-lints`限制（见下文），所以`rustc --cap-lints warn`将把 '禁止'lints 设置为警告。

## 配置警告级别

记住我们的`missing_docs`，来自“允许”lint 级别的示例？

```bash
$ cat lib.rs
pub fn foo() {}
$ rustc lib.rs --crate-type=lib
$
```

我们可以配置这个 lint ，升到更高的级别。可用编译器命令行标志，源代码中的属性也行。

您还可以“cap”lints，以便编译器可以选择忽略某些 lint 级别。我们最后再谈。

### 通过编译器标志

这个`-A`，`-W`，`-D`，和`-F`标志，允许您将一个或多个 lints 转换为允许、警告、拒绝或禁止级别，如下所示：

```bash
$ rustc lib.rs --crate-type=lib -W missing-docs
warning: missing documentation for crate
 --> lib.rs:1:1
  |
1 | pub fn foo() {}
  | ^^^^^^^^^^^^
  |
  = note: requested on the command line with `-W missing-docs`

warning: missing documentation for a function
 --> lib.rs:1:1
  |
1 | pub fn foo() {}
  | ^^^^^^^^^^^^
```

```bash
$ rustc lib.rs --crate-type=lib -D missing-docs
error: missing documentation for crate
 --> lib.rs:1:1
  |
1 | pub fn foo() {}
  | ^^^^^^^^^^^^
  |
  = note: requested on the command line with `-D missing-docs`

error: missing documentation for a function
 --> lib.rs:1:1
  |
1 | pub fn foo() {}
  | ^^^^^^^^^^^^

error: aborting due to 2 previous errors
```

还可以多次传递每个标志，以更改多个 lints：

```bash
$ rustc lib.rs --crate-type=lib -D missing-docs -D unused-variables
```

当然，您可以将这四个标志混合在一起：

```bash
$ rustc lib.rs --crate-type=lib -D missing-docs -A unused-variables
```

### 通过属性

您还可以使用箱的宽度属性，修改 lints 级别：

```bash
$ cat lib.rs
#![warn(missing_docs)]

pub fn foo() {}
$ rustc lib.rs --crate-type=lib
warning: missing documentation for crate
 --> lib.rs:1:1
  |
1 | / #![warn(missing_docs)]
2 | |
3 | | pub fn foo() {}
  | |_______________^
  |
note: lint level defined here
 --> lib.rs:1:9
  |
1 | #![warn(missing_docs)]
  |         ^^^^^^^^^^^^

warning: missing documentation for a function
 --> lib.rs:3:1
  |
3 | pub fn foo() {}
  | ^^^^^^^^^^^^
```

全部四个，`warn`，`allow`，`deny`，和`forbid`都是这样工作的。

您还可以为每个属性，传递多个 lints：

```rust
#![warn(missing_docs, unused_variables)]

pub fn foo() {}
```

同时使用多个属性：

```rust
#![warn(missing_docs)]
#![deny(unused_variables)]

pub fn foo() {}
```

### Capping lint

`rustc`支持一种标志，就是`--cap-lints LEVEL`，它设置“lint 天花板等级”。这是对所有 lint 的最大控制。例如，如果我们从上面的“拒绝”lint 级别，获取代码示例：

```rust,ignore
fn main() {
    100u8 << 10;
}
```

我们编译它，以 lint 警告等级作为天花板(封顶)：

```bash
$ rustc lib.rs --cap-lints warn
warning: bitshift exceeds the type's number of bits
 --> lib.rs:2:5
  |
2 |     100u8 << 10;
  |     ^^^^^^^^^^^
  |
  = note: #[warn(exceeding_bitshifts)] on by default

warning: this expression will panic at run-time
 --> lib.rs:2:5
  |
2 |     100u8 << 10;
  |     ^^^^^^^^^^^ attempt to shift left with overflow
```

它现在只发出警告，而不是错误。我们可以走得更远，允许所有的 lints：

> 以‘允许’级别作为天花板，但允许级别默认不输出信息，整洁界面。

```bash
$ rustc lib.rs --cap-lints allow
$
```

这个功能被 Cargo 大量使用；在编译依赖项时，传递`--cap-lints allow`，这样如果它们有任何警告，它们就不会污染构建的输出。
