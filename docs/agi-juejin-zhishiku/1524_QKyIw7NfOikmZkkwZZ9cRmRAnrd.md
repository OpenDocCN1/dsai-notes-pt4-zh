

# Qwen-Agent：从8k推广到1M上下文

> 来源：[https://agijuejin.feishu.cn/docx/P8dDdPUWwoPICWx4AdCczKoMn1c](https://agijuejin.feishu.cn/docx/P8dDdPUWwoPICWx4AdCczKoMn1c)
>
> 作者：lucas大叔
>
> 原文：[https://zhuanlan.zhihu.com/p/704654499](https://zhuanlan.zhihu.com/p/704654499)

Qwen 系列模型由阿里巴巴开发，凭借其强大的长上下文大语言模型，已经成为开源 AI 社区中的一名有力竞争者。最近，他们发布了新的智能体框架 Qwen-Agent，采用了最新的 Qwen 模型，具备 8K 的上下文长度，能够理解包含 100 万个token的文档，性能超过了 RAG 和原生长上下文模型。

Qwen-Agent 不仅仅是另一个LLM；它专注于解决一个经常被忽视的关键问题——长上下文的有效处理。我们最近看到 AI 社区中出现了一个增长趋势，OpenAI 和 Cohere 等公司分享了越来越多的agent教程和架构。像 Autogen、CrewAI 和 Langchain 这样的框架也已经涌现，专注于这个令人兴奋的领域。现在，有了 Qwen-Agent，我们又多了一种可以探索的架构。

## 长上下文的挑战

许多LLM的一个局限是它们在处理极长上下文时的困难。Qwen-Agent 框架使用聊天模型来开发能够处理 100 万个上下文的强大智能体，包含智能体开发、数据合成和模型微调（该智能体还用于生成新的长上下文 Qwen 模型的训练数据）。Qwen-Agent 采用了一个独特的三级方法来解决长上下文问题：

### Level 1：检索增强生成（RAG）

第一级使用检索增强生成，这是一种将长上下文分解为更易管理的块的方法。通过从用户查询中提取关键词（BM25），Qwen-Agent 能够高效地找到文档中最相关的部分。

![](img/85000c2a1a82b7940d229da66ad8a800.png)

### Level 2：分块阅读

但是，如果由于关键词不匹配而遗漏了一些相关的块怎么办？这就是第二级发挥作用的地方。在这个阶段，Qwen-Agent 采用暴力策略，并行评估每个块的相关性，确保没有重要信息被遗漏。这就像是对你的助手的工作进行复查，以确保没有忽略任何重要内容。

![](img/3f035d9557c7753f9e4a733dd2ea4acd.png)

### Level 3：逐步推理

现在，假设你有一个需要多步推理的复杂问题。Qwen-Agent 集成了tool-calling agent，这些智能体可以将复杂问题分解成多个子问题，检索必要的信息，并提供一个全面的答案。

![](img/e44110366b2098b62d473668b6625b2c.png)

## 实验表现

在两个针对256k上下文设计的基准测试上进行了实验：

+ NeedleBench，测试模型是否能在充满大量无关句子的语境中找到最相关句子的基准，类似于“大海捞针”。回答一个问题可能需要同时找到多根“针”，并进行多跳逐步推理。

+ LV-Eval是要求同时理解众多证据片段的基准测试。我们对LV-Eval原始版本中的评估指标进行了调整，因为其匹配规则过于严苛，导致了许多假阴性结果。

我们比较了以下方法：

+ 32k-模型：这是一个7B对话模型，主要在8k上下文样本上进行微调，并辅以少量32k上下文样本。为了扩展到256k上下文，我们采用了无需额外训练的方法，如基于RoPE的外推。

+ 4k-RAG：使用与32k-模型相同的模型，但采取了Lv1-智能体的RAG策略。它仅检索并处理最相关的4k上下文。

+ 4k-智能体：同样使用32k-模型的模型，但采用前文描述的更复杂的智能体策略。该智能体策略会并行处理多个片段，但每次请求模型时至多只使用模型的4k上下文。

![](img/8320e5c3fabf997a4f768764a4946318.png)

实验结果说明了以下几点：

+ 在短上下文场景中，4k-RAG的表现可能不如32k-模型。这可能是由于RAG方案难以检索到正确的信息或理解多个片段造成的。

+ 相反，随着文档长度的增加，4k-RAG越发表现出超越32k-模型的趋势。这一趋势表明32k-模型在处理长上下文方面并没有训练到最优的状态。

+ 值得注意的是，4k-智能体始终表现优于32k-模型和4k-RAG。它分块阅读所有上下文的方式使它能够避免原生模型在长上下文上训练不足而带来的限制。

总的来说，如果得到恰当的训练，32k-模型理应优于所有其他方案。然而，实际上由于训练不足，32k-模型的表现不及4k-智能体。

## 开始上手

### 安装

安装稳定的版本：

```
pip install -U qwen-agent
```

或者，直接从源代码安装最新的版本：

```
git clone https://github.com/QwenLM/Qwen-Agent.git
cd Qwen-Agent
pip install -e ./
```

如需使用内置GUI支持，请安装以下可选依赖项：

```
pip install -U "gradio>=4.0" "modelscope-studio>=0.2.1"
```

### 准备：模型服务

[Qwen-Agent](https://link.zhihu.com/?target=https%3A//help.aliyun.com/zh/dashscope/developer-reference/quick-start)支持接入阿里云 DashScope 服务提供的Qwen模型服务，也支持通过OpenAI API方式接入开源的Qwen模型服务。

+ 如果希望接入DashScope提供的模型服务，只需配置相应的环境变量 `DASHSCOPE_API_KEY` 为你的DashScope API Key。

+ 或者，如果希望部署并使用自己的模型服务，请按照 `Qwen2` 的 `README` 中提供的[指导进行操作](https://link.zhihu.com/?target=https%3A//github.com/QwenLM/Qwen2%3Ftab%3Dreadme-ov-file%23vllm)，以部署一个兼容OpenAI接口协议的API服务。具体来说，请参阅 vLLM 一节了解高并发的GPU部署方式，或者查看 [Ollama](https://link.zhihu.com/?target=https%3A//github.com/QwenLM/Qwen2%3Ftab%3Dreadme-ov-file%23ollama) 一节了解本地CPU（+GPU）部署。

### 快速开发

框架提供了大模型（LLM，继承自 `BaseChatModel` ，并提供了 Function Calling 功能）和工具（Tool，继承自 `BaseTool` ）等原子组件，也提供了智能体（Agent）等高级抽象组件（继承自 `Agent` ）。

以下示例演示了如何增加自定义工具，并快速开发一个带有设定、知识库和工具使用能力的智能体：

```
import pprint
import urllib.parse
import json5
from qwen_agent.agents import Assistant
from qwen_agent.tools.base import BaseTool, register_tool


# 步骤 1（可选）：添加一个名为 `my_image_gen` 的自定义工具。
@register_tool('my_image_gen')
class MyImageGen(BaseTool):
    # `description` 用于告诉智能体该工具的功能。
    description = 'AI 绘画（图像生成）服务，输入文本描述，返回基于文本信息绘制的图像 URL。'
    # `parameters` 告诉智能体该工具有哪些输入参数。
    parameters = [{
        'name': 'prompt',
        'type': 'string',
        'description': '期望的图像内容的详细描述',
        'required': True
    }]

    def call(self, params: str, **kwargs) -> str:
        # `params` 是由 LLM 智能体生成的参数。
        prompt = json5.loads(params)['prompt']
        prompt = urllib.parse.quote(prompt)
        return json5.dumps(
            {'image_url': f'https://image.pollinations.ai/prompt/{prompt}'},
            ensure_ascii=False)


# 步骤 2：配置您所使用的 LLM。
llm_cfg = {
    # 使用 DashScope 提供的模型服务：
    'model': 'qwen-max',
    'model_server': 'dashscope',
    # 'api_key': 'YOUR_DASHSCOPE_API_KEY',
    # 如果这里没有设置 'api_key'，它将读取 `DASHSCOPE_API_KEY` 环境变量。

    # 使用与 OpenAI API 兼容的模型服务，例如 vLLM 或 Ollama：
    # 本例使用ollama拉取qwen2:7b-instruct
    'model': 'qwen2:7b-instruct',
    'model_server': 'http://localhost:11434/v1/',  # base_url，也称为 api_base
    'api_key': 'EMPTY',

    # （可选） LLM 的超参数：
    'generate_cfg': {
        'top_p': 0.8
    }
}

# 步骤 3：创建一个智能体。这里我们以 `Assistant` 智能体为例，它能够使用工具并读取文件。
system_instruction = '''你是一个乐于助人的AI助手。
在收到用户的请求后，你应该：
- 首先绘制一幅图像，得到图像的url，
- 然后运行代码`request.get`以下载该图像的url，
- 最后从给定的文档中选择一个图像操作进行图像处理。
用 `plt.show()` 展示图像。
你总是用中文回复用户。'''
tools = ['my_image_gen', 'code_interpreter']  # `code_interpreter` 是框架自带的工具，用于执行代码。
files = ['./examples/resource/doc.pdf']  # 给智能体一个 PDF 文件阅读。
bot = Assistant(llm=llm_cfg,
                system_message=system_instruction,
                function_list=tools,
                files=files)

# 步骤 4：作为聊天机器人运行智能体。
messages = []  # 这里储存聊天历史。
while True:
    # 例如，输入请求 "绘制一只狗并将其旋转 90 度"。
    query = input('用户请求: ')
    # 将用户请求添加到聊天历史。
    messages.append({'role': 'user', 'content': query})
    response = []
    for response in bot.run(messages=messages):
        # 流式输出。
        print('机器人回应:')
        pprint.pprint(response, indent=2)
    # 将机器人的回应添加到聊天历史。
    messages.extend(response)
```

除了使用框架自带的智能体实现（如 `Assistant` ），也可通过继承 `Agent` 来自行开发智能体实现。更多使用示例，请参阅 [`examples`](https://link.zhihu.com/?target=https%3A//github.com/QwenLM/Qwen-Agent/blob/main/examples) 目录。

测试推荐的输入请求，生成小狗图片并将其旋转90°。

![](img/b5da7e8cf67f6f0abbfcb01dd5192a02.png)

## 应用：BrowserQwen

BrowserQwen 是一款基于 Qwen-Agent 构建的浏览器助手。如需了解详情，请参阅其[文档](https://link.zhihu.com/?target=https%3A//github.com/QwenLM/Qwen-Agent/blob/main/browser_qwen_cn.md)。下面展示BrowserQwen的有趣功能。

### 编辑模式

此模式旨在根据浏览的网页和PDF创建长文章。

![](img/c893c6d3115722156a12a92977eaa7df.png)

支持调用插件来帮助创建富文本。

![](img/0e84d553672572b8a7eed87685575a02.png)

### 聊天模式

该模式下可以进行多网页问答。

![](img/2150e998dbe1075c357db5423e8dec36.png)

用`code interpreter`创建数据图表。

![](img/3b526c06e2ae2bdaf03abcb634625eb3.png)

### 浏览器助手

+ 网页QA

![](img/7a57b2b8058391ed5443c81970087486.png)

+ PDF文档QA

![](img/f1c530320a17d8cffce6540b9a24a4d1.png)

## 参考文档

[https://qwenlm.github.io/zh/blog/qwen-agent-2405/](https://link.zhihu.com/?target=https%3A//qwenlm.github.io/zh/blog/qwen-agent-2405/)

[https://pypi.org/project/qwen-agent/](https://link.zhihu.com/?target=https%3A//pypi.org/project/qwen-agent/)

发布于 2024-06-21 20:08 ・IP 属地江苏

