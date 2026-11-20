
# 【多模态大模型】`LiDAR-LLM`：探索大模型对lidar的理解力

> 来源：[https://agijuejin.feishu.cn/docx/X6Osdl0LToKPeTxTvhccT6M1n4f](https://agijuejin.feishu.cn/docx/X6Osdl0LToKPeTxTvhccT6M1n4f)

> 作者：[yeyan](https://www.zhihu.com/people/yeyan-67)

> 原文：[https://zhuanlan.zhihu.com/p/695971027](https://zhuanlan.zhihu.com/p/695971027)

## 1. 简介

之前介绍过 [`3d-llm`](https://zhuanlan.zhihu.com/p/655265840)和 [`point-llm`](https://zhuanlan.zhihu.com/p/655265840)，主要是基于室内点云做的研究，场景有限，具体参考链接。对于室外点云，场景更复杂，`Lidar-llm`则探索了大模型在室外lidar数据上的能力，主要基于`nuScenes`自动驾驶场景数据做研究。

## 2. 数据集

对于室外点云，目前还没有大批量的lidar-文本数据集，`lidar-llm`基于`nuScenes`数据集，基于视觉多模态大模型，构造lidar场景的文本描述。构造了`lidar-text`数据集，论文此处没有细说，大概意思应该是`nuScenes`的lidar和图像是配准的，可以利用lidar对应的图像信息，结合图像多模态大模型，生成对应的文本描述。主要利用`LLaMA-AdapterV2`（先生成文本描述）和`chatgpt-4`(再用`gpt4`细化和增强)生成，最终形成了`nu-Caption`(420K个lidar-文本对，其中349k用于训练，72k用于验证)和`nu-Grounding` 2个数据集(280K,其中232K用于训练，48K用于验证)。

![](img/cfbb528212a61e53b54e061c305d7f5c.png)

![](img/c3e973688a32d4ab1e0749336b57dacb.png)

同时，还利用了 [`nuScenes-QA`](https://link.zhihu.com/?target=https%3A//github.com/qiantianwen/NuScenes-QA)数据集，包含了5种类型问题(是否存在，计数，查询对象，查询状态，对比)。

![](img/64f1d1de7a5c66b1d51973aac3f2b42d.png)

## 3. 模型设计

![](img/c0dd8d7f7afd8e19bdfca4b9d7281312.png)

模型设计上，基本遵循图像多模态大模型的设计，先对点云提取特征，然后压平到`bev`空间，这里主要设计了一个`vat`模块，除了学习本身的`bev`特征，还增加了可学习的`queries`，如上图，分成6块，分别对应`nuScenes`的6个视图，可只学习单个视图的信息(如图中标红部分`left view`)。

训练[采用了3步训练:第一步训](https://link.zhihu.com/?target=https%3A//github.com/qiantianwen/NuScenes-QA)练场景的全局理解能力(`3d captioning`能力)；第二步训练局部的感知能力(`Perception`，利用`nu-Grounding`数据集)；第三步训练高级别的指令能力(利用`nuScenes-QA`数据微调)，增强模型的空间理解能力等。

## 4. 参考链接

+   GitHub - `Yangsenqiao/LiDAR-LLM` ，目前代码还未开源

+   [`lidar-llm`论文](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2312.14074)

+   yeyan：【多模态大模型】点云-语言模型`3d-llm`解析

+   [`NuScenes-QA`数据集](https://link.zhihu.com/?target=https://github.com/qiantianwen/NuScenes-QA)
