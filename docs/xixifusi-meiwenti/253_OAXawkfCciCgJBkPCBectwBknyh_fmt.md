
# 笔记

> 来源：[https://docs.feishu.cn/docx/ZvQRd6oq5oBdhXxTn5Ic26NJnne](https://docs.feishu.cn/docx/ZvQRd6oq5oBdhXxTn5Ic26NJnne)

## 1 Intro & Syllabus

### XNU 内核

XNU 内核是一个 Unix-like 操作系统内核，它是 macOS 和 iOS 等苹果操作系统的核心组件。XNU 是 "X is Not Unix" 的缩写，表示它不完全遵循传统的 Unix 系统设计。

XNU 内核是苹果电脑开发用于 macOS 操作系统的操作系统内核，也是 iOS、tvOS 和 watchOS 的内核。它是 Darwin 操作系统的一部分，跟随着 Darwin 一同作为自由及开放源代码软件被发布。XNU 内核由 Mach、BSD 和 IOKit 组成，其中 Mach 是微内核，只负责最基本的职责，BSD 是基于 Unix 开发的，IOKit 负责硬件信息。苹果已经在 GitHub 上开源了 XNU 内核的源代码。

XNU 内核是一个分层体系结构，由 Mach、BSD 和 IOKit 组成。Mach 是微内核，是仅提供基础服务的一个薄层，如处理器管理和调度以及 IPC（inter-process communication，进程间通信），IPC 是 mach 内核的核心概念。BSD 层是围绕 mach 层的一个外环，为最终用户的应用程序提供编程接口，其职责包括进程管理、文件系统和网络。XNU 内核比较庞大复杂，据说是一个无限递归的缩写：XNU’s Not UNIX。Darwin 的内核是 XNU，而 XNU 是在 UNIX 的基础上做了很多改进以及创新。

### iOS

### macOS

### Software Supply Chain

### Famous Attacks

### Secure Software Supply Chain

### Operating Systems

### Boot Security

### Runtime Security

### The Evolution of OS Offenses and Defenses

### Hardware Security Mechanism

### Virtualization, TPM, TEE, PAC

### Container Systems (Optional)

### Mobile Systems (Optional)

### Android Security

![](img/89419195ca24bccc154d1ef8626bb601.png)

![](img/e9602078b07ae19ff24d2883e916c230.png)

### 读论文

- 看观点
- 看怎么 develop 一篇文章
- 看图片

Show how we advance the state-of-the-art?

![](img/afc34227cd08098c2ae01a813f624dae.png)

![](img/c2e5b78193d98728f47c2f9604e4e5c2.png)

### Privilege Escalation

"权限升级"（Privilege Escalation）是指在计算机系统中，攻击者利用系统漏洞、配置错误或设计缺陷，将其原本的低权限账户升级到高权限账户的过程。权限升级可分为两类：水平权限升级（Horizontal Privilege Escalation）和垂直权限升级（Vertical Privilege Escalation）。

1. 水平权限升级：在水平权限升级中，攻击者利用漏洞或错误从一个低权限账户访问另一个具有相同权限的账户。这使得攻击者可以访问其他用户的数据和资源，但权限级别仍然相同。
1. 垂直权限升级：在垂直权限升级中，攻击者从一个低权限账户成功地提升到更高权限的账户，例如从普通用户升级到管理员。这使得攻击者可以完全控制目标系统，执行任意命令并访问所有数据。

权限升级是网络安全领域的一个重要问题，因为它使攻击者能够突破系统的安全边界，并获得对受保护资源的访问权。为防止权限升级，需要定期更新系统补丁，确保系统配置正确，并遵循最佳安全实践。

ROP（Return-Oriented Programming）劫持是一种高级的缓冲区溢出攻击技术。在这种攻击中，攻击者通过控制程序的执行流程并使用程序原有的代码片段（称为 `gadgets`）来执行恶意操作，而不是直接注入恶意代码。ROP 劫持的主要目标是绕过执行保护（如数据执行保护，DEP）等安全机制。

在 ROP 攻击中，攻击者通常劫持以下数据：

1. 函数返回地址：攻击者通过溢出漏洞覆盖函数的返回地址，使程序在返回时跳转到攻击者选择的 `gadgets`。这样，攻击者可以控制程序的执行流程并逐步构建恶意操作。
1. 栈数据：攻击者可能会修改栈上的数据，以控制 `gadgets` 之间的执行顺序。在构建 ROP 链时，攻击者需要精确地布置栈数据以确保 `gadgets` 按预期执行。
1. 寄存器数据：ROP 攻击可能会修改寄存器的值，以传递参数或控制 `gadgets` 的行为。攻击者通常会利用一些 `gadgets` 来控制寄存器的值。

ROP 劫持的关键是找到足够多的 `gadgets` 并以正确的顺序执行它们以达到攻击者的目的。为了防御 ROP 攻击，研究人员和工程师开发了多种技术，如地址空间布局随机化（ASLR），控制流完整性（CFI）和 ROP-specific 保护机制。

![](img/9b824a178f518459458c8c92cbedc440.png)

![](img/fcb8d14fb5fb306e3683d2693412157e.png)

### eBPF

eBPF（扩展的 Berkeley Packet Filter，即扩展的伯克利包过滤器）是一种在 Linux 内核中运行的轻量级虚拟机，它允许用户自定义内核中的数据包处理和系统调用，以实现对网络和系统事件的实时监控、分析和调试。eBPF 是原始 BPF（Berkeley Packet Filter）的扩展和增强版本，具有更高的灵活性和性能。

eBPF 有以下特点：

1. 安全性：eBPF 程序在内核中运行，但它们需要通过内核的验证器进行安全检查，以确保程序不会导致内核崩溃或产生安全漏洞。
1. 性能：eBPF 程序可以在内核中高效地运行，而无需在用户空间和内核空间之间进行昂贵的上下文切换。
1. 可编程性：eBPF 程序可以用高级编程语言（如 C）编写，并使用 LLVM 工具链将其编译成 eBPF 字节码。这使得开发者能够编写复杂的自定义程序来处理网络和系统事件。
1. 可扩展性：eBPF 支持与其他内核功能和用户空间工具集成，如 Linux 跟踪工具（如 `BCC` 和 `bpftrace`）、网络功能（如 XDP）和容器技术（如 Cilium）。

eBPF 的应用场景包括：

1. 网络监控与分析：使用 eBPF 监控网络流量，实现实时的流量统计、安全审计和异常检测。
1. 系统调用跟踪：使用 eBPF 跟踪系统调用，以分析和调试内核和应用程序行为。
1. 容器网络与安全：eBPF 可用于实现高性能的容器网络解决方案，如 Cilium，以及提供细粒度的安全策略和隔离。
1. 性能分析与调优：eBPF 可以用于收集系统和应用程序的性能指标，帮助开发者识别性能瓶颈并进行优化。

### AI for System Security

用大模型来做这几个方向

## 2 Software Supply Chain Security

### Traditional Supply Chain

### What Is Software Supply Chain?

在计算机科学中，Artifact 通常指的是源代码、二进制文件、编译器产生的中间文件、构建工具生成的构建输出物等计算机程序开发过程中所产生的各种文件。这些文件包含了程序的各个版本和构建状态，可以用于构建、测试、部署、维护和追踪程序的历史记录和演变。

一些常见的 Artifact 包括：

- 源代码：程序员编写的原始代码文件，通常以文本形式存储。
- 二进制文件：编译器将源代码转换为机器可读的二进制代码，通常以可执行文件或库的形式存储。
- 中间文件：编译器在转换源代码为二进制文件的过程中生成的文件，通常以特定的格式存储。
- 构建输出物：构建工具生成的构建输出物，例如编译后的二进制文件、打包后的部署包等。

![](img/8a8ff4ade4dd01c56d253c14f6e59ec1.png)

PKM：包管理器

### What Is Software Supply Chain Security?

可能的安全问题

- Source code
- Build process
- Distribution channel
- Package manager
- Dependent libraries

![](img/203e4333ba9ae54c3d67795dd344da11.png)

![](img/8f1a6402dcc49040fafd68861cfe393e.png)

![](img/604bce26981b2ad4934c8dfa438034be.png)

### Famous Attacks/Vulnerabilities

- KTH
- Heartbleed
- SolarWinds
- Log4j2

### Secure Software Supply Chain

#### KTH

Ken Thompson Hack, 1983

我们究竟应该相信谁？

- 操作系统
- Binary
- Compiler

他把 `compiler` 修改了，有个后门，如果你不用他的 `compiler`，但是还是需要会用他的 `compiler`，当你用这个 `compiler` 编译其他的 `compiler`，还是会有后门

- The `compiler` looks for source code that looks like `login` program
- If found, insert login backdoor (allow special user to log in)

![](img/64a8528d5e1cb983ca4faeff6353a663.png)

我们发现除了 source code，还有一些影响 software？

- Build toolchain
- Build environment
- Build scripts
- 脚本：maintainer 不知情的情况下，给二进制里面插入脚本

防御：

- 多更新，多检查
- 验证签名，`SigStore`
- 如何验证 Src 和 Bin 的语义不多不少：太难了

#### Heartbleed

Heartbleed vulnerability, 2014 https://heartbleed.coml

![](img/00304001027f953456b477500809071a.png)

OpenSSL 存在漏洞

越界的读，读到秘钥

开源安全问题

![](img/e7d90cd6617d9c9fa4ea6fa3ee81eb7c.png)

OpenSSL 被广泛使用

![](img/8902cbc37aa6b911d78888474d5c1ce3.png)

![](img/0784cb88dab582dbe9752b73dac6894b.png)

问题在于 payload 越界的读，发送一个 Request，发送回一个同样 payload 的 Reply，不检查长度，容易溢出；溢出的部分容易泄露一些重要信息，读到账户和密码

几乎被 NSA 用了两年，其他国家损失惨重

RSA 秘钥都会被发现

如何防御？

payload 超出的部分

- 属于同一个 process
- 不属于同一个 process
- Intra-process-isolation：把内存分开，不能 overflow 其他的

![](img/799ec9bdf540d3e70b9382b031fc9fa9.png)

都可以

- Static code
- Dynamic analysis
- 禁止越界读取

#### SolarWinds

SolarWinds attack, 2020

为 B 端提供软件服务

![](img/653d930086114401fff725572f558011.png)

攻击时间线

攻击非常隐秘，满足 SolarWinds 公司的代码规范

会先隐藏 2 周，有一些抗分析

怎么防御？

![](img/d9e38c05d99575107eca6eb847b4d7df.png)

Reproducible build 多次编译，每次编译都要一样，但是很难执行

1. 及时更新补丁：SolarWinds 攻击利用了软件更新服务器的漏洞，因此及时更新软件更新服务器的补丁是至关重要的。此外，组织应该定期对所有软件进行更新和维护，以修复已知的漏洞和弱点，从而提高系统的安全性。
1. 加强网络安全：组织可以使用网络安全技术，例如防火墙、入侵检测和防御系统、反病毒软件和安全信息和事件管理（SIEM）系统等，以提高网络安全性。此外，组织还应该加强网络访问控制和身份验证，限制对关键系统和数据的访问权限。
1. 加强供应链安全：组织应该加强对供应链的管理和监控，确保所有供应商和合作伙伴都遵循安全最佳实践，并定期对其进行安全审计和漏洞扫描。此外，组织还应该对所有供应商和合作伙伴进行严格的身份验证和访问控制，以确保其安全性。
1. 加强审计和监控：组织应该定期进行安全审计和监控，以识别和防止潜在的安全威胁和攻击。此外，组织还应该使用安全信息和事件管理（SIEM）系统等工具来监控网络活动和安全事件，并及时响应和处理安全事件。

#### Log4j2

Log4j2 vulnerability, 2021

![](img/42a2b570c5916cac981a97f9082f2079.png)

攻击者使用 `${}` 关键标识符触发 Java Naming and Directory Interface ( JNDI）注入漏洞

两个必要成因

- 允许 `lookup` 加载远程代码
- JNDI 加载类可受用户输入影响

允许 `lookup` 加载远程代码

- Log4j2 提供 `lookup` 功能，日志中包含 `${}`，`lookup` 功能就会将表达式的内容替换为表达式解析后的内容，而不是表达式本身
- Log4j2 支持 Context Map、 Date、 Event、 Java、 JNDI 等多种的属性查找及替换选项，其中所支持的 JNDI 就是本次漏洞的触发点

![](img/2a982485f7380b3d960fd7f8d1f99509.png)

变量命名 `disable`，双重否定，把自己绕进去了；默认是开启的

允许 `lookup`

- 在 `Constants.java` 中配置的值默认为 `false`，因此 `Lookups` 功能在默认情况下是开启的

![](img/dfec8406b0dc5a7ddee06a5f8cd560e2.png)

![](img/019382d4b4169c3de7b8669b2f3828a3.png)

![](img/8aa25fa7f0f5351e29fd608c6871cf5e.png)

不要绕

Java 没有内存问题，但是往往有让别人远程加载的问题

![](img/86ae4f96d5cd0663aa8e5dd84dca0052.png)

- 防御
- 前：静态分析
- 中：monitor 这个操作，看看是否会加载远程的类
- 后：找到一些 trace，我们会发现谁部署了，把他 disable 掉

## Secure Software Supply Chain

我们主要是关注远程的

### 第三方库

- 你使用了哪些库
- 这些库容易受到漏洞的影响吗？理解依赖里是否有漏洞
- 如何防御漏洞？ 理解软件依赖什么代码？

![](img/8ffb209482f0c83912d345c9e2868fb1.png)

NPM：node 可能会有依赖问题，依赖循环



![](img/d5c328cd0e0ce61f148468ed0e370fd9.png)

# 2 软件组成分析（SCA）

## 你使用了哪些库

Software composition analysis (SCA)

+   Find out libraries your software are using

+   Usually two ways to use others' code

+   Code clone

+   C/C++：一般是binary

+   Use package

+   Java Node Python, Go Rust,.NET, PHP

+   Java就是`Jar`包复用，里面可以有源代码

![](img/bfbe7e9431bcb5a1d843f052015fd2dc.png)

不同**语言代码复用**方式不同，有两种分析技术

+   Code clone detection代码克隆检测

+   对海量代码以一定粒度进行索引，扫描软件源代码获得软件成分信息

+   Granularity 检测粒度（检测速度，精确度，大小）

+   `Library`, `File`, `Function`, `Line`?

+   What are the advantages and disadvantages?

+   优点：`Line`的话很精确

+   缺点：`Line`的话，容易误报，false Positive多；检测粒度越小越精确，但误报率会提升

+   Technique 索引方法

+   Hash, Control flow graph,

+   What are the advantages and disadvantages?

+   Hash优点：快

+   Hash缺点：只修改一点，就检查出不一样了，不容易检查出那些只修改一点点点

+   Control flow缺点：慢，越精确越慢

+   索引方法越精确，检测速度越慢

+   Centris: A Precise and Scalable Approach for Identifying Modified Open-Source Software Reuse, ICSE 21

+   Use per-function hash

> 一般商用是折中的方法，函数

![](img/ccc75d8b7eb9754c62cf182083314de3.png)

False positives：模型预测为正类的样本，但实际上是负类的样本。也可以说是模型错误地将负类样本分类为正类。在网络安全中，false positives是指安全系统或工具错误地将合法的活动、流量或文件标记为恶意或危险。

false negatives是指安全系统或工具未能检测或识别实际上是恶意或危险的活动、流量或文件。

+   优点：灵活，可移植性比较强，可维护性比较强；

+   Dependency detection 组件依赖分析

+   一般会直接看dependency file

+   像如果是Maven的话就是`Pom.xml`

+   递归的看

+   优点：自动更新

## 这些库容易受到漏洞的影响吗？

![](img/70c856c9e86c58a50206dea7c4f97285.png)

![](img/b33a3f8071ead9bb29ee41d7d8c84686.png)

![](img/918c3a004d63f8a35806a6695831f7c7.png)

CVE：一个漏洞vuInerability

CWE：一类漏洞weakness

以软件包做分析：太粗粒度了，有误报率

比如我用了`log4j`的包，但是我没有用他的类，其实我是没有被攻击的

![](img/bb2a9e242b5956f56da179b0b52be717.png)

![](img/defe59a553bd6ab9b42eff2d21295751.png)

需要静态可达

但是静态可达也不一定动态可以触发

![](img/16fa63b88daa44e11572a201258aae60.png)

动态可触发足够精确，但是比较难，复杂

## 如何防御漏洞？

![](img/5bb5e7000fa22bdbf9283d20d1fb24d9.png)

会有lag

通过隔离来在这段时间里防护

![](img/3ea686e1ffc6378958adfbd61989d879.png)

![](img/a8daa606cb216ee8ca164b43afbc88d7.png)

WebAssembly

+   安全

+   兼容

提出Nano-process：兼容，隔离

# 3 System Security Concepts

## Why is system security important?

+   电脑安全

+   物理安全

+   网络安全

为什么需要OS层的安全？

+   任何系统都可以通过一组庞大的网络间和网络内部连接进行全球访问。

+   系统控制很多软件，恶意攻击者已知的操作系统设计和实现中的单个安全漏洞可能会造成严重损害。

+   系统中有许多需要保护的数据

+   如账号密码、信用卡号、地理位置、照片视频等

+   操作系统需要允许这些数据被合法访问，但不允许被非法访问

+   系统中可能存在许多恶意应用

+   操作系统需要与这些恶意应用作斗争，保护自己，限制对方

+   操作系统不可避免的存在漏洞

+   操作系统需要考虑自己被完全攻破的情况下依然提供一定的保护

保护

+   硬件

+   软件

+   数据

主要防

+   未授权对用户

+   恶意攻击

## Security evaluation criteria

安全标准

+   TCSEC

+   Trusted Computer System Evaluation Criteria

+   ITSEC

+   Information Technology Security Evaluation Criteria

+   CC

+   Common Criteria

+   GB17859

![](img/5338ace1cf8aec3c97a79b2349b0d28e.png)

![](img/2636940da377e324e90efe61e059d448.png)

![](img/2b1678a4975b8312d5f420eafb322519.png)

![](img/3ed23e843899bdea2d39d6dc5bc03130.png)

![](img/cf3c885901a2434884271ee167360de0.png)

![](img/a718f340c21a7136ecc90f2e7ed738.png)

## Common concepts

### 可信计算基( Trusted Computing Base)

+   为实现计算机系统安全保护的所有安全保护机制的集合

+   包括软件、硬件和固件（硬件上的软件）

+   比如你在输密码的时候，你已经默认你相信了谁；这些你相信的就是TCB；TCB越小越好，越大越容易出bug；小到可以做验证

![](img/fca5d217174fce69469ba4f9c676d017.png)

高层的需要依赖低层的

### 攻击面(Attacking Surface )

+   一个组件被其他组件攻击的所有方法的集合

+   可能来自上层、同层和底层

+   可攻击面越小越好

### 防御纵深(Defense in-depth）

+   为系统设置多道防线，为防御增加冗余，以进一步提高攻击难度

![](img/589f02f844416f5d6827a5f08777f8de.png)

![](img/0ca4896427a053618d7b0eda9348560b.png)

`http://zerodium.com/program.html`

对漏洞明码标价网站

## Security models

### CIA

![](img/1d0c7f5ce44e49aa0d61d2d74619ee7c.png)

### Bell-LaPadula

+   为了实现机密性，no read-up；no write-down

+   不允许下面的读上面的数据；不允许上面的数据写到下面去

![](img/82db9ba345e68a3cb7b688c1c3d3b523.png)

### Biba

+   为了实现完整性

+   No read down

+   No write up

![](img/4af4f18155ba07ab62565d241cef19db.png)

# 4 Access Control

## Concepts

### Authentication 认证

+   验证某个发起访问请求的主体的身份

![](img/14c6c9e5e1a7021f8806ab82856ee991.png)

+   `cred`包含`uid`：credential

![](img/ba992ab086ac79aee611ce01c930bc6f.png)

### Authorization 授权

+   授予某个身份一定的权限以访问特定的对象

![](img/c43c7d0d2c559eee9bc08ebae052225a.png)

+   更新很麻烦

![](img/d0dce0c4497bd10054daa403e2c98a40.png)

提出了RBAC

![](img/3031998e3091180904ed6ff68342b0a2.png)

三个优点

![](img/dee82e4ad59bc5119a02cb4d1b043361.png)

#### `SetUID`

![](img/d80ee898ead99b474943f1e9296ccae2.png)

我们会发现`passwd`是不需要root身份的，我们使用`setuid`去进行操作的，执行的时候是root状态；password代码逻辑保证只能修改自己的密码

![](img/a7cae2eea43f1a98c4900a84b7545408.png)

权限远超必要

#### `Capabilities`

![](img/09945223fe26bec636f9b9dd281057a7.png)

![](img/106e2d145c2e406bffcef7a9a4b26b59.png)

更加细粒度的访问控制机制

可以组合

![](img/1a62268f1bc5f82cbbe923f784e5eced.png)

按bit存在`cred`里

![](img/40d9864fd06592778957d9c006234ea8.png)

> 进行权限检查

`Cap_SYS_RAWIO`

![](img/5e31f0536c5fdcb3a1d0d30543059fac.png)

![](img/51ece4fcd84dc4cd70478db0014c6e34.png)

拆分的不均匀，`CAP_SYS_ADMIN`占大多数，只需要几个capabilities就可以模仿root

![](img/7b3bc12ba1eb71517c0fb884478b1d28.png)

权限检查的冗余、缺失、不一致

`ioctl`：io control；系统调用可以调用

![](img/a8b206319f26dcabd01d883dce8ce15d.png)

ACL和Capabilities的优缺点，大部分系统都用了

### Auditing 审计

![](img/4b2146a7f18f5555254a49dbb7c3f9d7.png)

审计标准

## Reference monitor

> 具体实现方式

![](img/cd26ec1a2692aa8619f03e372c95356c.png)

主要保证认证和授权

![](img/4571a29209578888315c7e0a3880995f.png)

引用监视器机制必须保证其不可被绕过(Non-bypassable)。

## DAC and MAC

![](img/fb520d2471f6296564787d1841934a1c.png)

### SELinux

![](img/bb9cf6acd13ea424ebba2101f7e36a34.png)

![](img/5b46ec6bb265687cc5ebfb5b7dc13fda.png)

可以限制root

需要一开始定义规则

![](img/a6279cf0f1b9415c0f1ec54d34dc349a.png)

本质上是一个标签系统

![](img/5f828a85c1ecf3d67e756c22f77a48e1.png)

基于LSM

> 扩展一下

> LKM代表“加载内核模块”，是一种在Linux操作系统中动态加载内核模块的机制。内核模块是一种可加载的软件组件，可以动态地插入到Linux内核中，以扩展内核的功能或添加新的驱动程序。

> 尽管LKM机制在Linux操作系统中具有很多优点，但也存在一些缺点，包括以下几点：

1.  安全性问题：由于LKM可以加载到内核中并修改内核的行为，因此如果LKM被恶意软件滥用，可以导致系统崩溃或被攻击者利用。

1.  兼容性问题：由于Linux内核的版本和配置不同，因此编写的LKM可能无法在所有的Linux系统上运行。这可能导致软件开发人员需要编写多个版本的LKM以确保兼容性。

1.  性能问题：由于LKM需要加载和卸载，因此会产生一定的开销。此外，LKM可能会与其他内核模块发生冲突，从而影响系统的性能和稳定性。

1.  调试问题：由于LKM可以动态加载和卸载，因此在调试时可能会更加困难。此外，由于LKM可以修改内核的行为，因此找出LKM中的错误可能会更加困难。

> eBPF（Extended Berkeley Packet Filter）是一种在Linux内核中运行的虚拟机，可以用于动态扩展内核的功能。与LKM相比，eBPF具有以下优点：

1.  安全性：eBPF代码在运行之前会经过安全检查，可以避免由于恶意代码导致的安全问题。

1.  性能：由于eBPF使用JIT（Just-In-Time）编译技术，在运行时编译eBPF程序，因此可以获得非常高的性能。

1.  灵活性：eBPF可以用于所有的网络数据包和系统调用，可以扩展内核的功能，从而实现更多的应用。

> 补充：AppArmor

> AppArmor is a Linux kernel security module that allows the system administrator to restrict programs' capabilities with per-program profiles. Profiles can allow capabilities like network access, raw socket access, and permission to read, write, or execute files on matching paths. AppArmor supplements the traditional Unix discretionary access control (DAC) model by providing mandatory access control (MAC).

> AppArmor was first used in Immunix Linux 1998–2003. At the time, AppArmor was known as SubDomain, a reference to the ability for a security profile for a specific program to be segmented into different domains, which the program can switch between dynamically. AppArmor was first made available in SLES and openSUSE and was first enabled by default in SLES 10 and in openSUSE 10.1.

> AppArmor is now included in the mainline Linux kernel since version 2.6. 36 and its development has been supported by Canonical since 2009.

> AppArmor works by enforcing a set of rules that define what a program is allowed to do. These rules are stored in profiles, which are typically loaded into the kernel at boot time. Profiles can be modified or created by the system administrator.

> When a program attempts to do something that is not allowed by its profile, AppArmor will take action. The default action is to deny the request and log the event. However, the system administrator can configure AppArmor to take other actions, such as killing the program or sending a warning message to the user.

AppArmor和LSM，冲突，怎么兼容

一开始是各自都有`func`

后面就用函数指针，指向函数

固定代码换成函数指针：这种称之为`Hooks`

![](img/33d13bef34c35e3db482d99f98b084c6.png)

+   覆盖所有路径

+   不可以被绕过

![](img/eabbc90fec6573694ca945b0d3a0c35a.png)

一组的函数指针



![](img/aa35a6c5648828a8b4db015a5bf7253b.png)

检查Policy Engine

![](img/bf13a45372cfe734a74a80e31d183391.png)

Policy Overview

+ TE
+ RBAC

![](img/023f3cd27e9027d45af7c308761784e5.png)

被称为Policy，或者是Allow rule

![](img/142968e545b1430842eb73c1d99b42ad.png)

问题：

![](img/ca577ec7dc0e271dd544a550de335bcb.png)

rule太多，容易冲突

# 4 Our works

![](img/9455fe9c822a86f067e61b1a3b9227c6.png)

![](img/22dd39c0869c770402f7df8170f9e111.png)

给一个进程改名字

+ 可以bypass

![](img/b01c631d0c90d0e4655bff685ce7e79e.png)

Ad-hoc：随机

不能保证正确放置，不能保证可以防止bypass

![](img/8b4ef03fae86940064a79679650dad19.png)

![](img/b063b88de3e0b58b4b86171d099304fb.png)

![](img/74a8514bb85746dee2691d55188a5150.png)

![](img/55a925c4f2ac3e8fe66b595a44ba1cd0.png)

检测出了36个bugs

# 5 Attack and Defense

## Introduction

如果Access Control做得好，还是有存在Attack 和 Defense

![](img/804bd90b1c85e91f6959bc55ff25a890.png)

缺陷密度：不准

https://cve.mitre.org/

搜漏洞

### 威胁模型

What can the attacker do？

![](img/a595233e2f13bbcca830cad9bc7e882d.png)

+ Lab：国家实验室
+ Shack：硬件
+ Hack：软件

> 一般Lab防不住

## Run time attack and defense

![](img/65a6a67e615717ebbc83251d7ccc87d0.png)

进攻防守是猫鼠游戏，攻防演化

![](img/53add79dab8524df343b6ee78fe62f66.png)

![](img/dbf0cad112e0aee49ca4935be8a69211.png)

![](img/8d7444029ad0028f7de75ebb6288b809.png)

三种进攻方法

+ 代码注入：直接执行`root_access`
+ 代码重用：改变控制流
+ 非控制数据攻击：修改信号

### 内核注入攻击

![](img/468727294d4515bd8ffaa0a6953b3439.png)

![](img/1b2bd1a9ab1b55033d94634722ba98b4.png)

SMEP Intel 和 PXN ARM防止了`jump-to-user`攻击

![](img/af701edc0ef5f5708c4790cf5f7907af.png)

但是有问题，对内核页表没有保护

![](img/4ba090c6011227db252f9fd1d9b884c7.png)

#### 例子 `ret2usr`

![](img/163578d7544c24e4a00f5e53929b302c.png)

![](img/f6c6ca5af5f6fc6fc450a247c98e62d3.png)

![](img/b7ff391ed1463239eb379388c2182fcc.png)

> SMEP技术主要通过限制内核态代码执行权限，从而防止恶意软件和攻击者利用内核漏洞来攻击操作系统。

> SMEP技术是在Intel CPU的保护模式下实现的，通过在处理器上设置一个特殊的位来实现。当SMEP技术被启用时，处理器会阻止内核态代码执行用户态地址空间中的代码，从而防止内核态代码攻击用户态程序或者用户数据。

> SMEP技术的实现方式是通过在处理器的CR4寄存器中设置一个特定的位来实现。该位控制了处理器的执行保护模式（EPM）功能，当该位被设置时，处理器会将内核态代码执行的地址空间与用户态代码的地址空间进行区分，从而避免内核态代码越权访问用户态代码和数据。

防御方法：SMEP直接把直接映射区域变成不可执行的

### 代码重用攻击

![](img/3a3b410b295c0f1a11c730e711a08a5e.png)

拼接代码片段，这叫做`gadget`

+ 改变返回地址，ROP攻击，Return-oriented programming

![](img/26a32e58d356cba5a4c84cf509772773.png)

+ 改变函数指针，JOP攻击，Jump-oriented programming

![](img/be658d74e215a104c5df13c1fb5f1699.png)

知道`SP`，知道`task_struct`，就可以知道`jid`，`uid`

![](img/405e0a504af27024f09e5e102d0d282f.png)

`Canary`：检查预警

+ `canary`值暴露了，容易被绕过

Shadow Stack的基本思想是在程序执行函数调用和返回时，使用一个额外的栈来保存函数调用和返回地址，以便在后续需要返回时使用。与传统的栈不同，Shadow Stack是由硬件或者软件实现的，而不是由程序自己维护的。

Shadow Stack的实现方式是通过在程序堆栈的基础上增加一个额外的栈，用于保存函数调用和返回地址。每次进行函数调用或者返回时，程序会将当前的函数调用和返回地址保存到Shadow Stack中，并在后续需要返回时从Shadow Stack中获取保存的返回地址，从而保证代码执行的正确性和安全性。

+ 问题有可能我连你这个影子栈也改，所以如何防护保护这个影子栈很重要

![](img/602e9658825131f5b8bebaa630915a6c.png)

跳转：只能跳到要求的地址

CFI技术的基本思想是在程序的控制流中插入特殊的指令或者代码，用于检测程序的跳转和调用是否符合预期。例如，在函数调用时，CFI技术会检查调用的函数是否存在，并检查返回地址是否正确。在程序跳转时，CFI技术会检查跳转的目标地址是否正确，从而防止跳转到非法的地址。

指针保护是一种防护技术，用于保护指向函数的指针，防止被篡改。指针保护的基本思想是在程序运行时检查指针的合法性，确保指针指向的函数地址没有被篡改。指针保护可以通过多种方式实现，例如使用指针加密、指针随机化、指针签名等技术，从而提高指针的安全性。

跳转检查是一种防护技术，用于检查跳转目标地址是否合法。跳转检查的基本思想是在程序运行时检查跳转指令的目标地址是否在一个白名单中，如果不在白名单中，则阻止跳转。跳转检查可以通过多种方式实现，例如使用函数指针表、代码签名、控制流完整性检查等技术，从而提高程序的安全性。

![](img/2d2daaa170f54c7ac833a625e5567c32.png)

### 非控制数据攻击

![](img/bbbb2670c537179c1838a373c5c3e6cb.png)

![](img/1ae60351bf37becc41bad59bd89e5c9c.png)

![](img/95598accac84202efe7d14535fe22555.png)

# 6 Boot protection

> 运行时安全

![](img/ede5bfa909b46422a3562e02c1e1bab9.png)

给一个签名

用bootloader验证

+ 秘钥签名
+ 公钥烧到rom里，公钥验证

内核量级不大

![](img/ec93952f4e94b43bd64e3a19e2ecfe24.png)

![](img/5304653f5fe4566e3528703d04ef3db1.png)

可信根是硬件

![](img/d41395c1eea4682961869fc5e0aadcab.png)

+ Boot loader 去验证
+ 避免BL会升级，分成PBL和SBL，PBL写死不会变
+ 安卓的kernel image 100M
+ System image取决于厂商，会挺大，有很多垃圾app
+ 这么大，如何有效率的验证？
+ Merkle tree用于验证
+ 只需要验证`root`，不需要验证其他的
+ `dm-verify`：google发明的验证方法

## OS protection principles

![](img/c963f02244295c96da9a0729f252b428.png)

Complete mediation：所有的都需要被检查，reference monitor就是遵循这个

![](img/fd8e545bbed4a311fcdfbde38cb78a0e.png)

+ Trusted path：唯一的获得正确的信息的路径

![](img/247ea8dadb6ea72f460da86d1637a9df.png)

+ 分层

![](img/4b6d6b602766cda31f22503512d1db32.png)

## Our work

![](img/deb37b7fcd14af6a921b639955d15d1c.png)

### `TZ-RKP`

![](img/446a80aab86b0aba9a114ca510f50bce.png)

![](img/932548658a4d80054a0e315c1e0cf807.png)

支持`trustzone`，就分成了两块隔离环境

+ Secure world
+ Normal world
+ linux跑在normal world，因为linux有很多问题

![](img/867bafa82211fdeee3251a61fcb6761d.png)

页表还是在normal，但是操作都在secure world上操作，你如果去进行操作需要去secure world上操作，会进行一些检查

+ TrustZone world切换太慢，影响性能
+ 增加了TrustZone的可攻击面
+ RKP放在了TrustZone里

### `SKEE`

![](img/bf5c5e61198a24a96508a0ecab0f907e.png)

内核本身只有一个页表

![](img/1cbb4d83d28be889ba19db33a5253f6b.png)

用两个页表，实现隔离

![](img/a314064967c3a452bb2e5f057c6430a0.png)

### `RegVault`

![](img/17e5d5129f7d6af43fe8ff340e41b27c.png)

![](img/347a4404cdeedbc9448c487522855aea.png)

+ 敏感数据和其他数据混杂存储，无法隔离保护
+ 物理分离
+ 逻辑分离：加密
+ 硬件加密

![](img/2b96a670b0fc90fae931f9cc37f41b5e.png)

仅在寄存器里面是解密的

# 7 虚拟化

https://coolshell.cn/articles/22320.html
