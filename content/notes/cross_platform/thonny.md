---
title: 使用 Thonny 开发 QuecPython
date: 2024-08-02T16:36:16+08:00
draft: false
---

{{< callout type="error" >}}

Thonny 对于模组运行内存的占用较多，对于内存较小的型号，使用 Thonny 开发极易造成程序崩溃和重启。因此，推荐用户尽可能使用 QPYcom 工具进行开发和调试。

{{< /callout >}}

Thonny 是一款以其简洁易用和对初学者友好的特性而闻名的 Python IDE，尤其在 MicroPython 领域备受推崇。 它支持包括 STM32、ESP32、Raspberry Pi Pico 等多种硬件平台，并提供代码高亮、自动补全、调试器、变量查看器等实用功能，成为众多 MicroPython 开发者和教育者的首选工具。

QuecPython，作为移远通信推出的基于 MicroPython 的模块二次开发方式，也吸引了众多开发者的目光。 它将 Python 语言的简洁易用与移远通信模块的强大功能相结合，为物联网应用开发提供了全新的可能性。

Thonny 能否应用于 QuecPython 模块的开发呢？ 答案是肯定的，但需要进行一些调整。 这是因为 Thonny 在设计时主要面向的是通用 MicroPython 平台，而 QuecPython 在一些实现细节上与标准的 MicroPython 有所差异，导致 Thonny 的某些功能无法直接兼容。

本文将为您详细介绍如何修改 Thonny 源码，使其支持 QuecPython 开发，从而打破平台的限制，让您在熟悉的 Thonny 环境中自由地开发 QuecPython 应用。

{{< callout type="warning" >}}

**注意**

- **版本兼容性**: 本文内容仅在 Thonny 4.1.1 和 4.1.2 版本中进行过验证，不保证这些步骤在后续版本中的可用性。
- **旧版本不适用**: 本文内容不适用于 Thonny 4.0.0 之前的版本。
- **谨慎操作**: 贸然修改源码可能导致程序崩溃和其他严重后果，请谨慎尝试，并确保在修改之前备份原始文件。

{{< /callout >}}

## Thonny：初学者的 Python 良师益友

### Thonny 的优势

Thonny 之所以受到广大 Python 初学者和 MicroPython 开发者的青睐，主要得益于以下优势：

- **简洁易用**: Thonny 的界面简洁直观，操作简单易懂，即使是没有任何编程经验的用户也能快速上手。
- **功能强大**: Thonny 提供了丰富的功能，例如代码高亮、自动补全、调试器、变量查看器等，可以帮助您更轻松地编写和调试 Python 代码。
- **MicroPython 支持**: Thonny 对 MicroPython 提供了良好的支持，可以自动识别和连接多种 MicroPython 设备，并提供便捷的代码上传、下载和执行功能。

### 安装 Thonny

#### Windows 用户

