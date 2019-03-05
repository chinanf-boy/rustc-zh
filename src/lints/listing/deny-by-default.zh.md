# 拒绝默认的lints

默认情况下，这些lint都设置为'deny'级别。

## 超过-bitshifts

此lint检测到移位超出了类型的位数。一些触发此lint的示例代码：

```rust,ignore
1_i32 << 32;
```

这将产生：

```text
error: bitshift exceeds the type's number of bits
 --> src/main.rs:2:5
  |
2 |     1_i32 << 32;
  |     ^^^^^^^^^^^
  |
```

## 无效型PARAM默认

此lint检测在无效位置中错误允许的类型参数默认值。一些触发此lint的示例代码：

```rust,ignore
fn foo<T=i32>(t: T) {}
```

这将产生：

```text
error: defaults for type parameters are only allowed in `struct`, `enum`, `type`, or `trait` definitions.
 --> src/main.rs:4:8
  |
4 | fn foo<T=i32>(t: T) {}
  |        ^
  |
  = note: #[deny(invalid_type_param_default)] on by default
  = warning: this was previously accepted by the compiler but is being phased out; it will become a hard error in a future release!
  = note: for more information, see issue #36887 <https://github.com/rust-lang/rust/issues/36887>
```

## 传统的构造函数能见度

[RFC 1506](https://github.com/rust-lang/rfcs/blob/master/text/1506-adt-kinds.md)修改了一些可见性规则，并改变了struct构造函数的可见性。一些触发此lint的示例代码：

```rust,ignore
mod m {
    pub struct S(u8);
    
    fn f() {
        // this is trying to use S from the 'use' line, but because the `u8` is
        // not pub, it is private
        ::S;
    }
}

use m::S;
```

这将产生：

```text
error: private struct constructors are not usable through re-exports in outer modules
 --> src/main.rs:5:9
  |
5 |         ::S;
  |         ^^^
  |
  = note: #[deny(legacy_constructor_visibility)] on by default
  = warning: this was previously accepted by the compiler but is being phased out; it will become a hard error in a future release!
  = note: for more information, see issue #39207 <https://github.com/rust-lang/rust/issues/39207>
```

## 遗产目录所有权

发出legacy_directory_ownership警告时发出

-   有一个带有＃的非内联模块[路径]属性（例如＃[path =“foo"rs”.]mod bar;），
-   模块的文件（上例中的“foo.rs”）未命名为“mod.rs”，并且
-   模块的文件包含一个没有＃的非内联子模块[路径]属性。

可以通过将父模块重命名为“mod.rs”并将其移动到其自己的目录（如果适用）来修复警告。

## 丢失片段的符

当一个未使用的模式出现时，会发出missing_fragment_specifier警告`macro_rules!`宏定义有一个元变量（例如`$e`）后面没有片段说明符（例如`:expr`）。

通过删除中未使用的模式，可以始终修复此警告`macro_rules!`宏定义。

## 可变-蜕变

这种棉绒可以从中转化`&T`至`&mut T`因为它是未定义的行为。一些触发此lint的示例代码：

```rust,ignore
unsafe {
    let y = std::mem::transmute::<&i32, &mut i32>(&5);
}
```

这将产生：

```text
error: mutating transmuted &mut T from &T may cause undefined behavior, consider instead using an UnsafeCell
 --> src/main.rs:3:17
  |
3 |         let y = std::mem::transmute::<&i32, &mut i32>(&5);
  |                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
```

## 无撕裂const的项

这个lint检测到任何`const`用的物品`#[no_mangle]`属性。常量没有导出符号，因此，这可能意味着您打算使用`static`不是`const`。一些触发此lint的示例代码：

```rust,ignore
#[no_mangle]
const FOO: i32 = 5;
```

这将产生：

```text
error: const items should never be #[no_mangle]
 --> src/main.rs:3:1
  |
3 | const FOO: i32 = 5;
  | -----^^^^^^^^^^^^^^
  | |
  | help: try a static value: `pub static`
  |
```

## 四溢，文字

此lint检测其类型的字面值超出范围。一些触发此lint的示例代码：

```rust,compile_fail
let x: u8 = 1000;
```

这将产生：

```text
error: literal out of range for u8
 --> src/main.rs:2:17
  |
2 |     let x: u8 = 1000;
  |                 ^^^^
  |
```

## 括号的PARAMS-在类型和模块

此lint检测到不正确的括号。一些触发此lint的示例代码：

```rust,ignore
let x = 5 as usize();
```

这将产生：

```text
error: parenthesized parameters may only be used with a trait
 --> src/main.rs:2:21
  |
2 |   let x = 5 as usize();
  |                     ^^
  |
  = note: #[deny(parenthesized_params_in_types_and_modules)] on by default
  = warning: this was previously accepted by the compiler but is being phased out; it will become a hard error in a future release!
  = note: for more information, see issue #42238 <https://github.com/rust-lang/rust/issues/42238>
```

要修复它，请删除`()`秒。

## 酒馆的用的，私人的extern篓

此lint检测重新导出私有的特定情况`extern crate`;

## 安全-的extern-静

在旧版本的Rust中，存在健全问题`extern static`允许以安全代码访问s。这个棉绒现在抓住并否认这种代码。

## 未知箱类型

此lint检测到a中发现的未知包状态类型`#[crate_type]`指示。一些触发此lint的示例代码：

```rust,ignore
#![crate_type="lol"]
```

这将产生：

```text
error: invalid `crate_type` value
 --> src/lib.rs:1:1
  |
1 | #![crate_type="lol"]
  | ^^^^^^^^^^^^^^^^^^^^
  |
```

## 语无伦次，根本-impls

此lint检测到错误允许的潜在冲突的impl。一些触发此lint的示例代码：

```rust,ignore
pub trait Trait1<X> {
    type Output;
}

pub trait Trait2<X> {}

pub struct A;

impl<X, T> Trait1<X> for T where T: Trait2<X> {
    type Output = ();
}

impl<X> Trait1<Box<X>> for A {
    type Output = i32;
}
```

这将产生：

```text
error: conflicting implementations of trait `Trait1<std::boxed::Box<_>>` for type `A`: (E0119)
  --> src/main.rs:13:1
   |
9  | impl<X, T> Trait1<X> for T where T: Trait2<X> {
   | --------------------------------------------- first implementation here
...
13 | impl<X> Trait1<Box<X>> for A {
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ conflicting implementation for `A`
   |
   = note: #[deny(incoherent_fundamental_impls)] on by default
   = warning: this was previously accepted by the compiler but is being phased out; it will become a hard error in a future release!
   = note: for more information, see issue #46205 <https://github.com/rust-lang/rust/issues/46205>
   = note: downstream crates may implement trait `Trait2<std::boxed::Box<_>>` for type `A`
```
