---
type: doc
layout: reference
category: "Other"
title: "相等性"
---

# 相等性

Kotlin 中有两种类型的相等性：

* 结构相等（用 `equals()` 检测）；
* 引用相等（两个引用指向同一对象）。

## 结构相等

结构相等由 `==`（以及其否定形式 `!=`）操作判断。按照惯例，像 `a == b` 这样的表达式会翻译成：

<div class="sample" markdown="1" theme="idea" data-highlight-only>
```kotlin
a?.equals(b) ?: (b === null)
```
</div>

也就是说如果 `a` 不是 `null` 则调用 `equals(Any?)` 函数，否则（即 `a` 是 `null`）检测 b 是否与 `null` 引用相等。

请注意，当与 `null` 显式比较时完全没必要优化你的代码：`a == null` 会被自动转换为 `a === null`。

如需提供自定义的相等检测实现，请覆盖 [`equals(other: Any?): Boolean`]https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-any/equals.html) 函数。名称相同但签名不同的函数，如 `equals(other: Foo)` 并不会影响操作符 `==` 与 `!=` 的相等性检测。

结构相等与 `Comparable<……>` 接口定义的比较无关，因此只有自定义的 `equals(Any?)` 实现可能会影响该操作符的行为。

## 浮点数相等性

当相等性检测的两个操作数都是静态已知的（可空或非空的）`Float` 或 `Double` 类型时，该检测遵循 IEEE 754
浮点数运算标准。

否则会使用不符合该标准的结构相等性检测，这会导致 `NaN` 等于其自身，而 `-0.0` 不等于 `0.0`。

参见：[浮点数比较](basic-types.html#浮点数比较)。

## 引用相等

引用相等由 `===`（以及其否定形式 `!==`）操作判断。`a === b`
当且仅当 `a` 与 `b` 指向同一个对象时求值为 true。对于运行时表示为原生类型的值
（例如 `Int`），`===` 相等检测等价于 `==` 检测。
