# FramePack本地部署教程

> 来源：[https://xodnytdcaw.feishu.cn/docx/Iw3JdLwG7oOsPTxez9qcYz4TnOd](https://xodnytdcaw.feishu.cn/docx/Iw3JdLwG7oOsPTxez9qcYz4TnOd)

上传一张图片，写一段提示词，即可生成最长2分钟视频，低显存显卡也能玩转AI视频生成，斯坦福团队发布FramePack新技术，仅需6GB显存高清视频。

### 1.软件安装

1.  CUDA 引擎：[点此下载](https://developer.nvidia.com/cuda-toolkit)

1.  N卡驱动：[点此下载](https://www.nvidia.cn/geforce/drivers/)

1.  一键安装包：[点击下载](https://github.com/lllyasviel/FramePack)

下载后解压，双击update更新，在双击run运行，运行后将从 HuggingFace自动下载模型，30GB 的数据。

### 2.硬件要求：

+   **显卡要求**：支持 FP16、BF16 精度，兼容 RTX 30、40、50 系列显卡（除 RTX 3050 4GB 外基本全覆盖）。RTX 20 系列及更早版本尚未验证，暂不支持 AMD 或 Intel GPU。

+   **系统支持**：兼容 Windows 与 Linux 系统，部署灵活。

+   **性能表现****：在 RTX 4090 上经** Teacache 优化后，生成速度可达每秒 0.6 帧，效率不俗。

### 3.使用场景

+   让美女跳舞

+   让美女举牌

+   让老照片微笑会站起来挥手