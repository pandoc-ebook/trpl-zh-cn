
# 附录

> [appendix-00.md](https://github.com/rust-lang/book/blob/master/src/appendix-00.md)
> <br>
> commit 1fedfc4b96c2017f64ecfcf41a0a07e2e815f24f

附录部分包含一些在你的 Rust 之旅中可能用到的参考资料。


## 附录 A： 关键字

> [appendix-01-keywords.md](https://raw.githubusercontent.com/rust-lang/book/master/src/appendix-01-keywords.md)
> <br>
> commit 0e9061cbaf95adfb9f3ed36c6cef4c046f282e86 

下面的列表包含 Rust 中正在使用或者以后会用到的关键字。因此，这些关键字不能被用作标识符（除了 [原始标识符][raw-identifiers]），这包括函数、变量、参数、结构体字段、模块、crate、常量、宏、静态值、属性、类型、trait 或生命周期
的名字。

### 目前正在使用的关键字

如下关键字目前有对应其描述的功能。

* `as` - 强制类型转换，消除特定包含项的 trait 的歧义，或者对 `use` 和 `extern crate` 语句中的项重命名
* `break` - 立刻退出循环
* `const` - 定义常量或不变裸指针（constant raw pointer）
* `continue` - 继续进入下一次循环迭代
* `crate` - 链接（link）一个外部 **crate** 或一个代表宏定义的 **crate** 的宏变量
* `dyn` - 动态分发 trait 对象
* `else` - 作为 `if` 和 `if let` 控制流结构的 fallback 
* `enum` - 定义一个枚举
* `extern` - 链接一个外部 **crate** 、函数或变量
* `false` - 布尔字面值 `false`
* `fn` - 定义一个函数或 **函数指针类型** (*function pointer type*)
* `for` - 遍历一个迭代器或实现一个 trait 或者指定一个更高级的生命周期
* `if` - 基于条件表达式的结果分支
* `impl` - 实现自有或 trait 功能
* `in` - `for` 循环语法的一部分
* `let` - 绑定一个变量
* `loop` - 无条件循环
* `match` - 模式匹配
* `mod` - 定义一个模块
* `move` - 使闭包获取其所捕获项的所有权
* `mut` - 表示引用、裸指针或模式绑定的可变性性
* `pub` - 表示结构体字段、`impl` 块或模块的公有可见性
* `ref` - 通过引用绑定
* `return` - 从函数中返回
* `Self` - 实现 trait 的类型的类型别名
* `self` - 表示方法本身或当前模块
* `static` - 表示全局变量或在整个程序执行期间保持其生命周期
* `struct` - 定义一个结构体
* `super` - 表示当前模块的父模块
* `trait` - 定义一个 trait
* `true` - 布尔字面值 `true`
* `type` - 定义一个类型别名或关联类型
* `unsafe` - 表示不安全的代码、函数、trait 或实现
* `use` - 引入外部空间的符号
* `where` - 表示一个约束类型的从句
* `while` - 基于一个表达式的结果判断是否进行循环

### 保留做将来使用的关键字

如下关键字没有任何功能，不过由 Rust 保留以备将来的应用。

* `abstract`
* `async`
* `become`
* `box`
* `do`
* `final`
* `macro`
* `override`
* `priv`
* `try`
* `typeof`
* `unsized`
* `virtual`
* `yield`

### 原始标识符
[raw-identifiers]: #raw-identifiers

原始标识符（Raw identifiers）允许你使用通常不能使用的关键字，其带有 `r#` 前缀。

例如，`match` 是关键字。如果尝试编译这个函数：

```rust,ignore
fn match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}
```

会得到这个错误：

```text
error: expected identifier, found keyword `match`
 --> src/main.rs:4:4
  |
4 | fn match(needle: &str, haystack: &str) -> bool {
  |    ^^^^^ expected identifier, found keyword
```

该错误表示你不能将关键字`match`用作函数标识符。 你可以使用原始标识符将`match`作为函数名称使用：

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```

此代码编译没有任何错误。注意 `r#` 前缀需同时用于函数名和调用。

原始标识符允许使用你选择的任何单词作为标识符，即使该单词恰好是保留关键字。 此外，原始标识符允许你使用以不同于你的crate使用的Rust版本编写的库。比如，`try` 在 2015 edition 中不是关键字，而在 2018 edition 则是。所以如果如果用 2015 edition 编写的库中带有 `try` 函数，在 2018 edition 中调用时就需要使用原始标识符。有关版本的更多信息，请参见[附录E](https://github.com/KaiserY/trpl-zh-cn/blob/master/src/appendix-05-editions.md).



## 附录 B：运算符与符号

> [appendix-02-operators.md](https://github.com/rust-lang/book/blob/master/src/appendix-02-operators.md)
> <br />
> commit 1fedfc4b96c2017f64ecfcf41a0a07e2e815f24f

该附录包含了 Rust 语法的词汇表，包括运算符以及其他的符号，这些符号单独出现或出现在路径、泛型、trait bounds、宏、属性、注释、元组以及大括号上下文中。

### 运算符

表 B-1 包含了 Rust 中的运算符、运算符如何出现在上下文中的示例、简短解释以及该运算符是否可重载。如果一个运算符是可重载的，则该运算符上用于重载的相关 trait 也会列出。

<span class="caption">表 B-1: 运算符</span>

| 运算符 | 示例 | 解释 | 是否可重载 |
|----------|---------|-------------|---------------|
| `!` | `ident!(...)`, `ident!{...}`, `ident![...]` | 宏展开 |  |
| `!` | `!expr` | 按位非或逻辑非 | `Not` |
| `!=` | `var != expr` | 不等比较 | `PartialEq` |
| `%` | `expr % expr` | 算术取模 | `Rem` |
| `%=` | `var %= expr` | 算术取模与赋值 | `RemAssign` |
| `&` | `&expr`, `&mut expr` | 借用 | |
| `&` | `&type`, `&mut type`, `&'a type`, `&'a mut type` | 借用指针类型 |  |
| `&` | `expr & expr` | 按位与 | `BitAnd` |
| `&=` | `var &= expr` | 按位与与及赋值 | `BitAndAssign` |
| `&&` | `expr && expr` | 逻辑与 |  |
| `*` | `expr * expr` | 算术乘法 | `Mul` |
| `*=` | `var *= expr` | 算术乘法与赋值 | `MulAssign` |
| `*` | `*expr` | 解引用 | |
| `*` | `*const type`, `*mut type` | 裸指针 | |
| `+` | `trait + trait`, `'a + trait` | 复合类型限制 | |
| `+` | `expr + expr` | 算术加法 | `Add` |
| `+=` | `var += expr` | 算术加法与赋值 | `AddAssign` |
| `,` | `expr, expr` | 参数以及元素分隔符 | |
| `-` | `- expr` | 算术取负 | `Neg` |
| `-` | `expr - expr` | 算术减法| `Sub` |
| `-=` | `var -= expr` | 算术减法与赋值 | `SubAssign` |
| `->` | `fn(...) -> type`, <code>\|...\| -> type</code> | 函数与闭包，返回类型 | |
| `.` | `expr.ident` | 成员访问 | |
| `..` | `..`, `expr..`, `..expr`, `expr..expr` | 右排除范围 | |
| `..` | `..expr` | 结构体更新语法 | |
| `..` | `variant(x, ..)`, `struct_type { x, .. }` | “与剩余部分”的模式绑定 | |
| `...` | `expr...expr` | 模式: 范围包含模式 | |
| `/` | `expr / expr` | 算术除法 | `Div` |
| `/=` | `var /= expr` | 算术除法与赋值 | `DivAssign` |
| `:` | `pat: type`, `ident: type` | 约束 | |
| `:` | `ident: expr` | 结构体字段初始化 | |
| `:` | `'a: loop {...}` | 循环标志 | |
| `;` | `expr;` | 语句和语句结束符 | |
| `;` | `[...; len]` | 固定大小数组语法的部分 | |
| `<<` | `expr << expr` |左移 | `Shl` |
| `<<=` | `var <<= expr` | 左移与赋值| `ShlAssign` |
| `<` | `expr < expr` | 小于比较 | `PartialOrd` |
| `<=` | `expr <= expr` | 小于等于比较 | `PartialOrd` |
| `=` | `var = expr`, `ident = type` | 赋值/等值 | |
| `==` | `expr == expr` | 等于比较 | `PartialEq` |
| `=>` | `pat => expr` | 匹配准备语法的部分 | |
| `>` | `expr > expr` | 大于比较 | `PartialOrd` |
| `>=` | `expr >= expr` | 大于等于比较 | `PartialOrd` |
| `>>` | `expr >> expr` | 右移 | `Shr` |
| `>>=` | `var >>= expr` | 右移与赋值 | `ShrAssign` |
| `@` | `ident @ pat` | 模式绑定 | |
| `^` | `expr ^ expr` | 按位异或 | `BitXor` |
| `^=` | `var ^= expr` | 按位异或与赋值 | `BitXorAssign` |
| <code>\|</code> | <code>pat \| pat</code> | 模式选择 | |
| <code>\|</code> | <code>expr \| expr</code> | 按位或 | `BitOr` |
| <code>\|=</code> | <code>var \|= expr</code> | 按位或与赋值 | `BitOrAssign` |
| <code>\|\|</code> | <code>expr \|\| expr</code> | 逻辑或 | |
| `?` | `expr?` | 错误传播 | |

### 非运算符符号

下面的列表中包含了所有和运算符不一样功能的非字符符号；也就是说，他们并不像函数调用或方法调用一样表现。

表 B-2 展示了以其自身出现以及出现在合法其他各个地方的符号。

<span class="caption">表 B-2：独立语法</span>

| 符号 | 解释 |
|--------|-------------|
| `'ident` | 命名生命周期或循环标签 |
| `...u8`, `...i32`, `...f64`, `...usize`, 等 | 指定类型的数值常量 |
| `"..."` | 字符串常量 |
| `r"..."`, `r#"..."#`, `r##"..."##`, etc. | 原始字符串字面值, 未处理的转义字符 |
| `b"..."` | 字节字符串字面值; 构造一个 `[u8]` 类型而非字符串 |
| `br"..."`, `br#"..."#`, `br##"..."##`, 等 | 原始字节字符串字面值，原始和字节字符串字面值的结合 |
| `'...'` | 字符字面值 |
| `b'...'` | ASCII 码字节字面值 |
| <code>\|...\| expr</code> | 闭包 |
| `!` | 离散函数的总是为空的类型 |
| `_` | “忽略” 模式绑定；也用于增强整型字面值的可读性 |

表 B-3 展示了出现在从模块结构到项的路径上下文中的符号

<span class="caption">表 B-3：路径相关语法</span>

| 符号 | 解释 |
|--------|-------------|
| `ident::ident` | 命名空间路径 |
| `::path` | 与 crate 根相对的路径（如一个显式绝对路径） |
| `self::path` | 与当前模块相对的路径（如一个显式相对路径）|
| `super::path` | 与父模块相对的路径 |
| `type::ident`, `<type as trait>::ident` | 关联常量、函数以及类型 |
| `<type>::...` | 不可以被直接命名的关联项类型（如 `<&T>::...`，`<[T]>::...`， 等） |
| `trait::method(...)` | 通过命名定义的 trait 来消除方法调用的二义性 |
| `type::method(...)` | 通过命名定义的类型来消除方法调用的二义性 |
| `<type as trait>::method(...)` | 通过命名 trait 和类型来消除方法调用的二义性 |


表 B-4 展示了出现在泛型类型参数上下文中的符号。

<span class="caption">表 B-4：泛型</span>

| 符号 | 解释 |
|--------|-------------|
| `path<...>` | 为一个类型中的泛型指定具体参数（如 `Vec<u8>`） |
| `path::<...>`, `method::<...>` | 为一个泛型、函数或表达式中的方法指定具体参数，通常指 turbofish（如 `"42".parse::<i32>()`）|
| `fn ident<...> ...` | 泛型函数定义 |
| `struct ident<...> ...` | 泛型结构体定义 |
| `enum ident<...> ...` | 泛型枚举定义 |
| `impl<...> ...` | 定义泛型实现 |
| `for<...> type` | 高级生命周期限制 |
| `type<ident=type>` | 泛型，其一个或多个相关类型必须被指定为特定类型（如 `Iterator<Item=T>`）|

表 B-5 展示了出现在使用 trait bounds 约束泛型参数上下文中的符号。

<span class="caption">表 B-5: Trait Bound 约束</span>

| 符号 | 解释 |
|--------|-------------|
| `T: U` | 泛型参数 `T` 约束于实现了 `U` 的类型 |
| `T: 'a` | 泛型 `T` 的生命周期必须长于 `'a`（意味着该类型不能传递包含生命周期短于 `'a` 的任何引用）|
| `T : 'static` | 泛型 `T` 包含了除 `'static` 之外的非借用引用 |
| `'b: 'a` | 泛型 `'b` 生命周期必须长于泛型 `'a` |
| `T: ?Sized` | 使用一个不定大小的泛型类型 |
| `'a + trait`, `trait + trait` | 复合类型限制 |

表 B-6 展示了在调用或定义宏以及在其上指定属性时的上下文中出现的符号。

<span class="caption">表 B-6: 宏与属性</span>

| 符号 | 解释 |
|--------|-------------|
| `#[meta]` | 外部属性 |
| `#![meta]` | 内部属性 |
| `$ident` | 宏替换 |
| `$ident:kind` | 宏捕获 |
| `$(…)…` | 宏重复 |

表 B-7 展示了写注释的符号。

<span class="caption">表 B-7: 注释</span>

| 符号 | 注释 |
|--------|-------------|
| `//` | 行注释 |
| `//!` | 内部行文档注释 |
| `///` | 外部行文档注释 |
| `/*...*/` | 块注释 |
| `/*!...*/` | 内部块文档注释 |
| `/**...*/` | 外部块文档注释 |

表 B-8 展示了出现在使用元组时上下文中的符号。

<span class="caption">表 B-8: 元组</span>

| 符号 | 解释 |
|--------|-------------|
| `()` | 空元组（亦称单元），即是字面值也是类型 |
| `(expr)` | 括号表达式 |
| `(expr,)` | 单一元素元组表达式 |
| `(type,)` | 单一元素元组类型 |
| `(expr, ...)` | 元组表达式 |
| `(type, ...)` | 元组类型 |
| `expr(expr, ...)` | 函数调用表达式；也用于初始化元组结构体 `struct` 以及元组枚举 `enum` 变体 |
| `ident!(...)`, `ident!{...}`, `ident![...]` | 宏调用 |
| `expr.0`, `expr.1`, etc. | 元组索引 |

表 B-9 展示了使用大括号的上下文。

<span class="caption">表 B-9: 大括号</span>

| 符号 | 解释 |
|---------|-------------|
| `{...}` | 块表达式 |
| `Type {...}` | `struct` 字面值  |

表 B-10 展示了使用方括号的上下文。

<span class="caption">表 B-10: 方括号</span>

| 符号 | 解释 |
|---------|-------------|
| `[...]` | 数组 |
| `[expr; len]` | 复制了 `len`个 `expr`的数组 |
| `[type; len]` | 包含 `len`个 `type` 类型的数组|
| `expr[expr]` | 集合索引。 重载（`Index`, `IndexMut`） |
| `expr[..]`, `expr[a..]`, `expr[..b]`, `expr[a..b]` | 集合索引，使用 `Range`，`RangeFrom`，`RangeTo` 或 `RangeFull` 作为索引来代替集合 slice |



## 附录 C：可派生的 trait

> [appendix-03-derivable-traits.md](https://github.com/rust-lang/book/blob/master/src/appendix-03-derivable-traits.md)
> <br />
> commit a86c1d315789b3ca13b20d50ad5005c62bdd9e37

在本书的各个部分中，我们讨论了可应用于结构体和枚举定义的 `derive` 属性。`derive` 属性会在使用 `derive` 语法标记的类型上生成对应 trait 的默认实现的代码。

在本附录中提供了标准库中所有所有可以使用 `derive` 的 trait 的参考。这些部分涉及到：

* 该 trait 将会派生什么样的操作符和方法
* 由 `derive` 提供什么样的 trait 实现
* 由什么来实现类型的 trait
* 是否允许实现该 trait 的条件
* 需要 trait 操作的例子

如果你希望不同于 `derive` 属性所提供的行为，请查阅 [标准库文档](https://doc.rust-lang.org/std/index.html) 中每个 trait 的细节以了解如何手动实现它们。

标准库中定义的其它 trait 不能通过 `derive` 在类型上实现。这些 trait 不存在有意义的默认行为，所以由你负责以合理的方式实现它们。

一个无法被派生的 trait 的例子是为终端用户处理格式化的 `Display` 。你应该时常考虑使用合适的方法来为终端用户显示一个类型。终端用户应该看到类型的什么部分？他们会找出相关部分吗？对他们来说最相关的数据格式是什么样的？Rust 编译器没有这样的洞察力，因此无法为你提供合适的默认行为。

本附录所提供的可派生 trait 列表并不全面：库可以为其自己的 trait 实现 `derive`，可以使用 `derive` 的 trait 列表事实上是无限的。实现 `derive` 涉及到过程宏的应用，这在第十九章的 “宏” 有介绍。

### 用于程序员输出的 `Debug`

`Debug` trait 用于开启格式化字符串中的调试格式，其通过在 `{}` 占位符中增加 `:?` 表明。

`Debug` trait 允许以调试目的来打印一个类型的实例，所以使用该类型的程序员可以在程序执行的特定时间点观察其实例。

例如，在使用 `assert_eq!` 宏时，`Debug` trait 是必须的。如果等式断言失败，这个宏就把给定实例的值作为参数打印出来，如此程序员可以看到两个实例为什么不相等。

## 等值比较的 `PartialEq` 和 `Eq`

`PartialEq` trait 可以比较一个类型的实例以检查是否相等，并开启了 `==` 和 `!=` 运算符的功能。

派生的 `PartialEq` 实现了 `eq` 方法。当 `PartialEq` 在结构体上派生时，只有*所有* 的字段都相等时两个实例才相等，同时只要有任何字段不相等则两个实例就不相等。当在枚举上派生时，每一个成员都和其自身相等，且和其他成员都不相等。

例如，当使用 `assert_eq!` 宏时，需要比较比较一个类型的两个实例是否相等，则 `PartialEq` trait 是必须的。

`Eq` trait 没有方法。其作用是表明每一个被标记类型的值等于其自身。`Eq` trait 只能应用于那些实现了 `PartialEq` 的类型，但并非所有实现了 `PartialEq` 的类型都可以实现 `Eq`。浮点类型就是一个例子：浮点数的实现表明两个非数字（`NaN`，not-a-number）值是互不相等的。

例如，对于一个 `HashMap<K, V>` 中的 key 来说， `Eq` 是必须的，这样 `HashMap<K, V>` 就可以知道两个 key 是否一样了。

## 次序比较的 `PartialOrd` 和 `Ord`

`PartialOrd` trait 可以基于排序的目的而比较一个类型的实例。实现了 `PartialOrd` 的类型可以使用 `<`、 `>`、`<=` 和 `>=` 操作符。但只能在同时实现了 `PartialEq` 的类型上使用 `PartialOrd`。 

派生 `PartialOrd` 实现了 `partial_cmp` 方法，其返回一个 `Option<Ordering>` ，但当给定值无法产生顺序时将返回 `None`。尽管大多数类型的值都可以比较，但一个无法产生顺序的例子是：浮点类型的非数字值。当在浮点数上调用 `partial_cmp` 时，`NaN` 的浮点数将返回 `None`。

当在结构体上派生时，`PartialOrd` 以在结构体定义中字段出现的顺序比较每个字段的值来比较两个实例。当在枚举上派生时，认为在枚举定义中声明较早的枚举变体小于其后的变体。

例如，对于来自于 `rand` carte 中的 `gen_range` 方法来说，当在一个大值和小值指定的范围内生成一个随机值时，`PartialOrd` trait 是必须的。

`Ord` trait 也让你明白在一个带注解类型上的任意两个值存在有效顺序。`Ord` trait 实现了 `cmp` 方法，它返回一个 `Ordering` 而不是 `Option<Ordering>`，因为总存在一个合法的顺序。只可以在实现了 `PartialOrd` 和 `Eq`（`Eq` 依赖 `PartialEq`）的类型上使用 `Ord` trait 。当在结构体或枚举上派生时， `cmp` 和以 `PartialOrd` 派生实现的 `partial_cmp` 表现一致。

例如，当在 `BTreeSet<T>`（一种基于有序值存储数据的数据结构）上存值时，`Ord` 是必须的。

## 复制值的 `Clone` 和 `Copy`

`Clone` trait 可以明确地创建一个值的深拷贝（deep copy），复制过程可能包含任意代码的执行以及堆上数据的复制。查阅第四章 “变量和数据的交互方式：移动” 以获取有关 `Clone` 的更多信息。

派生 `Clone` 实现了 `clone` 方法，其为整个的类型实现时，在类型的每一部分上调用了 `clone` 方法。这意味着类型中所有字段或值也必须实现 `Clone` 来派生 `Clone` 。

例如，当在一个切片上调用 `to_vec` 方法时，`Clone` 是必须的。切片并不拥有其所包含实例的类型，但是从 `to_vec` 中返回的 vector 需要拥有其实例，因此，`to_vec` 在每个元素上调用 `clone`。因此，存储在切片中的类型必须实现 `Clone`。

`Copy` trait 允许你通过只拷贝存储在栈上的位来复制值而不需要其他代码。查阅第四章“只在栈上的数据：拷贝”的部分来获取有关 `Copy` 的更多信息。

`Copy` trait 并未定义任何方法来阻止编程人员重写这些方法或违反无代码可执行的假设。所以，所有的编程人员可以假设复制一个值非常快。

可以在类型内部全部实现 `Copy` trait 的任意类型上派生 `Copy`。 但只可以在那些同时实现了 `Clone` 的类型上使用 `Copy` trait ，因为一个实现 `Copy` 的类型在尝试实现 `Clone` 时执行和 `Copy` 相同的任务。

`Copy` trait 很少使用；实现 `Copy` 的类型是可以优化的，这意味着你无需调用 `clone`，这让代码更简洁。

使用 `Clone` 实现 `Copy` 也是有可能的，但代码可能会稍慢或是要使用 `clone` 替代。

## 固定大小的值到值映射的 `Hash`

`Hash` trait 可以实例化一个任意大小的类型，并且能够用哈希（hash）函数将该实例映射到一个固定大小的值上。派生 `Hash` 实现了 `hash` 方法。`hash` 方法的派生实现结合了在类型的每部分调用 `hash` 的结果，这意味着所有的字段或值也必须将 `Hash` 实现为派生 `Hash`。 

例如，在 `HashMap<K, V>` 的 key 上存储有效数据时，`Hash` 是必须的。

## 默认值的 `Default`

`Default` trait 使你创建一个类型的默认值。 派生 `Default` 实现了 `default` 函数。`default` 函数的派生实现调用了类型每部分的 `default` 函数，这意味着类型中所有的字段或值也必须把 `Default` 实现为派生 `Default` 。

`Default::default` 函数通常结合结构体更新语法一起使用，这在第五章的“使用结构体更新语法从其他实例中创建实例”部分有讨论。可以自定义一个结构体的一小部分字段而剩余字段则使用 `..Default::default()` 设置为默认值。

例如，当你在 `Option<T>` 实例上使用 `unwrap_or_default` 方法时，`Default` trait是必须的。如果 `Option<T>` 是 `None`的话, `unwrap_or_default` 方法将返回存储在 `Option<T>` 中 `T` 类型的 `Default::default` 的结果。



## 附录D - 宏

> [appendix-04-macros.md][appendix-04]
> <br>
> commit 32215c1d96c9046c0b553a05fa5ec3ede2e125c3

[appendix-04]: https://github.com/rust-lang/book/blob/master/second-edition/src/appendix-04-macros.md

我们已经在本书中像 *println!* 这样使用过宏了，但还没完全探索什么是宏以及它是如何工作的。本附录以如下方式解释宏：

* 什么是宏以及与函数有何区别
* 如何定义一个声明式宏（ declarative macro ）来进行元编程（metaprogramming）
* 如何定义一个过程式宏（ procedural macro ）来自定义 `derive` traits

因为在 Rust 里宏仍在开发中，该附录会介绍宏的细节。宏已经改变了，在不久的将来，和语言的其他部分以及从 Rust 1.0 至今的标准库相比，将会以更快的速率改变，因此，和本书其他部分相比，本部分更有可能变得过时。由于 Rust 的稳定性保证，该处的代码将会在后续版本中继续可运行，但可能会以更优的性能或更容易的方式来写那些在此次发布版中无法实现的宏。当你尝试实现该附录任何功能时，请记住这一点。

### 宏和函数的区别

从根本上来说，宏是一种为写其他代码而写代码的方式，即所谓的*元编程*。在附录C中，探讨了 `derive` 属性，其生成各种 trait 的实现。我们也在本书中使用过 `println!` 宏和 `vec!` 宏。所有的这些宏 *扩展开* 来以生成比你手写更多的代码。

元编程对于减少大量编写和维护的代码是非常有用的，它也扮演了函数的角色。但宏有一些函数所没有的附加能力。

一个函数标签必须声明函数参数个数和类型。而宏只接受一个可变参数：用一个参数调用 `println!("hello")` 或用两个参数调用 `println!("hello {}", name)` 。而且，宏可以在编译器翻译代码前展开，例如，宏可以在一个给定类型上实现 trait 。因为函数是在运行时被调用，同时 trait 需要在运行时实现，所以函数无法像宏这样。

实现一个宏而不是函数的消极面是宏定义要比函数定义更复杂，因为你正在为写 Rust 代码而写代码。由于这样的间接性，宏定义通常要比函数定义更难阅读、理解以及维护。

宏和函数的另一个区别是：宏定义无法像函数定义那样出现在模块命名空间中。当使用外部包（ external crate ）时，为了防止无法预料的名字冲突，在导入外部包的同时也必须明确地用 `#[macro_use]` 注解将宏导入到项目中。下面的例子将所有定义在 `serde` 包中的宏导入到当前包中：

```rust,ignore
#[macro_use]
extern crate serde;
```

如果 `extern crate` 能够将宏默认导入而无需使用明确的注解，则会阻止你使用同时定义相同宏名的两个包。在练习中，这样的冲突并不经常遇到，但使用的包越多，越有可能遇到。

宏和函数最重要的区别是：在一个文件中，必须在调用宏`之前`定义或导入宏，然而却可以在任意地方定义或调用函数。

### 通用元编程的声明式宏 `macro_rules!`

在 Rust 中，最广泛使用的宏形式是 *declarative macros* 。通常也指 *macros by example* 、*`macro_rules!` macros* 或简单的 *macros* 。在其核心中，声明式宏使你可以写一些和 Rust 的 `match` 表达式类似的代码。正如在第六章讨论的那样，`match` 表达式是控制结构，其接收一个表达式，与表达式的结果进行模式匹配，然后根据模式匹配执行相关代码。宏也将一个值和包含相关代码的模式进行比较；此种情况下，该值是 Rust 源代码传递给宏的常量，而模式则与源代码结构进行比较，同时每个模式的相关代码成为传递给宏的代码<!-- 这部分翻译自己不太满意-->。所有的这些都在编译时发生。

可以使用 `macro_rules!` 来定义宏。让我们通过查看 `vec!` 宏定义来探索如何使用 `macro_rules!` 结构。第八章讲述了如何使用 `vec!` 宏来生成一个给定值的 vector。例如，下面的宏用三个整数创建一个 vector `：

```rust
let v: Vec<u32> = vec![1, 2, 3];
```

也可以使用 `vec!` 宏来构造两个整数的 vector 或五个字符串切片的 vector 。但却无法使用函数做相同的事情，因为我们无法预先知道参数值的数量和类型。

在示例 D-1 中来看一个 `vec!` 稍微简化的定义。 

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

<span class="caption">示例 D-1: `vec!` 宏定义的一个简化版本</span>

> 注意：标准库中实际定义的 `vec!` 包括预分配适当量的内存。这部分为代码优化，为了让示例简化，此处并没有包含在内。

无论何时导入定义了宏的包，`#[macro_export]` 注解说明宏应该是可用的。 如果没有 `#[macro_export]` 注解，即使凭借包使用 `#[macro_use]` 注解，该宏也不会导入进来，

接着使用 `macro_rules!` 进行了宏定义，且所定义的宏并*不带*感叹号。名字后跟大括号表示宏定义体，在该例中是 `vec` 。

`vec!` 宏的结构和 `match` 表达式的结构类似。此处有一个单边模式 `( $( $x:expr ),* )` ，后跟 `=>` 以及和模式相关的代码块。如果模式匹配，该相关代码块将被执行。假设这只是这个宏中的模式，且只有一个有效匹配，其他任何匹配都是错误的。更复杂的宏会有多个单边模式。<!-- 此处 arm, one arm 并未找到合适的翻译-->

宏定义中有效模式语法和在第十八章提及的模式语法是不同的，因为宏模式所匹配的是 Rust 代码结构而不是值。回过头来检查下示例 D-1 中模式片段什么意思。对于全部的宏模式语法，请查阅[参考]。

[参考]: https://github.com/rust-lang/book/blob/master/reference/macros.html

首先，一对括号包含了全部模式。接下来是后跟一对括号的美元符号（ `$` ），其通过替代代码捕获了符合括号内模式的值。`$()` 内则是 `$x:expr` ，其匹配 Rust 的任意表达式或给定 `$x` 名字的表达式。

`$()` 之后的逗号说明一个逗号分隔符可以有选择的出现代码之后，这段代码与在 `$()` 中所捕获的代码相匹配。紧随逗号之后的 `*` 说明该模式匹配零个或多个 `*` 之前的任何模式。

当以 `vec![1, 2, 3];` 调用宏时，`$x` 模式与三个表达式 `1`、`2` 和 `3` 进行了三次匹配。

现在让我们来看看这个出现在与此单边模式相关的代码块中的模式：在 `$()*` 部分中所生成的 `temp_vec.push()` 为在匹配到模式中的 `$()` 每一部分而生成。`$x` 由每个与之相匹配的表达式所替换。当以 `vec![1, 2, 3];` 调用该宏时，替换该宏调用所生成的代码会是下面这样：

```rust,ignore
let mut temp_vec = Vec::new();
temp_vec.push(1);
temp_vec.push(2);
temp_vec.push(3);
temp_vec
```

我们已经定义了一个宏，其可以接收任意数量和类型的参数，同时可以生成能够创建包含指定元素的 vector 的代码。


鉴于大多数 Rust 程序员*使用*宏而不是*写*宏，此处不再深入探讨 `macro_rules!` 。请查阅在线文档或其他资源，如 [“The Little Book of Rust Macros”][tlborm] 来更多地了解如何写宏。

[tlborm]: https://danielkeep.github.io/tlborm/book/index.html

### 自定义 `derive` 的过程式宏

第二种形式的宏叫做*过程式宏*（ *procedural macros* ），因为它们更像函数（一种过程类型）。过程式宏接收 Rust 代码作为输入，在这些代码上进行操作，然后产生另一些代码作为输出，而非像声明式宏那样匹配对应模式然后以另一部分代码替换当前代码。在书写部分附录时，只能定义过程式宏来使你在一个通过 `derive` 注解来指定 trait 名的类型上实现 trait 。

我们会创建一个 `hello_macro` 包，该包定义了一个关联到 `hello_macro` 函数并以 `HelloMacro` 为名的trait。并非让包的用户为其每一个类型实现`HelloMacro` trait，我们将会提供一个过程式宏以便用户可以使用 `#[derive(HelloMacro)]` 注解他们的类型来得到 `hello_macro` 函数的默认实现。该函数的默认实现会打印 `Hello, Macro! My name is TypeName!`，其中 `TypeName` 为定义了 trait 的类型名。换言之，我们会创建一个包，让使用该包的程序员能够写类似示例 D-2 中的代码。 

<span class="filename">文件名: src/main.rs</span>

```rust,ignore
extern crate hello_macro;
#[macro_use]
extern crate hello_macro_derive;

use hello_macro::HelloMacro;

#[derive(HelloMacro)]
struct Pancakes;

fn main() {
    Pancakes::hello_macro();
}
```

<span class="caption">示例 D-2: 包用户所写的能够使用过程式宏的代码</span>

运行该代码将会打印 `Hello, Macro! My name is Pancakes!` 第一步是像下面这样新建一个库：

```text
$ cargo new hello_macro --lib
```

接下来，会定义 `HelloMacro` trait 以及其关联函数：

<span class="filename">文件名: src/lib.rs</span>

```rust
pub trait HelloMacro {
    fn hello_macro();
}
```

现在有了一个包含函数的 trait 。此时，包用户可以实现该 trait 以达到其期望的功能，像这样：

```rust,ignore
extern crate hello_macro;

use hello_macro::HelloMacro;

struct Pancakes;

impl HelloMacro for Pancakes {
    fn hello_macro() {
        println!("Hello, Macro! My name is Pancakes!");
    }
}

fn main() {
    Pancakes::hello_macro();
}
```

然而，他们需要为每一个他们想使用 `hello_macro` 的类型编写实现的代码块。我们希望为其节约这些工作。

另外，我们也无法为 `hello_macro` 函数提供一个能够打印实现了该 trait 的类型的名字的默认实现：Rust 没有反射的能力，因此其无法在运行时获取类型名。我们需要一个在运行时生成代码的宏。

下一步是定义过程式宏。在编写该附录时，过程式宏必须在包内。该限制后面可能被取消。构造包和包中宏的惯例如下：对于一个 `foo` 的包来说，一个自定义的派生过程式宏的包被称为 `foo_derive` 。在 `hello_macro` 项目中新建名为 `hello_macro_derive` 的包。

```text
$ cargo new hello_macro_derive --lib
```

由于两个包紧密相关，因此在 `hello_macro` 包的目录下创建过程式宏的包。如果改变在 `hello_macro` 中定义的 trait ，同时也必须改变在 `hello_macro_derive` 中实现的过程式宏。这两个包需要分别发布，编程人员如果使用这些包，则需要同时添加这两个依赖并导入到代码中。我们也可以只用 `hello_macro` 包而将 `hello_macro_derive` 作为一个依赖，并重新导出过程式宏的代码。但我们组织项目的方式使编程人员使用 `hello_macro` 成为可能，即使他们无需 `derive` 的功能。

需要将 `hello_macro_derive` 声明为一个过程式宏的包。同时也需要 `syn` 和 `quote` 包中的功能，正如注释中所说，需要将其加到依赖中。为 `hello_macro_derive` 将下面的代码加入到 *Cargo.toml* 文件中。

<span class="filename">文件名: hello_macro_derive/Cargo.toml</span>

```toml
[lib]
proc-macro = true

[dependencies]
syn = "0.11.11"
quote = "0.3.15"
```
为定义一个过程式宏，请将示例 D-3 中的代码放在 `hello_macro_derive` 包的 *src/lib.rs* 文件里面。注意这段代码在我们添加 `impl_hello_macro` 函数的定义之前是无法编译的。

<span class="filename">文件名: hello_macro_derive/src/lib.rs</span>

```rust,ignore
extern crate proc_macro;
extern crate syn;
#[macro_use]
extern crate quote;

use proc_macro::TokenStream;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    // Construct a string representation of the type definition
    let s = input.to_string();

    // Parse the string representation
    let ast = syn::parse_derive_input(&s).unwrap();

    // Build the impl
    let gen = impl_hello_macro(&ast);

    // Return the generated impl
    gen.parse().unwrap()
}
```

<span class="caption">示例 D-3: 大多数过程式宏处理 Rust 代码的代码</span>

注意在 D-3 中分离函数的方式，这将和你几乎所见到或创建的每一个过程式宏都一样，因为这让编写一个过程式宏更加方便。在 `impl_hello_macro` 被调用的地方所选择做的什么依赖于该过程式宏的目的而有所不同。

现在，我们已经介绍了三个包：`proc_macro` 、 [`syn`] 和 [`quote`] 。Rust 自带 `proc_macro`  ，因此无需将其加到 *Cargo.toml* 文件的依赖中。`proc_macro` 可以将 Rust 代码转换为相应的字符串。`syn` 则将字符串中的 Rust 代码解析成为一个可以操作的数据结构。`quote` 则将 `syn` 解析的数据结构反过来传入到 Rust 代码中。这些包让解析我们所要处理的有序 Rust 代码变得更简单：为 Rust 编写整个的解析器并不是一件简单的工作。

[`syn`]: https://crates.io/crates/syn
[`quote`]: https://crates.io/crates/quote

当用户在一个类型上指定 `#[derive(HelloMacro)]` 时，`hello_macro_derive`  函数将会被调用。
原因在于我们已经使用 `proc_macro_derive` 及其指定名称对 `hello_macro_derive` 函数进行了注解：`HelloMacro` ，其匹配到 trait 名，这是大多数过程式宏的方便之处。

该函数首先将来自 `TokenStream` 的 `输入` 转换为一个名为 `to_string` 的 `String` 类型。该 `String` 代表 派生 `HelloMacro` Rust 代码的字符串。在示例 D-2 的例子中，`s` 是 `String` 类型的 `struct Pancakes;` 值，这是因为我们加上了 `#[derive(HelloMacro)]` 注解。

> 注意：编写本附录时，只可以将 `TokenStream` 转换为字符串，将来会提供更丰富的API。

现在需要将 `String` 类型的 Rust 代码 解析为一个数据结构中，随后便可以与之交互并操作该数据结构。这正是 `syn` 所做的。`syn` 中的 `parse_derive_input` 函数以一个 `String` 作为参数并返回一个 表示解析出 Rust 代码的 `DeriveInput` 结构体。 下面的代码 展示了从字符串 `struct Pancakes;` 中解析出来的 `DeriveInput` 结构体的相关部分。

```rust,ignore
DeriveInput {
    // --snip--

    ident: Ident(
        "Pancakes"
    ),
    body: Struct(
        Unit
    )
}
```

该结构体的字段展示了我们解析的 Rust 代码是一个元组结构体，其 `ident` （ identifier，表示名字）为 `Pancakes` 。该结构体里面有更多字段描述了所有有序 Rust 代码，查阅 [`syn`
documentation for `DeriveInput`][syn-docs] 以获取更多信息。

[syn-docs]: https://docs.rs/syn/0.11.11/syn/struct.DeriveInput.html

此时，尚未定义 `impl_hello_macro` 函数，其用于构建所要包含在内的 Rust 新代码。但在定义之前，要注意 `hello_macro_derive` 函数的最后一部分使用了 `quote` 包中的 `parse` 函数，该函数将 `impl_hello_macro` 的输出返回给 `TokenStream` 。所返回的 `TokenStream` 会被加到我们的包用户所写的代码中，因此，当用户编译他们的包时，他们会获取到我们所提供的额外功能。

你也注意到，当调用 `parse_derive_input` 或 `parse` 失败时，我们调用 `unwrap` 来抛出异常。在过程式宏中，有必要错误上抛异常，因为 `proc_macro_derive` 函数必须返回 `TokenStream` 而不是 `Result` ，以此来符合过程式宏的 API 。我们已经选择用 `unwrap` 来简化了这个例子；在生产中的代码里，你应该通过 `panic!` 或 `expect` 来提供关于发生何种错误的更加明确的错误信息。

现在我们有了将注解的 Rust 代码从 `TokenStream` 转换为 `String` 和 `DeriveInput` 实例的代码，让我们来创建在注解类型上实现 `HelloMacro` trait 的代码。

<span class="filename">文件名: hello_macro_derive/src/lib.rs</span>

```rust,ignore
fn impl_hello_macro(ast: &syn::DeriveInput) -> quote::Tokens {
    let name = &ast.ident;
    quote! {
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, Macro! My name is {}", stringify!(#name));
            }
        }
    }
}
```

我们得到一个包含以 `ast.ident` 作为注解类型名字（标识符）的 `Ident` 结构体实例。示例 D-2 中的代码说明 `name` 会是 `Ident("Pancakes")` 。

`quote!` 宏让我们编写我们想要返回的代码，并可以将其传入进 `quote::Tokens` 。这个宏也提供了一些非常酷的模板机制；我们可以写 `#name` ，然后 `quote!` 会以 名为 `name` 的变量值来替换它。你甚至可以做些与这个正则宏任务类似的重复事情。查阅 [the `quote` crate’s
docs][quote-docs] 来获取详尽的介绍。

[quote-docs]: https://docs.rs/quote

我们期望我们的过程式宏能够为通过 `#name` 获取到的用户注解类型生成 `HelloMacro` trait 的实现。该 trait 的实现有一个函数 `hello_macro` ，其函数体包括了我们期望提供的功能：打印 `Hello, Macro! My name is` 和注解的类型名。

此处所使用的 `stringify!` 为 Rust 内置宏。其接收一个 Rust 表达式，如 `1 + 2` ， 然后在编译时将表达式转换为一个字符串常量，如 `"1 + 2"` 。这与 `format!` 或 `println!` 是不同的，它计算表达式并将结果转换为 `String` 。有一种可能的情况是，所输入的 `#name` 可能是一个需要打印的表达式，因此我们用 `stringify!` 。 `stringify!` 编译时也保留了一份将 `#name` 转换为字符串之后的内存分配。

此时，`cargo build` 应该都能成功编译 `hello_macro` 和 `hello_macro_derive` 。我们将这些 crate 连接到示例 D-2 的代码中来看看过程式宏的行为。在 *projects* 目录下用 `cargo new pancakes` 命令新建一个二进制项目。需要将 `hello_macro` 和 `hello_macro_derive` 作为依赖加到 `pancakes` 包的 *Cargo.toml*  文件中去。如果你正将 `hello_macro` 和 `hello_macro_derive` 的版本发布到 [*https://crates.io/*][crates.io] 上，其应为正规依赖；如果不是，则可以像下面这样将其指定为 `path` 依赖：

[crates.io]: https://crates.io/

```toml
[dependencies]
hello_macro = { path = "../hello_macro" }
hello_macro_derive = { path = "../hello_macro/hello_macro_derive" }
```

把示例 D-2 中的代码放在 *src/main.rs* ，然后执行 `cargo run` ： 其应该打印 `Hello, Macro! My name is Pancakes!` 。从过程式宏中实现的 `HelloMacro` trait 被包括在内，但并不包含 `pancakes` 的包，需要实现它。`#[derive(HelloMacro)]` 添加了该 trait 的实现。<!-- 中间句子翻译不是太好 -->

### 宏的前景

在将来，Rust 仍会扩展声明式宏和过程式宏。Rust会通过 `macro` 使用一个更好的声明式宏系统，以及为较之 `derive` 的更强大的任务增加更多的过程式宏类型。在本书出版时，这些系统仍然在开发中，请查阅 Rust 在线文档以获取最新信息。



## 附录 D：实用开发工具

> [appendix-04-useful-development-tools.md](https://github.com/rust-lang/book/blob/master/src/appendix-04-useful-development-tools.md)
> <br />
> commit 1fedfc4b96c2017f64ecfcf41a0a07e2e815f24f

本附录，我们将讨论 Rust 项目提供的用于开发 Rust 代码的工具。

## 通过 `rustfmt` 自动格式化

`rustfmt` 工具根据社区代码风格格式化代码。很多项目使用 `rustfmt` 来避免编写 Rust 风格的争论：所有人都用这个工具格式化代码！

`rustfmt` 工具的质量还未达到发布 1.0 版本的水平，不过目前有一个可用的预览版。请尝试使用并告诉我们它如何！

安装 `rustfmt`：

```text
$ rustup component add rustfmt-preview
```

这会提供 `rustfmt` 和 `cargo-fmt`，类似于 Rust 同时安装 `rustc` 和 `cargo`。为了格式化整个 Cargo 项目：

```text
$ cargo fmt
```

运行此命令会格式化当前 crate 中所有的 Rust 代码。这应该只会改变代码风格，而不是代码语义。请查看 [该文档][rustfmt] 了解 `rustfmt` 的更多信息。

[rustfmt]: https://github.com/rust-lang-nursery/rustfmt

## 通过 `rustfix` 修复代码

如果你编写过 Rust 代码，那么你可能见过编译器警告。例如，考虑如下代码：

<span class="filename">文件名: src/main.rs</span>

```rust
fn do_something() {}

fn main() {
    for i in 0..100 {
        do_something();
    }
}
```

这里调用了 `do_something` 函数 100 次，不过从未在 `for` 循环体中使用变量 `i`。Rust 会警告说：

```text
$ cargo build
   Compiling myprogram v0.1.0 (file:///projects/myprogram)
warning: unused variable: `i`
 --> src/main.rs:4:9
  |
4 |     for i in 1..100 {
  |         ^ help: consider using `_i` instead
  |
  = note: #[warn(unused_variables)] on by default

    Finished dev [unoptimized + debuginfo] target(s) in 0.50s
```

警告中建议使用 `_i` 名称：下划线表明该变量有意不使用。我们可以通过 `cargo fix` 命令使用 `rustfix` 工具来自动采用该建议：

```text
$ cargo fix
    Checking myprogram v0.1.0 (file:///projects/myprogram)
      Fixing src/main.rs (1 fix)
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s
```

如果再次查看 *src/main.rs*，会发现 `cargo fix` 修改了代码：

<span class="filename">文件名: src/main.rs</span>

```rust
fn do_something() {}

fn main() {
    for _i in 0..100 {
        do_something();
    }
}
```

现在 `for` 循环变量变为 `_i`，警告也不再出现。

`cargo fix` 命令可以用于在不同 Rust 版本间迁移代码。版本在附录 E 中介绍。

## 通过 `clippy` 提供更多 lint 功能

`clippy` 工具是一系列 lint 的集合，用于捕捉常见错误和改进 Rust 代码。

`clippy` 工具的质量还未达到发布 1.0 版本的水平，不过目前有一个可用的预览版。请尝试使用并告诉我们它如何！

安装 `clippy`：

```text
$ rustup component add clippy-preview
```

对任何 Cargo 项目运行 clippy 的 lint：

```text
$ cargo clippy
```

例如，如果程序使用了如 pi 这样数学常数的近似值，如下：

<span class="filename">文件名: src/main.rs</span>

```rust
fn main() {
    let x = 3.1415;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

在此项目上运行 `cargo clippy` 会导致这个错误：

```text
error: approximate value of `f{32, 64}::consts::PI` found. Consider using it directly
 --> src/main.rs:2:13
  |
2 |     let x = 3.1415;
  |             ^^^^^^
  |
  = note: #[deny(clippy::approx_constant)] on by default
  = help: for further information visit https://rust-lang-nursery.github.io/rust-clippy/v0.0.212/index.html#approx_constant
```

这告诉我们 Rust 定义了更为精确的常量，而如果使用了这些常量程序将更加准确。如下代码就不会导致 `clippy` 产生任何错误或警告：

<span class="filename">文件名: src/main.rs</span>

```rust
fn main() {
    let x = std::f64::consts::PI;
    let r = 8.0;
    println!("the area of the circle is {}", x * r * r);
}
```

请查看 [其文档][clippy] 来了解 `clippy` 的更多信息。

[clippy]: https://github.com/rust-lang-nursery/rust-clippy

## 使用 Rust Language Server 的 IDE 集成

为了帮助 IDE 集成，Rust 项目分发了 `rls`，其为 Rust Language Server 的缩写。这个工具采用 [Language Server Protocol][lsp]，这是一个 IDE 与编程语言沟通的规格说明。`rls` 可以用于不同的客户端，比如 [Visual Studio: Code 的 Rust 插件][vscode]。

[lsp]: http://langserver.org/
[vscode]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust

`rls` 工具的质量还未达到发布 1.0 版本的水平，不过目前有一个可用的预览版。请尝试使用并告诉我们它如何！

安装 `rls`：

```text
$ rustup component add rls-preview
```

接着为特定的 IDE 安装 language server 支持，如比便会获得如自动补全、跳转到定义和 inline error 之类的功能。

请查看 [其文档][rls] 来了解 `rls` 的更多信息。

[rls]: https://github.com/rust-lang-nursery/rls


## 附录 E：版本

> [appendix-05-editions.md](https://github.com/rust-lang/book/blob/master/src/appendix-05-editions.md)
> <br />
> commit 1fedfc4b96c2017f64ecfcf41a0a07e2e815f24f

早在第一章，我们见过 `cargo new` 在 *Cargo.toml* 中增加了一些有关 `edition` 的元数据。本附录将解释其意义！

Rust 语言和编译器有一个为期 6 周的发布循环。这意味着用户会稳定得到新功能的更新。其他编程语言发布大更新但不甚频繁；Rust 选择更为频繁的发布小更新。一段时间之后，所有这些小更新会日积月累。不过随着版本的发布，很难回顾说 “哇，从 Rust 1.10 到 Rust 1.31，Rust 的变化真大！”

每两到三年，Rust 团队会生成一个新的 Rust **版本**（*edition*）。每一个版本会结合已经落地的功能，并提供一个清晰的带有完整更新文档和工具的功能包。新版本会作为常规的 6 周发布过程的一部分发布。

这为不同的人群提供了不同的功能：

* 对于活跃的 Rust 用户，其将增量的修改与易于理解的功能包相结合。
* 对于非用户，它表明发布了一些重大进展，这意味着 Rust 可能变得值得一试。
* 对于 Rust 自身开发者，其提供了项目整体的集合点。

在本文档编写时，Rust 有两个版本：Rust 2015 和 Rust 2018。本书基于 Rust 2018 edition 编写。

*Cargo.toml* 中的 `edition` 字段表明代码应该使用哪个版本编译。如果该字段不存在，其默认为 `2015` 以提供后向兼容性。

每个项目都可以选择不同于默认的 2015 edition 的版本。这样，版本可能会包含不兼容的修改，比如新增关键字可能会与代码中的标识符冲突并导致错误。不过除非选择兼容这些修改，（旧）代码仍将能够编译，即便升级了 Rust 编译器的版本。所有 Rust 编译器都支持任何之前存在的编译器版本，并可以链接人恶化支持版本的 crate。编译器修改只影响最初的解析代码的过程。因此，如果你使用 Rust 2015 而某个依赖使用 Rust 2018，你的项目仍旧能够编译并使用该依赖。反之，若项目使用 Rust 2018 而依赖使用 Rust 2015 亦可工作。

有一点需要明确：大部分功能在所有版本中都能使用。开发者使用任何 Rust 版本将能继续接收最新稳定版的改进。然而在一些情况，主要是增加了新关键字的时候，则可能出现了只能用于新版本的功能。只需切换版本即可利用新版本的功能。

请查看 [Edition Guide](https://rust-lang-nursery.github.io/edition-guide/) 了解更多细节，这是一个完全介绍版本的书籍，包括如何通过 `cargo fix` 自动将代码迁移到新版本。


## 附录E - 本书翻译

> [appendix-05-translation.md](https://github.com/rust-lang/book/blob/master/second-edition/src/appendix-05-translation.md)
> <br />
> commit 9c3756a33e6c1aa07a762ffa853fcdfcffb48ddc

一些非英语语言的资源。多数仍在翻译中；查阅[翻译标签][label]来帮助我们或使我们知道新的翻译！

[label]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations

- [Português](https://github.com/rust-br/rust-book-pt-br) (BR)
- [Português](https://github.com/nunojesus/rust-book-pt-pt) (PT)
- [Tiếng việt](https://github.com/hngnaig/rust-lang-book/tree/vi-VN)
- [简体中文](https://github.com/KaiserY/trpl-zh-cn)
- [Українська](https://github.com/pavloslav/rust-book-uk-ua)
- [Español](https://github.com/thecodix/book)
- [Italiano](https://github.com/AgeOfWar/rust-book-it)
- [Русский](https://github.com/iDeBugger/rust-book-ru)
- [한국어](https://github.com/rinthel/rust-lang-book-ko)
- [日本語](https://github.com/hazama-yuinyan/book)
- [Français](https://github.com/quadrifoglio/rust-book-fr)
- [Polski](https://github.com/paytchoo/book-pl)
- [עברית](https://github.com/idanmel/rust-book-heb)
- [Cebuano](https://github.com/agentzero1/book)
- [Tagalog](https://github.com/josephace135/book)



## F - 最新功能

> [appendix-06-newest-features.md](https://github.com/rust-lang/book/blob/master/second-edition/src/appendix-06-newest-features.md)
> <br />
> commit b64de01431cdf1020ad3358d2f83e46af68a39ed

自从本书的主要部分完成之后，该附录文档中的特性已经加到 Rust 的稳定版中。

### 字段初始化缩写

我们可以通过具名字段来初始化一个数据结构（结构体、枚举、联合体），如将 `fieldname: fieldname` 缩写为 `fieldname` 。这可以以更少的重复代码来完成一个复杂的初始化语法。

```rust
#[derive(Debug)]
struct Person {
    name: String,
    age: u8,
}

fn main() {
    let name = String::from("Peter");
    let age = 27;

    // Using full syntax:
    let peter = Person { name: name, age: age };

    let name = String::from("Portia");
    let age = 27;

    // Using field init shorthand:
    let portia = Person { name, age };

    println!("{:?}", portia);
}
```


## 从循环中返回（ loop ）

`loop` 的用法之一是重试一个可以操作，比如检查线程是否完成其任务。然而可能需要将该操作的结果传到其他部分代码。如果加上 `break` 表达式来停止循环，则会从循环中断中返回：

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    assert_eq!(result, 20);
}
```

## `use` 声明中的内置组

如果有一个包含许多不同子模块的复杂模块树，然后需要从每个子模块中引入几个特性，那么将所有导入模块放在同一声明中来保持代码清洁同时避免根模块名重复将会非常有用。

`use` 声明所支持的嵌套在这些情况下对你有帮助：简单的导入和全局导入。例如，下面的代码片段导入了 `bar` 、 `Foo` 、 `baz` 中所有项和 `Bar` ：

```rust
# #![allow(unused_imports, dead_code)]
#
# mod foo {
#     pub mod bar {
#         pub type Foo = ();
#     }
#     pub mod baz {
#         pub mod quux {
#             pub type Bar = ();
#         }
#     }
# }
#
use foo::{
    bar::{self, Foo},
    baz::{*, quux::Bar},
};
#
# fn main() {}
```

## 范围包含

先前，当在表达式中使用范围（ `..` 或 `...` ）时，其必须使用排除上界的 `..` ，而在模式中，则要使用包含上界的 `...` 。而现在，`..=` 可作为语法用于表达式或范围上下文件中的包含范围中。

```rust
fn main() {
    for i in 0 ..= 10 {
        match i {
            0 ..= 5 => println!("{}: low", i),
            6 ..= 10 => println!("{}: high", i),
            _ => println!("{}: out of range", i),
        }
    }
}
```

`...` 语法也可用在匹配语法中，但不能用于表达式中，而 `..=` 则二者皆可。

## 128 字节的整数

Rust 1.26.0 添加了 128 字节的整数基本类型：

- `u128`: 一个在 [0, 2^128 - 1] 范围内的 128 字节的无符号整数
- `i128`: 一个在 [-(2^127), 2^127 - 1] 范围内的有符号整数

这俩基本类型由 LLVM 支持高效地实现。即使在不支持 128 字节整数的平台上，它们都是可用的，且可像其他整数类型那样使用。

这俩基本类型在那些需要高效使用大整数的算法中非常有用，如某些加密算法。



## 附录 F：本书译本

> [appendix-06-translation.md](https://github.com/rust-lang/book/blob/master/src/appendix-06-translation.md)
> <br />
> commit 1fedfc4b96c2017f64ecfcf41a0a07e2e815f24f

一些非英语语言的资源。多数仍在翻译中；查阅 [翻译标签][label] 来帮助我们或使我们知道新的翻译！

[label]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations

- [Português](https://github.com/rust-br/rust-book-pt-br) (BR)
- [Português](https://github.com/nunojesus/rust-book-pt-pt) (PT)
- [Tiếng việt](https://github.com/hngnaig/rust-lang-book/tree/vi-VN)
- [简体中文](http://www.broadview.com.cn/article/144), [另一个版本（译者注：已基本翻译完毕）](https://github.com/KaiserY/trpl-zh-cn)
- [Українська](https://github.com/pavloslav/rust-book-uk-ua)
- [Español](https://github.com/thecodix/book)
- [Italiano](https://github.com/AgeOfWar/rust-book-it)
- [Русский](https://github.com/iDeBugger/rust-book-ru)
- [한국어](https://github.com/rinthel/rust-lang-book-ko)
- [日本語](https://github.com/hazama-yuinyan/book)
- [Français](https://github.com/quadrifoglio/rust-book-fr)
- [Polski](https://github.com/paytchoo/book-pl)
- [עברית](https://github.com/idanmel/rust-book-heb)
- [Cebuano](https://github.com/agentzero1/book)
- [Tagalog](https://github.com/josephace135/book)


## 附录 G：Rust 是如何开发的与 “Nightly Rust”

> [appendix-07-nightly-rust.md](https://github.com/rust-lang/book/blob/master/src/appendix-07-nightly-rust.md)
> <br />
> commit 1fedfc4b96c2017f64ecfcf41a0a07e2e815f24f

本附录介绍 Rust 是如何开发的以及这如何影响作为 Rust 开发者的你。

### 无停滞稳定

作为一个语言，Rust **十分** 注重代码的稳定性。我们希望 Rust 成为你代码坚实的基础，不过如果事物不断改变的话，这也将是不可能的。同时，当事物不再改变时，如果不能实验新功能的话，在发布之前我们也无法发现其中重大的缺陷。

对于这个问题我们的解决方案被称为 “无停滞稳定”（“stability without stagnation”），其指导性原则是：无需担心升级到最新的稳定版 Rust。每次升级应该是无痛的，并应带来新功能，更少的 bug 和更快的编译速度。

### Choo, Choo!（开车啦，逃）发布通道和发布时刻表（Riding the Trains）

Rust 开发运行于一个 **发布时刻表**（*train schedule*）之上。也就是说，所有的开发工作都位于 Rust 仓库的 `master` 分支。发布采用 software release train 模型，其被用于 思科 IOS 等其它软件项目。Rust 有三个 **发布通道**（*release channel*）：

* Nightly
* Beta
* Stable （稳定版）

大部分 Rust 开发者主要采用稳定版通道，不过希望实验新功能的开发者可能会使用 nightly 或 beta 版。

如下是一个开发和发布过程如何运转的例子：假设 Rust 团队正在进行 Rust 1.5 的发布工作。该版本发布于 2015 年 12 月，不过这里只是为了提供一个真实的版本。Rust 新增了一项功能：一个 `master` 分支的新提交。每天晚上，会产生一个新的 nightly 版本。每天都是发布版本的日子，而这些发布由发布基础设施自动完成。所以随着时间推移，发布轨迹看起来像这样，版本一天一发：

```text
nightly: * - - * - - *
```

每 6 周时间，是准备发布新版本的时候了！Rust 仓库的 `beta` 分支会从用于 nightly 的 `master` 分支产生。现在，有了两个发布版本：

```text
nightly: * - - * - - *
                     |
beta:                *
```

大部分 Rust 用户不会主要使用 beta 版本，不过在 CI 系统中对 beta 版本进行测试能够帮助 Rust 发现可能的回归缺陷（regression）。同时，每天仍产生 nightly 发布：

```text
nightly: * - - * - - * - - * - - *
                     |
beta:                *
```

第一个 beta 版的 6 周后，是发布稳定版的时候了！`stable` 分支从 `beta` 分支生成：

```text
nightly: * - - * - - * - - * - - * - - * - * - *
                     |
beta:                * - - - - - - - - *
                                       |
stable:                                *
```

好的！Rust 1.5 发布了！然而，我们忘了些东西：因为又过了 6 周，我们还需发布 **新版** Rust 的 beta 版，Rust 1.6。所以从 `stable` 生成 `beta` 分支后，新版的 `beta` 分之也再次从 `nightly` 生成：

```text
nightly: * - - * - - * - - * - - * - - * - * - *
                     |                         |
beta:                * - - - - - - - - *       *
                                       |
stable:                                *
```

这被称为 “train model”，因为每 6 周，一个版本 “离开车站”（“leaves the station”），不过从 beta 通道到达稳定通道还有一段旅程。

Rust 每 6周发布一个版本，如时钟版准确。如果你知道了某个 Rust 版本的发布时间，就可以知道下个版本的时间：6 周后。每 6 周发布版本的一个好的方面是下一班车会来得更快。如果特定版本碰巧缺失某个功能也无需担心：另一个版本很快就会到来！这有助于减少因临近发版时间而偷偷释出未经完善的功能的压力。

多亏了这个过程，你总是可以切换到下一版本的 Rut 并验证是否可以轻易的升级：如果 beta 版不能如期工作，你可以向 Rust 团队报告并在发布稳定版之前得到修复！beta 版造成的破坏是非常少见的，不过 `rustc` 也不过是一个软件，可能会存在 bug。

### 不稳定功能

这个发布模型中另一个值得注意的地方：不稳定功能（unstable features）。Rust 使用一个被称为 “功能标记”（“feature flags”）的技术来确定给定版本的某个功能是否启用。如果新功能正在积极底开发中，其提交到了 `master`，因此会出现在 nightly 版中，不过会位于一个 **功能标记** 之后。作为用户，如果你希望尝试这个正在开发的功能，则可以在源码中使用合适的标记来开启，不过必须使用 nightly 版。

如果使用的是 beta 或稳定版 Rust，则不能使用任何功能标记。这是在新功能被标记未永久稳定之前获得实用价值的关键。这既满足了希望实用最尖端技术的同学，那些坚持稳定版的同学也知道其代码不会被破坏。这就是无停滞稳定。

本书只包含稳定的功能，因为还在开发中的功能仍可能改变，当其进入稳定版时肯定会与编写本书的时候有所不同。你可以在网上获取 nightly 版的文档。

### Rustup 和 Rust Nightly 的职责

Rustup 使得改变不同发布通道的 Rust 更为简单，其在全局或分项目的层次工作。其默认会安装稳定版 Rust。例如为了安装 nightly：

```text
$ rustup install nightly
```

你会发现 `rustup` 也安装了所有的 **工具链**（*toolchains*， Rust 和其相关组件）。如下是一位作者的 Windows 计算机上的例子：

```powershell
> rustup toolchain list
stable-x86_64-pc-windows-msvc (default)
beta-x86_64-pc-windows-msvc
nightly-x86_64-pc-windows-msvc
```

如你所见，默认是稳定版。大部分 Rust 用户在大部分时间使用稳定版。你可能也会这么做，不过如果你关心最新的功能，可以为特定项目使用 nightly 版。为此，可以在项目目录使用 `rustup override` 来设置当前目录 `rustup`使用 nightly 工具链作：

```text
$ cd ~/projects/needs-nightly
$ rustup override set nightly
```

现在，每次在 *~/projects/needs-nightly* 调用 `rustc` 或 `cargo`，`rustup` 会确保使用 nightly 版 Rust。在这有很多 Rust 项目时大有裨益！

### RFC 过程和团队

那么你如何了解这些新功能呢？Rust 开发模式遵循一个 **Request For Comments (RFC) 过程**。如果你希望改进 Rust，可以编写一个提议，也就是 RFC。

任何人都可以编写 RFC 来改进 Rust，同时这些 RFC 会被 Rust 团队评审和讨论，他们由很多不同分工的子团队组成。这里是 [Rust 官网上](https://www.rust-lang.org/en-US/team.html) 所有团队的总列表，其包含了项目中每个领域的团队：语言设计、编译器实现、基础设施、文档等。个个团队会阅读相应的提议和评论，编写回复，并最终达成接受或回绝功能的一致。

如果功能被接受了，在 Rust 仓库会打开一个 issue，人们就可以实现它。实现功能的人当人可能不是最初提议功能的人！当实现完成后，其会合并到 `master` 分支并位于一个功能开关（feature gate）之后，正如 [“不稳定功能”(#unstable-features) 部分所讨论的。

在稍后的某个时间，一旦使用 nightly 版的 Rust 团队能够尝试这个功能了，团队成员会讨论这个功能，它如何在 nightly 中工作，并决定是否应该进入稳定版。如果决定继续推进，功能开关会移除，然后这个功能就被认为是稳定的了！并会在新的稳定版 Rust 中出现。


