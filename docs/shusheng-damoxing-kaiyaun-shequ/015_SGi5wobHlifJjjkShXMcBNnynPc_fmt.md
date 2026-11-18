
# Intern-S1 玩转 Zotero，助力科研提效 (基本完结)

> 来源：[https://aicarrier.feishu.cn/docx/UaAfdBiNHoEWo6xudhQcmlpYntb](https://aicarrier.feishu.cn/docx/UaAfdBiNHoEWo6xudhQcmlpYntb)

相关链接：

[下载 Zotero 7](https://www.zotero.org/download/)、[Zotero 用户指南](https://zotero-chinese.com/user-guide/)、[Zotero 插件商店](https://zotero-chinese.com/plugins/)、[Zotero CSL 样式](https://zotero-chinese.com/styles/)、[Zotero 转换器](https://zotero-chinese.com/translators/)

## 1 快速上手 Zotero

Zotero 是一款免费、易于使用的工具，可以帮助您收集、管理、阅读、批注、引用和共享文献。

![](img/e5b67f8d4c4115a131eb50652495aa1a.png)

### 1.1 下载并安装 Zotero 7 及 Zotero Connector 插件

![](img/9be4597e88f2c5181e63c8f2df30abb2.png)

### 1.2 安装并使用 Awesome GPT 插件

插件链接：[Awesome GPT 项目主页](https://www.bilibili.com/video/BV17N4y1o7vx/)、下载 Awesome GPT 插件、Awesome GPT 用户指南、使用教程 [视频]

#### 1.2.1 基本操作

除了使用快捷键`ctrl + /`使用`Zotero GPT`，你也可以通过工具栏的图标来实现快速访问

![](img/63156d739f874d9594b5fe01a72b0872.png)

#### 1.2.2 获取书生 `API Token`

书生链接：[InternLM API 使用文档](https://internlm.intern-ai.org.cn/api/tokens)、创建 `API Tokens`

按照下面步骤，在书生大模型 API 控制台获取 `Token`（注意：每条 `Token` 只显示一次）。

![](img/a21f459195e2c15807c4c2bacd97db2f.png)

![](img/64d055b26c4b6161f4a7b8b539bfe8e2.png)

#### 1.2.3 配置插件 `Intern-S1 API Key`

按照下面步骤，依次填入 `Intern-S1 API Token` 并清空 `system prompt`。

书生 API 调用：`https://chat.intern-ai.org.cn/api/v1/chat/completions`

![](img/27f07f76169d679af0fedf2d134c7a49.png)

需要注意的是，由于书生大模型暂未提供嵌入模型，这里我们使用 chatanywhere 提供的免费 embedding API。

chatanywhere 项目主页：免费使用 chatanywhere

chatanywhere API 调用：`https://api.chatanywhere.tech/v1/embeddings`

![](img/67f5b4e8e32358dbb5b4a0382595d6d8.png)

测试 API 是否配置成功：

![](img/20da29d3a3e7c5efb040a98f6c7dec02.png)

## 2 调用 Intern-S1 分析论文

### 2.1 提问论文细节

### 2.2 总结论文工作

![](img/658a69ce1b5d09274931400831629504.png)
