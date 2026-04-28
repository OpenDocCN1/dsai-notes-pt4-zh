# Deepseek+CherryStudio 搭建知识库

> 来源：[https://vcn0zlxse9ua.feishu.cn/docx/Y9SkdvLdHopNXJxh936cP6bentb](https://vcn0zlxse9ua.feishu.cn/docx/Y9SkdvLdHopNXJxh936cP6bentb)

之前分享过 Deepseek 相关本地部署的内容和 Deepseek 搭配知识库的方案，这里结合一款 UI（CherryStudio）来搭建本地部署，同时可以搭建知识库。搭配 ollama+CherryStudio 进行本地部署。

## 安装 ollama

参考前面文档 ollama 部分

[https://vcn0zlxse9ua.feishu.cn/wiki/LFWHwooVGiX](https://vcn0zlxse9ua.feishu.cn/wiki/LFWHwooVGiXFeYkKs7dcUiqincg?fromScene=spaceOverview)FeYkKs7dcUiqincg?fromScene=spaceOverview

![](img/6dc7bfaa42e4cffa7ab9023c0536b01e.png)

### 安装 Deepseek R1

参考之前文档

### 安装嵌入模型

地址：https://ollama.com/library/mxbai-embed-large

```
ollama pull mxbai-embed-large
```

![](img/f546d3340420723a4ac9a7bc8ee04c03.png)

## 安装 Cherry Studio

比较轻量，安装很快。同时功能也比较强大。

> 更多文档：https://docs.cherry-ai.com/knowledge-base/knowledge-base

### 下载

点击打开下载：https://cherry-ai.com/download

然后直接安装

![](img/d9efaddab478959da03a7a904349c591.png)

### 介绍

#### 聊天

![](img/1ca12a0d75478187aaacf20dcf00dad6.png)

#### 智能体：预设一些内容

![](img/d3d40bab556a91854fc099decb1d25d9.png)

#### 生图

![](img/8a6805cb6830a6a765ed175845d43906.png)

#### 翻译

![](img/de7d23aa2a39fd9b55a180466358d843.png)

#### 各个模型应用入口

![](img/f0904eb95925a0fc306d26ddee090dbf.png)

#### 知识库

![](img/0cab3c21e1e81d5aab496748c9b46a2e.png)

#### 联网

在Cherry Studio 的提问窗口，点击 【小地球】 图标即可开启联网。

![](img/91a62e6c2f53cc3f6c5c4065bfa4a58b.png)

点击地球图标 - 开启联网

![](img/fdd635790a4f634bf409782bc6cd3156.png)

表示 - 已开启联网功能

##### 方式一：模型服务商的大模型自带联网功能

![](img/d3578d8fab54004fc40ca84e585d739d.png)

![](img/0def1af700454847a9b5eca3e8cdb05f.png)

##### 方式二：需要配置搜索引擎

使用 Tavily服务 实现联网功能

![](img/5e5e3434fff08da264d79c08943c9f42.png)

![](img/dbcdca0b41cb9d1d5c4c1a45c1dd15d1.png)

#### Notion功能

将对话导出到Notion

参考：https://docs.cherry-ai.com/advanced-basic/notion

### 添加模型

#### 添加大模型

使用 Deepseek

![](img/d5d974705ed944be0d93a563bcee41d0.png)

检查模型是否可用

![](img/352b48d92f36127d1f8ba4879992b66a.png)

#### 添加嵌入模型

使用模型 mxbai-embed-large:latest

![](img/fb6fa0b36401fdb194c8526cb548e50c.png)

### 添加知识库

![](img/7b90ed0c6c48143e2a750c8b93dcec9c.png)

![](img/b5994b924ec2f58bbd48cb7768a13e7c.png)

### 知识库对话

选择模型

![](img/ff53d171c6a66af05ab85b0daced77ca.png)

选择知识库

![](img/beaea4faf5547f9a970c9fd526220040.png)

聊天效果

![](img/f299cee9ac2ff31e3e8040acbb0e4958.png)