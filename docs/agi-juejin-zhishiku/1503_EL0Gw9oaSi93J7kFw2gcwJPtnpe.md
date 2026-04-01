# Qwen-Agent：Qwen2加持，强大的多代理框架 - 函数调用、代码解释器以及 RAG！

> 来源：[https://agijuejin.feishu.cn/docx/YdUkdw3IaocGdcxAQkuc02FRnYf](https://agijuejin.feishu.cn/docx/YdUkdw3IaocGdcxAQkuc02FRnYf)

> 作者：Aitrainee | AI进修生

> 原文：https://mp.weixin.qq.com/s/bVqcREmgI33kCFUaf_HvIA

> **🌟 Qwen-A**gent是一个开发框架。开发者可基于该框架开发 Agent应用 ，充分利用基于通义千问模型（Qwen）的指令遵循、工具使用、规划、记忆能力。该项目也提供了浏览器助手、代码解释器、自定义助手等示例应用。

Github：https://github.com/QwenLM/Qwen-Agent

Hello，大家好，我是Aitrainee，

阿**里巴巴最近发布了新的 **Qwen 2 大型语言模型和升级后的 Qwen Agent 框架，这个框架集成了 Qwen 2 模型，支持 函数调用、代码解释、RAG（检索增强生成） 等功能，还包含了 Chrome 扩展。Qwen Agent 能处理从 8K 到 100 万 tokens 的文档，性能超越了 RAG 和原生长上下文模型，并用于生成训练新长上下文模型的数据。

Qwen Agent 框架可用于创建复杂的 AI 代理，展示了其强大的任务处理能力。新框架采用四步法开发：初始模型开发、代理开发、数据综合和模型微调。通过 RAG 算法处理长文档，将文档分成小块，保留最相关的部分，从而提升上下文处理能力。

具体步骤包括检索增强生成、逐块阅读和逐步推理等三层复杂性，使用 RAG 算法处理并优化文档片段，以便提供准确的上下文理解和生成能力。实验表明，Qwen Agent 能显著提升模型的上下文长度和性能。

建议观看之前的视频以获取更多实用示例，Qwen 2 是目前最强大的开源语言模型之一，推荐尝试使用。框架操作简便，有详细教程帮助用户快速上手。

**这一框架的**目标是创建复杂的AI代理，其表现优于其他代理框架。 下面视频 展示了如何利用Qwen-2模型及其8K上下文窗口理解包含百万级词汇的文档，这比RAG和原生长上下文模型表现更好。

**Qwen-Agent 开发步骤**

1.  1. **初始模型 **：从8K上下文聊天模型开始。

1.  2. **代理开发 **：使用模型开发强大的代理，处理百万上下文。

1.  3. **数据合成 **：合成细化数据，进行自动过滤确保质量。

1.  4. **模型微调 **：利用合成数据微调预训练模型，最终得到强大的聊天机器人。

**分层复杂性**

Qwen-Agent在构建过程中分为三层复杂性，每层在前一层基础上构建：

1.  1. **增强型信息检索生成（RAG） **：使用RAG算法将上下文分成不超过512词的块，仅保留最相关的内容。

1.  2. **逐块阅读 **：采用暴力策略，每512词块检查相关性，保留最相关的内容生成答案。

1.  3. **逐步推理 **：使用多跳推理回答复杂问题，采用工具调用代理解决复杂查询。

下面提供官方的 **文档介绍、相关资源、部署教程 **等，进一步支撑你的行动，以提升本文的帮助力。

# **开始上手**

## **安装**

+   • 安装稳定的版本：

```
pip install -U qwen-agent
```

+   • 或者，直接从源代码安装最新的版本：

```
git clone https://github.com/QwenLM/Qwen-Agent.git
cd Qwen-Agent
pip install -e ./
```

如需使用内置GUI支持，请安装以下可选依赖项：

```
pip install -U &quot;gradio&gt;=4.0&quot; &quot;modelscope-studio&gt;=0.2.1&quot;
```

## **准备：模型服务**

Qwen-Agent支持接入阿里云 DashScope 服务提供的Qwen模型服务，也支持通过OpenAI API方式接入开源的Qwen模型服务。

+   • 如果希望接入DashScope提供的模型服务，只需配置相应的环境变量 DASHSCOPE_API_KEY 为您的DashScope API Key。

+   • 或者，如果您希望部署并使用您自己的模型服务，请按照Qwen2的README中提供的指导进行操作，以部署一个兼容OpenAI接口协议的API服务。具体来说，请参阅 vLLM 一节了解高并发的GPU部署方式，或者查看 Ollama 一节了解本地CPU（+GPU）部署。

## **快速开发**

框架提供了大模型（LLM，继承自 class BaseChatModel ，并提供了 Function Calling 功能）和工具（Tool，继承自 class BaseTool ）等原子组件，也提供了智能体（Agent）等高级抽象组件（继承自 class Agent ）。

以下示例演示了如何增加自定义工具，并快速开发一个带有设定、知识库和工具使用能力的智能体：

```
import pprint
import urllib.parse
import json5
from qwen_agent.agents import Assistant
from qwen_agent.tools.base import BaseTool, register_tool


# 步骤 1（可选）：添加一个名为 `my_image_gen` 的自定义工具。
@register_tool(&#x27;my_image_gen&#x27;)
class MyImageGen(BaseTool):
    # `description` 用于告诉智能体该工具的功能。
    description = &#x27;AI 绘画（图像生成）服务，输入文本描述，返回基于文本信息绘制的图像 URL。&#x27;
    # `parameters` 告诉智能体该工具有哪些输入参数。
    parameters = [{
        &#x27;name&#x27;: &#x27;prompt&#x27;,
        &#x27;type&#x27;: &#x27;string&#x27;,
        &#x27;description&#x27;: &#x27;期望的图像内容的详细描述&#x27;,
        &#x27;required&#x27;: True
    }]

    def call(self, params: str, **kwargs) -&gt; str:
        # `params` 是由 LLM 智能体生成的参数。
        prompt = json5.loads(params)[&#x27;prompt&#x27;]
        prompt = urllib.parse.quote(prompt)
        return json5.dumps(
            {&#x27;image_url&#x27;: f&#x27;https://image.pollinations.ai/prompt/{prompt}&#x27;},
            ensure_ascii=False)


# 步骤 2：配置您所使用的 LLM。
llm_cfg = {
    # 使用 DashScope 提供的模型服务：
    &#x27;model&#x27;: &#x27;qwen-max&#x27;,
    &#x27;model_server&#x27;: &#x27;dashscope&#x27;,
    # &#x27;api_key&#x27;: &#x27;YOUR_DASHSCOPE_API_KEY&#x27;,
    # 如果这里没有设置 &#x27;api_key&#x27;，它将读取 `DASHSCOPE_API_KEY` 环境变量。

    # 使用与 OpenAI API 兼容的模型服务，例如 vLLM 或 Ollama：
    # &#x27;model&#x27;: &#x27;Qwen2-7B-Chat&#x27;,
    # &#x27;model_server&#x27;: &#x27;http://localhost:8000/v1&#x27;,  # base_url，也称为 api_base
    # &#x27;api_key&#x27;: &#x27;EMPTY&#x27;,

    # （可选） LLM 的超参数：
    &#x27;generate_cfg&#x27;: {
        &#x27;top_p&#x27;: 0.8
    }
}

# 步骤 3：创建一个智能体。这里我们以 `Assistant` 智能体为例，它能够使用工具并读取文件。
system_instruction = &#x27;&#x27;&#x27;你是一个乐于助人的AI助手。
在收到用户的请求后，你应该：
- 首先绘制一幅图像，得到图像的url，
- 然后运行代码`request.get`以下载该图像的url，
- 最后从给定的文档中选择一个图像操作进行图像处理。
用 `plt.show()` 展示图像。
你总是用中文回复用户。&#x27;&#x27;&#x27;
tools = [&#x27;my_image_gen&#x27;, &#x27;code_interpreter&#x27;]  # `code_interpreter` 是框架自带的工具，用于执行代码。
files = [&#x27;./examples/resource/doc.pdf&#x27;]  # 给智能体一个 PDF 文件阅读。
bot = Assistant(llm=llm_cfg,
                system_message=system_instruction,
                function_list=tools,
                files=files)

# 步骤 4：作为聊天机器人运行智能体。
messages = []  # 这里储存聊天历史。
while True:
    # 例如，输入请求 &quot;绘制一只狗并将其旋转 90 度&quot;。
    query = input(&#x27;用户请求: &#x27;)
    # 将用户请求添加到聊天历史。
    messages.append({&#x27;role&#x27;: &#x27;user&#x27;, &#x27;content&#x27;: query})
    response = []
    for response in bot.run(messages=messages):
        # 流式输出。
        print(&#x27;机器人回应:&#x27;)
        pprint.pprint(response, indent=2)
    # 将机器人的回应添加到聊天历史。
    messages.extend(response)
```

除了使用框架自带的智能体实现（如 class Assistant ），您也可以通过继承 class Agent 来自行开发您的智能体实现。更多使用示例，请参阅 examples 目录。

# **FAQ**

## 支持函数调用（也称为工具调用）吗？

支持，LLM类提供了 函数调用 的支持。此外，一些Agent类如FnCallAgent和ReActChat也是基于函数调用功能构建的。

## 如何让AI基于超长文档进行问答？

我们已发布了一个 快速的RAG解决方案 ，以及一个虽运行成本较高但 准确度较高的智能体 ，用于在超长文档中进行问答。它们在两个具有挑战性的基准测试中表现出色，超越了原生的长上下文模型，同时更加高效，并在涉及100万字词上下文的“大海捞针”式单针查询压力测试中表现完美。欲了解技术细节，请参阅 博客 。

![](img/16053d98490a1986c0130d72ed3886c3.png)

# **应用：BrowserQwen**

BrowserQwen 是一款基于 Qwen-Agent 构建的浏览器助手。如需了解详情，请参阅其 文档 。

文档：https://pypi.org/project/qwen-agent/

博客：https://qwenlm.github.io/blog/qwen-agent

**知音难求，自我修炼亦艰**

抓住前沿技术的机遇，与我们一起成为创新的超级个体

**（把握AIGC时代的个人力量）**

![](img/c378feee34ad2e5f74bddd463da22ebe.png)