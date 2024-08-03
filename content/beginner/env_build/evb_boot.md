---
title: 开发板上电和连接
date: 2024-07-27T18:29:35+08:00
draft: false
weight: 43
prev: /beginner/env_build/install_tools
next: /beginner/env_build/choose_fw
---

在开始 QuecPython 固件烧录之前，我们需要对模块进行上电和开机操作，确保其能够正常运行。同时，我们需要使用 USB 线缆将模块与电脑连接，并确认电脑能够正常识别模块并与之通信。只有完成这些准备工作，才能进行后续的固件烧录和开发步骤。

{{< callout type="info" >}}

**提示**

本文仅针对几种常见开发板，介绍其上电、开机和连接电脑的相关步骤。对于用户自行设计的电路板，请查阅相应型号的《硬件设计手册》和《参考设计手册》，了解电源和 USB 部分的设计要求和相关功能的使用方法。在开始烧录固件前，请务必确认自行设计的电路板可正常运行，且模块可被电脑端正常识别。其他细节本文不做赘述。

{{< /callout >}}

## 准备工作：降低功耗，规避风险

在上电之前，我们需要进行一些必要的准备工作，以降低开发板的功耗，避免因供电不足导致的潜在问题，例如开机失败、功能异常或器件损坏。

- **断开外接部件**: 请断开与开发板相连接的各类外部配件，例如 LCD 屏、摄像头、传感器、LED 灯等。 这些外设会消耗额外的电流，如果在上电时连接这些外设，可能会导致模块无法正常启动，或者出现其他异常情况。
- **取出 SIM 卡和天线**: 插入 SIM 卡也会显著增加开发板的功耗，因为模块需要搜索并连接到蜂窝网络，这个过程会消耗大量的电流。 强烈建议您在上电前从开发板中取出 SIM 卡并断开各天线的连接。当 QuecPython 固件烧录完成后，需要测试物联网通信相关功能时，再安装 SIM 卡和天线。

{{< callout type="error" >}}

**警告**

为避免器件损坏，所有外接部件的安装和断开操作，请务必在断电状态下完成。

{{< /callout >}}

## 上电与开机：唤醒您的模块

不同型号的开发板，接入供电和开机的方式存在一定差异。请根据您使用的开发板型号，参考以下说明进行操作。

### EC600X / EC800X QuecPython EVB

EC600X / EC800X QuecPython EVB 开发板支持两种供电方式：USB 供电和外部直流电源供电。

#### USB 供电

1.  将板载的电源输入选择开关（K1，位于 USB Type-C 接口附近）拨至 USB 一侧。
2.  使用质量可靠的 USB Type-C 数据线连接开发板与电脑的 USB **3.0** 接口（USB 2.0 接口的电流输出能力较低 ，无法满足模块的启动需求）。
3.  等待板载的 POW 灯（红色灯）亮起，表示开发板已经成功上电。
4.  长按 PWK 按钮（S1，位于开发板左下角） **2 秒以上** 使模块开机。
5.  等待板载的 NET 灯（蓝色或绿色灯）开始正常闪烁，表示模块内部的操作系统已经正常启动。

![](/images/qpy_evb_poweron.png "电源输入选择开关和 PWK 按钮在 EC600X / EC800X QuecPython EVB 上的位置")

{{< callout type="warning" >}}

**注意**

- **USB 3.0 接口**: 若用户使用的是台式计算机，请将开发板连接到机箱后部的 USB 3.0 接口。USB 2.0 接口通常无法满足开发板的供电需求，可能会导致模块无法正常工作。
- **高质量数据线**: 请确保使用高质量的数据线。部分劣质数据线可能导致供电不足或通信异常。
- **避免使用 USB Hub**: 在大部分情况下，请勿使用 USB Hub（集线器或扩展坞）。若因电脑端接口数量不足，必须使用 USB Hub，请选择支持外接供电的 Hub 型号，并在将开发板连接到 Hub 之前，预先将 Hub 的外接供电端连接到最大输出功率不低于 10 W 的外部直流电源（如手机充电器）。

{{< /callout >}}

#### 外部直流电源供电

