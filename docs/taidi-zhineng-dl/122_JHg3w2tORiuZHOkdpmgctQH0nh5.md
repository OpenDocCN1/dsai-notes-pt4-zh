# SoC 软件栈

> 来源：[https://learning.feishu.cn/docx/CTotdEPPHo8nunxsgkvcMvNMnvf](https://learning.feishu.cn/docx/CTotdEPPHo8nunxsgkvcMvNMnvf)

在很多的 SoC 中，软件设计的复杂度和开发周期，都超过整个硬件的设计。

-- 需要一些更具体的例子。

应用软件的 runtime 包括：

1.  应用程序

1.  OS Kernel。不需要任务管理、资源管理及进程管理的应用，一般不采用 OS。

1.  drivers

1.  Chip Hardware

interface of software and hardware

1.  memory map

1.  device driver

1.  bootloader

1.  interrupt

1.  I/O pin (引脚) 的复用