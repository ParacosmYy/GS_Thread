
# 类RT-Thread内核复刻项目

> 本项目由本人独立开发，参考野火 RT-Thread 内核，逐步实现了多线程调度、对象容器、定时器、时间片轮转、互斥锁、信号量等 RTOS 基础功能，并成功移植到 STM32F407ZGT6 平台。所有功能均为本人亲自设计、编码、调试和完善，遇到的每一个问题都由我独立分析和解决。

## 项目简介
本项目旨在深入理解嵌入式实时操作系统（RTOS）原理，独立实现多线程调度、对象管理、定时器、同步机制等核心功能，并完成在 STM32F407ZGT6 平台的移植与验证。适合嵌入式开发、系统架构、底层驱动等岗位面试展示。

## 快速上手
1. 克隆仓库，导入 Keil 或 VSCode 工程。
2. 连接 STM32F407ZGT6 开发板，编译下载即可运行。
3. 通过串口可交互 Shell，体验多线程与同步机制。

## 主要功能

- 多线程切换与优先级调度
- 对象容器（线程、定时器、信号量、邮箱等）
- 空闲线程与阻塞延时
- 多优先级调度与时间片轮转
- 定时器实现与延时机制
- 互斥锁、信号量
- Shell 线程与环形队列
- STM32F407ZGT6 移植与硬件验证

## 技术亮点


## 核心优势
- 完整实现 RTOS 线程管理与优先级调度，支持多种对象类型及链表管理。
- 定时器与系统时钟中断驱动，兼容 STM32F4 系列 Cortex-M4 内核。
- 通过仿真与硬件测试，功能稳定可靠，所有功能均为本人独立设计与调试。

## 项目目录

```
gs_thread/
├─ README.md
├─ gsthread/
│  ├─ include/
│  │  ├─ gsdef.h
│  │  ├─ gshw.h
│  │  ├─ gsservice.h
│  │  ├─ gsthread.h
│  │  ├─ list.h
│  │  ├─ object.h
│  │  └─ shell.h
│  ├─ components/
│  │  ├─ cmd.c
│  │  ├─ cmd.h
│  │  ├─ ringbuffer.c
│  │  ├─ ringbuffer.h
│  │  ├─ shell.c
│  │  └─ shell.h
│  ├─ libcpu/
│  │  └─ arm/
│  │     └─ cortex-m4/
│  │        ├─ context_rvds.s
│  │        └─ cpuport.c
│  └─ src/
│     ├─ clock.c
│     ├─ components.c
│     ├─ idle.c
│     ├─ ipc.c
│     ├─ irq.c
│     ├─ kservice.c
│     ├─ list.c
│     ├─ object.c
│     ├─ scheduler.c
│     ├─ shell.c
│     ├─ thread.c
│     └─ timer.c
├─ Libraries/
│  ├─ CMSIS/
│  └─ STM32F4xx_StdPeriph_Driver/
├─ Project/
├─ User/
├─ Listing/
├─ Output/
├─ keilkill.bat
└─ 必读说明.txt
```

**目录重点说明：**
- `gsthread/include/`：RTOS内核头文件，定义线程、对象、链表、硬件抽象等核心接口。
- `gsthread/components/`：Shell命令、环形缓冲区等功能组件，支持交互和调试。
- `gsthread/libcpu/arm/cortex-m4/`：CPU移植层，包含上下文切换、端口适配等底层实现。
- `gsthread/src/`：RTOS内核源码，线程调度、定时器、同步机制等核心逻辑。
- `Libraries/`：第三方硬件库，支持 STM32F4 系列芯片。
- 其他文件夹为工程配置、输出和用户代码。

## 我如何解决难题
- 线程切换失败：发现栈初始化遗漏，调度器未启用，经过反复调试逐步定位并修复。
- 对象容器链表断链：链表插入/删除指针处理不当，优先级调度异常，通过分析链表结构和调试波形，最终修正链表操作逻辑。
- 位掩码与优先级判断错误：自研 ffs 函数与查表法不符，深入分析位运算和优先级掩码，完善算法。
- 定时器延时与中断同步：延时跳转卡死，tick变量未自增，定位到线程挂载与位掩码同步问题，逐步修复。
- STM32 移植兼容性：中断服务函数冲突，CMSIS 文件缺失，独立查阅资料并调整中断屏蔽和头文件替换，保证平台兼容。
- Shell 环形队列边界处理：命令解析时队列结尾判断失误，独立调试串口通信，完善队列边界处理逻辑。


## 运行效果与测试
- 多线程 LED 翻转，互不影响，验证调度与同步。
- Shell 命令解析与串口通信，支持交互与调试。
- 互斥锁与信号量功能通过测试，保证同步机制可靠。
- 硬件平台：STM32F407ZGT6，Keil/VSCode 工程，支持仿真与真机测试。


---
### 技术细节补充
1. SCB_VTOR EQU 0xE000ED08  == #define SCB_VTOR 0xE000ED08
2. PRIMASK(特殊功能寄存器) 为0则屏蔽中断，为1则开启中断（只有NMI和HardFault会响应）

---
如需源码或详细交流，欢迎联系我！