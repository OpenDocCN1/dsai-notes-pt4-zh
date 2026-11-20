
# 5 个个人 AI 知识库 GitHub 爆火项目

> 来源：[https://yunyinghui.feishu.cn/docx/WTX5dJqygozF8cxkN6LcbX1UnSg](https://yunyinghui.feishu.cn/docx/WTX5dJqygozF8cxkN6LcbX1UnSg)

> **概述：** 篇文章介绍了 5 个热门的个人 AI 知识库开源 GitHub 项目，并对每个项目进行了简要概述。

## 一、siyuan：个人知识管理工具

这个叫 `思源笔记` 的开源项目，肯定很多人用过。在 `GitHub` 上都有 4 万的 Star 了。

![](img/e969481a1290b61016bba6a4f3697f54.png)

它在极致的编辑体验与绝对的数据隐私之间找到平衡点，它不仅是一个笔记工具，更是一个基于本地的个人知识管理系统。

而且还能接入 AI 进行写作和问答聊天。

![](img/156483267f62bf96bb9b6d6d0944c2c7.png)

`思源笔记` 采用了 `Block` 作为数据的基本单位，一个段落、一张图片、一个列表甚至一个表格，都被视为一个独立的 `Block`，都有唯一的 `ID`。

这意味着你可以像搭积木一样组合内容，并且可以在不同的文档中，精确地引用某一个具体的段落，而不是只能引用整篇文章。

当你修改原处的块内容时，所有引用该块的地方都会同步更新。

![](img/1426a694d5081a58933d6358dc91768f.png)

而且也接入了 AI 能力，帮你完成 续写，翻译，摘取提要，进行头脑风暴，修正语法、拼写和错别字。

也可以和它直接聊天。

![](img/4df5199df59b12c494025d4942fa1ede.png)

虽然它支持 `Markdown` 语法输入，但在底层，`思源笔记` 并没有直接存储为 `.md` 文件，而是采用了结构化的 `JSON` 数据，并通过 `SQL` 数据库索引。

这种设计是为了实现上述的 `块级引用` 和 `毫秒级的全文搜索`。不过，项目充分考虑了通用性，支持一键将笔记导出为标准的 `Markdown` 或 `PDF` 格式。

```
开源地址：https://github.com/siyuan-note/siyuan
```

## 二、Lobe Chat：AI 操作台

这个开源项目现在已经接近 7 万 Star 了。

你可以把它理解成一个 AI 大模型 操作台，能让你手中的各种模型能力发挥到极致。

它的 `UI` 非常炫酷，不粗糙。有流畅的动画、精心设计的排版以及对移动端的完美适配。

![](img/c644012cc6bdf60a526946e2e043ffda.png)

它不仅用极高的颜值统一了所有 `AI` 模型，更通过强大的知识库功能，让 `AI` 真正拥有了属于你的第二大脑。

在 `Lobe Chat` 中，你可以直接将 `PDF` 论文、`Excel` 表格、`Word` 文档甚至 `Markdown` 笔记拖入界面。

![](img/e47ee72a6069d951438dc9823d481227.png)

[0bc3guabcaaaimaitzjcmzuvanodce2qaeia.f10002.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DPBybxGHhosZdAxTIEicEDzGnhf)

系统会自动对这些文件进行分块（`Chunking`）和向量化（`Embedding`）处理。

当你向 `AI` 提问时，它不再是瞎编乱造，而是会先在你的文档中检索相关信息，结合上下文给你精准的答案。

更重要的是，它支持知识库管理。你可以建立不同的知识库，比如公司财报、`Python` 教程、装修指南，并在不同的对话助手中有选择地挂载它们。

```
开源地址：https://github.com/lobehub/lobe-chat
```

## 三、anything-llm：私有知识库

![](img/d7cc691a010051dff462f6f589d29aef.png)

`AnythingLLM` 是由 `Mintplex Labs` 开发的一款全栈式 `AI` 知识库应用程序。

现在已经有 5 万的 Star 了。

与许多仅提供 `API` 或简单的 `Web UI` 的项目不同，`AnythingLLM` 的定位是企业级的文档聊天解决方案。

简单来说，它不仅仅是一个聊天机器人，更是一个完整的私有数据管理与交互平台。

![](img/b67b075aa081ea2c5858382a6a7b01cd.png)

[0bc3jyabeaaajiaithzcnbuvatwdcjhaaeqa.f10002.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QhxQbEzPKoBfhJxA84NcBYCXnTf)

它有工作区（`Workspace`）的概念，类似一个独立的文件夹或项目。

在每个工作区里，你可以上传特定的文档，配置特定的 `Prompt`（提示词），甚至选择不同的语言模型。

```
开源地址：https://github.com/Mintplex-Labs/anything-llm
```

## 四、KHOJ：你的 AI 第二大脑

开源项目 `Khoj` 的 Star 数也挺多的，已经有 32K 的 Star 了。`Khoj` 名字在印地语中意为搜索或探索。

它就像一个为你私人定制的 `AI` 搜索专家，不仅能索引你所有的本地文档和笔记，还能让你用自然语言和它们聊天。

它支持完全离线运行，隐私安全，同时也能联网进行深度研究。

![](img/2c9e93436e3906698139c19c4aec52e0.png)

基于文档的 `AI` 问答，原理都差不多。

`AI` 阅读你指定文件夹里的所有资料，转化为机器能理解的向量数据。提问时，`AI` 理解你的意图，精准定位到你笔记中相关的段落，结合上下文给你一个完整的答案。

![](img/29c397999392ce86811868574dab5dd1.png)

与其他工具相比，`Khoj` 的可玩性和极客属性极强。

它不仅是一个聊天机器人，更是一个自动化平台。你可以设置它每天早上自动帮你搜索行业新闻并生成摘要发到邮箱。也可以在 `Emacs` 或 `Obsidian` 这种生产力工具中直接调用它辅助写作。

![](img/b83d6ad99e6370ae05f1554c08fdbe04.png)

```
开源地址：https://github.com/khoj-ai/khoj
```

## 五、DocsGPT：告别文档大海捞针

这个叫 `DocsGPT` 的开源项目，现在也有 1.7 万的 Star 了。

它可以在堆积如山或超级长的 `PDF` 文档中，通过关键词搜索精准找到上下文相关的答案，效率很高。

![](img/59c5a05b1ebae1082da3c8d3b5d207b1.png)

除了常见的文档解析和问答功能，还提供了深层次的研究工具和智能体构建能力。

你可以通过其内置的 `智能体构建器` 快速定制专属 `AI` 助手，赋予其执行任务、调用工具的能力。

![](img/978e6aaa0c02c88ca9e2c70bc8e90515.png)

在模型支持上，`DocsGPT` 兼容性极强，既可以对接 `OpenAI`、`Google`、`Anthropic` 等云服务提供商的模型，也能运行 `Ollama`、`llama_cpp` 等本地模型。

`DocsGPT` 前端采用 `React`，后端基于 `Flask` 框架，支持通过 `Docker` 快速部署。

```
开源地址：https://github.com/arc53/DocsGPT
```
