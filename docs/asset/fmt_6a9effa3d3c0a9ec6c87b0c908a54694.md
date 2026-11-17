
# LazyLLM: 低代码构建多Agent大模型应用的开源项目

> 来源：https://agijuejin.feishu.cn/docx/Uj0ndReF6oR3ZpxzDn0ci451nkh
> 作者：PaperAgent
> 原文：https://mp.weixin.qq.com/s/0-FfCGTmRHORJHLKA_r0iQ

`LazyLLM`是一款低代码构建多Agent大模型应用的开发工具，协助开发者用极低的成本构建复杂的AI应用，可以持续的迭代优化效果。提供了便捷的搭建应用的workflow，为应用开发过程中的各个环节提供了大量的标准流程和工具。

基于`LazyLLM`的AI应用构建流程：原型搭建 -> 数据回流 -> 迭代优化，可以先基于`LazyLLM`快速跑通应用的原型，再结合场景任务数据进行bad-case分析，然后对应用中的关键环节进行算法迭代和模型微调，进而逐步提升整个应用的效果。

![](img/074f187f1354e97f8be9e08b316884ef.png)

## LazyLLM基本概念

### Component
`Component`是`LazyLLM`中最小的执行单元，它既可以是一个函数，也可以是一个`bash`命令。

### Module
`Module`是`LazyLLM`中的顶层组件，具备训练、部署、推理和评测四项关键能力，每个模块可以选择实现其中的部分或者全部的能力，每项能力都可以由1到多个`Component`组成。如下表所示，内置了一些基础的`Module`供大家使用。

![](img/0cf32aa5a51adcd5a6a0800ec0ef8bc4.png)

### Flow
`Flow`是`LazyLLM`中定义的数据流，描述了数据如何从一个可调用对象传递到另一个可调用的对象，可以利用`Flow`直观而高效地组织和管理数据流动。基于预定义好的各种`Flow`，可以借助`Module`、`Component`、`Flow`甚至任一可调用的对象，轻松地构建和管理复杂的应用程序。目前`LazyLLm`中实现的`Flow`有`Pipeline`、`Parallel`、`Diverter`、`Warp`、`IFS`、`Loop`等，几乎可以覆盖全部的应用场景。

## 使用指南

### 对话机器人

如果使用一个本地部署的模型，请确保自己安装了至少一个推理框架(`lightllm`或`vllm`)

```
# set environment variable: LAZYLLM_OPENAI_API_KEY=xx 
# or you can make a config file(~/.lazyllm/config.json) and add openai_api_key=xx
import lazyllm
t = lazyllm.OnlineChatModule(source="openai", stream=True)
w = lazyllm.WebModule(t)
w.start().wait()
```

```
import lazyllm
# Model will be download automatically if you have an internet connection
t = lazyllm.TrainableModule('internlm2-chat-7b')
w = lazyllm.WebModule(t)
w.start().wait()
```

### 检索增强生成（RAG）

支持RAG常用组件：`Document`、`Parser`、`Retriever`、`Reranker`等。

![](img/bddcb1a6e1a1c03dca6d5138d84085f2.png)

```
import os
import lazyllm
from lazyllm import pipeline, parallel, bind, _0, Document, Retriever, Reranker

prompt = '你将扮演一个人工智能问答助手的角色，完成一项对话任务。在这个任务中，你需要根据给定的上下文以及问题，给出你的回答。'
documents = Document(dataset_path='/file/to/yourpath', embed=TrainableModule('bge-large-zh-v1.5'))
with pipeline() as ppl:
    with parallel().sum as ppl.prl:
        prl.retriever1 = Retriever(documents, parser='CoarseChunk', similarity_top_k=6)
        prl.retriever2 = Retriever(documents, parser='SentenceDivider', similarity='chinese_bm25', similarity_top_k=6)
    ppl.reranker = Reranker(types='ModuleReranker', model='bge-reranker-large') | bind(ppl.input, _0)
    ppl.post_processer = lambda nodes: f'《{nodes[0].metadata["file_name"].split(".")[0]}》{nodes[0].get_content()}' if len(nodes) > 0 else '未找到'
    ppl.formatter = (lambda ctx, query: dict(context_str=ctx, query_str=query)) | bind(query=ppl.input)
    ppl.llm = lazyllm.TrainableModule('internlm2-chat-7b').prompt(lazyllm.ChatPrompter(prompt, extro_keys=['context_str'])) 
mweb = lazyllm.WebModule(ppl, port=23456).start().wait()
```

