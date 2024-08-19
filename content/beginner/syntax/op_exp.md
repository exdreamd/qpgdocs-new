---
title: 运算符和表达式
date: 2024-08-18T16:41:35+08:00
draft: false
weight: 55
---

在编程世界中，我们经常需要对数据进行各种运算，例如加减乘除、比较大小、逻辑判断等。Python 提供了丰富的**运算符**和**表达式**来完成这些任务，它们是构建程序逻辑和实现复杂功能的基石。本章节将深入探讨 Python 的运算符、表达式以及运算优先级，帮助你掌握 Python 的计算能力，并理解其背后的逻辑。

## 表达式

表达式是 Python 代码中用于执行计算的指令，它由**运算符**和**操作数**组成。

- **操作数:** 参与运算的数据，可以是字面常量、变量、函数调用等。
- **运算符:** 用于执行特定操作的符号或关键字，例如 `+`、`-`、`*`、`/`、`>`、`<`、`==`、`and`、`or` 等。

例如，`2 + 3` 是一个简单的表达式，其中 `2` 和 `3` 是操作数，`+` 是运算符。

### 表达式的求值

当你运行包含表达式的 Python 代码时，Python 解释器会按照一定的规则对表达式进行求值，并返回一个结果。

```python
>>> 2 + 3  # 表达式求值，返回结果 5
5
```

## 运算符

Python 提供了丰富的运算符，可以分为以下几类：

### 算术运算符

用于执行基本的数学运算。

| 运算符 | 描述   | 示例      | 结果                |
| ------ | ------ | --------- | ------------------- |
| `+`    | 加法   | `3 + 5`   | `8`                 |
| `-`    | 减法   | `50 - 24` | `26`                |
| `*`    | 乘法   | `2 * 3`   | `6`                 |
| `**`   | 幂运算 | `3 ** 4`  | `81`                |
| `/`    | 除法   | `13 / 3`  | `4.333333333333333` |
| `//`   | 整除   | `13 // 3` | `4`                 |
| `%`    | 取模   | `13 % 3`  | `1`                 |

### 位运算符

用于对整数的二进制位进行操作。

| 运算符 | 描述     | 示例      | 结果 |
| ------ | -------- | --------- | ---- |
| `<<`   | 左移     | `2 << 2`  | `8`  |
| `>>`   | 右移     | `11 >> 1` | `5`  |
| `&`    | 按位与   | `5 & 3`   | `1`  |
| `\|`   | 按位或   | `5 \| 3`  | `7`  |
| `^`    | 按位异或 | `5 ^ 3`   | `6`  |
| `~`    | 按位取反 | `~5`      | `-6` |

### 比较运算符

用于比较两个值的大小关系，返回布尔值 `True` 或 `False`。

| 运算符 | 描述     | 示例                   | 结果    |
| ------ | -------- | ---------------------- | ------- |
| `<`    | 小于     | `5 < 3`                | `False` |
| `>`    | 大于     | `5 > 3`                | `True`  |
| `<=`   | 小于等于 | `x = 3; y = 6; x <= y` | `True`  |
| `>=`   | 大于等于 | `x = 4; y = 3; x >= 3` | `True`  |
| `==`   | 等于     | `x = 2; y = 2; x == y` | `True`  |
| `!=`   | 不等于   | `x = 2; y = 3; x != y` | `True`  |

### 逻辑运算符

用于组合多个布尔表达式，返回布尔值 `True` 或 `False`。

| 运算符 | 描述                                    | 示例                           | 结果    |
| ------ | --------------------------------------- | ------------------------------ | ------- |
| `not`  | 对布尔值取反                            | `x = True; not x`              | `False` |
| `and`  | 两个表达式都为 `True` 时才返回 `True`   | `x = False; y = True; x and y` | `False` |
| `or`   | 至少有一个表达式为 `True` 时返回 `True` | `x = True; y = False; x or y`  | `True`  |

### 赋值运算符

用于将值赋给变量。

