
# 基于RAG，三分钟制作飞书数字分身

> 来源：[https://aiyanxishe.feishu.cn/docx/OHWgd3JbPo1wpcxzQiecfkaDnXg](https://aiyanxishe.feishu.cn/docx/OHWgd3JbPo1wpcxzQiecfkaDnXg)

## 程序员AI破局简介

> [《程序员AI破局指南》](https://t.zsxq.com/19EOflzkQ)由字节、百度、阿里等互联网大厂的AI业务相关高管共创，分享开箱即用、面向市场的AI编程知识和工具。
>
> 指南包括六个专栏：**AI编程实战、AI编程工具、AI高效办公、AI副业赚钱、AI提示词工程、AI私人助手**
>
> [程序员AI破局指南](https://t.zsxq.com/19EOflzkQ) 原价 599 元，限时特惠 199 元/年，5月1日恢复原价。
>
> 加入我们：`https://t.zsxq.com/19EOflzkQ`
>
> 购买后，添加群主🛰️加入AI研习社陪伴社群，限时领取超值福利。

## 飞书数字分身：小白也能轻松上手！

### 数字分身搭建

FastGPT 是一个基于 LLM 大语言模型的知识库问答系统，提供开箱即用的数据处理、模型调用等能力。同时可以通过 Flow 可视化进行工作流编排，从而实现复杂的问答场景！

GitHub地址: `https://github.com/labring/FastGPT`

体验地址: `https://fastgpt.run/`

#### AI知识库原理

核心流程图如下：

![](img/1686412221a55edb364846718d66168d.png)

1. 把私有数据进行切分后，使用模型进行向量化，然后存储到向量库中。
2. 用户提问时先在向量库中进行向量搜索，匹配相关联的 `n` 条数据。
3. 把用户的问题和向量库匹配结果格式化成一个 Prompt，发送给大模型，得到最终结果。

#### 知识库搭建

**1. 创建知识库**

默认使用 GPT 的 embedding 模型把文本转成向量（不懂什么是向量也不影响使用哈）。

还可以设置标签，方便知识库进行分类

![](img/9043f31ae667015b67dd6ee48c99a245.png)

**2. 导入私有数据**

支持丰富的导入方式。可以手动输入 QA，也可以从网络/本地文件中提取文本的方式导入

![](img/8066684e2e74e35957eaab0cd4e7c57b.png)

**3. 测试向量搜索**

**可以输入文本测试**向量库的搜索，下面 `0.8884` 就是文本和知识库片段的相似度。

![](img/c7a70cfbc0f1cd3c1238d9b6896b4b5a.png)

#### 应用搭建

**1. 创建应用**

我们选 **知识库+对话引导** 这个最经典的场景

![](img/3c6e69d7babb5b4b6e10ffca095ef1f4.png)

**2. 关联知识库**

关联刚创建的知识库，填写 Prompt 控制 GPT 的输出。

想要用好 GPT，重中之重就是 **Prompt**。由于篇幅限制后续逐渐展开

![](img/dd658626284c1a1096442ee5ebd69be9.png)

**3. 测试知识库问答**

这里就跟我们正常使用 GPT 一样，通过文本的方式对话，也支持聊天上下文。

同时还会显示向量搜索的结果，上下文情况，token 消耗，完整响应日志，帮助我们优化应用。

![](img/38dedd787f7d60ce95fe72b0965386c0.png)

## 飞书集成

由于 FastGPT 的 OpenAPI 功能和 GPT 的对齐，可以无需变更原来的应用即可使用 [FastGPT](https://doc.fastgpt.run/docs/workflow/intro) 上编排好的应用。API 使用，可参考[这篇文章](https://doc.fastgpt.run/docs/use-cases/openai/)。编排示例，可参考[高级编排介绍](https://doc.fastgpt.run/docs/workflow/intro)。

所以可以很方便的把 fastgpt 的对话能力通过 API，对接到飞书机器人中。

GitHub地址: `https://github.com/ConnectAI-E/Feishu-OpenAI`

**1. 获取 API 秘钥**

依次选择应用 -> 「API访问」，然后点击「API 密钥」来创建密钥。

![](img/1f9bf7c9c3238fb81d635c1f218c9bec.png)

利用刚复制的 API 秘钥加上 `AppId` 组合成一个新的秘钥，格式为：`API 秘钥-AppId`，例如：`fastgpt-z51pkjqm9nrk03a1rx2funoy-642adec15f04d67d4613efdb`。

**2. 服务部署**

推荐使用 Railway 一键部署：`https://railway.app/template/10D-TF?referralCode=oMcVS2`

参考配置文件：

![](img/e7a2fc4ebe60bef2fcc620dc7d4b3d0f.png)

fastgpt 集成**重点参数**：

```text
OPENAI_KEY=fastgpt-z51pkjqm9nrk03a1rx2funoy-642adec15f04d67d4613efdb
API_URL=https://fastgpt.run/api/openapi
```

**3. 创建飞书机器人**

1. 前往[开发者平台](https://open.feishu.cn/app?lang=zh-CN)创建应用，并获取到 `APPID` 和 `Secret`。
2. 前往应用功能-机器人，创建机器人。
3. 从 cpolar、serverless 或 Railway 获得公网地址，在飞书机器人后台的“事件订阅”板块填写。例如：
   - `http://xxxx.r6.cpolar.top` 为 cpolar 暴露的公网地址
   - `/webhook/event` 为统一的应用路由
   - 最终的回调地址为 `http://xxxx.r6.cpolar.top/webhook/event`
4. 在飞书机器人后台的“机器人”板块，填写消息卡片请求网址。例如：
   - `http://xxxx.r6.cpolar.top` 为 cpolar 暴露的公网地址
   - `/webhook/card` 为统一的应用路由
   - 最终的消息卡片请求网址为 `http://xxxx.r6.cpolar.top/webhook/card`
5. 在“事件订阅”板块，搜索三个词“机器人进群”、“接收消息”、“消息已读”，把他们后面所有的权限全部勾选。进入“权限管理”界面，搜索“图片”，勾选“获取与上传图片或文件资源”。最终会添加下列回调事件：
   - `im:resource`（获取与上传图片或文件资源）
   - `im:message`
   - `im:message.group_at_msg`（获取群组中所有消息）
   - `im:message.group_at_msg:readonly`（接收群聊中@机器人消息事件）
   - `im:message.p2p_msg`（获取用户发给机器人的单聊消息）
   - `im:message.p2p_msg:readonly`（读取用户发给机器人的单聊消息）
   - `im:message:send_as_bot`（获取用户在群组中@机器人的消息）
   - `im:chat:readonly`（获取群组信息）
   - `im:chat`（获取与更新群组信息）
6. **测试机器人问答**

   私聊机器人，或者群里艾特它，就可以基于导入的知识库进行回答啦。nice

   ![](img/b0758428e33b5706c430af724b501cf9.png)

## 总结

使用 **高级编排** 功能可以做出非常复杂应用，后续会出教程敬请期待

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
