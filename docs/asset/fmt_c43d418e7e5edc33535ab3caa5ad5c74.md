
# Stability AI 产品、模型与商业化分析

DeepFloyd AI Research 是 `Stability AI` 旗下的独立研发团队，团队成员为 `Stability AI` 的雇员。`Stability AI` 为其发放工资和提供研究资助基金。该团队近期发布了的全新开源模型 `DeepFloyd IF`，解决了文生图的两大难题：准确绘制文字、准确理解空间关系。`DeepFloyd IF` 第一次使得目前已经获得了 5.1k 星。

![](img/2e7a9c7bff892b313d5765f493703052.png)

![](img/56d41ba44e973b1cdd99860fe5c713e0.png)

## DeepFloyd IF 与相关产品

### 工作原理

准确绘制文字对文生图的商业化应用有很大价值：在此之前，文生图模型无法准确生成文字，但运用 `DeepFloyd IF` 可以将想要的文字放置在画面的任何地方，进一步打通了海报、插图、服装等渲染图的工作流。

![](img/a5bb530769d39b3b439625485f8acfb7.png)

![](img/00777096462f36d38e65bc76cafc1010.png)

### DeepFloyd IF demo

### Stable Animation

`Stability AI` 发布的 `Stable Animation SDK` 是基于文生图模型的动画领域尝试，使得用户可以实现 `prompt`、`prompt + 图像`、`prompt + 视频` 三种方式生成动画，该模型目前仅能通过 `SDK` 和付费 `API` 使用。

，时长00:58

但用户表示，`Stable Animation` 的生成的动画效果欠佳，仍不能达到 `Runway` 的标准，且门槛更高，`SDK` 及 `API` 必须通过 `Python` 脚本调用。

### StableStudio

`StableStudio` 是付费应用 `DreamStudio` 的开源版本，此举进一步表现了其开源的决心，但在产品维度并没有显著的创新。

![](img/71bcecabe573874de4775759671a48e6.png)

而其在公告中提到的即将推出的聊天界面，与几天前发布的 `StableVicuna` 所提到的界面一致。

## LLM 底座模型

### StableLM

`Stability AI` 4 月 20 日发布了可商用开源语言大模型 `StableLM`，目前包括 `3B` 和 `7B` 参数版本，接下来将推出 `15B`、`65B` 参数的版本。在 `CC BY-SA-4.0` 许可条款的约束下，开发者可以自由查看、使用、调整基础模型，用于商业或研究目的。

`StableLM` 基础模型建立在 `EleutherAI` 开源早期语言模型 `GPT-J`、`GPT-NeoX` 和 `Pythia` 套件的经验之上，基于 `The Pile` 构建了 1.5 万亿 `token` 的新数据集进行训练。团队称这个数据集使 `StableLM` 在会话和编码任务方面具有惊人的高性能。未来还将发布经过指令微调的模型，使用的数据集包括 `Alpaca`、`GPT4All`、`Dolly`、`ShareGPT` 及 `HH`，仅供研究使用。

但 `StableLM` 看起来又是一个营销胜于实际工作的例子。根据用户测评，与其他开源模型相比结果相当平庸，与 `GPT` 也相差甚远，其输出的真值几乎不存在：

• `stablelm-base-alpha-3b` 平均准确率为 25.6%；

• `flan-t5-xl(3B)` 平均准确率为 49.3%；

• `flan-t5-small(80M)` 平均准确率为 29.4%。

该模型明显缺乏对某些敏感内容的保护；而且在实际使用中，`Stability AI` 发布的模型中并不比 `LLaMa` 更开源：`StableLM` 的许可证实际上是 `ShareAlike` 许可证，需要在相同的许可下共享修改后的模型权重。模型还遇到了过度拟合的问题，可能是由于他们数据集中存在重复数据。

用户表示“很多这样的开源项目似乎只是试图追随和（糟糕地）重现 `OpenAI` 的突破，而不是试图超越它们。”

### StableVicuna

`Stability AI` 近日又发布了 `StableVicuna`，是旗下开源团队 `CarperAI` 的研究成果。`CarperAI` 专注人类偏好学习研究，从 `EleutherAI` 孵化并独立出来。团队声称 `StableVicuna` 是第一个开源的基于人类反馈的增强学习（`RLHF`）`LLM Chatbot`。`AI Chat` 模型的成功得益于两种训练范式：指令微调和通过人类反馈的强化学习（`RLHF`）。但因为 `RLHF` 的复杂性，在大多数模型中指令微调是在没有 `RLHF` 训练的情况下应用的。`StableVicuna` 是 `Vicuna v0 13b`（指令微调的 `LLaMA 13b` 模型）的进一步指令微调和 `RLHF` 训练的版本。

