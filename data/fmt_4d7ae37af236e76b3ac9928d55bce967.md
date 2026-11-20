
# DeepSeek+`AnythingLLM`搭建私人知识库

> 来源：[https://vcn0zlxse9ua.feishu.cn/docx/MxeFddw4MojjsSxZr71cLccPn4e](https://vcn0zlxse9ua.feisch.cn/docx/MxeFddw4MojjsSxZr71cLccPn4e)

`AnythingLLM` 是 Mintplex Labs 推出的开源全栈 AI 应用程序，支持检索增强生成（RAG）和 AI 代理功能，用户无需编程即可快速搭建私有知识库与智能问答系统

## AnythingLLM

### 功能特点

+ 支持多种文档类型：能处理 PDF、TXT、DOCX 等多种文档，将其转化为 LLM 可理解的上下文，方便用户通过对话获取文档关键信息。

+ 自定义 AI 代理：允许用户根据需求设置个性化的 AI 代理，为特定任务提供支持，如在工作区内浏览网页、运行代码等。

+ 多模态支持：不仅支持闭源 LLM，还兼容各种开源 LLM，如 OpenAI、Hugging Face、Google Gemini Pro 等，同时与多个著名的向量数据库平台兼容，如 LanceDB、AstraDB、Pinecone 等。

+ 多用户支持及权限管理：仅限 `Docker` 版本，便于团队协作和资源分配，管理员可控制用户访问权限，确保数据安全和隐私。

+ 可嵌入聊天组件：提供可嵌入网站的自定义聊天组件，提升产品交互体验和用户黏性。

+ 两种聊天模式：支持对话模式和查询模式，对话模式保留历史记录，查询模式适用于简洁的问答。

+ 聊天内引用：在对话中可直接引用文档内容，并链接到原始源，保证回答的准确性和可追溯性。

+ 成本节约：处理大型文档或转录内容时，采用向量缓存技术，用户无需为嵌入多次的相同内容支付额外费用，相比其他方案，可节省约 90% 成本。

## 安装 `ollama`

参考前面的本地部署：

`https://vcn0zlxse9ua.feishu.cn/wiki/LFWHwooVGiXFeYkKs7dcUiqincg`

![](img/e56f1c0a33ff135072c4964c1c571844.png)

## `AnythingLLM` 搭建（win）

`AnythingLLM` 是一款开源的全栈 AI 应用，旨在帮助用户构建私有化智能知识库和个性化问答系统。它基于 RAG（检索增强生成）技术，支持用户上传多种格式的文档，并通过自然语言与这些内容进行交互。

### 特性

+ 支持的模型大多数是国外的，如 Openai 等

+ RAG 知识库，多种文件格式，PDF、TXT 等

+ 多种向量数据库

+ 实时网络搜索

+ 云端本地都可以部署

### 下载 `AnythingLLM` 软件

官网：`https://anythingllm.com/`

![](img/362957ce0f6f61372b3532e6c6de55cd.png)

![](img/1685e62b1c2d47f9cc6b579fc79f3d33.png)

### 安装启动

下载完成后，打开安装包一直下一步等待安装结束即可。

![](img/d2949d4e1ef369cda0194186c6628e09.png)

![](img/25e75e9fc2186a9c7d44b2ca5ae3ddeb.png)

![](img/481c074b8d20fc4f9f7fd5f7fe60285a.png)

### 配置 `ollama` 模型

![](img/406440c53254344692cae3373c26cc2f.png)

![](img/4f528acd1a6f32557d2157fded0c8ac2.png)

![](img/738566b245999824d7cb1ee0f572f561.png)

### 配置模型

![](img/e2f0c97df765fc264a9a4cdadd36a2d9.png)

#### 添加大模型

![](img/f5342da30ed6c44a61f74a9d192c1d8c.png)

#### 添加嵌入模型

![](img/7025fa27e171708cb819cee95367bacf.png)

### 创建知识库

每个工作区自己管理自己的文档，同时支持多会话。

![](img/3f3602d4a621ebb02e3980f40c022aa2.png)

#### 知识库

![](img/22b9f48c77e6246d5336978a28e62bd1.png)

#### 数据连接

![](img/7fa3337706f59a4e918fdba26d02ed9c.png)

#### 知识库使用

通过知识库来回答问题，有两种 moshi

+ 聊天模式：聊天 将提供 LLM 的一般知识 和 找到的文档上下文的答案。

+ 查询模式：查询 将 仅 提供找到的文档上下文的答案。

![](img/f028c89a9ca00f3c9f1945d701a3fab8.png)

也可以上传文件提问

![](img/48eeff7c4d5c9e7f00048d5ddaa3cb72.png)

![](img/2119c28616cc642533fedaf1f1922755.png)

### `agent` 能力

支持代理能力，通过 `Agent` 完成特定功能。

#### 配置

![](img/7f97cd625a55f926cf829e439b322628.png)

![](img/c22b4ba047783059a911de8c425eea37.png)

#### 体验

使用 Scrape websites 抓取数据 的示例：

启动 agent 会话，选择 agent，然后通过提示词提问

![](img/95abd5b0e00a53286a0dcef2e41f50d0.png)

抓取数据返回结果

### 聊天

![](img/429c994ed7b335a1d2ab019eba6cb72c.png)

## 参考

`https://zhuanlan.zhihu.com/p/18056846015`
