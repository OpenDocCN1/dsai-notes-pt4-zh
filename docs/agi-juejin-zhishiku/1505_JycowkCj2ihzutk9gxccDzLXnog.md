# 使用Coze（扣子）为👸号/企业🛰️接入：月之暗面 · Kimi聊天机器人（操作教程）

> 来源：[https://agijuejin.feishu.cn/docx/PvDEdaZnJo4pYrxIxLrc7iINnag](https://agijuejin.feishu.cn/docx/PvDEdaZnJo4pYrxIxLrc7iINnag)

> 作者：Aitrainee | 👸号：AI进修生

> 原文：https://mp.weixin.qq.com/s/E7zBXZB1PHTvC-fNb4Bo9A

![](img/1dea04e55b17840f0c08d367f9a3f077.png)

> 🌟 扣子是字节团队开发的，Kimi是月之暗面团队开发的；Coze国内版可以接入👸号/企业🛰️充当客服机器人。国外版可以免费使用GPT4模型。

C oze 国外 网址：https://www.coze.com/

![](img/2bedd77608cf3adfc6f29a74da7ae8ea.png)

Coze国内版网址：https://www.coze.cn/

![](img/406b7ab8a74267bc7a23feadccffbe03.png)

### **扣子国内版发布机器人**

#### **概述**

扣子是新一代一站式 AI Bot 开发平台。无论你是否有编程基础，都可以在扣子平台上快速搭建基于 AI 模型的各类问答 Bot，从解决简单的问答到处理复杂逻辑的对话。而且你可以将搭建的 Bot 发布到各类社交平台和通讯软件上，让更多的用户与你搭建的 Bot 聊天。

只有国内版能发布到🛰️生态环境

![](img/c3992070109489ea48c2847f0fbcf979.png)

#### **点击创建机器人**

![](img/15639b1842039aafe95284e81473882e.png)

![](img/fa3e67f683f899ccb257eee1dde5c096.png)

#### **选择KIMI·128k模型**

![](img/294227368547261e4b61b605d33191fa.png)

#### **填写人设**

Bot 的提示(人设与回复逻辑) 是一种自然语言指令，告诉大语言模型（LLM）执行什么任务。搭建 Bot 的第一步就编写提示词，为 Bot 设定身份和目标。Bot 会根据大语言模型对人物设定和回复逻辑的理解，来响应用户问题。因此提示编写的越清晰明确，Bot 的回复也会越符合预期。

![](img/b1d3e54292a79c55e0f80394218d9d9a.png)

#### **添加你想用的插件**

插件是一个工具集，一个插件内可以包含一个或多个工具（API）。

目前，扣子集成了超过 60 种类型的插件，包括资讯阅读、旅游出行、效率办公、图片理解等 API 及多模态模型。使用这些插件，可以帮助您拓展 Bot 能力边界。例如，在您的 Bot 内添加新闻搜索插件，那么您的 Bot 将拥有搜索新闻资讯的能力。

如果扣子集成的插件不满足您的使用需求，您还可以创建自定义插件来集成需要使用的 API。

![](img/8a57a3fc8dbee88c00c454bfe6700e4c.png)

#### **添加知识库（可选）**

上传👸号文章或资料作为👸号聊天机器人、企业🛰️客服助手，扣子的知识库功能支持上传外部数据（例如本地文件、实时在线数据），通过向量搜索来检索最相关的内容以回答用户的问题，这可以帮助你的 Bot 更精确地回答问题。

![](img/ed94140f4d74e374d2d6c46ff54b629e.png)

![](img/4a5d115ba9dae9bdb647994b57867cba.png)

#### **长期记忆**

总结聊天对话的内容，并用于更好的响应用户的消息。

![](img/a73d3e864b891cea1c22a97e614f5d23.png)

#### **调试预览**

用于发布前自检机器人效果

![](img/dbca57d68ebc04d28e101d968ee7d0c2.png)

#### **点击发布**

![](img/201610749e89c6a2025f94984c3be30e.png)

#### **选择发布平台 **

![](img/c366cfffe89276cdb3704514973b5526.png)

🛰️客服按照他的提示配置（企业🛰️）

详细：https://www.coze.cn/docs/guides/wechat_service_account

![](img/00c4fec9f3c06f4f321424f6b732f26d.png)

👸号的去后台开通开发权限复制ID到此，如图

详细： https://www.coze.cn/docs/guides/wechat_subscription

![](img/49403f6e33c2eb0a8fd8a8ad8283d92a.png)

至此，结束

**效果**

![](img/3c36cb1aed4e5699a3c169552d1147ed.png)

![](img/9dec24ca254a108ad2165a7edd545bfa.png)

![](img/f3138483ccff2d865e48645fb8f21230.png)

### **进阶**

多Agent模式下定制自己的工作流

用于完成复杂任务、增加稳定性， 工作流支持通过可视化的方式，对插件、大语言模型、代码块等功能进行组合，从而实现复杂、稳定的业务流程编排，例如旅行规划、报告分析等。

当目标任务场景包含较多的步骤，且对输出结果的准确性、格式有严格要求时，适合配置工作流来实现。

![](img/2d66e94cb12e1479988e4fe3ebb1955a.png)

![](img/a2594771e91660ab9ac6ee95278ae724.png)

官方文档： https://www.co ze.cn/docs/guides/wechat_subscription