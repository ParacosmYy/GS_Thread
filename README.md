# GS_Thread

> 类 RT-Thread 内核复刻项目：用于深入理解 RTOS 调度、对象管理、IPC、定时器和 Cortex-M4 移植。

## 项目定位

本项目参考 RT-Thread 内核思想，独立实现一套轻量级 RTOS 内核，并移植到 STM32F407ZGT6 平台验证。

| 项目 | 说明 |
|------|------|
| 平台 | STM32F407ZGT6 / Cortex-M4 |
| 类型 | RTOS 内核学习与复刻项目 |
| 核心内容 | 线程调度、对象容器、定时器、IPC、Shell、CPU 移植 |
| 推荐用途 | 嵌入式底层能力展示、RTOS 原理学习、面试项目说明 |

## 核心能力

- 多线程创建、切换与优先级调度；
- 时间片轮转与阻塞延时；
- 对象容器管理线程、定时器、信号量、邮箱等对象；
- 定时器与系统 tick；
- 互斥锁、信号量等同步机制；
- Shell 线程与环形队列；
- Cortex-M4 上下文切换与 STM32F407 硬件移植。

## 目录结构

```text
GS_Thread/
├── gsthread/
│   ├── include/                 # 内核公开头文件
│   ├── src/                     # 调度器、线程、对象、IPC、定时器等核心实现
│   ├── components/              # Shell、命令、ringbuffer 等组件
│   └── libcpu/arm/cortex-m4/    # Cortex-M4 移植层和上下文切换
├── Libraries/                   # STM32F4 标准外设库 / CMSIS
├── Project/                     # Keil 工程
├── User/                        # 用户应用与板级入口
├── Listing/                     # 编译中间产物
├── Output/                      # 输出文件
└── README.md
```

## 推荐阅读顺序

1. `gsthread/include/list.h`：链表基础；
2. `gsthread/src/object.c`：对象容器；
3. `gsthread/src/thread.c`：线程生命周期；
4. `gsthread/src/scheduler.c`：调度器；
5. `gsthread/src/timer.c`：系统定时器；
6. `gsthread/src/ipc.c`：同步与通信机制；
7. `gsthread/libcpu/arm/cortex-m4/`：Cortex-M4 移植与上下文切换；
8. `User/`：硬件验证入口。

## 编译与运行

1. 打开 `Project/` 下的 Keil 工程；
2. 选择 STM32F407ZGT6 对应目标；
3. 编译并下载到开发板；
4. 通过串口 Shell 观察线程、调度和同步行为。

## 项目亮点

- 不是只调用现成 RTOS，而是从调度器和对象模型开始复刻；
- 同时包含内核代码和真实硬件移植；
- 适合展示对 PendSV、SysTick、线程栈、优先级调度的理解；
- 通过 Shell 和串口交互验证内核运行状态。

## 后续优化方向

- 补充调度流程图；
- 增加 PendSV 上下文切换说明；
- 整理 Shell 命令表；
- 增加典型问题复盘：栈初始化、链表断链、tick 不递增等。
