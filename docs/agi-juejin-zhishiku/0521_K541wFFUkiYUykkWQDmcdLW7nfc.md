# NCCL/HCCL 的基础 MPI 通信介绍！#大模型 #集合通信 #MPI

> 来源：[https://agijuejin.feishu.cn/docx/DjaTdTj3bom78IxCNhOcpUCXnEe](https://agijuejin.feishu.cn/docx/DjaTdTj3bom78IxCNhOcpUCXnEe)

# 👀快看👀

> **作者：**[**ZOMI酱**](https://space.bilibili.com/517221395)

> **原文链接：**[**NCCL/HCCL 的基础 MPI **](https://www.bilibili.com/video/BV1Di421h7u2/?spm_id_from=333.788&vd_source=5a54577a6bc7d876f6e5df88640c8e9f)通信介绍！#大模型 #集合通信 #MPI

> **代码链接**：https://github.com/chenzomi12/AIFoundation

---

# ⚡️摘要⚡️

> 主要介绍了 MPI（Message Passing Interface）的基础概念，以及它在集合通讯库中的作用和实现，包括点对点通讯、集合通讯以及相关的 API 和原语。

---

# 🤖分集🤖

### NCCL/HCCL 的基础 MPI 通信介绍！#大模型 #集合通信 #MPI

阐述 MPI 跨语言通信协议及与网络协议关系，介绍通信原语和集合通信 API，强调其在大模型系统重要性，提出 MPI 编程特点，作者认为了解 MPI 对理解现代集合通信库原理至关重要。

![](img/14cfda64de47350e725c55e6cff49fa6.png)

[NCCL_HCCL 的基础 MPI 通信介绍！#大模型 #集合通信 #MPI.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Oz7bbhxSnokMHtxQZ8rcnGdgngb)

---

# ✏️课代表✏️

+   MPI 是集合通信的基础：MPI 作为一个标准和协议，为集合通信库如 NCCL 和 HCCL 提供了基础 API 和原语，是理解现代集合通信库工作原理的关键。

>

+   MPI 与网络协议的关系：MPI 位于 OSI 模型的更高层次，依赖于底层的网络协议如 TCP/IP，它定义了通信的语言和接口，而不是具体的网络传输协议。

>

+   集合通信的重要性：在大模型系统中，集合通信的性能直接影响到整个系统的效率，MPI 提供了一系列原语来优化这一过程。

>

+   MPI 的实现与优化：不同的 MPI 实现采用了不同的优化策略，这些策略对于提高大规模并行计算的性能至关重要。

>

+   MPI 编程模型：MPI 编程模型基于计算与通信的迭代过程，强调了通信同步的重要性，并且允许开发者灵活地划分计算与通信任务。

>

+   MPI 在并行计算中的作用：MPI 不仅提供了同步和通信的手段，还允许开发者通过MPI_Init初始化进程，为并行计算提供了基础支持。