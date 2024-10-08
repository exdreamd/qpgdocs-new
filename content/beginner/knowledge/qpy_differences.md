---
title: QuecPython 与传统开发方式的差异
date: 2024-07-23T09:50:12+08:00
weight: 26
---

很多 QuecPython 的初学者此前曾有过各类软硬件平台的开发经验，例如基于 C 语言开发普通单片机、基于 AT 指令开发模块、基于 MicroPython 开发 ESP32、在电脑端开发标准 CPython 等。然而，QuecPython 作为一种全新的物联网开发方式，与这些传统的开发方式存在着显著的差异。

这篇文章旨在帮助那些具备其他平台开发经验的用户快速了解 QuecPython 开发的特点和注意事项，避免他们被过往经验所误导，从而更顺利地迁移到 QuecPython 开发上来。在正式上手开发前，充分理解这些差异至关重要。

## 和标准 AT 开发的差异

如果您此前主要使用 AT 指令进行模块开发，那么在使用 QuecPython 时，您会发现两者是截然不同的开发模式。**QuecPython 并不是在传统 AT 开发的基础上新增的功能，两者可视作两种相互独立的开发方式，是“二选一”而非共存的关系**。

- **开发理念**: 传统的 AT 指令开发侧重于通过简单的指令控制模块的基本功能，例如发送短信、拨打电话、获取 GPS 数据等。开发者需要了解每个 AT 指令的语法和参数，以及模块的响应格式，并通过串口发送指令和接收响应。而 QuecPython 是一种基于脚本语言的二次开发 (OpenCPU) 方式，它直接运行在模块的操作系统之上，允许开发者使用 Python 语言调用模块提供的各种功能库，实现更加复杂和灵活的应用，例如数据采集、远程监控、边缘计算等。开发者可以使用 Python 语言编写完整的程序逻辑，并通过 QuecPython 提供的 API 访问模块的硬件和软件资源。
- **固件**: QuecPython 需要烧录专门的固件，该固件包含 MicroPython 解释器和 QuecPython 特有的功能库。烧录 QuecPython 固件后，模块的 AT 指令功能将受到限制，大部分功能性 AT 指令会失效或返回异常结果。这是因为 QuecPython 固件修改了模块的底层配置，一些 AT 指令不再适用，而另一些 AT 指令的功能可以通过 QuecPython 的 API 实现。因此，在使用 QuecPython 开发时，**请尽可能避免使用 AT 指令控制模块**，否则可能导致 QuecPython 程序运行异常。
- **恢复 AT 功能**: 如果您需要恢复模块的 AT 指令功能，可以重新烧录标准 AT 固件。一般可以通过 QFlash 等工具烧录。
- **资源占用**: QuecPython 固件占用的模块资源 (例如 Flash 和 RAM) 比标准 AT 固件更多，因此在选择模块型号和开发应用时，需要注意资源的限制。**同一款模块在烧录 QuecPython 固件后，所具备的功能通常会少于标准 AT 固件**。例如，如果您的应用需要 VoLTE、短信等高级功能，建议选择 Flash 和 RAM 容量更大的模块型号。

## 和 QuecOpen (CSDK) 开发的差异

QuecPython 和 QuecOpen (CSDK) 都是移远模块提供的二次开发方式，但两者在开发语言、开发效率、性能、资源和灵活性、外设接口等方面存在显著差异。

- **开发语言和环境**: QuecPython 使用 Python 脚本语言进行开发，语法简单易学，无需配置复杂的编译和烧录环境。开发者可以使用任何文本编辑器编写代码，例如记事本、VS Code、PyCharm 等。QuecOpen 使用 C 语言进行开发，需要开发者熟悉嵌入式开发流程，并使用 Arm Development Studio 等 IDE 进行代码编写、编译和调试。
- **开发效率**: QuecPython 的开发效率通常高于 QuecOpen。QuecPython 无需编译和烧录固件，修改代码后可以立即运行，开发周期更短。QuecOpen 需要进行固件重编译和烧录操作，开发周期较长。
- **性能**: QuecPython 的代码需要在 MicroPython 解释器中运行，而 QuecOpen 的代码直接编译成机器码运行。因此，QuecPython 的执行效率通常低于 QuecOpen，**不适用于对实时性要求极高的应用**。例如，在进行高速数据采集、精确控制电机等应用时，QuecOpen 的性能优势更加明显。
- **资源和灵活性**: QuecOpen 提供了对模块硬件和软件资源的更底层访问权限，开发者可以更加灵活地控制模块的功能，例如直接访问硬件寄存器、编写中断处理程序等，但同时也需要开发者承担更多的底层管理责任，例如内存管理、中断处理等。QuecPython 屏蔽了很多底层细节，开发者只能使用 QuecPython 提供的 API 来控制模块，虽然易用性提高了，但灵活性有所降低。例如，开发者无法直接访问模块的某些硬件寄存器，也无法自定义中断处理程序。对于一些需要精细化控制硬件或进行底层优化的应用，QuecOpen 可能更合适。
- **外设接口**: QuecPython 和 QuecOpen 在外设接口的数量、编号、功能定义、初始电平等方面可能存在差异。在使用 QuecPython 开发时，**请务必参考 QuecPython 的官方文档，不要直接使用 QuecOpen 的资料**，避免因为信息不一致导致程序错误。例如，某些 GPIO 引脚在 QuecOpen 中默认是高电平，但在 QuecPython 中默认是低电平，如果开发者没有注意到这一点，可能会导致程序出现意外行为。此外，**某些外设的功能在 QuecPython 中可能被简化或限制**。

