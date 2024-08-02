---
title: 在常用 Linux 系统上连接 QuecPython 设备
date: 2024-08-02T16:08:16+08:00
draft: false
---

QuecPython 为开发者提供了在移远通信模块上使用 Python 语言进行嵌入式开发的便捷方式。尽管目前 QuecPython 的固件烧录和一些高级功能（例如固件合并、量产等）只能在 Windows 操作系统上进行，但日常的程序开发和调试工作主要依赖于串口指令交互，因此不受操作系统限制。这意味着您可以在支持 USB CDC 串口的大部分操作系统，如 macOS 和 Linux 中，连接 QuecPython 开发板（或模块），进行代码编写、上传、执行和调试等操作。

本文将重点介绍如何在常用的桌面和服务器级 Linux 操作系统中连接 QuecPython 开发板（或模块），并建立串口通信，从而让您在熟悉的 Linux 环境中自由地进行 QuecPython 开发。

## 准备工作：确认兼容性

为了确保您能够顺利地在 Linux 系统上连接 QuecPython 设备，请您首先确认以下几点：

### 模块和固件兼容性

本文所述的相关操作流程和技术细节已在以下模块和固件版本上进行过验证：

| 模块型号   | 固件版本                      |
| ---------- | ----------------------------- |
| EC600NCNLA | QPY_OCPU_V0006_EC600N_CNLA_FW |
| EC600NCNLC | QPY_OCPU_V0006_EC600N_CNLC_FW |
| EC600UCNLB | QPY_V0007_EC600U_CNLB_FW      |
| EC600UCNLC | QPY_V0005_EC600U_CNLC_FW      |
| ...        | ...                           |

请确保您所使用的模块和固件版本在兼容性列表中。如果您的模块或固件版本不在列表中，您可以在 QuecPython 官网或开发者论坛查询相关信息，或联系移远技术支持人员确认兼容性。

### 操作系统兼容性

本文所述的操作流程和指令主要基于 Fedora 35 Workstation 操作系统。以下操作系统版本也已经过验证，可以作为参考：

| 系统名称        | 版本            | 架构  |
| --------------- | --------------- | ----- |
| Ubuntu          | 20.04.3 Desktop | AMD64 |
| Ubuntu          | 20.04.3 Server  | AMD64 |
| Ubuntu          | 22.04 Desktop   | AMD64 |
| Ubuntu          | 22.04 Server    | ARM64 |
| Fedora          | 36 Workstation  | AMD64 |
| Fedora          | 36 Server       | AMD64 |
| CentOS          | 7.9.2009        | AMD64 |
| CentOS          | 8.5.2111        | AMD64 |
| Raspberry Pi OS | 10 (Buster)     | ARM64 |
| ...             | ...             | ...   |

{{< callout type="info" >}}

**提示**

从 22.04 版开始，Ubuntu 操作系统会自动加载基于 ASR 芯片的产品 (EC600S、EC600N、EC800N、EC600M、EC800M、EG810M 等模块) 的驱动程序。当您将已经烧录有 QuecPython 固件的上述模块连接到电脑时，会自动出现 `ttyACM0` 串口，您可以直接连接到该串口，开始 Python 交互，无需其他操作。

{{< /callout >}}

## 连接模块：建立通信桥梁

完成准备工作后，就可以开始连接模块了。

{{% steps %}}

### 连接硬件

使用 USB 线缆将模块连接到您的 Linux 计算机，并确保模块已正常上电、开机和运行。您可以通过观察模块上的电源指示灯或网络指示灯来确认模块是否正常运行。

### 查找 USB 设备

在系统终端中执行 `lsusb` 命令，列出所有连接的 USB 设备。在返回的内容中，您可以找到 Quectel 设备及其 USB ID。

```bash
$ lsusb
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 005: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
Bus 003 Device 004: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
Bus 003 Device 003: ID 2c7c:0901 Quectel Wireless Solutions Co., Ltd. Android
Bus 003 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
```

通常情况下，EC600N 模块的 USB ID 为 `2c7c 6001`，EC600U 模块的 USB ID 为 `2c7c 0901`。请以 `lsusb` 命令的实际返回值为准。

{{< callout type="warning" >}}

如果在 `lsusb` 命令的输出中没有找到 Quectel 设备，请检查线缆连接和模块运行情况，或更换计算机再试。

{{< /callout >}}

### 添加 USB 串口设备

大多数 Linux 发行版默认不会自动创建 QuecPython 模块的虚拟串口设备。您需要使用以下命令手动添加：

```bash
$ sudo modprobe option
$ sudo sh -c 'echo "2c7c 0901" > /sys/bus/usb-serial/drivers/option1/new_id'
# 请根据您模块的实际 USB ID 修改以上命令中的 ID
```

执行以上命令后，系统会自动创建模块的虚拟串口设备。

### 确认串口设备

执行 `ls /dev/ttyUSB*` 命令，列出所有 USB 串口设备。您应该能够看到多个以 `ttyUSB` 开头的设备文件，这些就是模块的虚拟串口。

```bash
$ ls /dev/ttyUSB*
/dev/ttyUSB0  /dev/ttyUSB2  /dev/ttyUSB4  /dev/ttyUSB6
/dev/ttyUSB1  /dev/ttyUSB3  /dev/ttyUSB5
```

通常情况下，EC600U 模块会新增 6 至 7 个端口，EC600N 模块会新增 3 个端口。

{{< callout type="info" >}}

一般而言，在没有接入其他 USB 串口设备的情况下，EC600N 模块的 USB AT 口为 `ttyUSB1`，Python 交互口为 `ttyACM0`；EC600U 模块的 USB AT 口为 `ttyUSB0`，Python 交互口为 `ttyUSB6`。请以实际情况为准。

{{< /callout >}}

{{% /steps %}}

## 开始 QuecPython 开发

完成以上步骤后，您就可以使用串口工具连接到模块的 REPL 串口，开始 QuecPython 开发了。

{{< callout type="warning" >}}

**注意**

- 新版的 QuecPython 固件在编译时未开启 MicroPython 的 `WEAK_LINKS` 特性，这会导致一些 MicroPython 上位机工具无法直接与烧录有 QuecPython 固件的模块进行通信。建议您改用 `cutecom` 、 `picocom` 等通用的串口工具。
- 使用虚拟机可能会降低串口通信的稳定性，或者导致串口间歇性失灵，因此不建议在虚拟机中进行 QuecPython 开发。
- 操作系统重启后，上述涉及 USB ID 的改动会失效。必要情况下，您可以将相关命令添加到开机启动脚本中，例如 `~/.bashrc` 文件。
- 部分操作系统可能会自动将模块识别为 4G 网卡，并尝试使用它进行网络连接。建议您手动检查并及时禁用该网卡，以避免无谓的流量消耗。
- 对于部分旧版系统或嵌入式系统，如果内核中没有包含 `option` 驱动，您可以参考 [官方教程](https://github.com/QuecPython/Quectel_Linux_USB_Serial_Option_Driver) 自行编译驱动程序。

{{< /callout >}}

## 总结

在 Linux 系统上连接 QuecPython 设备虽然需要一些额外的步骤，但这可以让您在熟悉的 Linux 环境中进行开发，并享受 Linux 系统提供的强大功能和灵活性。

希望这篇指南能够帮助您顺利地在 Linux 系统上连接 QuecPython 设备，开启您的 QuecPython 跨平台开发之旅！