`Stability AI` 在公告中也发布了未来的产品界面：

![](img/897ccccf718822edbd3739e9647c2fe1.png)

![](img/87f449607923ed7508d91db25079e446.png)

## 04. 商业化与竞争

类似于 `MongoDB` 和 `Red Hat` 的模式，根据 `Emad` 的 `Twitter` 表示，“`Stable models` 是基于公开数据的每一种模型的基准开源版本。`Stability AI` 会通过合作伙伴（如 `AWS`）为客户的私有数据提供部门/商业许可的模型；也为大型公司、政府建立定制化模型。”

`Emad` 表示 `Stability AI` 有一个很快会产生巨大利润的商业模式，和美国以外的其他国家政府合作。但是他没有给出更多的细节，但他表示 `Stability AI` 不需要在硅谷，因为他将把技术带到整个世界：所有的国家广播公司、世界各地的家庭办公室。

具体的收入和成本拆分如下：

### 收入

#### 1. 服务大公司，提供定制模型和咨询服务

类似 `Huggingface`/`Adobe`，`Stability AI` 的核心业务是为大公司建立专门的团队，形成合作伙伴关系，出售模型（扩展和定制 `Stable Diffusion` 或其他大型生成模型，每个模型的报价约几千万美元），并为企业提供咨询服务，帮助大型公司和政府对 `AI` 模型的采用。

团队早期表示尤其聚焦于创意产业，为内容生产公司定制专属模型，这也与团队技术背景以及 `Stable Diffusion` 的适用场景紧密联系。`Emad` 在最近的采访中表示，正在为世界上最大的资产管理公司建立它们的专有模型。他表示，“最大的人工智能公司将是为大型企业提供大模型服务的公司。”

#### 2. 付费应用

孵化社区生态中的技术与应用，推出商业化版本，如以 `Stable Diffusion` 为基础的 `DreamStudio`，上线第一个月，收入就达到数百万美金，用户数量达到 180 万。

#### 3. API

通过提供开源模型的 `API` 收费，并提供增值服务。团队自己的具体尝试包括 `Photoshop` 插件，使用户可以在 `Photoshop` 中使用 `Stable Diffusion` 生成和编辑图像；以及 `Blender` 插件，可以生成纹理，从渲染图中生成 `AI` 视频等等。

4 月 13 日，`Stability AI` 发布 `Stable Diffusion XL（SDXL）`，通过 `API` 提供服务，为企业客户构建，擅长照片写实主义。团队表示 `XL` 版本使用了 23 亿个参数，远远超过 `SD V2.1` 的 9 亿个参数，所以生成的图像和构图细节处理更好。不过也有用户表示 `XL` 与原模型的模型设计、使用效果并没有明显差别。

### 成本

#### 1. 另一方面，`Stability AI` 作为开源生态的基础设施，为开源社区提供算力及资金支持是一笔极大的开支。

据说目前 `Stability AI` 拥有在 `AWS` 运行的、由 4000 多个 `Nvidia A100` `GPU` 组成的集群，用于训练包括 `Stable Diffusion` 的 `AI` 系统，导致其运营和云服务方面的支出超过了 5000 万美元。

#### 2. 另一方面，`Stability AI` 在全世界招募顶尖团队，并且给予其极高的自由，这使得养团队的成本也不容小觑。

### 竞争

#### 1. `Hugging Face`：开源模型的流量入口

首先，`Stability AI` 和 `Hugging Face` 设想的商业化路径都是把开源模型定制化部署到企业，但开源模型目前均依赖 `Hugging Face` 做 `demo`，是获取开源模型的必经之处，`Stability AI` 绕不开的场景。由于开源模型的特性，`Hugging Face` 也可以为企业提供基于 `Stable Diffusion` 的定制化服务。

而且 `Hugging Face` 上有更多符合各类企业定制化需求的模型，其丰富度也是 `Stability AI` 暂时无法企及的。因此在很长的一段时间里，`Stability AI` 都将与 `Hugging Face` 处于竞合的关系，由于其对 `Private Model Private Data` 市场的相同切入点，`Hugging Face` 也有着很完善的 `Diffuser` 库和 `app` 产品，将成为 `Stability AI` 开源 `AI` 基建的主要竞争对手。