## 和传统单片机开发的差异

如果您此前主要使用 C 语言进行裸机单片机（如 STM32）开发，那么在学习 QuecPython 时需要特别注意以下一些差异：

### 原理和特性差异

- **操作系统**: QuecPython 并非裸机开发，而是基于实时操作系统 (RTOS)。这意味着开发者无需关心底层的硬件初始化和配置，例如时钟配置、中断向量表设置等，QuecPython 已经处理好了这些细节。开发者可以直接使用 Python 语言编写应用程序，专注于应用逻辑的开发。例如，在使用 QuecPython 控制 GPIO 引脚时，无需手动逐项配置 GPIO 的模式、上下拉电阻等，只需调用 `Pin` 对象的 `write()` 方法即可控制 LED 灯的亮灭。
- **抽象层次**: QuecPython 屏蔽了很多底层硬件细节，开发者无需直接操作寄存器、配置中断、管理内存等。QuecPython 提供了丰富的 API 函数，开发者可以直接调用这些函数来控制模块的功能，例如控制 GPIO 引脚、连接网络等。在传统的单片机开发中，开发者需要直接操作寄存器来控制外设，例如配置 GPIO 的模式、设置定时器参数等。而在 QuecPython 中，这些操作都被封装成 API 函数，开发者只需调用相应的函数即可，无需了解底层的寄存器操作。
- **执行速度**: 由于 QuecPython 采用解释执行的方式，其执行速度比编译型语言 (例如 C 语言) 慢。这是因为脚本语言的代码需要在运行时被固件内置的解释器逐行解释成机器码，而编译型语言的代码在固件编译阶段就已经被转换成机器码，可以直接被 CPU 执行。因此，QuecPython 不适用于对实时性要求极高的应用场景，例如高速数据采集、电机控制、单总线传感器连接等。例如，在需要精确控制电机转速的应用中，使用 C 语言编写的代码可以直接控制 PWM 信号的占空比，而使用 QuecPython 编写的代码则需要通过解释器执行，可能会导致一定的延迟。
- **内存管理**: 在传统的单片机开发中，开发者需要手动管理内存，例如使用 `malloc()` 和 `free()` 函数分配和释放内存空间。而 QuecPython 具有自动垃圾回收机制。不过，开发者仍然需要注意内存的使用情况，避免内存溢出。例如，避免创建过大的列表或字典，及时释放不再使用的变量，并主动调用垃圾回收机制来释放内存。开发者可以使用 `gc` 模块来查看内存使用情况和进行垃圾回收。

### 开发流程差异

与传统的单片机开发流程相比，QuecPython 的开发流程更加简化，更接近于电脑端 Python 开发或手机 App 开发。

- **环境搭建**: QuecPython 作为脚本语言开发方式，无需安装复杂的 SDK 和工具链。开发者只需下载 QuecPython 固件，并将其烧录到移远模块中即可。开发环境的搭建非常简单，通常只需要一台电脑和一根 USB 线缆。与之相比，传统的单片机开发需要安装编译器、烧录器、调试器等工具，并配置相应的开发环境，例如设置编译选项、烧录参数等。
- **代码编写**: QuecPython 代码可以使用任何文本编辑器编写，例如记事本、VS Code、PyCharm 等。开发者无需使用专门的 IDE，也无需进行复杂的项目配置。而传统的单片机开发通常需要使用专门的 IDE，例如 Keil、IAR 等，并创建工程、添加源文件、配置编译选项等。
- **程序下载**: 编写好的 QuecPython 代码可以直接通过 USB 线缆复制到模块的内部存储器中。无需使用烧录器或调试器，也无需进行编译和链接等操作。代码的下载过程类似于将文件复制到 U 盘。传统的单片机开发需要使用烧录器将编译好的程序烧录到单片机的 Flash 存储器中，这个过程通常需要连接调试器，并设置烧录参数。
- **调试运行**: QuecPython 支持 REPL (Read-Eval-Print Loop) 模式，开发者可以通过串口或 USB 连接到模块，实时输入 Python 代码并查看执行结果，方便进行代码调试和功能验证。此外，开发者也可以使用 `print()` 函数输出调试信息。**需要注意的是，QuecPython 不具备传统单片机开发中常用的断点、单步运行、内存分析等调试手段，这些调试方法在 QuecPython 中并不适用。**
- **程序管理**: QuecPython 允许在模块中存储多个 Python 脚本文件，每个脚本文件可以看作是一个独立的应用程序。开发者可以根据需要选择运行不同的脚本文件，就像在手机上运行不同的 App 一样。这与传统的单片机开发不同，传统的单片机开发通常只能运行一个程序。

