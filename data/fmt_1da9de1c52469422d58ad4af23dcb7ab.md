
# GLM-4（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/VDD6dGZ1Pop3tGxtu0OcudvhnEe](https://kq4b3vgg5b.feishu.cn/docx/VDD6dGZ1Pop3tGxtu0OcudvhnEe)

## 模型介绍

GLM-4 是一款由中国 AI 初创公司智谱 AI 开发的新一代基座大模型，于 2024 年 1 月发布。其性能被认为与 OpenAI 的 GPT-4 相当，特别适合需要复杂推理和多模态处理的场景。对于开发者来说，GLM-4 提供了多种变体和丰富的功能，使其成为构建对话系统、代码生成工具和多模态应用的理想选择。

而和此前ChatGLM3模型所不同的是，GLM-4不再采用开源模式，而是采用了OpenAI和Google大模型的在线大模型模式，即模型无需本地部署，而是通过联网的方式调用智谱算力中心的模型进行推理或微调，用户通过`API-KEY`进行身份验证，同时根据实际使用模型不同、以及不同的Token数量进行计费。

GLM-4 基于通用语言模型（GLM）架构，采用自回归空白填充训练方法。这种方法结合了掩码语言建模和自回归建模的特点，训练模型预测序列中的缺失部分。通过添加 2D 位置编码，GLM-4 能够灵活处理不同长度的空白，提升了其在自然语言理解和生成任务上的表现。

> **关键引用：**

+   [GLM-4 GitHub 仓库](https://github.com/THUDM/GLM-4)

+   [技术报告](https://arxiv.org/pdf/2406.12793)

+   Hugging Face 模型页面

+   [ModelScope 模型页面](https://modelscope.cn/models/ZhipuAI/glm-4-9b)

+   [WiseModel 模型页面](https://wisemodel.cn/models/ZhipuAI/glm-4-9b)

+   [GLM-4V-9B 模型页面](https://huggingface.co/THUDM/glm-4v-9b)

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️ 实际评测

+   ⭐️GLM-4在线大模型生态介绍（课件）

+   GLM4在线大模型技术生态（视频）

+   GLM4在线知识库与模型调用计费（视频）

+   GLM4账户注册与管理方法（视频）

+   GLM-4-9B开源模型介绍与深度解析（视频）

### ☑️ 部署调用

⭐️GLM-4 API调用流程 & 关键参数详解丨技术干货（课件）

+   GLM-4 API获取与环境部署（视频）

+   GLM-4 API调用流程 & 关键参数详解（视频）

⭐️GLM-4模型介绍与本地调用方法（课件）

+   GLM-4在线模型生态介绍（视频）

+   GLM-4 `API-KEY`获取与本地调用方法（视频）

⭐️[GLM-4模型参数详解（课件）](https://kq4b3vgg5b.feishu.cn/wiki/DYWMw41JSikDI6kcYoHcKPsInrh?from=from_copylink)

+   GLM-4本地调用方法详解（视频）

+   GLM-4 API全参数详解（视频）

+   GLM-4多角色对话系统与`Message`参数设置方法（视频）

⭐️GLM-4-9B开源模型零门槛部署与调用指南（课件）

### ☑️ 进阶之路

#### 1、Funciton Calling入门

⭐️GLM-4+Function+calling入门与实现（课件）

+   [GLM-4 函数调用功能实战 | ](https://www.bilibili.com/video/BV1Af421m7r1?p=1)Function Calling（视频）

#### 2、Agents开发入门

+   0基础入门Agents开发！GLM-4 Agents 入门全集（视频）

## ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/7b65bf0de82cf2062d6bdd773b5a1393.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流**氛围浓厚，**不定期开设硬核干货&前沿技术公开课噢**~

---