#### 2. `MidJourney`：Gen AI 领域数据飞轮最强的产品

图像依然是 `Stability AI` 的主战场，因为开源模型最适合的场景还是类似 `Stable Diffusion` 等图像生成的中小模型，大模型开源的意义并不大。`Midjourney` 作为目前最成功的闭源模型-产品-社区一体化公司对 `Stable Diffusion` 形成了强烈的竞争关系。

普遍的观点是，`Midjourney` 的进化之快得益于其出色的产品设计和闭源属性带来的的数据飞轮。相比之下 `Stable Diffusion` 中大量的数据反馈由于其开源属性无法形成优化模型的反馈回路，`SD` 更多的体现在开源生态中的创新，如 `Lora/ControlNet`，但 `Midjourney` 同步受益于这些技术创新。具体内容可见我们之前关于 `Midjourney` 的研究。

但近期研究表明，`LLM` 领域使用精挑细选的数据做 `fine-tune`，比 `blindly` 的选更有用，在图片领域数据量级的重要程度也没这么高，够用即可。`Stable Diffusion 2.1` 之前的版本，只做了最基础的美学工作，没做风格化，`XL` 之后才开始加入风格；更注重通用性，`MJ` 做了较强的风格化处理，使得 `to C` 用户体感更好，但是我们注意到，`SD` 开源社区的 `Finetune`、`Alignment` 模型风格化后也能达到较好的效果，所以不排除 `Stability AI` 也有能力做到，只是重心不同。

## 05. 开源社区及关系

### 开源社区

目前，`Stability AI` 官网上包括 `Harmon.ai`、`CarperAI`、`OpenBioML`、`MedARC`、`DeepFloyd` 等开源社区及产品 `Clipdrop`，删除了 `EleutherAI` 和 `LAION` 这两个核心社区，但 `Stability AI` 与这两个社区仍有较为密切的合作关系。

![](img/6275a199077d0e1b704d6489869e4375.png)

开源社区简介及合作模式如下：

![](img/1e4c05b732f3d5523b12aed6fe44b36a.png)

### 关系与组织的边界

团队表示 `Stability AI` 是社区里面成长出来的公司，是开发者社区的一员，并驱动、赋能整个开发者社区更好的成长，但不是管理开发者社区，更不是从属关系。`Stability AI` 支持这些社区，比如说 `Laion`、`Clip ViT-H` 使用了 `Stability AI` 提供的算力进行训练，训练完成后开源，成为 `Stable Diffusion 2.0` 的基础编码器，`Stability AI` 再将 `Stable Diffusion` 进行商业化，所以是共生的关系。

### 开源与闭源

其实不同机构之间的 `AI` 研究并没有完全闭源，只有开源程度的区别：`Stability AI` 所理解的开源是完全暴露权重、底层架构、在用户可以在自己的消费级 `GPU` 上面去运行。但 `Meta`、`Google`、`OpenAI` 也并不是完全的闭源，也通过发论文，开源部分小模型的方式来促进 `AI` 研究的发展，这些论文的方法部分会被社区吸收使用，推动 `AI` 行业的进步。不过近期，`OpenAI`、`Google` 等大公司呈现出更加闭源的趋势。

• 推进 `AI` 行业发展的维度来看：不同于大语言模型，小模型尤其是文生图领域，整体发展似乎是基于开源模型的发展速度向前推进，现有的模型都不会和开源社区的内容有太大的差距，这也是开源的魅力。

• 安全的维度来看：或许发布开源模型可能被用于不光彩的事情，如钓鱼邮件或恶意攻击，但 `Stability AI` 认为，将模型开源，可以提高透明度和培养信任。研究人员可以对模型进行开放的、细颗粒度的访问，充分的验证模型性能，研究可解释性技术，识别潜在风险，并帮助开发保障措施，这是封闭模型无法做到的。

• 公司发展维度来看：开源和闭源的选择也取决于团队的特质，当创始人对未来发展有非常明确认知的时候，闭源是更高效的，比如 `OpenAI`；但是 `Emad` 没有技术背景，对未来没有明晰的认知，所以开源是 `Stability AI` 合理的战略选择。

## 06. 收购与合作

### 收购 `Clipdrop`