![](img/dbbe3e76309ef8e0cc9b0b72b3777ec6.png)

### 故事创作

```
import lazyllm
from lazyllm import pipeline, warp, bind
from lazyllm.components.formatter import JsonFormatter

toc_prompt=''' 你现在是一个智能助手。你的任务是理解用户的输入，将大纲以列表嵌套字典的列表。每个字典包含一个 `title` 和 `describe`，其中 `title` 中需要用Markdown格式标清层级，`describe` `describe` 是对该段的描述和写作指导。

请根据以下用户输入生成相应的列表嵌套字典：

输出示例:
[
    {
        "title": "# 一级标题",
        "describe": "请详细描述此标题的内容，提供背景信息和核心观点。"
    },
    {
        "title": "## 二级标题",
        "describe": "请详细描述标题的内容，提供具体的细节和例子来支持一级标题的观点。"
    },
    {
        "title": "### 三级标题",
        "describe": "请详细描述标题的内容，深入分析并提供更多的细节和数据支持。"
    }
]
用户输入如下：
'''

completion_prompt='''
你现在是一个智能助手。你的任务是接收一个包含 `title` 和 `describe` 的字典，并根据 `describe` 中的指导展开写作
输入示例:
{
    "title": "# 一级标题",
    "describe": "这是写作的描述。"
}

输出:
这是展开写作写的内容
接收如下：

'''

writer_prompt = {"system": completion_prompt, "user": '{ "title": {title}, "describe": {describe} }'}

with pipeline() as ppl:
    ppl.outline_writer = lazyllm.OnlineChatModule(source="openai", stream=False).formatter(JsonFormatter()).prompt(toc_prompt)
    ppl.story_generater = warp(lazyllm.OnlineChatModule(source="openai", stream=False).prompt(writer_prompt))
    ppl.synthesizer = (lambda *storys, outlines: "\n".join([f"{o['title']}\n{s}" for s, o in zip(storys, outlines)])) | bind(outlines=ppl.outline_writer)

print(ppl({'query':'请帮我写一篇关于人工智能在医疗领域应用的文章。'}))
```

## 规划

`LazyLLM` v0.2 的目标是打造一个支持常见的AI-Agent应用场景，并在各个场景下支持灵活定制的AI应用开发框架。

### LazyLLM支持10个场景的任务：

![](img/7a5d8e27735f65c482c09e010266e7f5.png)

```
LazyLLM v0.2 PRD：https://aicarrier.feishu.cn/wiki/BeFfwBFv8iXq7vkVGyrcv1kGnIh
docs：https://lazyllm.readthedocs.io/
github https://github.com/LazyAGI/LazyLLM
```

## 推荐阅读

+   对齐LLM偏好的直接偏好优化方法：DPO、IPO、KTO

+   2024：ToB、Agent、多模态

+   [RAG全景图：从RAG启蒙到高级RAG之3](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247487375&idx=1&sn=e16bc2fdaac04e58e99cfd2a1dc0b0cb&chksm=c2ce35fef5b9bce80dcf0a70b753707036fe7f962d7888d60396490841a538b0f46e952f53f0&scene=21#wechat_redirect)6技，再到终章Agentic RAG！

+   [A](http://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247485322&idx=1&sn=71ffb345fca514aa5ce2848cb2c9f071&chksm=c2ce3dfbf5b9b4edd5b98e45c6179890bdea748fb5220636d25f42006954ea5c81afa8735725&scene=21#wechat_redirect)gent到多模态Agent再到多模态Multi-Agents系统的发展与案例讲解（1.2万字，20+文献，27张图）
