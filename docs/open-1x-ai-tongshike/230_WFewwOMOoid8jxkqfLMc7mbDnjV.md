# 【零基础】千帆Appbuilder

> 来源：[https://datawhaler.feishu.cn/docx/GFngdEisto1qkjx29LxcbiNenIc](https://datawhaler.feishu.cn/docx/GFngdEisto1qkjx29LxcbiNenIc)

# AppBuilder简介

<https://datawhaler.feishu.cn/sync/AhGwdZTJQs5aZVbIafBc2cS8nsf>

# Step 0：热身，先来看看**应用示例**

> 通过他人已经完成的作品获得灵感，方便创作自己的应用

1.  [应用广场](https://console.bce.baidu.com/ai_apaas/appCenter)：由官方制作的应用合集

![](img/5ec55785fd180d3b2df7ccf0f33d86a6.png)

1.  [千帆杯常规赛&练习赛参赛作品墙](https://cloud.baidu.com/qianfandev/matchs)

![](img/76e5e361844169e38a5fade9dde4ca7d.png)

# Step 1：在[主页](https://console.bce.baidu.com/ai_apaas/dialogHome)通过对话创建应用

> 随意点击一个推荐提问，或输入需求，即可自动创建应用

![](img/ad86d3a79bce9ac13e749e920acb1263.png)

> 需注意：如果未点击应用就打开其他页面，主页的内容会被清空，

> 但在**【个人空间】**处可查看创建出的应用

+   在**【应用配置】**界面，还可以通过**【快速入门】**了解该界面的功能的基本使用攻略

+   三步骤：创建应用-体验调试-发布分享

![](img/4fca6d0db097deb66e9bcca3caa9114b.png)

+   也可以直接点击[**主页**](https://console.bce.baidu.com/ai_apaas/dialogHome)左上角的**【创建应用】**进行应用创建

+   通过**【AI自动生成配置】**的按钮填写需求，自动生成配置

# Step2：优化应用配置，调试应用效果

1.  在【我的应用】页面找到想编辑的应用，进入对应的【应用配置】页面

![](img/8a35c42373d837c02b2628d116beaff6.png)

![](img/eb2b87873bfc37236bd4455452750050.png)

1.  修改应用配置

1.  **应用介绍相关**：【头像】、【应用名称】、【开场白】、【推荐问】

> 应用设定-基本信息中的应用描述不会展示，仅展示开场白内容

![](img/d1b66dc64058f12c436a44c80010473a.png)

+   另外，可以注意到几乎每个配置元素，都有一个【AI优化】的按钮

![](img/946001ff19db936c9dae6aadb58f5014.png)

1.  **【核心】角色指令**，instruction，即Prompt

![](img/f62675dd96ed8acc8b5ea30b270d8cd7.png)

+   在**【角色指令区】**[2.【 0代码】应用调优：Prompt工程入门](https://datawhaler.feishu.cn/wiki/C6CiwV0LFi8xWQk8wI1c3w92nSg)可应用各种Prompt工程技巧，更多入门细节可参考

![](img/025880d772f903781b7b7282da69629d.png)

![](img/f6fd5fc19f15f6688207df6e89154dc7.png)

+   如果在【预览与调试】测试时发现效果并不满足需求，且不是很了解Prompt工程时，可直接【优化】

![](img/fadd90009e00d29a3f96b67d00eb5a10.png)

> 如果对优化效果不满意，还可【重新生成】或【进一步优化】

1.  **添加工具组件：**不限制添加数量，但官方建议最多4个，否则可能影响调用效果

![](img/1b4262983d4699a55226b3aec0bb7298.png)

![](img/16148dc7126c24445b349a9ef47fd6c5.png)

![](img/62ce86579921852ea4e934fd51dd0862.png)

在[【组件广场】](https://console.bce.baidu.com/ai_apaas/componentCenter)还可查看组件详情

1.  **知识库**：知识问答可圈定回复内容的范围，

![](img/edfd9467d64ac8a873c012db091b6601.png)

1.  **【重要】模型配置**：选择调用模型的方式

> 免费额度耗尽后，预览和调试对话，以及使用应用，均需要购买付费资源包抵扣

+   Appbuilder的应用，会调用两次模型服务，可分别配置所用的模型和多样性，多样性推荐值为0，多样性越高则模型每次输出内容的差异性更大，其取值等于 temperature 和 top_p 的值。

![](img/8196268e6856c8f586c45bb0b4256625.png)

![](img/26edad190cd1cd2bc1b3942919506412.png)

![](img/b4f5024f8e50187571145d3fa956a240.png)

但每次与应用对话，仅消耗一次免费应用资源（quota）

# Step3：发布应用

<https://datawhaler.feishu.cn/sync/VWJ9dA1X1sm4c0beKnkcDDBhn8Y>

# 随机掉落的知识点

## AI Agent基础概念

![](img/54aa54724451dba4177cfacf57d71bd1.png)

![](img/e308381be690ab4cfb69182ed30b0300.png)

![](img/4fc90142cf3d4a656c0f5bcc00bc4d87.png)

![](img/1739cc9dbecfd95bcfcb69725ad75f81.png)

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