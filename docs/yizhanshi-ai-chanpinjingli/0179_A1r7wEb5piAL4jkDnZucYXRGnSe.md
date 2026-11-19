
# 开源 · CogAgent：带 Agent 能力的视觉模型，免费商用

> 来源：`https://v11enp9ok1h.feishu.cn/docx/ReCrdjQAroqSsLx5FuAcvZtHnQf`

10月11日，我们发布了智谱AI新一代多模态大模型 CogVLM，该模型在不牺牲任何 NLP 任务性能的情况下，实现视觉语言特征的深度融合，其中 CogVLM-17B 在 14 个多模态数据集上取得最好或者第二名的成绩。

12月15日，我们再次升级。基于 CogVLM，提出了视觉 GUI Agent，并研发了多模态大模型 CogAgent。

其中，视觉 GUI Agent 能够使用视觉模态（而非文本）对 GUI 界面进行更全面直接的感知，从而做出规划和决策。

![](img/81935b8db3d097567735c25d06d36940.png)

而多模态模型 CogAgent，可接受 1120×1120 的高分辨率图像输入，具备视觉问答、视觉定位（Grounding）、GUI Agent 等多种能力，在 9 个经典的图像理解榜单上（含 VQAv2，STVQA, DocVQA，TextVQA，MM-VET，POPE 等）取得了通用能力第一的成绩，并在涵盖电脑、手机的 GUI Agent 数据集上（含 Mind2Web，AITW 等），大幅超过基于 LLM 的 Agent，取得第一。

为了更好地促进多模态大模型、Agent 社区的发展，我们已将 CogAgent-18B 开源至 GitHub 仓库（申请可免费商用），并提供了网页版 Demo。

+ 论文：`https://arxiv.org/abs/2312.08914` Demo：`http://36.103.203.44:7861/`

+ 代码：`https://github.com/THUDM/CogVLM`

## 模型

+ Huggingface：`https://huggingface.co/THUDM/cogagent-chat-hf`

+ 魔搭社区：`https://modelscope.cn/models/ZhipuAI/cogagent-chat`

## 一、视觉 GUI Agent

基于语言预训练模型（LLM）的 Agent 是当下热门的研究话题，具备良好的应用前景。但是，一个严重的问题是，受限于 LLM 的模态，它只能接受语言形式的输入。

以网页 agent 为例，WebAgent [3] 等工作将网页 HTML 连同用户目标（例如“Can you search for CogAgent on google”）作为 LLM 的输入，从而获得 LLM 对下一步动作的预测（例如点击按钮，输入文本）。

然而，一个有趣的观察是，人类是通过视觉与 GUI 交互的。

比如，面对一个网页，当给定一个操作目标时，人类会先观察他的 GUI 界面，然后决定下一步做什么；与此同时，GUI 天然是为了人机交互设计的，相比于 HTML 等文本模态的表征，GUI 更为直接简洁，易于获取有效信息。

也就是说，在 GUI 场景下，视觉是一种更为直接、本质的交互模态，能更高效完整提供环境信息；更进一步地，很多 GUI 界面并没有对应的源码，也难以用语言表示。因此，若能将大模型改进为视觉 Agent，将 GUI 界面以视觉的形式直接输入大模型中用于理解、规划和决策，将是一个更为直接有效、具备极大提升空间的方法。

对此，我们提出了多模态大模型 CogAgent，可以实现基于视觉的 GUI Agent。下图展现了其工作路径与能力。

![](img/c3c1782378065807a10ed379169dd9c4.png)

CogAgent 模型同时接受当前 GUI 截图（图像形式）和用户操作目标（文本形式，例如“search for the best paper in CVPR 2023”）作为输入，就能预测详细的动作，和对应操作元素的位置坐标。

## 二、模型结构

CogAgent 的模型结构基于 CogVLM [2]。

![](img/fba95b66be63b912da8543bccc4f1daf.png)

为了使模型具备对高分辨率图片的理解能力，可以看清 ~720p 的 GUI 屏幕输入，我们将图像输入的分辨率大幅提升至 1120×1120（以往的模型通常小于 500*500）。

然而，分辨率的提升会导致图像序列急剧增长，带来难以承受的计算和显存开销——这也是现有多模态预训练模型通常采用较小分辨率图像输入的原因之一。

对此，我们设计了轻量级的“高分辨率交叉注意力模块”，在原有低分辨率大图像编码器（4.4 B）的基础上，增加了高分辨率的小图像编码器(0.3 B)，并使用交叉注意力机制与原有的 VLM 交互。在交叉注意力中，我们也使用了较小的 hidden size，从而进一步降低显存与计算开销。

结果表明，该方法可以使模型成功理解高分辨率的图片，并有效降低了显存与计算开销。

![](img/2ca612bb11160514ee5d939f3f991041.png)

在消融实验中，我们比较了该结构与 CogVLM 原始方法的计算量。结果表明，当分辨率提升时，使用文中提出的方案（with cross-module，橙色）将会带来极少量的计算量增加，并与图像序列的增长成线性关系。

特别的，1120×1120 分辨率的 CogAgent 的计算开销（FLOPs），甚至比 490×490 分辨率的 CogVLM 的 1/2 还要小。在 INT4 单卡推理测试中，1120×1120 分辨率的 CogAgent 模型占用约 12.6 GB 的显存，相较于 224×224 分辨率的 CogVLM 仅高出不到 2 GB。

## 三、实验 GUI Agent 能力

在电脑、手机等 GUI Agent 的数据集上，CogAgent 具有较大的优势，大幅超过所有基于 LLM 的 Agent。

### 1、在网页 Agent 数据集 Mind2Web 上的性能

![](img/b4213a6b9706bbc0bb1f10cd293d6f33.png)

### 2、在手机 Agent 数据集 AITW 上的性能

![](img/a134ba43da96547a31138ee81e14f97b.png)

### 图像理解综合能力

CogAgent 在图像理解的综合能力也有相当的提升，在 9 个经典的图像理解榜单上（含 VQAv2，STVQA, DocVQA，TextVQA，MM-VET，POPE 等）取得了通用能力第一的成绩。

### 3、在 VQA 数据集上的通用性能，涵盖常识、OCR、图表、文档等方面：

![](img/7267e406ed7d1d3de4d5bce9b041d65b.png)

### 4、在多模态大模型通用榜单 MM-VET，POPE 上的结果：

![](img/2582e143c5eeb5013ca348e903e567bb.png)

下面我们展示一个《原神》场景的实例：

![](img/487e962297a4b36cc8d079e1a33a63c2.png)
