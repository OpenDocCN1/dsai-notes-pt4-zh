# 1. 【 0代码】千帆Appbuilder使用入门

> 来源：[https://datawhaler.feishu.cn/docx/JgIDdfITKowDNpxVSvKcWSKZnkf](https://datawhaler.feishu.cn/docx/JgIDdfITKowDNpxVSvKcWSKZnkf)

<https://datawhaler.feishu.cn/sync/DThjdz4ucsio9sbVLnKc0Gc2nHh>

# AppBuilder简介

<https://datawhaler.feishu.cn/sync/AhGwdZTJQs5aZVbIafBc2cS8nsf>

# Step 0：热身，先来看看**应用示例**

> 通过他人已经完成的作品获得灵感，方便创作自己的应用

1.  [应用广场](https://console.bce.baidu.com/ai_apaas/appCenter)：由官方制作的应用合集

![](img/7b6ce4b086049434df45c869743bafaf.png)

1.  [千帆杯常规赛&练习赛参赛作品墙](https://cloud.baidu.com/qianfandev/matchs)

![](img/db1db3ad87d2bdeb8b73453c07ad2c5c.png)

# Step 1：在[主页](https://console.bce.baidu.com/ai_apaas/dialogHome)通过对话创建应用

> 随意点击一个推荐提问，或输入需求，即可自动创建应用

![](img/f35e5dc3bb2435a446b67ea1193e9fc2.png)

> 需注意：如果未点击应用就打开其他页面，主页的内容会被清空，

> 但在**【个人空间】**处可查看创建出的应用

+   在**【应用配置】**界面，还可以通过**【快速入门】**了解该界面的功能的基本使用攻略

+   三步骤：创建应用-体验调试-发布分享

![](img/7f7edb7a3786e64644ea6b6b407c2a81.png)

+   也可以直接点击[**主页**](https://console.bce.baidu.com/ai_apaas/dialogHome)左上角的**【创建应用】**进行应用创建

+   通过**【AI自动生成配置】**的按钮填写需求，自动生成配置

# Step2：优化应用配置，调试应用效果

1.  在【我的应用】页面找到想编辑的应用，进入对应的【应用配置】页面

![](img/bfd4dda041b159bfe378cc6bcd5d1ddb.png)

![](img/ec0b0b0578e28351b54ee167a7aba779.png)

1.  修改应用配置

1.  **应用介绍相关**：【头像】、【应用名称】、【开场白】、【推荐问】

> 应用设定-基本信息中的应用描述不会展示，仅展示开场白内容

![](img/7c7434668f975e80f7eae9a386a15d77.png)

+   另外，可以注意到几乎每个配置元素，都有一个【AI优化】的按钮

![](img/2e6562e62b75fc421160102eab47bf48.png)

1.  **【核心】角色指令**，instruction，即Prompt

![](img/622db8f0228434f116575c2dbc368500.png)

+   在**【角色指令区】**[2.【 0代码】应用调优：Prompt工程入门](https://datawhaler.feishu.cn/wiki/C6CiwV0LFi8xWQk8wI1c3w92nSg)可应用各种Prompt工程技巧，更多入门细节可参考

![](img/e2acd0eb1e0f197cb7ba86a346825abd.png)

![](img/83c551597aa6e0f0d604c7cbc84912a5.png)

+   如果在【预览与调试】测试时发现效果并不满足需求，且不是很了解Prompt工程时，可直接【优化】

![](img/ba8a4ce35e4c26509ac4a7cc140134dc.png)

> 如果对优化效果不满意，还可【重新生成】或【进一步优化】

1.  **添加工具组件：**不限制添加数量，但官方建议最多4个，否则可能影响调用效果

![](img/e4ff824bf235b3d1d7745fa8014564ed.png)

![](img/27cd417fb05c3e6855e2ce8d97c9ba8f.png)

![](img/bf1b0fab847d076b94e07975d19d415b.png)

在[【组件广场】](https://console.bce.baidu.com/ai_apaas/componentCenter)还可查看组件详情

1.  **知识库**：知识问答可圈定回复内容的范围，

![](img/5c7924c80ede5352f8d9ce4e16f4b7d7.png)

1.  **【重要】模型配置**：选择调用模型的方式

> 免费额度耗尽后，预览和调试对话，以及使用应用，均需要购买付费资源包抵扣

+   Appbuilder的应用，会调用两次模型服务，可分别配置所用的模型和多样性，多样性推荐值为0，多样性越高则模型每次输出内容的差异性更大，其取值等于 temperature 和 top_p 的值。

![](img/f96cf2dead00a421ea21b02d8448e9ae.png)

![](img/c2cdc47c8f8b013e6001bf28a5cec1d3.png)

![](img/f780a08255157d9894571e3e8ccffecc.png)

但每次与应用对话，仅消耗一次免费应用资源（quota）

# Step3：发布应用

<https://datawhaler.feishu.cn/sync/VWJ9dA1X1sm4c0beKnkcDDBhn8Y>

# 随机掉落的知识点

## AI Agent基础概念

![](img/43576e29d86c5f2e7ade528eb012c543.png)

![](img/865180cfec1cb6c0526b25bfcdec0648.png)

![](img/d026d11ae0d32169b3aef62f2b8a6589.png)

![](img/2e28d2c99b0b494ed40190db45294c06.png)

https://arxiv.org/abs/2309.07864

OpenAI应用研究主管翁丽莲(Lilian Weng)撰写过一**篇blog: LLM Powered Au**tonomous Agents，将 Agents 定义为LLM + memory + planning skills + tool use，即大语言模型、记忆、任务规划、工具使用的集合。

其中，LLM是Agent的大脑，属于“中枢”模型，要求有以下3种能力：

1.  planning skills：对问题进行拆解得到解决路径，既进行任务规划

1.  tool use：评估自己所需的工具，进行工具选择，并生成调用工具请求

1.  memory：短期记忆包括工具的返回值，已经完成的推理路径；长期记忆包括可访问的外部长期存储，例如知识库

# ❓QA

<https://datawhaler.feishu.cn/sync/G7ZTdwcz9s0WjgbGxhnc7VSWnuo>

> 本篇的贡献者们都在很用心在为大家提供内容~ 期待大家给到点赞和反馈呀~

> 贡献者分别是： 、