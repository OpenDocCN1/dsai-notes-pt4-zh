
# 了解英伟达`NCCL`通信库一个视频就够 #大模型 #集合通信 #NCCL

> 来源：[https://agijuejin.feishu.cn/docx/CUiTdp53gobzJcxYQRLcc72rnMe](https://agijuejin.feishu.cn/docx/CUiTdp53gobzJcxYQRLcc72rnMe)

## 👀快看👀

> **作者：**[**ZOMI酱**](https://space.bilibili.com/517221395)

> **原文链接：**了解英伟达`NCCL`通信库一个视频就够 #大模型 #集合通信 #NCCL

> **代码链接**：`https://github.com/chenzomi12/AIFoundation`

---

## ⚡️摘要⚡️

> 介绍了 NVIDIA 的集合通信库 `NCCL` 的基本概念、初始化过程、通信架构、节点间通信以及通信协议。

---

## 🤖分集🤖

### 了解英伟达`NCCL`通信库一个视频就够 #大模型 #集合通信 #NCCL

`NCCL` 库作用与重要性。介绍其为 NVIDIA 开源用于多 GPU 及多机通信，包括初始化过程、通信架构、节点间通信及通信协议，最后总结其在大模型训练中的关键作用。

![](img/ef7b8f3efd29828fbe646f078af37098.png)

[了解英伟达`NCCL`通信库一个视频就够 #大模型 #集合通信 #NCCL.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Kh9AbxiSYohAj0xrRwrcQYF7nFh)

---

## ✏️课代表✏️

+   `NCCL` 是一个关键的集合通信库，它允许多个 GPU 或多个节点之间进行高效的通信，这对于大模型的训练和推理至关重要。

+   `NCCL` 的初始化过程涉及到 `workers` 和 `rank` 的概念，这些概念是理解 `NCCL` 如何在多个 GPU 和节点之间协调通信的基础。

+   通信架构的设计对于 `NCCL` 的性能至关重要，它包括对物理拓扑的感知、图搜索和图连接，以及如何选择最佳的通信路径和算法。

+   节点间的通信方式（如多轨连接和不同的网络拓扑）对于整体的通信效率有显著影响。

+   `NCCL` 提供了多种通信协议，每种协议都有其特定的特点和适用场景，例如 `LOL` 协议优化了小数据量的传输，而 `LL128` 协议提供了一个折中的方案，既保证了低时延又能够充分利用带宽。

+   随着集群规模的增加，网络的吞吐量也会随之增加，但同时也会遇到带宽使用率的瓶颈，这要求在设计和选择通信协议时必须考虑到这一点。

+   `GPUDirect RDMA` 技术能够显著提高跨节点的通信效率，通过直接在 GPU 之间传输数据，减少了 CPU 和主机内存的中转。

+   `NCCL` 库的开源特性使得社区能够了解其内部的工作原理，并且可以根据自己的需求进行定制和优化。
