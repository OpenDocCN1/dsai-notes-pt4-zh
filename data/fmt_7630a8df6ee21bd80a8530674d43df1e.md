
# Reader：用大模型将网页内容转化成格式化的知识库

> 来源：[https://geekbang.feishu.cn/docx/ZxBPdpfoaonyK7xfenic1NMXnUo](https://geekbang.feishu.cn/docx/ZxBPdpfoaonyK7xfenic1NMXnUo)

板块主理人：尼福财  
·向量光年科技有限公司创始人  
·《开源AI项目落地》👸号及社群主理人  

专注于AI产品开发落地，移动端、PC端软件快速高质量交付  
愿景：加速全行业向AI转变  

![](img/593846c4e33587024d55d1c82cd8c4f0.png)

## 项目简介

Jina AI的Reader项目是一个强大的工具，它通过简单的URL前缀转换，将任何网页内容转化为对大型语言模型（LLM）友好的输入格式。

Reader主要提供两个功能：阅读和搜索。

通过“阅读”功能，用户可以将网页内容转换成优化后的格式，以提高语言代理和可检索生成模型的输出质量。而“搜索”功能则允许用户查询并直接获取网络上的最新信息，自动格式化为LLM友好的形式。这两个功能都是免费、稳定且可扩展的，适用于生产环境。

![](img/ef5a5ea5a0fccda3848a52bace8292d5.png)

扫码加入AI交流群  
获得更多技术支持和交流  
（请注明自己的职业）  

![](img/1c6b9b5db344c2c20c10ca15d224d059.png)

## DEMO

![](img/08cac4d45d2ab8b3fcf2543bf42102c0.png)

用Reader把github上的页面转换成了结构化的内容，信息非常完善，可以用于批量的内容抓取工作。

## 实际应用

Reader可以应用于多种场景，包括：

### 1.内容摘要和转换

将网页内容转换成对语言模型友好的格式，便于内容的进一步处理和分析。

### 2.信息检索

通过特定的URL前缀查询，快速获取网络上的信息，并自动将其格式化，使其更易于语言模型的处理和理解。

### 3.增强学习和研究

为学术研究或机器学习模型提供更准确、更结构化的数据输入。

### 4.内容审核和分析

自动抓取网页内容，进行内容审核或情感分析，有助于监测和评估网络上的信息。

这些功能使Reader成为处理和优化网络内容的有力工具，特别适用于需要高效信息处理的环境。

## 用法

### 1.单个URL获取

使用`r.jina.ai`可以简化单个URL的处理。只需在任何URL前添加 `https://r.jina.ai/`。

例如，要将URL `https://en.wikipedia.org/wiki/Artificial_intelligence` 转换为LLM（大型语言模型）友好的输入，可以使用以下URL：

`https://r.jina.ai/https://en.wikipedia.org/wiki/Artificial_intelligence`。

这个服务会自动为缺少`alt`标签的页面图片添加说明，并通过视觉语言模型（VLM）进行格式化。

### 2.网络搜索

使用`s.jina.ai`进行网络搜索时，只需在搜索查询前添加 `https://s.jina.ai/`。如果在代码中使用，应确保先对搜索查询进行编码。

例如，如果查询是“Who will win 2024 US presidential election?”，则URL应该是：`https://s.jina.ai/Who%20will%20win%202024%20US%20presidential%20election%3F`。

在幕后，Reader会搜索网络，获取前5个结果，访问每个URL，并应用`r.jina.ai`技术。这与许多只返回搜索引擎API提供的标题、URL和描述的代理/RAG框架中的网页搜索功能调用不同。

如果你想更深入地阅读某个结果，Reader会自动为你从顶部5个搜索结果的URL中获取内容，无需你自己处理浏览器渲染、阻塞或任何与JavaScript和CSS相关的问题。

### 3.交互式代码片段生成器

推荐使用代码生成器来探索Reader API的不同参数组合。

![](img/9e660f7b9fb5f4ae10c3e450067ee0ed.png)

### 4.JSON模式

#### 使用 curl 获取 JSON 数据

代码示例：

```
curl -H "Accept: application/json" https://r.jina.ai/https://en.m.wikipedia.org/wiki/Main_Page
```

此命令通过`curl`，设置 HTTP 头部为`Accept: application/json`，向`r.jina.ai`发送请求，并通过该服务访问维基百科主页，返回的数据格式为 JSON，包含`url`、`title`和`content`三个字段。

#### s.jina.ai API 结构

在 JSON 模式下，`s.jina.ai` 返回一个包含五个结果的列表，每个结果均以字典形式组织，包括`title`、`content`和`url`三个字段。这说明 `s.jina.ai` 更适合于处理搜索请求，返回的是多个相关搜索结果的数据结构。

## 安装

为了运行项目，你需要准备以下工具和步骤：

### 1.Node.js v18

使用`Node.js`版本18，因为更高版本可能导致构建失败。

### 2.Firebase CLI

需要全局安装`Firebase CLI`命令行工具，可以使用以下命令进行安装：

```
npm install -g firebase-tools
```

### 3.克隆项目代码库

```
git clone git@github.com:jina-ai/reader.git
```

### 4.安装后端依赖

· 进入后端功能目录：

```
cd backend/functions
```

· 安装所需的`npm`依赖：

```
npm install
```

## 项目链接

`https://github.com/jina-ai/reader`

关注「 开源AI项目落地 」👸号  
与AI时代更靠近一点
