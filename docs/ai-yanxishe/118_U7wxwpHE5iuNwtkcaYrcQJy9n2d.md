

# DeepSeek实战：30分钟搭建AI采购智能体应用

> 来源：[https://aiyanxishe.feishu.cn/docx/Yt7vdkW7ToRJ5Gxh9ZocMafinCb](https://aiyanxishe.feishu.cn/docx/Yt7vdkW7ToRJ5Gxh9ZocMafinCb)

# 本期使用平台

> DeepSeek：https://chat.deepseek.com/

> 网易CodeWave：https://sf.163.com/product/lcap?fromlcap=qzzu_COOPERATION_AIyxs

# 智能体效果展示

在采购管理系统中，增加智能体，可以进行交互问答+自动生成商品列表。

![](img/110ec350db4cdb4d71c013d5458c195e.png)

体验地址：https://agentsse-jystudy2.app.codewave.163.com/expressage/applyOrder

# 第一部分：DeepSeek大模型科普

## DeepSeek模型演进历程

2023年11月：开源 DeepSeek LLM 7B 和 67B 的 Base 和 Chat 模型

2024年2月：开源 DeepSeek Coder 系列模型

2024年2月：开源 DeepSeek Math 模型

2024年3月：开源 DeepSeek-VL 系列模型

2024年5月：开源 DeepSeek-V2 系列模型

2024年7月：开源 DeepSeek-Coder-V2 系列模型

2024年12月：开源 DeepSeek-V3 系列模型

2025年1月：开源推理模型 DeepSeek-R1

## DeepSeek-V3和DeepSeek-R1差异

![](img/428763c55802cc762461cf786e0600f1.png)

+ 核心能力及应用场景差异：V3通用型语言模型，擅长自然语言生成和多领域任务（如写作、多语言处理）；R1专注于逻辑推理领域，强化数学解题、代码生成等复杂推理能力。

+ 技术特性差异：V3强调训练效率，能以较低GPU资源实现高性能；R1采用"强化学习优先"策略，先培养推理能力再优化语言流畅度

+ 部署使用差异：API调用时，V3对应名称`deepseek-chat`，R1为`deepseek-reasoner`

# 第二部分：DeepSeek和CodeWave化学反应

## 为什么需要整合DS到CodeWave

应用智能化，开发独立AI应用，私域流量运营

## DeepSeek、Coze、CodeWave作用与关系

+ CodeWave：通用应用开发平台，可以集成coze、dify、DeepSeek等开发通用AI应用

+ Coze、Dify：大语言应用开发平台，工作流编排，智能体应用开发，运行在公域上

+ DeepSeek、豆包：大语言模型，是底层AI能力提供者

![](img/e8ecc2ec9f8235b122e921a3e5bd7331.png)

以采购管理系统为例，我们借助DeepSeek+网易CodeWave做了一个采购智能体，用户可以在系统里跟DeepSeek产生联动：打开右下角的采购智能体，与他直接对话。对话通过连接器发送到用户侧，用户即可得到采购建议。

比如，跟智能体说：“假设我是一个市场专员，接下来我要准备一个小活动，我想采购一些价值在100块钱左右的小礼品”，AI就会基于已有条件给出推荐。

# 实操部分：0基础教你开发更智能的采购管理系统

## 准备工作

+ 注册CodeWave：https://sf.163.com/product/lcap?fromlcap=qzzu_COOPERATION_AIyxs

+ 创建DeepSeek Key：https://platform.deepseek.com

![](img/848044200c8cd94ba601733081f815b6.png)

## 创建CodeWave项目

通过模板“采购管理系统”创建项目：

![](img/30ab01315221f25510e19ddb3352e5d5.png)

## 在项目中引入DeepSeek连接器

导入连接器：第一步，集成中心 - 导入连接器

![](img/5aa66e75252a6e50dcc41c10d2d72820.png)

第二步，资产市场搜索DeepSeek连接器并导入：

![](img/5384aeedf9e8d54520de05adbea15532.png)

第三步，点击添加，在弹框中填入DeepSeek平台获得的`API_Key`：

![](img/c1d5d6b0bb77cb5ef2a23e233a35b630.png)

这样就导入完毕了！任意逻辑中通过调用连接器`chat`操作即可与DeepSeek交互。此外，开发者也可以在逻辑线上对代码进行任意编辑，包含最简单的工作流编排。

![](img/71f08d8d9b71cdd268ba6c4619dcd955.png)

采购智能体这个界面为什么能做到这么智能？首先它有一个流式的内容输出；其次在回答完问题之后，它会直接填入左侧的列表表单中，而这个功能如果用高代码开发是非常费力的。

**而在网易CodeWave上**有一个好用的业务组件的功能：聊天对话框。这个组件可以直接以流式的方式直接输出DeepSeek的结果，所以对用户来说，会有非常类似平时使用大模型的直接对话的交互体验。

所有这些操作都不需要开发者写代码，只需要写一个调用和数据的处理接收代码。也就是让智能体在自然语言输出的结果基础上，变成一个让我们的程序可以识别的结构化的JSON数据。

## 知识储备

和DeepSeek交互主要通过[chat接口](https://api-docs.deepseek.com/zh-cn/api/create-chat-completion)，调用时我们发送消息并接收回复，这里补充一下所需的知识：

`RequestBody`：请求消息

必须的参数如下：

```json
{
  &quot;messages&quot;: [
    {
      &quot;content&quot;: &quot;You are a helpful assistant&quot;,
      &quot;role&quot;: &quot;system&quot;
    },
    {
      &quot;content&quot;: &quot;Hi&quot;,
      &quot;role&quot;: &quot;user&quot;
    }
  ],
  &quot;model&quot;: &quot;deepseek-chat&quot;,
  &quot;response_format&quot;: {
    &quot;type&quot;: &quot;text&quot;
  },
  &quot;stream&quot;: true
}
```

`ResponseBody`：响应消息

响应消息在`choices`中存放：

```json
{
  &quot;id&quot;: &quot;930c60df-bf64-41c9-a88e-3ec75f81e00e&quot;,
  &quot;choices&quot;: [
    {
      &quot;finish_reason&quot;: &quot;stop&quot;,
      &quot;index&quot;: 0,
      &quot;message&quot;: {
        &quot;content&quot;: &quot;Hello! How can I help you today?&quot;,
        &quot;role&quot;: &quot;assistant&quot;,
        &quot;reasoning_content&quot;:&quot;think&quot;
      }
    }
  ],
  &quot;created&quot;: 1705651092,
  &quot;model&quot;: &quot;deepseek-chat&quot;,
  &quot;object&quot;: &quot;chat.completion&quot;,
  &quot;usage&quot;: {
    &quot;completion_tokens&quot;: 10,
    &quot;prompt_tokens&quot;: 16,
    &quot;total_tokens&quot;: 26
  }
}
```

## 使用范例

下面我们把做一个情感专家机器人作为范例演示：

![](img/9ff55ab98acbc46cc70dc2353ccb8467.png)

**第一步构造消息列表**

![](img/97433fd5bdb4f2f7451aa37ba313c78e.png)

**第二步发送消息**

![](img/0efbf27f2be0a8aa764f6e4b51073cdd.png)

**第三步处理返回信息**

![](img/b856920c69de44290d2314f3651ca855.png)

## 采购案例开发思路

a. 在发送信息后调用连接器中的`chat`方法

![](img/d36132a77318d5a016eb75ecd6689ece.png)

b. 在连接器子逻辑`onMessage`中处理返回的流式信息

![](img/80da5f92ec0b5c17aa74a427decfd40c.png)

c. 在上述流失输出结束后`onClose`子逻辑中调用非流式接口进行数据转换

![](img/558d8c56e7557885f38f147d34983d04.png)

![](img/d7f8054fd606a7ffa6bdcd4156bf583d.png)

体验地址👉 CodeWave智能低代码开发平台_网易数帆