1.  将板载的电源输入选择开关（K1，位于 USB Type-C 接口附近）拨至 DC 一侧。
2.  使用最大输出功率不低于 10 W 的外部直流电源 (例如，手机充电器)，连接开发板的外部供电排针（J2，位于 USB Type-C 接口附近），为开发板提供 **5V** 直流供电。
3.  等待板载的 POW 灯（红色灯）亮起，表示开发板已经成功上电。
4.  长按 PWK 按钮（S1，位于开发板左下角） **2 秒以上** 使模块开机。
5.  等待板载的 NET 灯（蓝色或绿色灯）开始正常闪烁，表示模块内部的操作系统已经正常启动。
6.  使用质量可靠的 USB Type-C 数据线连接开发板与电脑的 USB 接口。

### EC600X / EC800X 核心板

EC600X / EC800X 核心板通常需要使用外部直流电源供电。

1.  使用最大输出功率不低于 10 W 的外部直流电源，通过 VIN 和 GND 排针向核心板提供 5 V 直流供电。
2.  等待板载的 NET 灯（蓝色或绿色灯）开始正常闪烁，表示模块内部的操作系统已经正常启动。
3.  使用质量可靠的 USB 数据线连接开发板与电脑的 USB 接口。

{{< callout type="warning" >}}

**注意**

- **避免使用 USB 供电**: 直接使用电脑 USB 接口进行供电可能会因供电不足导致核心板无法正常工作，具体请参考相应型号的核心板的用户手册。如用户执意使用 USB 对核心板进行供电，请务必连接至电脑的 USB 3.0 接口，且避免使用 USB Hub。
- **高质量电源**: 建议用户选用高质量的直流电源，供电纹波过大可能导致模块工作异常。
- **高质量数据线**: 请确保使用高质量的数据线。部分劣质数据线可能导致供电不足或通信异常。
- **电压选择**: 核心板通常支持宽电压供电。但在开发阶段，不建议向核心板提供高于 5V 的外部直流供电，否则可能导致 USB 连接和识别异常。

{{< /callout >}}

### DPYOS DTU 开发板

DPYOS DTU 开发板可以使用 USB 供电，也可以使用外部直流电源供电。

#### USB 供电

1.  使用质量可靠的 Micro USB 数据线连接开发板与电脑的 USB **3.0** 接口。
2.  等待板载的 NET 灯（蓝色或绿色灯）开始正常闪烁，表示模块内部的操作系统已经正常启动。

{{< callout type="warning" >}}

**注意**

- **USB 3.0 接口**: 若用户使用的是台式计算机，请将开发板连接到机箱后部的 USB 3.0 接口。USB 2.0 接口通常无法满足开发板的供电需求，可能会导致模块无法正常工作。
- **高质量数据线**: 请确保使用高质量的数据线。部分劣质数据线可能导致供电不足或通信异常。
- **避免使用 USB Hub**: 在大部分情况下，请勿使用 USB Hub（集线器或扩展坞）。若因电脑端接口数量不足，必须使用 USB Hub，请选择支持外接供电的 Hub 型号，并在将开发板连接到 Hub 之前，预先将 Hub 的外接供电端连接到最大输出功率不低于 10 W 的外部直流电源（如手机充电器）。

{{< /callout >}}

#### 外部直流电源供电

1.  使用最大输出功率不低于 10 W 的外部直流电源，连接开发板的 VCC 和 GND 接线端子，为开发板提供 5 V 直流供电。
2.  等待板载的 NET 灯（蓝色或绿色灯）开始正常闪烁，表示模块内部的操作系统已经正常启动。
3.  使用质量可靠的 Micro USB 数据线连接开发板与电脑的 USB 接口。

{{< callout type="warning" >}}

**注意**

- **电压选择**: DPYOS DTU 开发板通常支持宽电压供电。但在开发阶段，不建议向开发板提供高于 5V 的外部直流供电，否则可能导致 USB 连接和识别异常。

{{< /callout >}}

## 检查串口：确认连接状态

