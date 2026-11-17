
# 业界集合通信库XCCL大串烧(基本介绍)下篇 #大模型 #集合通信 #NCCL

> 来源：<https://agijuejin.feishu.cn/docx/LPTcdBT8UoFAllxT7R8ct0Bbndg>

## 👀 快看 👀

> **作者：** [ZOMI酱](https://space.bilibili.com/517221395)

> **原文链接：** [业界集合通信库XCCL大串烧(基本介绍)下篇 #大模型 #集合通信 #NCCL](https://www.bilibili.com/video/BV1bS421X7uN/?spm_id_from=333.788&vd_source=5a54577a6bc7d876f6e5df88640c8e9f)

> **代码链接**：`https://github.com/chenzomi12/AIFoundation`

---

## ⚡️ 摘要 ⚡️

> 介绍了业界不同公司的集合通讯库（`XCCL`），包括腾讯的 `TCCL`、英特尔的 `WCCL`、`AMD` 的 `RCCL` 以及 `meta` 的 `group`，并对它们的特性、优化点和网络拓扑进行了分析和比较。

---

## 🤖 分集 🤖

### 业界集合通信库 XCCL 大串烧 (基本介绍) 下篇 #大模型 #集合通信 #NCCL

探讨业界集合通讯库 `XCCL`。介绍腾讯 `TCCL` 基于 `NICO` 并针对腾讯云架构优化；英特尔 `WCCL` 是 `win open API` 一部分用于英特尔推理引擎；`AMD` 的 `RCCL` 开源支持特定网络拓扑；`meta` 的 `group` 集成在 `PYTORCH` 后端。最后对比各库，总结集合通讯库创新三方面。

![](img/abce95793c7e1a1a34c8a2be71568bc4.png)

[业界集合通信库XCCL大串烧 (基本介绍) 下篇 #大模型 #集合通信 #NCCL.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RO30bdk4Qo0REXxtjhZcfDbknPg)

---

## ✏️ 课代表 ✏️

+ 腾讯的 `TCCL`：基于 `NVIDIA` 的 `NICO`，专为腾讯云的新麦网络架构优化，提供高效的网络通信和自愈能力。

+ 英特尔的 `WCCL`：作为 `win open API` 的一部分，支持英特尔的 `NPU` 和低精度数据类型通信，主要用于推理引擎。

+ `AMD` 的 `RCCL`：开源的集合通讯库，支持 `AMD` 的 `infinity fabric`，提供与 `NICO` 相似的 `API`，并采用了自家的流概念 `HIP`。

+ `meta` 的 `group`：集成在 `PYTORCH` 后端，支持多种通信类型，推荐在 `CPU` 通信或以太网交换机使用。

+ 国内外 `CCL` 库的差异：国外的库通常自研并开源，而国内的库多基于 `NICO` 魔改，华为的 `HICO` 是完全自研并开源的。

+ 集合通讯库创新的方向：算法创新、网络传输层创新和针对特定硬件或网络拓扑的优化。
