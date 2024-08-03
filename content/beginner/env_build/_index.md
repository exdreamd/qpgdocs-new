---
title: 开发环境搭建
date: 2024-07-27T16:41:35+08:00
draft: false
weight: 40
next: /beginner/env_build/hardware_preparation
---

QuecPython 作为基于脚本语言的开发方式，不存在传统意义上的 SDK，无需准备专门的开发环境和开发工具链。在开发板到手之后，用户仅需在模块内手动烧录专门的包含有 QuecPython 脚本语言解释器的固件，即可开始开发。

这一章节将详细介绍 QuecPython 固件烧录的全过程。通过阅读本章节，用户可以方便地完成 QuecPython 开发的前期准备工作，快速投入具体开发中。

本章节的内容相对冗长，细节和注意点较多，忽略细节很可能导致固件烧录失败。我们强烈建议您从头开始认真阅读本章节，避免因忽略潜在的细节导致失败。如果您未能成功完成固件烧录，我们也建议您从头重新阅读本章节，逐项核查相关操作是否执行到位。

{{< callout type="info" >}}

**提示**

移远无线通信模块以及搭载这些模块的开发板（核心板）在出厂时通常烧录有标准（AT 指令）固件或 QuecOpen（CSDK）固件，如需基于 QuecPython 对模块进行开发，需要手动为其重新烧录专门的 QuecPython 固件。原则上，您在拿到开发板之后，无论此前是否为其烧录过固件，都建议为其重新烧录最新的 QuecPython 固件。

{{< /callout >}}