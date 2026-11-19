
# 发布 · 新一代视频生成模型 CogVideoX（240726）

> 来源：[https://ai.feishu.cn/docx/IACzdbfpQoaDlexUQJrcHI5fnQf](https://ai.feishu.cn/docx/IACzdbfpQoaDlexUQJrcHI5fnQf)

![](img/6f1b7397ff15e7637d68c56543e2835f.png)

人类作为一个高度复杂的系统，其认知功能依赖于大脑各区域间的协同作业，这不仅涉及文字、语言的加工，还包括视觉理解与听觉处理等多个方面。

我们坚信，多模态感知与理解的整合和提升，与认知能力的发展密切相关。

作为致力于实现通用人工智能（AGI）的公司，智谱 AI 始终高度重视多模态技术的发展。自2021年起，智谱 AI 技术团队便开始着手布局包括 `text-2-img`、`text-2-video`、`img-2-text`、`video-2-text` 在内的多模态模型，并陆续研发并开源了 `CogView`、`CogVideo`、`Relay Diffusion`、`CogVLM`、`CogVLM-Video` 等多个先进模型。

在此，我们荣幸地宣布，对视频生成模型进行全新升级，并正式推出新一代产品——`CogVideoX`。

`CogVideoX`的核心技术特点如下：

针对内容连贯性问题，智谱AI自主研发了一套高效的三维变分自编码器结构（3D VAE）。该结构能够将原始视频数据压缩至原始大小的2%，显著降低了视频扩散生成模型的训练成本和难度。结合3D RoPE位置编码模块，该技术有效提升了在时间维度上对帧间关系的捕捉能力，从而建立了视频中的长期依赖关系。

在可控性方面，智谱AI打造了一款端到端的视频理解模型，该模型能够为大量视频数据生成精确且内容相关的描述。这一创新增强了模型对文本的理解和对指令的遵循能力，确保生成的视频更加符合用户的输入需求，并能够处理超长且复杂的 `prompt` 指令。

我们的模型采纳了一种将文本、时间、空间三维一体融合的 `transformer` 架构。该架构摒弃了传统的cross attention模块，创新性地设计了Expert Block以实现文本与视频两种不同模态空间的对齐，并通过Full Attention机制优化模态间的交互效果。

`CogVideoX` 模型目前已在智谱清言的PC端、移动应用端以及小程序端正式上线。所有C端用户均可通过智谱清言的AI视频生成功能「清影」（Ying），免费体验AI文本生成视频和图像生成视频的服务。

![](img/463c15422be3bbd1b598d4333ad08226.png)

（链接：`https://chatglm.cn/video`）

「清影」的主要特点如下：

1. 快速生成：仅需30秒即可完成6秒视频的生成。

1. 高效的指令遵循能力：即使是复杂的 `prompt`，清影也能准确理解并执行。

1. 内容连贯性：生成的视频能够较好地还原物理世界中的运动过程。

1. 画面调度灵活性：例如，镜头能够流畅地跟随画面中的三只狗狗移动，效果宛如专业摄影师的跟拍。

此外，我们在智谱大模型开放平台 `bigmodel.cn` 上也部署了「清影」。企业和开发者可通过API调用方式，体验并使用「清影」的文本生成视频和图像生成视频功能。

## DEMO

### 文生视频

#### 示例 01

`Prompt`：低角度向上推进，缓缓抬头，冰山上突然出现一条恶龙，然后恶龙发现你，冲向你。好莱坞电影风

生成：

![](img/675a69ecfd7d5dfb41f1be9a48442c5d.png)

[1.冰山恶龙.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZAz4b7QYFoBcNSxd4DGctMtJn7b)

#### 示例 02

`Prompt`：蘑菇变成小熊

生成：

![](img/a285b07c137802a3807b4e3e718bfe13.png)

[badd49fb-bbdc-593d-b6b2-d0af0b18d9c9_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZCEVb59GooDPAHx8vCmcUd8nnkd)

#### 示例 03

`Prompt`：末日来临，穿着宇航服的航天员缓缓朝着飞船走去，眼神中透露着对地球的不舍，注意细节刻画，废土风

生成：

![](img/03748a06db692bf895ebfabe2752dff5.png)

[3.末日宇航员.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/O1khbqPI5o4W68xglmicDMEQnwC)

### 图生视频

#### 示例 01

输入图片

![](img/13f60c289e9638df3194cd3d2cc0f803.png)

`prompt`：游戏场景在下雨，人物看起来非常担心

生成：

![](img/09394bc8bc789c45595d11135b4a7227.png)

[6.废土黑雨.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GGsibSMHAoWjtNxVZfsczeshnwe)

#### 示例 02

输入图片

![](img/3657f527085f99d5ef4594f62a2356d4.png)

`prompt`

男孩拿出一颗心形的红色毛线球在手掌心

生成

![](img/b6b68b20e7e7f7ec6bd46d25c2a7d552.png)

[ca7fa5e2-2771-533c-a7a2-934e4719a245_0.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TX1Mb4esLoKr13xDjNLciFJ3nwe)

#### 示例 03

输入图片

![](img/a46e697aeee48e2e0e22f317d13cae09.png)

`prompt`

水豚慵懒地用吸管喝可乐，扭头朝向相机

生成

![](img/684e652096f6fbb0b2560158a4bd2733.png)

[8.卡皮巴拉喝可乐.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/🐧HrbyCYhohSFtx0MOUcTrGNnbe)

我们已验证了 Scaling Law在视频生成领域的有效性。展望未来，我们将在持续扩大数据规模和模型规模的基础上，深入研究具有突破性创新的新型模型架构，以更高效地压缩视频信息，并更全面地融合文本与视频内容。

![](img/490b3dc8896f22d2a501c77352726082.png)
