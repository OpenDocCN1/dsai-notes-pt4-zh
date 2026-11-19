
# 03月15日 AI情报助手早报

> 来源：[https://v11enp9ok1h.feishu.cn/docx/ALpTd9KhMofLKnxw3nZcRIY6nlh](https://v11enp9ok1h.feishu.cn/docx/ALpTd9KhMofLKnxw3nZcRIY6nlh)

## 1️⃣ 🧠 [ICLR 2025 | 东方理工等提出「语境对齐」新范式，让大模型更懂时间序列](https://mp.weixin.qq.com/s/d4PfLM5jEmECAECc8ZSl2w)

东方理工大学等提出语境对齐（`Context-Alignment`）新范式，通过双尺度图结构将时间序列数据融入自然语言语境，增强大模型对时序数据的理解。

该方法不仅提升了大模型在时序分析任务中的能力，还在`Few-Shot`和`Zero-Shot`预测任务中超越了其他方法。

双尺度图结构包括细粒度和粗粒度图结构，通过图卷积网络更新节点嵌入，并利用交互机制传递信息。

`FSCA`模型结合了`Few-Shot` prompting技术，提升了模型在分类和预测任务中的性能。

`FSCA`在计算效率上优于基于词`token`对齐的方法，降低了计算复杂性，同时提升了模型性能。

关键词：语境对齐，时间序列，大模型，`Few-Shot`，计算效率

### 🔗 文中引用的链接

- [论文链接](https://openreview.net/forum?id=syC2764fPc)
- [代码链接](https://github.com/tokaka22/ICLR25-FSCA)

## 2️⃣ 🌏 当中国企业进入「国际化 3.0」，压力给到阿里云了

中国企业国际化进入新阶段，速度更快、“海外适配”投入更小，以产品和技术为核心形成“同心圆”国际化。

轻资产+“特长生”模式开始涌现，企业以技术、产品和审美为支点，以更高效率拥抱世界。

阿里云提供的云服务成为企业国际化3.0的“外骨骼支撑”，助力企业快速扩张和客户增长。

云原生时代，企业需要获得`SoC`的能力封装加持，大模型和云服务的本质是对`ICT`与软实力的封装。

阿里云在全球范围内服务大量客户，成为中国企业国际化3.0阶段的重要基础设施。

关键词：国际化3.0，云服务，阿里云，技术封装，企业全球化

### 🔗 文中引用的链接

- [阿里云](https://www.aliyun.com/)

## 3️⃣ 🌏 [`FP8`模型不再挑卡！`DeepSeek`推理成本减半速度翻番，清华团队开源「`Chitu`」推理引擎](https://mp.weixin.qq.com/s/Ev9x6AWGF4S6zQpPpWtg9g)

清华团队开源大模型推理引擎「`Chitu`」，实现非`H卡`设备运行原生`FP8`模型的突破。

`Chitu`引擎在`A800`集群上部署`DeepSeek-671B`，`GPU`数量减少50%，输出速度提升3.15倍。

国产大模型+国产引擎+国产芯片技术闭环加速形成，`Chitu`引擎提供国产开源新选择。

`Chitu`引擎特别聚焦实现`DeepSeek` `FP8`模型在存量英伟达`GPU`上的无损高效部署。

核心优势包括全场景性能优化、架构适应性，支持多元算力适配和全场景可伸缩。

关键词：大模型，推理引擎，`FP8`模型，国产芯片，开源

### 🔗 文中引用的链接

- [赤兔引擎代码](https://github.com/thu-pacman/chitu)

## 4️⃣ 🚀 出海应用也能享受高速稳定的`DeepSeek-R1`？亚马逊云科技出手了

亚马逊云科技宣布`DeepSeek-R1`正式作为全托管的无服务器模型提供，无需配置硬件资源或设置软件环境，一个`API`就能直接调用。

`DeepSeek-R1`结合自然语言处理与高级推理能力，性能对标`OpenAI` `o1`且开源，为AI应用打开新方向。

`Amazon Bedrock`提供多种形式的选择，包括`Marketplace`和`Custom Model Import`中的`DeepSeek-R1`模型支持以及全托管式`DeepSeek-R1`。

`Amazon Bedrock`的全托管式`DeepSeek-R1`在性能和稳定性上表现优异，`TTFT`和`TPOT`均优于`DeepSeek`官方`API`。

亚马逊云科技提供了多样化且方便实用的AI落地途径，`Amazon Bedrock`成为增长最快的云服务之一，支持超过50个`API`可调用的大模型。

关键词：`DeepSeek-R1`，亚马逊云科技，全托管模型，AI应用，性能优化

### 🔗 文中引用的链接

- [亚马逊云科技新闻博客](https://aws.amazon.com/blogs/)

## 5️⃣ 🧠 `MM-Eureka`：极少数据实现多模态推理的`R1-Zero`时刻

上海人工智能实验室等机构提出多模态学科推理模型`MM-Eureka`，复现`DeepSeek-R1`关键特性。

利用`54K`图文数据规则型`RL`训练，性能超`1M`数据的`MPO`模型，与`12M`数据`CoT` `SFT`训练模型相当。

极简`RL`设计和数据筛选策略下，实现稳定的`Accuracy Reward`和`Response Length`提升。

`MM-Eureka-Zero`仅用`8K`图文数学推理数据，在`K12`基准测试上超越指令模型8.2%。

开源全套数据、代码和模型，推动多模态推理发展。

关键词：多模态推理，`MM-Eureka`，数据效率，强化学习，开源应用

### 🔗 文中引用的链接

- [代码地址](https://github.com/ModalMinds/MM-EUREKA)
- [技术报告](https://arxiv.org/pdf/2503.07365)
- [模型地址](https://huggingface.co/FanqingM/MM-Eureka-Zero-38B)
- [数据集地址](https://huggingface.co/datasets/FanqingM/MM-Eureka-Dataset)

## 6️⃣ 🚀 [`CVPR 2025` | `VAST`和北航开源`MIDI`，从单张图像端到端生成三维组合场景](https://mp.weixin.qq.com/s/PE3JUGsHKjFWEiCJMg9tHQ)

北航和`VAST`等机构推出的`MIDI`模型，能从单张图像生成高几何质量、实例可分离的3D组合场景。

`MIDI`通过多实例扩散模型，同时生成具有精确空间关系的多个3D实例，提高了场景生成的效率和质量。

模型引入多实例自注意力机制，有效捕获实例间的空间关联和整体场景的连贯性。

`MIDI`在多个数据集上超越现有方法，在几何质量、空间关系和生成速度等多个维度上表现突出。

`MIDI`技术在建筑设计、虚拟现实、影视特效和游戏开发等领域展现了广阔的应用潜力。

关键词：3D场景生成，`MIDI`模型，多实例扩散模型，空间布局建模，AI技术应用

### 🔗 文中引用的链接

- [论文链接](https://arxiv.org/abs/2412.03558)
- [项目主页](https://huanngzh.github.io/MIDI-Page/)
- [代码仓库](https://github.com/VAST-AI-Research/MIDI-3D)
- [在线Demo](https://huggingface.co/spaces/VAST-AI/MIDI-3D)