完成上电和开机操作后，我们需要确认电脑是否能够正确识别模块。 这是因为模块通过 USB 接口连接到电脑后，会虚拟出多个串口，用于不同的功能。只有在电脑正确识别了这些虚拟串口后，我们才能进行后续的开发工作。

{{% steps %}}

### 打开设备管理器

右击 **开始** 按钮，在弹出菜单中选择 **设备管理器** ，展开 **端口（COM 和 LPT）** 分类。

![](/images/check_com_port.png "检查串口是否正常识别")

### 检查串口名称

在模块成功开机，且电脑端驱动程序安装无误的情况下，您将在 **端口（COM 和 LPT）** 类别下看到多个名称以 `Quectel` 开头的串口，如下图所示。

{{< figure src="/images/ec600u_ports.png" caption="EC600U 模块（标准固件）连接电脑时出现的串口" width=50% >}}

{{< figure src="/images/ec600n_ports.png" caption="EC600N 模块（标准固件）连接电脑时出现的串口" width=50% >}}

{{% /steps %}}

{{< callout type="info" >}}

**常见问题**

- **串口数量和名称不一样？**: 不同型号的模块，在连接电脑时，显示的串口数量和名称会存在差异，请以实际情况为准。
- **存在未识别设备？**: 如果出现 `Mobile ECM Network Adapter` 或 `CDC Ethernet Control Modle (ECM)` 等网卡设备未被识别，属于正常现象，不影响固件烧录和后续开发。
- **找不到 Python 交互串口？**: 部分型号的交互串口（例如 EC600N 的 `Quectel USB MI05 COM Port`）只有在烧录完 QuecPython 或 QuecOpen 固件后才会出现。如果您的模块出厂时带有标准 AT 固件，那么在连接电脑时可能不会出现该串口，这属于正常现象。
- **已经内置了 QuecPython 固件？**: 设备管理器中交互串口的有无**不能**作为判断模块是否曾经烧录过 QuecPython 固件的标志。建议新用户一律按照后续文章所述的步骤重新为模块烧录最新版固件。

{{< /callout >}}

{{< callout type="info" >}}

**串口异常情况排查指南**

| 异常表现                                          | 排查思路                                                                                                                                                                                                                     |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 设备管理器中出现多个未识别的设备                  | **驱动安装失败或未生效**：尝试重启电脑、重新安装驱动，或更换另一台电脑进行操作。                                                                                                                                             |
| 设备管理器中未出现任何新增串口                    | **驱动安装失败或未生效**：尝试重启电脑、重新安装驱动，或更换另一台电脑进行操作。<br />**模块未成功开机**：根据前文检查供电要求是否满足、开机步骤是否正确。<br />**模块与电脑间接触不良**：更换 USB 数据线和 USB 端口后重试。 |
| 设备管理器中出现多个串口，但名称不带 Quectel 字样 | **电脑此前曾经安装过其他品牌或其他型号的模块的驱动程序**：手动卸载原有驱动、重装移远官方驱动，或更换另一台电脑进行操作。                                                                                                     |
| 设备管理器中出现多个串口，但片刻后即消失          | **模块供电不足**：根据前文检查供电要求是否满足，SIM 卡和天线是否取下；如有必要，使用外部直流电源进行供电。                                                                                                                   |

若出现其他异常情况，请在 QQ 交流群中咨询技术支持人员。

{{< /callout >}}

## 尝试通信：验证模块运行

为了确保模块与电脑之间的通信正常，我们可以使用 QPYcom 工具进行简单的 AT 指令测试。

1.  打开 QPYcom 工具，在 **选择串口** 的下拉列表中选中 `Quectel USB AT Port`，波特率选择 115200，点击 **打开串口**。
2.  在交互栏内输入 `AT` 指令，然后按回车键（Enter）进行发送。如果模块能够正常响应，您将在 QPYcom 的接收窗口看到 `OK` 的回复，这说明模块已经成功开机，并且与电脑之间的通信正常。
3.  接下来，您可以发送 `ATI` 指令，查询模块的版本信息。如果模块能够正常响应，您将在 QPYcom 的接收窗口看到模块的软件版本（Revision）信息。

![](/images/at_test.png "使用 QPYcom 工具进行 AT 通信测试")