推荐的安装方式是从 [GitHub Release](https://github.com/thonny/thonny/releases) 上下载安装包。

- **初学者**: 建议下载 Portable 压缩包 (例如 `thonny-4.1.x-windows-portable.zip`)，它内置了 Python 运行环境，开箱即用，无需安装其他程序。 只需将压缩包解压到您想要安装的目录，然后双击 `thonny.exe` 即可启动 Thonny。
- **高级用户**: 如果您已经安装了 Python 3 和 pip，则可以使用以下命令安装最新版本的 Thonny：

```powershell
python -m pip install thonny
```

#### macOS 用户

建议您按照 [官方 wiki](https://github.com/thonny/thonny/wiki/MacOSX) 的说明进行操作。

#### Linux 用户

Thonny 已经被加入到大部分 Linux 发行版的软件仓库中，您可以使用系统的包管理器轻松安装 Thonny。 例如，在 Ubuntu 系统中，可以使用以下命令安装：

```bash
sudo apt install python3-tk thonny
```

在 Fedora 系统中，可以使用以下命令安装：

```bash
sudo dnf install python3-tkinter thonny
```

{{< callout type="info" >}}

**提示**

- 通过 pip 安装的 Thonny 通常不会在桌面和应用程序菜单中生成快捷方式。您可以通过在命令行 (PowerShell、Terminal 等) 中输入 `thonny` 的命令来启动 Thonny。
- 如果您需要使用 Thonny 的某些高级功能，例如调试器、变量查看器等，您可能需要安装一些额外的 Python 库。

{{< /callout >}}

## 修改 Thonny 源码：连接 QuecPython 模块

Thonny 默认情况下无法与 QuecPython 模块建立稳定的连接，这主要是因为 QuecPython 固件在编译时未开启 MicroPython 的 `WEAK_LINKS` 特性。`WEAK_LINKS` 特性允许 MicroPython 在找不到标准库模块时，自动尝试导入带 `u` 前缀的同名模块。 例如，当代码中使用 `import sys` 导入 `sys` 模块时，如果 MicroPython 解释器找不到 `sys` 模块，它会自动尝试导入 `usys` 模块。

然而，QuecPython 固件为了节省存储空间，默认关闭了 `WEAK_LINKS` 特性。 因此，当 Thonny 尝试导入 `sys` 模块时，由于模块中不存在 `sys` 模块，且 `WEAK_LINKS` 特性未启用，导入操作会失败，导致 Thonny 无法与 QuecPython 模块正常通信。

为了解决这个问题，我们需要手动修改 Thonny 的部分源码，使其在导入标准库模块时，能够**直接尝试导入带 `u` 前缀的模块名称**，而无需依赖 `WEAK_LINKS` 特性。

{{< callout type="warning" >}}

**注意**

- 您需要具备基本的 Python 编程技能和编辑器 (例如 Visual Studio Code) 使用技能，才能进行源码修改。
- 在您理解要修改的代码含义之前，不建议您独自尝试。如果您执意尝试修改，请务必提前对原文件进行备份，以防止修改错误导致 Thonny 无法正常运行。

{{< /callout >}}

### 找到 Thonny 的安装目录

1.  启动 Thonny。
2.  在 Thonny 主界面顶部的菜单栏中，选择 **工具 -> 打开 Thonny 安装目录...**，这会打开 Thonny 的安装目录。

### 修改 `mp_back.py` 文件

`mp_back.py` 文件是 Thonny 用于与 MicroPython 设备通信的后端程序，我们需要修改它来适配 QuecPython 的特殊实现。

1.  在 Thonny 的安装目录中，找到 `thonny/plugins/micropython/mp_back.py` 文件，并使用文本编辑器打开它。
2.  找到 `_get_all_helpers()` 函数，它位于文件中的 200-250 行之间。
3.  在该函数的代码中，找到以下代码块：

```python
class __thonny_helper:
    import builtins
    try:
        import uos as os
    except builtins.ImportError:
        import os
    import sys
```

4. 将其修改为：

```python
class __thonny_helper:
    import builtins
    try:
        import uos as os
    except ImportError:
        import os
    try:
        import sys
    except ImportError:
        import usys as sys  # 直接导入 usys 模块
```

这段代码的作用是导入 `sys` 模块，并将其赋值给 `__thonny_helper.sys`。由于 QuecPython 未启用 `WEAK_LINKS` 特性，直接导入 `sys` 模块会失败。因此，我们修改代码，**直接导入 `usys` 模块**，从而确保 Thonny 能够找到正确的模块。

5. 找到 `_fetch_epoch_year()` 函数，它位于文件中的 400-420 行之间。
6. 在该函数的代码中，找到以下代码块：

```python
try:
    from time import localtime as __thonny_localtime
    __thonny_helper.print_mgmt_value(__thonny_helper.builtins.tuple(__thonny_localtime(%d)))
    del __thonny_localtime
except __thonny_helper.builtins.Exception as e:
    __thonny_helper.print_mgmt_value(__thonny_helper.builtins.str(e))
```

7. 将其修改为：

```python
try:
    try:
        from time import localtime as __thonny_localtime
    except ImportError:
        from utime import localtime as __thonny_localtime  # 直接导入 utime 模块
    __thonny_helper.print_mgmt_value(__thonny_helper.builtins.tuple(__thonny_localtime(%d)))
    del __thonny_localtime
except __thonny_helper.builtins.Exception as e:
    __thonny_helper.print_mgmt_value(__thonny_helper.builtins.str(e))
```

这段代码的作用是导入 `localtime` 函数。 在 QuecPython 中，`localtime` 函数位于 `utime` 模块中，而 Thonny 默认会尝试从 `time` 模块中导入该函数。 由于 QuecPython 未启用 `WEAK_LINKS` 特性，从 `time` 模块导入 `localtime` 函数会失败。 因此，我们修改代码，**直接从 `utime` 模块导入 `localtime` 函数**，从而确保 Thonny 能够找到正确的函数。

8. 保存修改后的 `mp_back.py` 文件。

### 修改 `bare_metal_backend.py` 文件

`bare_metal_backend.py` 文件是 Thonny 用于处理 MicroPython 设备底层通信的程序，我们也需要对它进行一些调整，使其能够正确处理 QuecPython 使用的模块名称。

1.  在 Thonny 的安装目录中，找到 `thonny/plugins/micropython/bare_metal_backend.py` 文件，并使用文本编辑器打开它。

2.  找到 `_read_file_via_serial()` 函数，它位于文件中的 1200 行左右。

3.  在该函数的代码中，找到以下代码行：

    ```python
    if hex_mode:
        self._execute_without_output("from binascii import hexlify as __temp_hexlify")
    ```

4.  将其修改为：

    ```python
    if hex_mode:
        self._execute_without_output("from ubinascii import hexlify as __temp_hexlify")  # 直接导入 ubinascii 模块
    ```

    这段代码的作用是在十六进制模式下导入 `hexlify` 函数。 在 QuecPython 中，`hexlify` 函数位于 `ubinascii` 模块中，而 Thonny 默认会尝试从 `binascii` 模块中导入该函数。 由于 QuecPython 未启用 `WEAK_LINKS` 特性，从 `binascii` 模块导入 `hexlify` 函数会失败。 因此，我们修改代码，**直接从 `ubinascii` 模块导入 `hexlify` 函数**，从而确保 Thonny 能够找到正确的函数。

5.  找到 `_write_file_via_serial()` 函数，它位于文件中的 1300-1400 行之间。

6.  在该函数的代码中，找到以下代码块：

    ```python
    from binascii import unhexlify as __thonny_unhex
    def __W(x):
        global __thonny_written
        __thonny_written += __thonny_fp.write(__thonny_unhex(x))
        __thonny_fp.flush()
        if __thonny_helper.builtins.hasattr(__thonny_helper.os, "sync"):
            __thonny_helper.os.sync()
    ```

7.  将其修改为：

    ```python
    try:
        from binascii import unhexlify as __thonny_unhex
    except ImportError:
        from ubinascii import unhexlify as __thonny_unhex  # 直接导入 ubinascii 模块
    def __W(x):
        global __thonny_written
        __thonny_written += __thonny_fp.write(__thonny_unhex(x))
        __thonny_fp.flush()
        if __thonny_helper.builtins.hasattr(__thonny_helper.os, "sync"):
            __thonny_helper.os.sync()
    ```

    这段代码的作用是导入 `unhexlify` 函数。 在 QuecPython 中，`unhexlify` 函数位于 `ubinascii` 模块中，而 Thonny 默认会尝试从 `binascii` 模块中导入该函数。 由于 QuecPython 未启用 `WEAK_LINKS` 特性，从 `binascii` 模块导入 `unhexlify` 函数会失败。 因此，我们修改代码，**直接从 `ubinascii` 模块导入 `unhexlify` 函数**，从而确保 Thonny 能够找到正确的函数。

8.  保存修改后的 `bare_metal_backend.py` 文件。

{{< callout type="info" >}}

在使用 BC25 模块进行开发时，您还需要修改 `bare_metal_backend.py` 文件中的串口波特率。 将文件 50-60 行附近的 `BAUDRATE = 115200` 修改为 `BAUDRATE = 57600`。 这是因为 BC25 模块默认使用 57600 的波特率进行通信。

{{< /callout >}}

## 连接设备：让 Thonny 与您的模块“对话”

完成以上源码修改后，关闭并重新启动 Thonny。 现在，您可以将 Thonny 连接到您的 QuecPython 模块了。

1. 在 Thonny 主界面顶部的菜单栏中，选择 **工具 -> 设置...** ，打开 Thonny 设置窗口。
2. 切换到 **解释器** 选项卡，在上方的解释器列表中选择 **MicroPython(一般)**。
3. 在下方的端口列表中选择 QuecPython 的交互串口。例如，对于 EC600N 模块，选择 `Quectel USB MI05 COM Port`。
4. 点击 **确定** 按钮。

Thonny 会自动尝试和 QuecPython 设备建立连接。 当连接成功后，Thonny 主界面下方的 Shell 窗口内会出现类似以下内容的输出：

```
  ____                         __  __
 / __ \__ _____ _______  __ __/ /_/ /  ___  ___
/ /_/ / // / -_) __/ _ \/ // / __/ _ \/ _ \/ _ \
\___\_\_,_/\__/\__/ .__/\_, /\__/_//_/\___/_//_/
                 /_/   /___/

Quecpython v1.12 on Thu_Jan_13_2022_4:46:28_PM ; EC600N with QUECTEL

 Type "help()" for more information.
>>>
```

## 开始开发：体验 Thonny 的强大功能

连接成功后，您就可以像开发一般的 MicroPython 设备一样，使用 Thonny 对 QuecPython 设备进行开发了。 您可以使用 Thonny 的代码编辑器编写 Python 代码，使用文件上传功能将代码上传到模块，并使用 REPL 进行交互式调试。

### 已知问题

尽管经过源码修改后，Thonny 的大部分功能可以正常使用，但仍然存在一些已知问题：

- **变量查看**: 点击 **变量** 窗格内的变量项目时，后台可能会发生错误并弹出报错窗口，但这不影响主程序的运行。 这是因为 QuecPython 返回的变量 ID 可能与 Thonny 的预期不符，导致 Thonny 无法正确解析变量信息。
- **中断执行**: 在用户脚本运行时，中断执行 (`Ctrl+C`)、软重启 (`Ctrl+D`) 和重启后端进程 (`Ctrl+F2`) 功能可能会失灵。这通常出现在脚本中包含死循环，且循环中未设计任何跳出机制的情况下。 此时，您需要重启硬件或重新烧录固件才能解决问题。

{{< callout type="info" >}}

如果以上问题对您的开发造成不便，建议您使用 QuecPython 官方的 QPYcom 工具进行开发。

{{< /callout >}}

## 注意事项

在使用 Thonny 开发 QuecPython 应用时，还需要注意以下事项：

- **时间同步**: Thonny 在连接设备时会自动将设备端的时间与上位机 (电脑) 时间进行同步。这可能会导致部分与日期和时间相关的用户脚本在运行时出现意料之外的结果。 如果您的应用需要使用精确的时间，建议您在代码中手动设置模块的时间，或使用 NTP 进行网络时间同步。
- **REPL 界面无响应**: 在使用 Thonny 连接过 QuecPython 设备后，如果再使用其他串口工具连接设备，可能会出现 REPL 界面无响应的情况。 此时，您需要手动重置设备（按下开发板上的 Reset 按钮或重启开发板），或是按下 `Ctrl+B` 键（串口发送 `0x02` 控制字符）切换到普通模式，方可进行交互。
- **根目录写入**: 由于文件系统权限的限制，您无法向 QuecPython 设备的文件系统根目录 `/` 写入文件。 因此，在使用 Thonny 将脚本保存至设备时，您需要手动将存储路径修改为 `/usr` 分区下。

## 总结

本文介绍了如何修改 Thonny 源码，使其支持 QuecPython 开发，并提供了一些使用 Thonny 进行 QuecPython 开发的技巧和注意事项。希望这些内容能够帮助您使用 Thonny 工具顺利进行 QuecPython 开发，体验跨平台开发的自由与便捷。