| 运算符 | 描述         | 示例      | 等价于       |
| ------ | ------------ | --------- | ------------ |
| `=`    | 赋值         | `a = 2`   | `a = 2`      |
| `+=`   | 加法赋值     | `a += 3`  | `a = a + 3`  |
| `-=`   | 减法赋值     | `a -= 5`  | `a = a - 5`  |
| `*=`   | 乘法赋值     | `a *= 2`  | `a = a * 2`  |
| `/=`   | 除法赋值     | `a /= 4`  | `a = a / 4`  |
| `//=`  | 整除赋值     | `a //= 3` | `a = a // 3` |
| `%=`   | 取模赋值     | `a %= 10` | `a = a % 10` |
| `**=`  | 幂运算赋值   | `a **= 2` | `a = a ** 2` |
| `<<=`  | 左移赋值     | `a <<= 2` | `a = a << 2` |
| `>>=`  | 右移赋值     | `a >>= 2` | `a = a >> 2` |
| `&=`   | 按位与赋值   | `a &= 3`  | `a = a & 3`  |
| `\|=`  | 按位或赋值   | `a \|= 5` | `a = a \| 5` |
| `^=`   | 按位异或赋值 | `a ^= 7`  | `a = a ^ 7`  |

### 其他运算符

Python 还提供了一些其他的运算符，例如成员运算符 `in` 和 `not in`，身份运算符 `is` 和 `is not` 等。这些运算符将在后续章节中介绍。

## 运算优先级

当一个表达式包含多个运算符时，Python 解释器会按照一定的优先级顺序来执行运算。这与数学中的运算优先级规则类似。

例如，表达式 `2 + 3 * 4` 的结果是 `14`，而不是 `20`，因为乘法运算符 `*` 的优先级高于加法运算符 `+`。

**优先级表：**

以下表格列出了 Python 运算符的优先级，从低到高排列：

| 运算符                                                                        | 描述                                   |
| ----------------------------------------------------------------------------- | -------------------------------------- |
| `lambda`                                                                      | Lambda 表达式                          |
| `if - else`                                                                   | 条件表达式                             |
| `or`                                                                          | 布尔“或”                               |
| `and`                                                                         | 布尔“与”                               |
| `not x`                                                                       | 布尔“非”                               |
| `in`, `not in`, `is`, `is not`, `<`, `<=`, `>`, `>=`, `!=`, `==`              | 比较运算符，包括成员测试和身份测试     |
| `\|`                                                                          | 按位或                                 |
| `^`                                                                           | 按位异或                               |
| `&`                                                                           | 按位与                                 |
| `<<`, `>>`                                                                    | 移位运算符                             |
| `+`, `-`                                                                      | 加法和减法                             |
| `*`, `/`, `//`, `%`                                                           | 乘法、除法、整除和取模                 |
| `+x`, `-x`, `~x`                                                              | 正号、负号和按位取反                   |
| `**`                                                                          | 幂运算                                 |
| `x[index]`, `x[index:index]`, `x(arguments...)`, `x.attribute`                | 下标、切片、调用和属性引用             |
| `(expressions...)`, `[expressions...]`, `{key: value...}`, `{expressions...}` | 用于创建元组、列表、字典和集合的表达式 |

**改变运算顺序：**

可以使用括号 `()` 来改变运算顺序。例如，`(2 + 3) * 4` 的结果是 `20`，因为括号内的加法运算会优先执行。

## 结合性

当多个具有相同优先级的运算符出现在同一个表达式中时，Python 解释器会根据运算符的结合性来决定它们的执行顺序。

大多数 Python 运算符都是**从左到右结合**的，这意味着它们会按照从左到右的顺序依次执行。例如，`2 + 3 + 4` 会被解释为 `(2 + 3) + 4`。

但也有一些运算符是**从右到左结合**的，例如幂运算符 `**`。例如，`2 ** 3 ** 2` 会被解释为 `2 ** (3 ** 2)`，结果是 `512`，而不是 `64`。

## 代码示例

```python
length = 5
breadth = 2

area = length * breadth
print('面积为', area)
print('周长为', 2 * (length + breadth))
```

**输出：**

```
面积为 10
周长为 14
```

**代码解析：**

- 首先，我们将矩形的长度和宽度分别存储在名为 `length` 和 `breadth` 的变量中。
- 然后，我们使用表达式 `length * breadth` 计算矩形的面积，并将结果存储在变量 `area` 中。
- 接下来，我们使用 `print()` 函数输出面积的值，并使用字符串拼接将文本 "面积为" 和变量 `area` 的值组合在一起输出。
- 最后，我们使用表达式 `2 * (length + breadth)` 计算矩形的周长，并直接在 `print()` 函数中输出结果。

这个例子展示了如何使用变量、运算符和表达式来进行简单的几何计算。