---
title: 特殊的双分区文件系统
date: 2024-07-31T22:56:16+08:00
draft: false
---

在 QuecPython 开发中，文件系统至关重要。它不仅是存储用户脚本和数据的地方，也深刻影响着程序的运行和交互方式。与传统的单片机平台不同，QuecPython 采用了一种特殊的双分区文件系统，为开发带来了独特的特性和挑战。

本文将深入浅出地介绍 QuecPython 双分区文件系统的概念、结构和使用方法，解释它与传统单片机文件系统的差异，并通过实际示例，帮助您更好地理解 QuecPython 的运行机制，避免常见错误，提升开发效率。

## QuecPython 的双分区文件系统：存储代码和数据

在 QuecPython 开发中，文件系统至关重要。它不仅是存储用户脚本和数据的地方，也深刻影响着程序的运行和交互方式。与传统的单片机平台不同，QuecPython 采用了一种特殊的双分区文件系统，为开发带来了独特的特性和挑战。

本文将深入浅出地介绍 QuecPython 双分区文件系统的概念、结构和使用方法，解释它与传统单片机文件系统的差异，并通过实际示例，帮助您更好地理解 QuecPython 的运行机制，避免常见错误，提升开发效率。

### 传统的单分区文件系统

在传统的单片机平台上，例如使用 MicroPython 进行开发的 STM32 或 RP2040 等，文件系统通常只有一个分区，即根分区 (`/`)，类似于电脑上的 C 盘。所有用户文件，包括脚本文件、配置文件、数据文件等，都存储在这个分区中。这种单分区文件系统结构简单，易于理解和使用。

### QuecPython 的双分区文件系统

作为一类复杂的 SoC 芯片组成的产品，模块内部的 Flash 存储器被划分为多个独立的文件系统分区。QuecPython 用户在使用时，通常会涉及到以下两个分区：

- **用户文件系统 (`/usr`)**: 存储用户编写的 Python 脚本 (`.py` 文件)、配置文件、数据文件和其他资源文件。用户拥有对该分区的读写权限。
- **备份文件系统 (`/bak`)**: 主要面向需要用模组批量制造商业化产品并量产的用户。在产品出厂前，他们可以将原版的脚本和配置文件等存储一份到 `/bak` 分区中，这样即使后期产品运行时 `/usr` 分区内的脚本和配置文件发生损坏，也能够从 `/bak` 分区自行拷贝并恢复，从而增强产品的稳定性和可靠性。该分区对普通用户而言通常是只读的，且在开发过程中几乎不会涉及到，因此可以暂不关注。

QuecPython 默认的运行路径是根目录 `/`，而用户脚本的存储路径是在用户文件系统分区 `/usr` 下。这就导致了 QuecPython 与其他 MicroPython 平台之间的一个显著差异： **用户脚本的存储路径和运行路径不一致**。因此，您需要特别注意，在 QuecPython 中，程序运行时访问的根目录 `/` 与您存储代码和文件的 `/usr` 分区是不同的。

## 跨文件操作：路径问题的解决之道

由于用户脚本的存储路径和运行路径不一致，因此在进行跨文件操作时，例如导入模块、打开文件、保存文件等，都需要格外注意路径问题。

**举例说明**:

假设您在 `/usr` 目录下创建了一个名为 `my_module.py` 的 Python 模块文件，然后在 REPL 环境中尝试导入该模块：

```python
>>> import my_module
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: no module named 'my_module'
```

您会发现导入失败，并提示 `no module named 'my_module'`。这是因为 QuecPython 解释器默认在根目录 `/` 下查找模块文件，而 `my_module.py` 文件存储在 `/usr` 目录下。

为了解决这个问题，您需要在导入模块之前，将当前工作目录切换到 `/usr` 目录，或者在导入语句中指定模块文件的完整路径。

### 切换工作目录

您可以使用 `uos` 模块的 `chdir()` 函数来切换当前工作目录：

```python
>>> import uos  # 导入文件系统操作模块
>>> uos.chdir("/usr")  # 将当前工作目录切换到 /usr 目录
>>> import my_module  # 导入模块
```

切换工作目录后，解释器会在 `/usr` 目录下查找 `my_module.py` 文件，因此导入操作会成功。

### 指定完整路径

您也可以在导入语句中指定模块文件的完整路径：

```python
>>> from usr import my_module  # 从 /usr 目录导入 my_module 模块
```

这样，解释器会直接从 `/usr` 目录下查找 `my_module.py` 文件，无需切换工作目录。

### 其他跨文件操作

除了导入模块之外，其他跨文件操作，例如打开文件、保存文件等，也需要注意路径问题。您需要根据实际情况，选择使用完整路径或相对路径。

**建议**:

- **完整路径**: 完整路径是指从根目录开始的路径，例如 `/usr/my_file.txt`。使用完整路径可以避免路径歧义，确保文件操作的正确性。
- **相对路径**: 相对路径是指相对于当前工作目录的路径，例如 `my_file.txt`。使用相对路径可以简化代码，但需要确保当前工作目录正确。

## 操作示例

以下是一些常见的跨文件操作示例，演示了如何正确处理路径问题：

### 导入模块

```python
# 错误示例：解释器会在根目录下查找 settings 模块，导致导入失败
>>> import settings
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: no module named 'settings'

# 正确示例 1：使用完整路径导入模块
>>> from usr import settings

# 正确示例 2：切换工作目录后再导入模块
>>> import uos
>>> uos.chdir("/usr")
>>> import settings
```

### 打开文件

```python
# 错误示例：解释器会在根目录下查找文件，导致打开失败
>>> myfile = open("settings.py")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
OSError: [Errno 19] ENODEV

# 正确示例 1：使用完整路径打开文件
>>> myfile = open("/usr/settings.py")

# 正确示例 2：切换工作目录后再使用相对路径打开文件
>>> import uos
>>> uos.chdir("/usr")
>>> myfile = open("settings.py")
```

### 存储文件

```python
# 错误示例：解释器会在根目录下保存文件，导致保存失败
>>> import ql_fs
>>> data = {"test": 1}
>>> ql_fs.touch("myconfig.json", data)
-1  # 返回 -1 表示保存失败

# 正确示例 1：使用完整路径保存文件
>>> import ql_fs
>>> data = {"test": 1}
>>> ql_fs.touch("/usr/myconfig.json", data)
0  # 返回 0 表示保存成功

# 正确示例 2：切换工作目录后再使用相对路径保存文件
>>> import uos
>>> import ql_fs
>>> uos.chdir("/usr")
>>> data = {"test": 1}
>>> ql_fs.touch("myconfig.json", data)
0  # 返回 0 表示保存成功
```

## 总结

QuecPython 的双分区文件系统为用户提供了更大的存储空间和更灵活的文件管理方式，但也需要开发者在进行跨文件操作时格外注意路径问题。通过本文，您应该已经了解了 QuecPython 双分区文件系统的概念、结构和使用方法，并理解了它与传统单片机文件系统的差异。希望这些信息能够帮助您避免常见的错误，并编写更加健壮和可靠的 QuecPython 程序。
