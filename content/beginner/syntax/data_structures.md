---
title: 数据结构
date: 2024-08-18T16:41:35+08:00
draft: false
weight: 59
---

在编程世界中，数据结构就像建筑师手中的图纸，它们为我们提供了组织和管理数据的蓝图。Python 提供了多种内置的数据结构，例如列表、元组、字典和集合，每种数据结构都有其独特的特性和用途，帮助我们高效地存储和操作各种类型的数据。本章节将深入探讨这些数据结构，并辅以丰富的示例，带你领略 Python 数据结构的魅力。

## 列表

**列表** 是一种用于保存一系列有序项目的集合，你可以用它来存储任何类型的数据，例如数字、字符串、甚至是其他列表。

### 创建列表

使用方括号 `[]` 来创建列表，并用逗号 `,` 分隔列表中的元素：

```python
shopping_list = ["苹果", "香蕉", "牛奶", "面包"]
```

### 列表的特性

- **有序性：** 列表中的元素按照添加的顺序排列，并保持其顺序不变。
- **可变性：** 列表是可变的，这意味着你可以添加、删除或修改列表中的元素。
- **可重复性：** 列表可以包含重复的元素。

### 访问列表元素

使用索引操作符 `[]` 来访问列表中的元素，索引从 0 开始。

```python
print(shopping_list[0])  # 输出: 苹果
print(shopping_list[-1])  # 输出: 面包
```

### 列表操作

Python 提供了丰富的列表操作方法，例如：

- **添加元素:** `append()`、`insert()`、`extend()`
- **删除元素:** `remove()`、`pop()`、`del`
- **修改元素:** 直接通过索引赋值
- **查找元素:** `index()`、`count()`
- **排序:** `sort()`、`reverse()`

**示例：**

```python
# 创建一个列表
numbers = [1, 3, 5, 2, 4]

# 添加元素
numbers.append(6)  # 在列表末尾添加元素 6
numbers.insert(2, 7)  # 在索引 2 处插入元素 7

# 删除元素
numbers.remove(3)  # 删除元素 3
numbers.pop(0)  # 删除索引 0 处的元素，并返回该元素

# 修改元素
numbers[1] = 8  # 将索引 1 处的元素修改为 8

# 查找元素
print(numbers.index(4))  # 查找元素 4 的索引，输出: 3
print(numbers.count(2))  # 统计元素 2 出现的次数，输出: 1

# 排序
numbers.sort()  # 对列表进行升序排序
numbers.reverse()  # 反转列表

# 输出列表
print(numbers)  # 输出: [8, 7, 5, 4, 2, 1]
```

### 列表推导式

列表推导式是一种简洁高效的创建列表的方式。

```python
squares = [x ** 2 for x in range(1, 6)]  # 创建一个包含 1 到 5 的平方数的列表
print(squares)  # 输出: [1, 4, 9, 16, 25]
```

## 元组

**元组** 与列表类似，也是用于存储一系列数据的集合，但元组是**不可变的**，这意味着一旦创建元组后，就不能修改其内容。

### 创建元组

使用圆括号 `()` 来创建元组，并用逗号 `,` 分隔元组中的元素：

```python
coordinates = (10, 20)
empty_tuple = ()    # 定义一个空元组
single_tuple = (1,) # 带有一个元素的元组，不能忘记逗号
```

### 元组的特性

- **有序性：** 元组中的元素按照添加的顺序排列，并保持其顺序不变。
- **不可变性：** 元组是不可变的，不能添加、删除或修改元素。
- **可重复性：** 元组可以包含重复的元素。

### 访问元组元素

与列表类似，使用索引操作符 `[]` 来访问元组中的元素，索引从 0 开始。

```python
print(coordinates[0])  # 输出: 10
```

### 元组的用途

由于元组是不可变的，因此它们通常用于以下场景：

- **存储不可变的数据：** 例如，存储坐标、日期等数据。
- **作为字典的键：** 字典的键必须是不可变的，因此可以使用元组作为字典的键。
- **函数的返回值：** 函数可以返回多个值，这些值可以封装成一个元组返回。

## 字典

**字典** 是一种用于存储键值对的集合，它允许你通过键来访问对应的值，就像一个现实中的字典，你可以通过单词来查找其定义。

### 创建字典

使用花括号 `{}` 来创建字典，并使用冒号 `:` 分隔键和值，用逗号 `,` 分隔不同的键值对：

```python
person = {"name": "Alice", "age": 30, "city": "New York"}
```

### 字典的特性

- **无序性：** 字典中的元素没有固定的顺序。
- **可变性：** 字典是可变的，可以添加、删除或修改键值对。
- **键的唯一性：** 字典中的键必须是唯一的，不能重复。

### 访问字典元素

使用键来访问字典中对应的值：

```python
print(person["name"])  # 输出: Alice
```

### 字典操作

Python 提供了丰富的字典操作方法，例如：

- **添加键值对:** 直接通过键赋值
- **删除键值对:** `del`、`pop()`
- **获取所有键:** `keys()`
- **获取所有值:** `values()`
- **获取所有键值对:** `items()`

**示例：**

```python
# 创建一个字典
person = {"name": "Alice", "age": 30, "city": "New York"}

# 添加键值对
person["occupation"] = "Software Engineer"

# 删除键值对
del person["city"]

# 获取所有键
print(person.keys())  # 输出: dict_keys(['name', 'age', 'occupation'])

# 获取所有值
print(person.values())  # 输出: dict_values(['Alice', 30, 'Software Engineer'])

# 获取所有键值对
print(person.items())  # 输出: dict_items([('name', 'Alice'), ('age', 30), ('occupation', 'Software Engineer')])
```

