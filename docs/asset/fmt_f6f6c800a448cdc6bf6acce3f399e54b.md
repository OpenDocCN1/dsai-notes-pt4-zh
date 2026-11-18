
# 奇绩大模型日报（6月 4日）

> 来源：[https://miracleplus.feishu.cn/docx/N9VbdfwKVoEWKNxW9qBcVinDn2g](https://miracleplus.feishu.cn/docx/N9VbdfwKVoEWKNxW9qBcVinDn2g)

## 资讯

### 再战Transformer！原作者带队的Mamba 2来了，新架构训练效率大幅度提升

https://mp.weixin.qq.com/s/31t6pJqcXrZDjT6XiJZC_g

自 2017 年被提出以来，Transformer 已经成为 AI 大模型的主流架构，一直稳居语言建模方面 C 位。但随着模型规模的扩展和需要处理的序列不断变长，Transformer 的局限性也逐渐凸显。一个很明显的缺陷是：Transformer 模型中自注意力机制的计算量会随着上下文长度的增加呈平方级增长。几个月前，Mamba的出现打破了这一局面，它可以随上下文长度的增加实现线性扩展。随着 Mamba 的发布，这些状态空间模型 (SSM) 在中小型规模上已经实现了与 Transformers 匹敌，甚至超越 Transformers。Mamba 的作者只有两位，一位是卡内基梅隆大学机器学习系助理教授 Albert Gu，另一位是 Together.AI 首席科学家、普林斯顿大学计算机科学助理教授 Tri Dao。Mamba 面世之后的这段时间里，社区反应热烈。可惜的是，Mamba 的论文却惨遭 ICLR 拒稿，让一众研究者颇感意外。仅仅六个月后，原作者带队，更强大的 Mamba 2 正式发布了。

![](img/d1f15e8d85ee591fc5296a38ec00512c.png)

### 多模态模型学会打扑克：表现超越GPT-4v，全新强化学习框架是关键

https://mp.weixin.qq.com/s/bAf-5NzOD3fdTwYzdKsELw

只用强化学习来微调，无需人类反馈，就能让多模态大模型学会做决策！这种方法得到的模型，已经学会了看图玩扑克、算“12点”等任务，表现甚至超越了GPT-4v。这是来自UC伯克利等高校最新提出的微调方法，研究阵容也是相当豪华：

+ 图灵奖三巨头之一、Meta首席AI科学家、纽约大学教授LeCun
+ UC伯克利大牛、ALOHA团队成员Sergry Levine
+ `ResNeXt()`一作、Sora基础技术DiT作者谢赛宁
+ 香港大学数据科学学院院长、UC伯克利教授马毅

![](img/0b852225751ada27910ad955f9ad60e1.png)

### Oculus创始人帕尔默·拉奇宣称自己正在研发一款全新的头显设备

https://news.nweon.com/121452

AWE USA 2024大会将于6月18日至6月20日在美国加州洛杉矶举行，而Oculus创始人帕尔默·拉奇日前宣布，他计划在本届活动正式宣布自己在研究的VR头显设备。

![](img/f1a52e35ce73fa020b1156258103be95.png)

### Karpathy点赞，这份报告教你如何用 LLaMa 3创建高质量网络数据集

https://mp.weixin.qq.com/s/luZGMG1RRUT4X_ckt8hsCQ

众所周知，对于 Llama3、GPT-4 或 Mixtral 等高性能大语言模型来说，构建高质量的网络规模数据集是非常重要的。然而，即使是最先进的开源 LLM 的预训练数据集也不公开，人们对其创建过程知之甚少。最近，AI 大牛 Andrej Karpathy推荐了一项名为 FineWeb-Edu 的工作。

![](img/c0541b4085bf1c88a9cee87ff43b956e.png)

### 单个4090可推理，2000亿稀疏大模型「天工MoE」开源

https://mp.weixin.qq.com/s/h5bxuWca65t3LsQwqGq-Og

在大模型浪潮中，训练和部署最先进的密集 LLM 在计算需求和相关成本上带来了巨大挑战，尤其是在数百亿或数千亿参数的规模上。为了应对这些挑战，稀疏模型，如专家混合模型（MoE），已经变得越来越重要。这些模型通过将计算分配给各种专门的子模型或「专家」，提供了一种经济上更可行的替代方案，有可能以极低的资源需求达到甚至超过密集型模型的性能。6 月 3 日，开源大模型领域又传来重要消息：昆仑万维宣布开源 2 千亿稀疏大模型 Skywork-MoE，在保持性能强劲的同时，大幅降低了推理成本。Skywork-MoE 基于此前昆仑万维开源的 Skywork-13B 模型中间 checkpoint 扩展而来，是首个完整将 MoE Upcycling 技术应用并落地的开源千亿 MoE 大模型，也是首个支持用单台 4090 服务器推理的开源千亿 MoE 大模型。让大模型社区更为关注的是，Skywork-MoE 的模型权重、技术报告完全开源，免费商用，无需申请。

![](img/2d2d3a110c73af6f22ca55d931b7e475.png)

### AI训练数据的版权保护:公地的悲剧还是合作的繁荣?

https://mp.weixin.qq.com/s/5D4lpqucMwWQF6VQpSe-iw

GPT-4o内置声音模仿「寡姐」一案闹的沸沸扬扬，虽然以OpenAI发布声明暂停使用疑似寡姐声音的「SKY」的语音、否认曾侵权声音为阶段性结束。但是，一时间「即便是AI，也得保护人类版权」这一话题甚嚣尘上，更刺激起了人们本来就对AI是否可控这一现代迷思的焦虑。近日，普林斯顿大学、哥伦比亚大学、哈佛大学和宾夕法尼亚大学共同推出了一项关于生成式AI版权保护的新方案，题为《An Economic Solution to Copyright Challenges of Generative AI》。

![](img/bf1d50f7ab1b8f4b86d420fc4250527f.png)

### 华尔街盘点全球芯片战！市场规模将达 1 万亿美元

https://mp.weixin.qq.com/s/cfUb4mysQIwO-K3qvaN3aQ

根据《华尔街日报》，随着全球芯片之争日益升级，到本世纪末，芯片行业的规模预计将翻一番，达到 1 万亿美元。各国政府加大力度，推动国内芯片生产，这些芯片为汽车、电子产品和人工智能等各行各业提供动力。全球各地的公司都在竞相加入这股热潮。据芯片行业咨询公司 International Business Strategies预测，到本世纪末，全球半导体收入预计将超过 1 万亿美元。 国内生产能力增强可能会使高度专业化的半导体供应链多样化，在某些地区，半导体供应链在某些工艺领域具有优势，而在其他领域则存在弱点。例如，美国公司在芯片设计的许多领域处于领先地位，而台湾、韩国和中国大陆的公司则在后期的生产和组装阶段占据主导地位。

![](img/7d6e4836e98d7ef7587666ee9833a10e.png)

## 推特

### 用任何声音制作歌曲：哼唱一首曲调，但要赋予它节奏布鲁斯。即将推出 VOL-3，

https://x.com/suno_ai_/status/1797635285368680826

用任何声音制作歌曲。即将推出 VOL-3:哼唱一首曲调,但要赋予它节奏布鲁斯(R&B)的感觉,由 Rebecca (Suno 产品经理)演唱。

![](img/9df4d5ea2498f78fcb5d2cda93bf16e2.png)

[ssstwitter.com_1717427541788.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IQEFbq2EVoCbrmxQBsLcZywGnHe)

### Tri Dao分享状态空间对偶理论框架：证明了许多线性注意力变体和状态空间模型是等价的

https://x.com/tri_dao/status/1797650443218436165

Tri Dao 和 Albert Gu 合作构建了一个丰富的状态空间对偶(State-Space Duality, SSD)理论框架,证明了许多线性注意力(linear attention)变体和状态空间模型(State-Space Models, SSMs)是等价的。由此产生的模型 Mamba-2 比 Mamba-1 更好、更快,并且在语言建模任务上与强大的 Transformer 架构相匹配。

SSD 理论通过与矩阵分解的联系,让我们可以为 SSM 推导出更快的算法,利用矩阵乘法作为基本运算。大多数 GPU 的浮点运算都是矩阵乘法,因此充分利用张量核心(tensor cores)对于提高运算速度至关重要。

SSD 还启发了 Mamba-2 模型的设计,引入了并行投影、头部结构,并绑定参数以创建"多输入 SSM",类似于"多值注意力"或"分组值注意力"。

此外,SSD 使得将许多最初为 Transformer 开发的系统优化更容易应用到 SSM 中,例如:

+ 张量并行和序列并行以扩展到更大的模型和更长的序列
+ 可变序列长度以加速微调和推理

Mamba-2 的 SSD 层比 Mamba-1 中的关联扫描(associative scan)要快得多,使得增加状态维度和改善模型质量成为可能。在 3B 参数规模下训练 300B tokens 后,Mamba-2 超过了同等规模的 Mamba-1 和旧的 Transformers。

最近的模型如 Jamba 和 Zamba 通过添加一些注意力层来提高模型质量。研究发现,将 4-6 个注意力层与 Mamba-2 层混合,可以超过 Transformer++ 和纯 Mamba-2。这表明注意力机制和 SSM 是互补的。

未来还有许多值得探索的方向,例如:

+ 在混合模型中,注意力层如何提高质量?
+ 如何在新硬件(如 H100)上优化 SSM 训练?
+ 如果不再使用键值缓存(KV cache),推理过程会有什么变化?

总之,这项研究揭示了注意力机制和 SSM 之间的深层联系,为构建更快、更好的语言模型提供了新的思路。

![](img/ad3df46dfa8ff82d65f59e6d6133053c.png)

### Bilawal Sidhu分享：深入解析"将神经辐射场视频烘焙成沉浸式分层深度图像"

https://x.com/bilawalsidhu/status/1797807189400104985

当苹果公司表示他们将推出 "空间媒体" 时,我真正期望的是这样的效果——具有六个自由度的 3D 视频。相反,Vision Pro 用户只得到了装在新瓶子里的旧酒(苹果的 MV-HEVC 格式中的 VR180 视频)。不过很巧的是,这里有一个基于 NeRF 的开源体积视频引擎,可以通过 WebXR 流式传输到 Vision Pro 和 Quest。哦,它也支持 Mac。这一切都来自一家名不见经传的公司 Lifecast(YC '21)。

让我们来深入了解他们的新论文 "将神经辐射场视频烘焙成沉浸式分层深度图像",该论文旨在证明在当今的硬件上,体积视频捕捉、编辑和回放是完全可能的。

中心方法:多视图视频 → NeRFs → LDIs

你也可以使用一个简单的 VR180 相机(例如佳能 R5C)或一个由 GoPro 组成的大球形相机来捕捉场景的 3D 视频,制作一些 NeRF,然后将其烘焙成基于分层深度图像(LDI)的视频格式。这有点像拿一个精心制作的雕塑(NeRF),然后创建一个简化的 3D 打印版本(LDI),它捕捉了原始作品的本质,但更易于分发和展示。根据论文,它是一个 3 层 LDI——这不是超高层数,但与平面立体相比仍然是一个相当大的飞跃。然而,作者坚持只使用 3 层的原因非常实用。

为什么 3 层是体积视频的最佳选择

事实证明,你可以轻松地将 1920x1920 分辨率的 3x3 网格层打包成一个 5760x5760 的图像——这是大多数带有移动芯片的 VR 头显可以解码的最大分辨率视频。此外,论文使用等角(相对于等矩形)投影,这使得中心区域的像素密度与 "8K" 分辨率的 180° VR 视频相当。在我看来,这是一个相当不错的选择。

事情进展很快!然而,旧的想法常常找到新的归宿

我对看到 "LDIs" 和 "等角投影" 被用来烘焙这些 4D 辐射场感到低调兴奋。大约在 2017-2020 年,我在谷歌的团队(@broxtronix 等人)使用完全相同的方法来实现光场视频和 YouTubeVR 上的高分辨率视频流。

4 年后的这篇论文与 Deep View Video 论文相比,速度提高了 238 倍,而且该流水线可以在单个 GPU 工作站上运行,而不是在谷歌数据中心运行,这一点非常疯狂。

结束语

在我看来,对于复杂的主题,视觉保真度还不够——例如,3 层只能让你达到一定程度。但这篇论文/流水线向我证明,在移动硬件上进行高分辨率体积视频流式传输绝对是可能的。像这样一个更简单的 3 层格式也可以使体积视频在现有工具如 Premiere/Resolve 中的编辑比我们预期的要容易得多。

![](img/ef7adc2d8065bdd1947443f9a90a362d.png)

[ssstwitter.com_1717472601980.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZD39bzr0AosrnfxvrGdcYOAYnDf)

### 对话Near Protocol，Opentensor和SaharalabsAI：AI和Web3的交集

https://x.com/PanteraCapital/status/1797657047162622306

与我们一起探索 #AI 和 #Web3 的交集,参与一场由该领域的三位先驱者的对话。

时间:美国东部时间 6 月 4 日中午 12 点

演讲者:

@NEARProtocol | @ilblackdragon

@opentensor | @const_reborn

@SaharaLabsAI | @xiangrenNLP

在此注册:https://panteracapital.com/future-conference-calls/

![](img/a04d56cb94cfbd991ac583439465c7fe.png)

### ElevenLabs分享：将 Jensen Huang 精彩的 Computex 主题演讲的部分片段用 AI 配音成普通话

https://x.com/elevenlabsio/status/1797655062350565433

我们与 @nvidia 合作,通过将 Jensen Huang 精彩的 Computex 主题演讲的部分片段用 AI 配音成普通话,帮助他扩大了演讲的影响力。此外,我们很高兴能够合作,通过将我们高质量的 AI 语音引入 NVIDIA ACE NIM 微服务,为视频游戏创建动态的非玩家角色(NPC)。

阅读更多:https://elevenlabs.io/blog/nvidia-ace-at-computex/

![](img/fd67c5b4bba4694e14d91a11fbfd0ea3.png)

[ssstwitter.com_1717488634262.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QmTubaE7UovBZmxDTDGcYbsPnVh)

### Awni Hannun分享MLX教程：基础知识、神经网络、微调等

https://x.com/awnihannun/status/1797750024639787045

以下是一些指导性的演练,帮助学习 MLX:

+ 基础知识,使用 MLX 核心进行线性回归:

https://ml-explore.github.io/mlx/build/html/examples/linear_regression.html

+ 神经网络,在 MNIST 上训练多层感知机(MLP):

https://ml-explore.github.io/mlx/build/html/examples/mlp.html

+ 大语言模型(LLM)推理:

https://ml-explore.github.io/mlx/build/html/examples/llama-inference.html

+ 使用 MLX LM 进行 LoRA 微调:

https://gist.github.com/awni/773e2a12079da40a1cbc566686c84c8f

MLX 教程中还缺少什么?如果你有关于演练、教程或示例的想法,请告诉我。

![](img/ef1df4cc6c9040a00ca50ea13d6edf45.png)

## 论文

### D-CPT法则：面向领域的大语言模型持续预训练缩放定律

贡献人：

链接：http://arxiv.org/abs/2406.01375v1

持续预训练（CPT）在大语言模型（LLMs）上被广泛应用于扩展模型对特定下游领域（如数学和代码）的基本理解。对于特定领域LLMs的CPT，一个重要问题是如何选择通用语料库（例如Dolma，Slim-pajama）和特定领域语料库之间的最佳混合比例。现有方法通常通过在一组混合比例上进行网格搜索来采用费时费力的人力工作，这需要高GPU训练消耗成本。此外，我们无法保证所选比例对特定领域是最佳的。为解决现有方法的局限性，受性能预测的Scaling Law启发，我们提出了调查特定领域持续预训练（D-CPT Law）的Scaling Law来确定不同大小的LLMs的可接受训练成本的最佳混合比例。通过拟合D-CPT Law，我们可以轻松预测任意混合比例、模型大小和数据集大小的通用和下游性能，使用有限实验中的小规模训练成本。此外，我们还将标准的D-CPT Law扩展到跨领域设置，并提出了交叉域D-CPT Law来预测目标领域的D-CPT Law，在目标领域只需要非常小的训练成本（约为正常训练成本的1%）。对六个下游领域的全面实验结果展示了我们提出的D-CPT Law和交叉域D-CPT Law的有效性和泛化性。

![](img/ad3a8b94338e256baadbd99d13acc43e.png)
