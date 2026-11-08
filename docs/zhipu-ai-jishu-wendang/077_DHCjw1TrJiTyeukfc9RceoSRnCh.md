# 1.5 模型 · CogVideoX 系列

> 来源：[https://ai.feishu.cn/docx/UpZJdQnhuoWnIAxGrLycKaIpnSc](https://ai.feishu.cn/docx/UpZJdQnhuoWnIAxGrLycKaIpnSc)

> CogVideoX 官方仓库：https://github.com/THUDM/CogVideo

>

> 无需下载和配置，点击下方即可在线体验：

> CogVideoX体验中心地址：CogVideoX Huggingface Space

# 模型介绍

**CogVideoX**是智谱开放平台中最新上线的视频模型，现已支持文生视频、图生视频多个能力，让用户可以在开放平台使**用和调用视频模型能力，轻松**高效地完成**艺术视频创作。体验中心支持多种**生成方式，包括文本生成视频、图片生成视频，可应用于广告制作、电影剪辑、短视频制作等领域。

CogVideoX-5B 是中质量较高视频生成模型，提升了视频生成的质量与视觉效果，适合对生成效果有更高要求的用户。相较于入门级的 CogVideoX-2B，该模型具有更大的规模，支持更高精度的推理和优化，能够在保持高生成质量的同时减少资源消耗。

针对内容连贯性问题，智谱AI研发了一套高效的三维变分自编码器结构（3D VAE）。该结构能够将原始视频数据压缩至原始大小的2%，显著降低了视频扩散生成模型的训练成本和难度。结合3D RoPE位置编码模块，该技术有效提升了在时间维度上对帧间关系的捕捉能力，从而建立了视频中的长期依赖关系。

在可控性方面，智谱AI打造了一款端到端的视频理解模型，该模型能够为大量视频数据生成精确且内容相关的描述。这一创新增强了模型对文本的理解和对指令的遵循能力，确保生成的视频更加符合用户的输入需求，并能够处理超长且复杂的prompt指令。

我们的模型采纳了一种将文本、时间、空间三维一体融合的transformer架构。该架构摒弃了传统的cross attention模块，创新性地设计了Expert Block以实现文本与视频两种不同模态空间的对齐，并通过Full Attention机制优化模态间的交互效果。

# 生成案例

目前视频生成在众多模型厂商都存在很多解决方案，我们从社区中整理了一部分案例，也许你在寻找的解决方案也在其中一种

[社区支持](https://zhipu-ai.feishu.cn/wiki/NoSGwDagci9FNYkQVgBctmaan3e)包含了基本的提示词编写、风格转绘、姿态生成等可控的生成方案

# 文档布局

在下面的子文档中有模型的详细介绍，一步步从下载安装到进阶操作和辅助工具。您可以点击进入并查看具体的技术文档。

+   [模型下载和安装](https://zhipu-ai.feishu.cn/wiki/KbO8w8sdfi4rUpk3s8QceNJMnWg)

+   [模型推理](https://zhipu-ai.feishu.cn/wiki/NZ0PwiXoUieHgakT7JmcLrIcnWh)

+   用 Diffusers 进行视频生成推理

+   [用 SAT 进行视频生成推理](https://zhipu-ai.feishu.cn/wiki/Bpc3wLhPRieJ53kdGiocDzAPnRf)

+   [文生视频量化版本](https://zhipu-ai.feishu.cn/wiki/YQxIwMcKEirjBakmRdccCoArnpf)

+   用 Diffusers VAE 模型进行视频编码和解码

+   增强 CogVideoX 模型的提示

+   [Gradio 网页界面生成视频](https://zhipu-ai.feishu.cn/wiki/A0Cmwdn2viwEj4kVUKXcp5Gfnlh)

+   用 Streamlit 网页应用生成视频

+   [模型微调](https://zhipu-ai.feishu.cn/wiki/X3BdwLErgi8k5Hk4qGrcJ1mrnQe)

+   [用 SAT 进行模型微调](https://zhipu-ai.feishu.cn/wiki/ZluWwNNC1iB46TkrWoMc7IGfnud)

+   [Lora微调CogVideoX详细教程](https://zhipu-ai.feishu.cn/wiki/OjIDwMEKniIby1kHQa4cMKibnhP)

+   [辅助工具](https://zhipu-ai.feishu.cn/wiki/K1GOwMtaFiuCtNk1AsscntEBnyc)

+   [用 Huggingface Diffus](https://zhipu-ai.feishu.cn/wiki/Sz3Nw22HdisvwJkcKRXcVASHnkh)ers Pipeline 转换并文生视频

+   [生成视频的描述性文本](https://zhipu-ai.feishu.cn/wiki/ZboewkrNqiCNv0kA8R8c15E3n5d)

# 模型信息

| **模型名称** | **CogVideoX-2B** | **CogVideoX-5B** |
| --- | --- | --- |
| **模型描述** | 入门级模型，兼顾兼容性，运行和二次开发成本低 | 模型更大，视频生成质量更高，视觉效果更好。 |
| **推理精度** | **FP16* (推荐)**, BF16, FP32, FP8*, INT8, 不支持 INT4 | **BF16 (推荐)**, FP16, FP32, FP8*, INT8, 不支持 INT4 |
| 单 GPU 推理 VRAM 消耗量 | FP16: 使用 [SAT](https://github.com/THUDM/SwissArmyTransformer) 18GB / **使用 diffusers 12.5**GB*  INT8: 与 torchao 一起使用 diffusers 7.8GB* | BF16: 使用 [SAT](https://github.com/THUDM/SwissArmyTransformer) 26GB / 使用 diffusers 20.7GB* |
| 多 GPU 推理 VRAM 消耗量 | FP16: 使用 diffusers 10GB* | BF16: 使用 diffusers 15GB* |
| **推理速度**  (步骤 = 50, FP/BF16) | 单 A100: ~90 秒  单 H100: ~45 秒 | 单 A100: ~180 秒  单 H100: ~90 秒 |
| **微调精度** | **FP16** | **BF16** |
| 微调 VRAM 消耗量 (每个 GPU) | 47 GB (bs=1, LORA)  61 GB (bs=2, LORA)  62GB (bs=1, SFT) | 63 GB (bs=1, LORA)  80 GB (bs=2, LORA)  75GB (bs=1, SFT) |
| **提示词语言** | 英文* |  |
| **提示词长度限制** | 226 Tokens |  |
| **视频长度** | 6 Seconds |  |
| **帧率** | 每秒 8 帧 |  |
| **视频分辨率** | 720 x 480, 不支持其他分辨率 (包括微调) |  |
| **位置编码** | 3d_sincos_pos_embed | 3d_rope_pos_embed |
| 下载页面 (Diffusers) | [🤗 HuggingFace  ](https://huggingface.co/THUDM/CogVideoX-2b)[🤖 ModelScope  ](https://modelscope.cn/models/ZhipuAI/CogVideoX-2b)[🟣 WiseModel](https://wisemodel.cn/models/ZhipuAI/CogVideoX-2b) | [🤗 HuggingFace  ](https://huggingface.co/THUDM/CogVideoX-5b)[🤖 ModelScope  ](https://modelscope.cn/models/ZhipuAI/CogVideoX-5b)[🟣 WiseModel](https://wisemodel.cn/models/ZhipuAI/CogVideoX-5b) |
| **下载页面 (SAT)** | [SAT](https://github.com/THUDM/CogVideo/blob/main/sat/README.md) |  |

---

# 社区

**🛰️交流群：**

![](img/02228ea854a7cd56b188906315aa0f9d.png)

**Discord社区：**https://discord.com/invite/Ewaabk6s