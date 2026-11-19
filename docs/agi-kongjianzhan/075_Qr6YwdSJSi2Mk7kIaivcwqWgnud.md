
# Amazon（Titan）

> 来源：[https://agispace.feishu.cn/docx/MYN7dEtyRog0mGxVXjtcU9cMnIb](https://agispace.feishu.cn/docx/MYN7dEtyRog0mGxVXjtcU9cMnIb)

![](img/1943c211f88bd6571f34d1d00cbe5082.png)

## 一、简介

亚马逊Titan大模型是亚马逊推出的一系列高性能基础模型，旨在支持广泛的生成式人工智能应用，如内容创建、图像生成以及搜索和推荐体验。这些模型由AWS创建并在大型数据集上进行预训练，使其成为强大的通用模型，支持各种用例，同时还支持负责任地使用AI。

## 二、主要功能

+ 文本生成：使用Titan Text模型可以提高各种文本相关任务的生产力和效率，例如为博客文章和网页创建副本、将文章分类、开放式问答、对话式聊天、信息提取等。

+ 图像生成：Titan Image Generator模型具备“图片编辑”及“隐藏水印”等功能，允许用户以英语输入提示词句，以生成“专业等级”的图像。

+ 多模态和文本模型：为广泛的生成式人工智能应用提供支持，例如内容创建、图像生成以及搜索和推荐体验。

## 三、模型版本

### Amazon Titan Text Premier

Amazon Titan Text Premier 是 Amazon Titan Text 系列中一款功能强大的先进模型，旨在为各种企业应用程序提供卓越的性能。该模型针对与 Amazon Bedrock 的代理和知识库集成进行了优化，使其成为构建可使用 API 并与数据交互的交互式生成式人工智能应用程序的理想选择。

最大令牌数：32000

语言：英语

支持微调：是（预览版）

支持的使用案例：RAG、代理、聊天、思维链、开放式文本生成、头脑风暴、总结、代码生成、表格创建、数据格式化、释义、重写、提取和问答。

### Amazon Titan Text Express

一款兼顾价格和性能的大型语言模型（LLM）。

最大令牌数：8000

语言：英语（GA），提供 100 多种语言（预览版）

支持微调：是

支持的使用案例：RAG、开放式文本生成、头脑风暴、汇总、代码生成、表格创建、数据格式化、释义、思维链、重写、提取、问答和聊天。

### Amazon Titan Text Lite

经济实惠且高度可定制的 LLM。大小适中，适合特定用例，非常适合文本生成任务和微调。

最大令牌数：4 千

语言：英语

支持微调：是

支持的使用案例：摘要和文案写作。

### Amazon Titan Text Embeddings

将文本转换为数字表示的 LLM。

最大令牌数：8K

语言：25 种以上的语言

支持微调：否

嵌入：1536

支持的使用案例：文本检索、语义相似度和集群化。

### Amazon Titan Text Embeddings V2

LLM 针对较小维度的高精度和检索性能进行了优化，从而减少了存储和延迟。

最大令牌数：8000

语言：100 多种预训练语言

支持微调：否

支持标准化：是

嵌入：256、512、1024

支持的使用案例：进行语义相似度搜索以查找文档（例如，检测抄袭），将标签分类为基于数据的学习表现形式（例如，将电影按类型分类），以及提高检索或生成的搜索结果的质量和相关性。

### Amazon Titan 多模态嵌入

提供准确的多模态搜索和推荐体验。

最大令牌数：128

最大图像大小：25MB

语言：英语

支持微调：是

嵌入：1024（默认）、384、256

支持的使用案例：搜索、推荐和个性化。

### Amazon Titan 图像生成器

使用文本提示生成逼真、录音室品质的图像。

最大令牌数：77

最大输入图像大小：25MB

语言：英语

支持微调：是

支持的使用案例：文本到图像生成、图像编辑和图像变体。

## 四、使用案例

使用全新的 Amazon Titan Text Premier模型构建 RAG 和基于代理的生成式人工智能应用程序：

`https://aws.amazon.com/cn/blogs/aws/build-rag-and-agent-based-generative-ai-applications-with-new-amazon-titan-text-premier-model-available-in-amazon-bedrock/`

Amazon Bedrock现已提供 AmazonTitan Text Embeddings V2：

`https://aws.amazon.com/cn/blogs/aws/build-rag-and-agent-based-generative-ai-applications-with-new-amazon-titan-text-premier-model-available-in-amazon-bedrock/`

Amazon Titan 图像生成器水印检测:

`https://aws.amazon.com/cn/blogs/aws/build-rag-and-agent-based-generative-ai-applications-with-new-amazon-titan-text-premier-model-available-in-amazon-bedrock/`

Amazon Bedrock 现已提供 Amazon Titan lmage Generator、 Amazon TitanMultimodal Embeddings和AmaoznTitan Text 模型:

`https://aws.amazon.com/cn/blogs/aws/build-rag-and-agent-based-generative-ai-applications-with-new-amazon-titan-text-premier-model-available-in-amazon-bedrock/`