2023 年 3 月，`Stability AI` 官宣收购 `Init ML` 的 `Clipdrop`。`Clipdrop` 是成像应用套件，可以进行背景去除、光线调整、图像扩展等编辑。累计用户超过 1500 万，适用于 `iOS`、`Android`、`macOS`、`Windows` 等平台，曾六次获得 `Product Hunt` 的 “每日最佳产品” 奖。`Clipdrop` 目前作为 `Stability AI` 的全资子公司独立运营。

![](img/3d194d0812a39805dc8de6b74fe61999.png)

目前 `Clipdrop` 集成了 `Stable Diffusion XL`（基于模型做产品包装），并且发布了新产品 `Stable Diffusion Reimagine`，允许用户无限制地生成一张图片的多种变化。原理是用图像编码器取代了原有的文本编码器，不是根据文本输入来生成图像，而是从图像中生成图像。

![](img/329e4739d6b425ef1e061f6ec999669b.png)

集成 `Stable Diffusion XL`

![](img/c00c2442e3d0ed482f9efd2e61c418a7.png)

`Stable Diffusion Reimagine`

![](img/faba1a686c5b89bbb9e1a51387d82f2a.png)

可以生成不同视角、不同风格元素

### 与 `Krikey AI` 合作

2023 年 2 月，`Stability AI` 与 `Krikey AI` 合作推出了 `Text-to-Animation` 的 `AI` 工具，使用户能够在几分钟内通过输入文本短语生成动画，并把动画作为视频文件或 `fbx` 文件输出到社交媒体账户、`3D` 游戏引擎或电影编辑软件，还允许用户定制三维头像并生成 `AI` 动画。该生成 `AI` 动画工具结合了人工智能、计算机视觉和 `ML` 模型。

![](img/e8ff5b7ede6628844c4300db284bcba4.png)

近期 `Youtube` 视频效果

`Krikey AI` 的开发者工具还包括定制 `3D` 头像工具、`AR` 移动应用工具包等，使创作者能够生成动画，带入任何平台进行 `3D` 内容创作。`Krikey AI` 已与 `Snapchat`、`T-Mobile`、`Google Maps`、`The Ellen DeGeneres Wildlife Foundation`、`Sony Pictures Entertainment` 和 `the National Audubon Society` 合作，为品牌合作伙伴建立了定制的 `AR` 游戏，达到 500 多万用户。

`Krikey AI` `CTO` `Ketaki Shriram` 认为“这是关于如何创建 `3D` 内容的革命的开始。”生成性 `AI` 动画在电影、游戏、`Metaverse` 化身、社交媒体和云计算等行业中都有丰富的应用场景，`AI` 动画可能是下一个创造者经济的基础。但是虽然目前 `Krikey AI` 的 `Twitter` 保持了持续更新，但几乎没有用户关注，`Product hunt` 等平台上也找不到产品 `Review`，产品效果与市面上常见的 `Text-to-Code` 产品并没有明显优势。

### 与 `Amazon` 合作

`Stability AI` 与 `Amazon` 的合作包括 `AWS`、`SageMaker` 及 `Bedrock` 等几个方面。

#### 在算力层面

`Stability AI` 选择 `AWS` 作为首选云供应商，使用其云服务来建立和扩展其图像、语言、音频、视频和 `3D` 内容生成的 `AI` 模型。使用 `Amazon SageMaker`（端到端机器学习服务），`Stability AI` 在拥有数千个 `GPU` 或 `AWS Trainium` 芯片的计算集群上建立、训练 `AI` 模型，将训练时间和成本降低 58%。

`Stability AI` 与 `AWS` 的合作使世界各地的学生、研究人员、初创公司和企业都可以使用其开源工具和模型。据称，`Stability AI` 获得了来自亚马逊的超过 4000 个英伟达 `A100`，以 50% 的 `margin` 与 `AWS` 合作，为计算密集型模型提供服务。在此之前，`Stability AI` 计算资源来自创始人自己购买的 32 个 `GPU`。

#### 在模型层面

`Stability AI` 宣布与 `Amazon` 的 `Bedrock AI` 平台结成战略商业联盟，使 `Stability AI` 的模型在该平台上可用。`Bedrock` 为 `Stability AI` 的客户提供了灵活、可扩展的平台，并可以使用大量的工具。

除此之外，`Stability AI` 还与拥有 1.2 万部的电影资料库的印度的投资机构 `Eros Investments`（爱神投资）成立了合资企业；与 `Peter Gabriel` 合作推出名为 `#DiffuseTogether` 的系列 `AI` 动画挑战赛 等等。

## 07. 成功要素与风险

### 成功要素

#### 1. 为开源模型的商业模式能否跑通
