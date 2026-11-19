
# 1.5 模型 · CogVideoX 系列

> 来源：[https://v11enp9ok1h.feishu.cn/docx/SBsBdMfFLoii2bxqKoncLCCEnJd](https://v11enp9ok1h.feishu.cn/docx/SBsBdMfFLoii2bxqKoncLCCEnJd)

<https://v11enp9ok1h.feishu.cn/sync/PaIIdslbxsIJUIbkTfPcsMCFnph>

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

> `CogVideoX` 官方仓库：https://github.com/THUDM/CogVideo

>

> 无需下载和配置，点击下方即可在线体验：

> `CogVideoX`体验中心地址：`CogVideoX` Huggingface Space

## 模型介绍

`CogVideoX`是智谱开放平台中最新上线的视频模型，现已支持文生视频、图生视频多个能力，让用户可以在开放平台使用和调用视频模型能力，轻松高效地完成艺术视频创作。体验中心支持多种生成方式，包括文本生成视频、图片生成视频，可应用于广告制作、电影剪辑、短视频制作等领域。

`CogVideoX-5B` 是中质量较高视频生成模型，提升了视频生成的质量与视觉效果，适合对生成效果有更高要求的用户。相较于入门级的 `CogVideoX-2B`，该模型具有更大的规模，支持更高精度的推理和优化，能够在保持高生成质量的同时减少资源消耗。

针对内容连贯性问题，智谱AI研发了一套高效的三维变分自编码器结构（`3D VAE`）。该结构能够将原始视频数据压缩至原始大小的2%，显著降低了视频扩散生成模型的训练成本和难度。结合`3D RoPE`位置编码模块，该技术有效提升了在时间维度上对帧间关系的捕捉能力，从而建立了视频中的长期依赖关系。

在可控性方面，智谱AI打造了一款端到端的视频理解模型，该模型能够为大量视频数据生成精确且内容相关的描述。这一创新增强了模型对文本的理解和对指令的遵循能力，确保生成的视频更加符合用户的输入需求，并能够处理超长且复杂的`prompt`指令。

我们的模型采纳了一种将文本、时间、空间三维一体融合的`transformer`架构。该架构摒弃了传统的`cross attention`模块，创新性地设计了`Expert Block`以实现文本与视频两种不同模态空间的对齐，并通过`Full Attention`机制优化模态间的交互效果。

## 生成案例

[361438140-cf5953ea-96d3-48fd-9907-c4708752c714.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Rs48b0VmKoY3fxxl6PZcWV20nYf)

**Prompt:** A garden comes to life as a kaleidoscope of butterflies flutters amidst the blossoms, their delicate wings casting shadows on the petals below. In the background, a grand fountain cascades water with a gentle splendor, its rhythmic sound providing a soothing backdrop. Beneath the cool shade of a mature tree, a solitary wooden chair invites solitude and reflection, its smooth surface worn by the touch of countless visitors seeking a moment of tranquility in nature's embrace.

[361438175-7db2bbce-194d-434d-a605-350254b6c298.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/VWy2b1kRHop2QMxuBB9cB2WTnPh)

**Prompt:** An elderly gentleman, with a serene expression, sits at the water's edge, a steaming cup of tea by his side. He is engrossed in his artwork, brush in hand, as he renders an oil painting on a canvas that's propped up against a small, weathered table. The sea breeze whispers through his silver hair, gently billowing his loose-fitting white shirt, while the salty air adds an intangible element to his masterpiece in progress. The scene is one of tranquility and inspiration, with the artist's canvas capturing the vibrant hues of the setting sun reflecting off the tranquil sea.

![](img/4a3ef8fe08d3d0de1aa9679e349eb4b0.png)

[361438198-30894f12-c741-44a2-9e6e-ddcacc231e5b.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ERbebBFhPoJEjlxLsnUcK26nnbg)

**Prompt:** On a brilliant sunny day, the lakeshore is lined with an array of willow trees, their slender branches swaying gently in the soft breeze. The tranquil surface of the lake reflects the clear blue sky, while several elegant swans glide gracefully through the still water, leaving behind delicate ripples that disturb the mirror-like quality of the lake. The scene is one of serene beauty, with the willows' greenery providing a picturesque frame for the peaceful avian visitors.

![](img/52182eb164b305c88a73518baca0e7e6.png)

[361438191-d78e552a-4b3f-4b81-ac3f-3898079554f6.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RmuBbV57Wo1khFxWUksc3KY5nlh)

**Prompt:** A golden retriever, sporting sleek black sunglasses, with its lengthy fur flowing in the breeze, sprints playfully across a rooftop terrace, recently refreshed by a light rain. The scene unfolds from a distance, the dog's energetic bounds growing larger as it approaches the camera, its tail wagging with unrestrained joy, while droplets of water glisten on the concrete behind it. The overcast sky provides a dramatic backdrop, emphasizing the vibrant golden coat of the canine as it dashes towards the viewer.

## 文档布局

在下面的子文档中有模型的详细介绍，一步步从下载安装到进阶操作和辅助工具。您可以点击进入并查看具体的技术文档。

+   [模型下载和安装](https://zhipu-ai.feishu.cn/wiki/KbO8w8sdfi4rUpk3s8QceNJMnWg)

+   [模型推理](https://zhipu-ai.feishu.cn/wiki/NZ0PwiXoUieHgakT7JmcLrIcnWh)

+   用 `Diffusers` 进行视频生成推理

+   [用 `SAT` 进行视频生成推理](https://zhipu-ai.feishu.cn/wiki/Bpc3wLhPRieJ53kdGiocDzAPnRf)

+   [文生视频量化版本](https://zhipu-ai.feishu.cn/wiki/YQxIwMcKEirjBakmRdccCoArnpf)

+   用 `Diffusers` `VAE` 模型进行视频编码和解码

+   增强 `CogVideoX` 模型的提示

+   [Gradio 网页界面生成视频](https://zhipu-ai.feishu.cn/wiki/A0Cmwdn2viwEj4kVUKXcp5Gfnlh)

+   用 `Streamlit` 网页应用生成视频

+   [模型微调](https://zhipu-ai.feishu.cn/wiki/X3BdwLErgi8k5Hk4qGrcJ1mrnQe)

+   [用 `SAT` 进行模型微调](https://zhipu-ai.feishu.cn/wiki/ZluWwNNC1iB46TkrWoMc7IGfnud)

+   [Lora微调`CogVideoX`详细教程](https://zhipu-ai.feishu.cn/wiki/OjIDwMEKniIby1kHQa4cMKibnhP)

+   [辅助工具](https://zhipu-ai.feishu.cn/wiki/K1GOwMtaFiuCtNk1AsscntEBnyc)

+   [用 Huggingface Diffus](https://zhipu-ai.feishu.cn/wiki/Sz3Nw22HdisvwJkcKRXcVASHnkh)ers Pipeline 转换并文生视频

+   [生成视频的描述性文本](https://zhipu-ai.feishu.cn/wiki/ZboewkrNqiCNv0kA8R8c15E3n5d)

## 模型信息

| **模型名称** | `CogVideoX-2B` | `CogVideoX-5B` |

| --- | --- | --- |

| **模型描述** | 入门级模型，兼顾兼容性，运行和二次开发成本低 | 模型更大，视频生成质量更高，视觉效果更好。 |

| **推理精度** | `FP16*` (推荐), `BF16`, `FP32`, `FP8*`, `INT8`, 不支持 `INT4` | `BF16` (推荐), `FP16`, `FP32`, `FP8*`, `INT8`, 不支持 `INT4` |

| 单 GPU 推理 VRAM 消耗量 | `FP16`: 使用 [`SAT`](https://github.com/THUDM/SwissArmyTransformer) 18GB / 使用 `diffusers` 12.5GB* `INT8`: 与 `torchao` 一起使用 `diffusers` 7.8GB* | `BF16`: 使用 [`SAT`](https://github.com/THUDM/SwissArmyTransformer) 26GB / 使用 `diffusers` 20.7GB* |

| 多 GPU 推理 VRAM 消耗量 | `FP16`: 使用 `diffusers` 10GB* | `BF16`: 使用 `diffusers` 15GB* |

| **推理速度**  (步骤 = 50, `FP`/`BF16`) | 单 A100: ~90 秒  单 H100: ~45 秒 | 单 A100: ~180 秒  单 H100: ~90 秒 |

| **微调精度** | `FP16` | `BF16` |

| 微调 VRAM 消耗量 (每个 GPU) | 47 GB (`bs=1`, `LORA`)  61 GB (`bs=2`, `LORA`)  62GB (`bs=1`, `SFT`) | 63 GB (`bs=1`, `LORA`)  80 GB (`bs=2`, `LORA`)  75GB (`bs=1`, `SFT`) |

| **提示词语言** | 英文* |  |

| **提示词长度限制** | 226 Tokens |  |

| **视频长度** | 6 Seconds |  |

| **帧率** | 每秒 8 帧 |  |

| **视频分辨率** | 720 x 480, 不支持其他分辨率 (包括微调) |  |

| **位置编码** | `3d_sincos_pos_embed` | `3d_rope_pos_embed` |

| 下载页面 (`Diffusers`) | [🤗 HuggingFace  ](https://huggingface.co/THUDM/CogVideoX-2b)[🤖 ModelScope  ](https://modelscope.cn/models/ZhipuAI/CogVideoX-2b)[🟣 WiseModel](https://wisemodel.cn/models/ZhipuAI/CogVideoX-2b) | [🤗 HuggingFace  ](https://huggingface.co/THUDM/CogVideoX-5b)[🤖 ModelScope  ](https://modelscope.cn/models/ZhipuAI/CogVideoX-5b)[🟣 WiseModel](https://wisemodel.cn/models/ZhipuAI/CogVideoX-5b) |

| **下载页面 (`SAT`)** | [`SAT`](https://github.com/THUDM/CogVideo/blob/main/sat/README.md) |  |

**Discord社区：**https://discord.com/invite/Ewaabk6s