## 和电脑端 Python 的差异

如果您此前有 Python 开发经验，那么在使用 QuecPython 时，需要注意它与标准 CPython 之间存在一些差异，这些差异主要源于 MicroPython 对资源受限的嵌入式环境的适配。

- **语法**: QuecPython 基于 MicroPython，它与 Python 3.4 在核心语法上保持兼容，但由于资源限制和嵌入式开发的特殊性，一些高级语法特性在 QuecPython 中被简化或移除。例如，QuecPython 不支持列表推导式、生成器表达式等语法特性。
- **标准库**: QuecPython 只包含部分 Python 标准库，并且某些库的功能有所简化或调整。例如，`ujson` 库只提供了 `dump()`、`dumps()`、`load()`、`loads()` 四个函数，而 CPython 的 `json` 库提供了更多功能。此外，一些常用的标准库模块在 QuecPython 中被移除，例如 `datetime` 模块。因为 MicroPython 的原作者认为用户可以基于 `utime` 库提供的日期和时间信息，自行拼凑和格式化，完成输出，因此不再内置类似 `datetime` 库的功能。
- **第三方库**: QuecPython 不支持使用 `pip` 在线安装第三方库。开发者需要手动将所需的第三方库代码下载到模块中，并进行必要的修改和适配。 此外，许多电脑端 Python 开发中常用的功能较为复杂的库，如 OpenCV、PyQt、Flask、NumPy、Scrapy 等，都不支持在 QuecPython 环境中直接运行。这是因为这些库通常依赖于大量的外部资源或操作系统功能，而 QuecPython 运行的环境资源有限，无法满足这些库的运行需求。
- **开发工具**: 由于语法和功能库的差异，电脑端常用的 Python 开发工具 (例如 VSCode、PyCharm) 在 QuecPython 开发中只能用于代码编辑，不能用于调试和运行。开发者需要将代码上传到模块中（通常使用移远官方提供的 QPYcom 工具完成），并使用 REPL 模式或 `print()` 函数进行调试。

## 和 MicroPython 的差异

QuecPython 本质上是运行于移远通信模块之上的 MicroPython。然而，由于 MicroPython 缺少完善的统一规范，根据开发者和硬件平台的不同，其内置的功能 (库) 数量和各类功能函数的用法均可能存在差异。部分 QuecPython 用户此前曾有过使用 MicroPython 开发 ESP32、ESP8266、STM32 等芯片的经验。为了便于这类用户完成平台迁移，需要了解 QuecPython 与其他 MicroPython 平台的差异。

- **功能库**: 部分 MicroPython 的标准库或专用库，例如 `framebuf`、`network` 等，在 QuecPython 中未实现或未内置。一些标准库的实现方式和功能完整度也可能存在差异，例如 `utime` 库。这主要是由于不同模块的硬件资源和功能不同，导致 MicroPython 的移植和实现有所区别。
- **硬件接口**: 涉及具体硬件接口的 API，例如 UART、I2C、SPI 等，在 MicroPython 和 QuecPython 中存在较大差异，不能直接混用。例如，在 ESP32 上使用 MicroPython 控制 I2C 设备的代码可能无法直接在 QuecPython 上运行，需要根据 QuecPython 的 API 进行修改。
- **第三方库**: QuecPython 目前尚未内置 `upip` 功能，无法实现功能库的快速在线安装，只能手动移植。这与其他 MicroPython 平台有所不同，一些 MicroPython 平台（如 ESP32）支持使用 `upip` 工具在线安装第三方库。
- **开发工具**: QuecPython 不保证与 Thonny、uPyCraft 等 MicroPython 上位机工具之间的兼容性。这主要是因为 QuecPython 的功能库和 API 与其他 MicroPython 平台存在差异，这些上位机工具可能无法识别或支持 QuecPython 特有的功能库。

因此，即使您熟悉 MicroPython 开发，在使用 QuecPython 时也需要仔细阅读官方文档，了解其功能库和 API 的具体使用方法，并进行必要的代码修改和适配。

## 总结

QuecPython 作为一种新兴的物联网开发方式，为开发者提供了更便捷的开发体验。然而，由于 QuecPython 与其他开发方式存在显著差异，开发者需要在开发过程中关注这些差异，并及时调整自己的开发思路和习惯，避免被过往经验误导。

希望这篇文章能够帮助您更好地理解 QuecPython 开发的特点和注意事项，顺利过渡到 QuecPython 开发，并充分利用 QuecPython 的优势，快速构建高效、稳定的物联网应用。
