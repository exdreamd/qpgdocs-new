---
title: QuecPython 简介
date: 2024-07-23T09:46:59+08:00
weight: 25
---

## 从 Python 到 MicroPython

Python 是一个高层次的结合了解释性、编译性、互动性和面向对象的脚本语言。作为一种代表极简主义的编程语言，和传统的 C/C++、Java、C# 等语言相比，Python 对代码格式的要求相对宽松。在开发 Python 程序时，可以专注于解决问题本身，而不用顾虑语法的细枝末节。

以下两个例子对比了 Python 语言和 C 语言对同一功能的实现方式的区别。可以看出，Python 语法较为简洁，易于掌握，可读性较高。

{{< tabs items="使用 C 语言, 使用 Python 语言" >}}

{{< tab >}}

```c {linenos=table,filename="读取并显示键盘输入"}
#include<stdio.h>

int main(){
    int number;
    float decimal;
    char string[20];
    scanf("%d", &number);
    scanf("%f", &decimal);
    scanf("%s", string);

    printf("%d\n", number);
    printf("%f\n", decimal);
    printf("%s\n", string);

    return 0;
}
```

```c {linenos=table,filename="简单的 for 循环"}
#include<stdio.h>

int main()
{
    for(int i = 0; i < 10; i++){
        printf("%d\n", i);
    }
}
```

{{< /tab >}}

{{< tab >}}

```python {linenos=table,filename="读取并显示键盘输入"}
number = int(input())
decimal = float(input())
string = input()

print(number)
print(decimal)
print(string)
```

```python {linenos=table,filename="简单的 for 循环"}
for i in range(0, 10):
    print(i)
```

{{< /tab >}}

{{< /tabs >}}

作为一种解释型语言，Python 具备较好的可移植性，可在多种不同软硬件平台上运行。使用 Python 编写的程序不需提前编译成二进制代码，可以直接从源代码运行。此外，Python 具有脚本语言中最丰富和强大的功能库。这些库覆盖了文件 IO、GUI、网络编程、数据库访问等绝大部分应用场景。用 Python 语言开发程序时，许多功能可以通过调用库实现，开发者通常无需从零编写。当前，Python 语言因其语法简单、功能丰富的特点，已被广泛应用于服务器、数据库、图像处理、人工智能等领域。

MicroPython 是 Python 语言的精简高效实现，可理解为一个可以运行在微处理器上的 Python 解释器。它使得用户可以编写 Python 脚本来控制硬件。MicroPython 继承了 Python 的完备的 REPL 交互功能，可以通过 REPL 串口随时输入代码执行，便于测试。MicroPython 还内置了文件系统，用户可以随意向设备端上传任意文件内容，并对目录结构进行修改。基于这一点，设备端可以同时储存多个程序脚本和其他文件，用户可根据需要手动选择并运行，类似于手机 App 的机制，极为灵活。此外，得益于 Python 解释型语言的特性，用户在使用 MicroPython 进行开发时，无需因为代码的更改而反复编译代码和烧录固件，仅需将修改过的代码重新上传至设备内即可。

{{% details title="关于解释器" %}}

解释器（Interpreter）是一个和编译器（Compiler）相对的概念。作为一种解释型语言，Python 的源码是在运行中（而非运行前）被转换为机器可识别和执行的二进制形式。实现这一流程的工具称为解释器。

![](/images/compiler-and-interpreter.png "解释型语言和编译型语言的执行流程差异")

对于初学者而言，解释器可以粗略地理解为 Python 脚本的运行环境。目前，在电脑端，CPython 是最为常用的 Python 解释器。绝大部分资料和工具中涉及的“Python”默认指的就是 CPython。在嵌入式领域，除了 MicroPython，CircuitPython、PikaPython 等解释器也受到国内开发者的欢迎。

{{% /details %}}

{{% details title="关于 REPL" %}}

REPL，全称 Read-Eval-Print Loop，即“读取-求值-输出”循环，是一种简单的交互式编程环境。REPL 通常会提供一个 CLI（Command-Line Interface，命令行界面），接收用户的输入，解析并执行后，再将结果返回给用户。从功能和使用方法上，它类似于 Windows 的命令提示符（CMD）或 macOS / Linux 的 Shell。

![](/images/python-repl.gif "Python 的 REPL 环境")

REPL 的优势在于可以使得探索性的编程和调试更加便捷，因为“读取-求值-输出”循环通常会比经典的“编辑-编译-运行-调试”模式要更加方便快捷。REPL 对于学习一门新的编程语言具有很大的帮助，因为它能立刻对初学者的尝试做出回应。

{{% /details %}}

![](/images/mpy-and-rpi-pico.png "使用 MicroPython 开发 Raspberry Pi Pico")

目前，MicroPython 已经支持在包括 STM32、Raspberry Pi Pico、ESP32 在内的数十种硬件平台上运行。

## 认识 QuecPython

![](/images/qpy-intro.png)

移远将 MicroPython 移植到了多款无线通信模块上，并增加了大量与通信相关的功能库，称之为 QuecPython。在之前的文章中，我们介绍了基于脚本语言的模块二次开发方式，QuecPython 正是这样的一种开发方式。基于 QuecPython，用户可以使用 Python 脚本对移远通信模块进行快速便捷的二次开发，轻松实现远程控制、数据上云等常用物联网功能。

与传统的单片机开发和 QuecOpen（CSDK）开发相比，QuecPython 开发的最大优势在于其简便性。下图分别展示了传统开发方式和 QuecPython 开发方式的大致流程。不难看出，QuecPython 由于采用了脚本语言，只需要在首次开发前模块内烧录专门的 QuecPython 固件，此后当代码编写和修改完成时即可立即运行，无需繁琐的编译和烧录步骤，显著提升了代码调试的速度。

![](/images/csdk-and-qpy-dev-steps1.png "基于 QuecOpen 和 QuecPython 的模块开发流程对比")

QuecPython 的另一优势在于内置了丰富而实用的各类功能库。除了 MicroPython 的核心标准库外，QuecPython 还提供了包括语音通话、短信、MQTT、基站定位等在内的一系列与物联网相关的功能库，并实现了针对阿里云、腾讯云等主流云平台的支持。用户仅需不到 20 行代码就可以实现与阿里云的简单对接。

![](/images/qpy-solutions.png "基于 QuecPython 开发的典型产品")

目前，QuecPython 方案已经在智能家电、工业控制、智慧交通等场景得到应用。各类公司基于 QuecPython 方案推出的产品包括车载定位器、DTU、4G 对讲机等数十种。同时，因为 QuecPython 上手难度低、开发周期短的特性，特别适用于以下物联网应用场景：

- **原型验证和产品演示**: QuecPython 可以帮助开发者快速构建产品原型，验证设计方案，并进行产品演示，缩短开发周期。
- **数据采集和远程监控**: QuecPython 可以方便地连接各种传感器，采集数据并通过网络上传到云平台，实现远程监控和数据分析。
- **简单控制和逻辑处理**: QuecPython 可以实现简单的设备控制和逻辑处理，例如控制 LED 灯、读取按钮状态、响应传感器事件等。
- **教育和创客项目**: QuecPython 对于初学者和创客来说非常友好，可以帮助他们快速入门物联网开发，实现各种创意项目。

QuecPython 已经支持多款移远通信模块，并提供了完善的开发文档和示例代码。QuecPython 社区也日益活跃，开发者们互相交流经验，分享代码，共同推动 QuecPython 的发展。未来，QuecPython 将继续完善功能，扩展支持的模块型号，并进一步提升性能和易用性，为物联网开发者提供更加便捷的开发体验。
