# Lint 组

`rustc`具有“lint 组”的概念，您可以通过一个名称切换多个警告。

例如，`nonstandard-style`lint 一次设置 `non-camel-case-types`，`non-snake-case`，和`non-upper-case-globals`全部。所以下面命令行是等价的：

```bash
$ rustc -D nonstandard-style
$ rustc -D non-camel-case-types -D non-snake-case -D non-upper-case-globals
```

这是每个 lint 组的列表，以及它们由以下组成的 lint：

| 组                  | 描述                                 | lints                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| nonstandard-style   | 违反标准命名约定                     | non-camel-case-types, non-snake-case, non-upper-case-globals                                                                                                                                                                                                                                                                                                                                                                                                                           |
| warnings            | 所有会发出警告的 lints               | 所有会发出警告的 lints                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| edition-2018        | 在 2018 Rust 时变为错误的 lints      | tyvar-behind-raw-pointer                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| rust-2018-idioms    | 倾向 Rust 2018 的惯用功能的 lints    | bare-trait-object, unreachable-pub                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| unused              | 这些 lints 检测到声明但未使用的东西  | unused-imports, unused-variables, unused-assignments, dead-code, unused-mut, unreachable-code, unreachable-patterns, unused-must-use, unused-unsafe, path-statements, unused-attributes, unused-macros, unused-allocation, unused-doc-comment, unused-extern-crates, unused-features, unused-parens                                                                                                                                                                                    |
| future-incompatible | 检测具有功能兼容性问题的代码的 lints | private-in-public, pub-use-of-private-extern-crate, patterns-in-fns-without-body, safe-extern-statics, invalid-type-param-default, legacy-directory-ownership, legacy-imports, legacy-constructor-visibility, missing-fragment-specifier, illegal-floating-point-literal-pattern, anonymous-parameters, parenthesized-params-in-types-and-modules, late-bound-lifetime-arguments, safe-packed-borrows, incoherent-fundamental-impls, tyvar-behind-raw-pointer, unstable-name-collision |

此外，还有一个`bad-style`组 lint，它是不推荐使用的`nonstandard-style`别名。

最后，您还可以通过调用`rustc -W help`。 给出已安装特有版本编译器的确切值。