## 集合

**集合** 是一种用于存储唯一元素的无序集合，它不允许重复元素，就像一个数学集合。

### 创建集合

使用花括号 `{}` 或 `set()` 函数来创建集合：

```python
my_set = {1, 2, 3}
my_set = set([1, 2, 3, 3])  # 重复元素会被自动去除
```

### 集合的特性

- **无序性：** 集合中的元素没有固定的顺序。
- **唯一性：** 集合中的元素必须是唯一的，不能重复。
- **可变性：** 集合是可变的，可以添加或删除元素。

### 集合操作

Python 提供了丰富的集合操作方法，例如：

- **添加元素:** `add()`、`update()`
- **删除元素:** `remove()`、`discard()`、`pop()`
- **集合运算:** 并集 `\|`、交集 `&`、差集 `-`、对称差集 `^`

**示例：**

```python
# 创建两个集合
set1 = {1, 2, 3}
set2 = {3, 4, 5}

# 添加元素
set1.add(4)
set1.update([5, 6])

# 删除元素
set1.remove(3)  # 如果元素不存在，会抛出 KeyError 异常
set1.discard(7)  # 如果元素不存在，不会抛出异常

# 集合运算
print(set1 | set2)  # 并集，输出: {1, 2, 4, 5, 6}
print(set1 & set2)  # 交集，输出: {4, 5}
print(set1 - set2)  # 差集，输出: {1, 2, 6}
print(set1 ^ set2)  # 对称差集，输出: {1, 2, 3, 6}
```

## 索引操作符

**索引操作符 `[]`** 用于访问序列（例如列表、元组、字符串）中的元素。它支持多种索引方式，包括正索引、负索引和切片。

### 正索引

从序列的开头开始计数，第一个元素的索引为 0，第二个元素的索引为 1，以此类推。

```python
my_list = ["apple", "banana", "cherry"]
print(my_list[0])  # 输出: apple
print(my_list[2])  # 输出: cherry
```

### 负索引

从序列的末尾开始计数，最后一个元素的索引为 -1，倒数第二个元素的索引为 -2，以此类推。

```python
print(my_list[-1])  # 输出: cherry
print(my_list[-2])  # 输出: banana
```

### 切片

切片用于获取序列的一部分，语法为 `[start:end:step]`，其中：

- `start`：切片的起始索引（包含）。
- `end`：切片的结束索引（不包含）。
- `step`：切片的步长，默认为 1。

```python
print(my_list[1:3])  # 输出: ['banana', 'cherry']
print(my_list[::2])  # 输出: ['apple', 'cherry']
print(my_list[::-1])  # 输出: ['cherry', 'banana', 'apple']
```

### 组合使用

可以组合使用正索引、负索引和切片来访问序列中的元素。

```python
print(my_list[1:-1])  # 输出: ['banana']
print(my_list[-2:0:-1])  # 输出: ['banana', 'apple']
```

### 注意事项

- 索引操作符 `[]` 只能用于序列类型，例如列表、元组、字符串。
- 索引值必须是整数。
- 如果索引值超出范围，会抛出 `IndexError` 异常。

## 字符串

**字符串** 是一种表示文本数据的数据类型，它是字符的序列。在之前的章节中，我们已经学习了字符串的基本操作。接下来，我们将介绍一些字符串的高级用法。

### 字符串方法

Python 提供了丰富的字符串方法，可以对字符串进行各种操作，例如：

- **大小写转换：** `upper()`、`lower()`、`title()`、`capitalize()`
- **查找和替换：** `find()`、`index()`、`replace()`
- **分割和连接：** `split()`、`join()`
- **格式化：** `format()`、`f-strings`
- **判断：** `startswith()`、`endswith()`、`isalpha()`、`isdigit()`、`isalnum()`

**示例：**

```python
# 创建一个字符串
message = "Hello, world!"

# 大小写转换
print(message.upper())  # 输出: HELLO, WORLD!
print(message.lower())  # 输出: hello, world!

# 查找和替换
print(message.find("world"))  # 输出: 7
print(message.replace("world", "Python"))  # 输出: Hello, Python!

# 分割和连接
words = message.split(",")  # 将字符串按逗号分割成列表
print(words)  # 输出: ['Hello', ' world!']
print("-".join(words))  # 使用 "-" 连接列表中的元素，输出: Hello- world!

# 格式化
name = "Alice"
age = 30
print(f"{name} is {age} years old.")  # 使用 f-strings 格式化字符串

# 判断
print(message.startswith("Hello"))  # 输出: True
print(message.isalpha())  # 输出: False
```

### 字符串切片

字符串也支持切片操作，可以像列表一样获取字符串的一部分。

```python
message = "Hello, world!"
print(message[7:12])  # 输出: world
```

### 字符串不可变性

字符串是不可变的，这意味着你不能修改字符串中的单个字符。如果你尝试修改字符串中的字符，会抛出 `TypeError` 异常。

```python
message = "Hello"
message[0] = "J"  # 抛出 TypeError 异常
```

要修改字符串，你需要创建一个新的字符串。

## 总结

数据结构是编程的基石，它们为我们提供了组织和管理数据的强大工具。Python 提供了丰富的内置数据结构，包括列表、元组、字典和集合，每种数据结构都有其独特的特性和用途。熟练掌握这些数据结构和索引操作符的使用，并了解字符串的高级用法，你将能够编写更加高效、灵活、易于维护的 Python 代码。
