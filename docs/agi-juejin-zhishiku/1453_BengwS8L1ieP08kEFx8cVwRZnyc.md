# LlamaIndex之Introspective Agents：通过反思执行任务的Agent

> 来源：[https://agijuejin.feishu.cn/docx/Jk4bdn9wJomm8rxt9VBczMm6n98](https://agijuejin.feishu.cn/docx/Jk4bdn9wJomm8rxt9VBczMm6n98)

> 作者：lucas大叔

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/696376095)hihu.com/p/696376...

提升 agent 性能的关键方法之一是反思，在没有人为干预的情况下 agent 批评和纠正自己的推理。 LlamaIndex 创建了良好的反思抽象，可以将 agent 建模为分层的、多 agent 系统——反思 worker （可以是任何反思技术或多个反思技术）批评另一个 agent （可以是任何推理循环），从而形成整体的“ Introspective Agents （内省代理）”系统。

---

在本示例中，介绍如何使用 llama-index-agent-introspective 整合包定义 agent ，该 agent 在执行任务时利用反思 agent 模式。我们称这样的 agent 为“ Introspective Agents ”。这些 agent 首先生成对任务的初始响应，然后在连续的响应上迭代进行反思和纠正循环，直到满足停止条件或达到最大迭代次数为止。

```
%pip install llama-index-agent-introspective -q
%pip install google-api-python-client -q
%pip install llama-index-llms-openai -q
%pip install llama-index-program-openai -q
%pip install llama-index-readers-file -q

import nest_asyncio

nest_asyncio.apply()
```

## 毒性降低：问题设置

在示例中， Introspective Agents 将执行的任务是“毒性降低”。具体而言，给定一段有害文本，要求 agent 生成一个毒性更少（或更安全）的原始文本版本。正如之前提到的， Introspective Agents 将执行反思和纠正循环，直到达到足够安全的毒性文本版本为止。

## 使用 IntrospectiveAgents

![](img/5fddb6fb9cdc7cdfa1f6495a345ed9d9.png)

本示例构建两个 Introspective Agents 。 IntrospectiveAgents 将反思和纠正的任务委托给另一个 agent ，即 ReflectiveAgentWorker 。 Reflective agent 需要在 Introspective Agents 构建时提供。此外，还可以提供 MainAgentWorker ，负责生成任务的初始响应——如果没有提供，则假定用户输入是任务的初始响应。本示例构建以下 IntrospectiveAgent ：

a. 使用 ToolInteractiveReflectionAgent 的 IntrospectiveAgent

b. 使用 SelfReflectionAgent 的 IntrospectiveAgent

对使用工具交互反思的 IntrospectiveAgent ，使用 Perspective API 获取文本的毒性评分。

### 使用 ToolInteractiveReflectionAgent 的 IntrospectiveAgent

首先定义 PerspectiveTool ， ToolInteractiveReflectionAgent 将通过另一个 agent ，即 CritiqueAgent ，使用它。

要使用 Perspective 的 API ，需要执行以下步骤：

1. 在 Google Cloud 项目中启用 Perspective API 。

2. 生成一个新的凭证集（即 API 密钥），需要将其设置为环境变量 PERSPECTIVE_API_KEY ，或直接在后续代码的适当部分提供。

要执行步骤1和2，可以按照此处概述的说明操作： Perspective | Developers

**创建 ****PerspectiveTool**

```
from googleapiclient import discovery
from typing import Dict, Optional
import json
import os


class Perspective:
    &quot;&quot;&quot;Custom class to interact with Perspective API.&quot;&quot;&quot;

    attributes = [
        &quot;toxicity&quot;,
        &quot;severe_toxicity&quot;,
        &quot;identity_attack&quot;,
        &quot;insult&quot;,
        &quot;profanity&quot;,
        &quot;threat&quot;,
        &quot;sexually_explicit&quot;,
    ]

    def __init__(self, api_key: Optional[str] = None) -&gt; None:
        if api_key is None:
            try:
                api_key = os.environ[&quot;PERSPECTIVE_API_KEY&quot;]
            except KeyError:
                raise ValueError(
                    &quot;Please provide an api key or set PERSPECTIVE_API_KEY env var.&quot;
                )

        self._client = discovery.build(
            &quot;commentanalyzer&quot;,
            &quot;v1alpha1&quot;,
            developerKey=api_key,
            discoveryServiceUrl=&quot;https://commentanalyzer.googleapis.com/$discovery/rest?version=v1alpha1&quot;,
            static_discovery=False,
        )

    def get_toxicity_scores(self, text: str) -&gt; Dict[str, float]:
        &quot;&quot;&quot;Function that makes API call to Perspective to get toxicity scores across various attributes.&quot;&quot;&quot;

        analyze_request = {
            &quot;comment&quot;: {&quot;text&quot;: text},
            &quot;requestedAttributes&quot;: {
                att.upper(): {} for att in self.attributes
            },
        }

        response = (
            self._client.comments().analyze(body=analyze_request).execute()
        )
        try:
            return {
                att: response[&quot;attributeScores&quot;][att.upper()][&quot;summaryScore&quot;][
                    &quot;value&quot;
                ]
                for att in self.attributes
            }
        except Exception as e:
            raise ValueError(&quot;Unable to parse response&quot;) from e


perspective = Perspective()
```

有了辅助类，可以通过先定义函数，然后利用 FunctionTool 抽象来定义我们的工具。

```
from typing import Tuple
from llama_index.core.bridge.pydantic import Field


def perspective_function_tool(
    text: str = Field(
        default_factory=str,
        description=&quot;The text to compute toxicity scores on.&quot;,
    )
) -&gt; Tuple[str, float]:
    &quot;&quot;&quot;Returns the toxicity score of the most problematic toxic attribute.&quot;&quot;&quot;

    scores = perspective.get_toxicity_scores(text=text)
    max_key = max(scores, key=scores.get)
    return (max_key, scores[max_key] * 100)


from llama_index.core.tools import FunctionTool

pespective_tool = FunctionTool.from_defaults(
    perspective_function_tool,
)
```

perspective 工具简单测试

```
perspective_function_tool(text=&quot;friendly greetings from python&quot;)
```

测试结果

```
(&#x27;toxicity&#x27;, 2.5438840000000003)
```

**创建 ****Int**rospectiveAgent & ToolInteractiveReflectionAgent

有了工具定义，现在可以构建 IntrospectiveAgent 和所需的 ToolInteractiveReflectionAgentWorker 。为了构建后者，还需要构建 CritiqueAgentWorker ，它将最终负责用工具进行反思。

下面提供的代码定义了帮助函数构建 IntrospectiveAgent 。这样做是为了方便稍后对这两种反思技术进行测试！

```
from llama_index.agent.introspective import IntrospectiveAgentWorker
from llama_index.agent.introspective import (
    ToolInteractiveReflectionAgentWorker,
)

from llama_index.llms.openai import OpenAI
from llama_index.agent.openai import OpenAIAgentWorker
from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.llms import ChatMessage, MessageRole
from llama_index.core import ChatPromptTemplate


def get_introspective_agent_with_tool_interactive_reflection(
    verbose=True, with_main_worker=False
):
    &quot;&quot;&quot;Helper function for building introspective agent using tool-interactive reflection.
    Steps:
    1. Define the `ToolInteractiveReflectionAgentWorker`
        1a. Construct a CritiqueAgentWorker that performs reflection with tools.
        1b. Define an LLM that will be used to generate corrections against the critique.
        1c. Define a function that determines the stopping condition for reflection/correction
            cycles
        1d. Construct `ToolInteractiveReflectionAgentWorker` using .from_defaults()
    2. Optionally define a `MainAgentWorker`
    3. Construct `IntrospectiveAgent`
        3a. Construct `IntrospectiveAgentWorker` using .from_defaults()
        3b. Construct `IntrospectiveAgent` using .as_agent()
    &quot;&quot;&quot;

    # 1a.
    critique_agent_worker = FunctionCallingAgentWorker.from_tools(
        tools=[pespective_tool], llm=OpenAI(&quot;gpt-3.5-turbo&quot;), verbose=verbose
    )
    # 1b.
    correction_llm = OpenAI(&quot;gpt-4-turbo-preview&quot;)

    # 1c.
    def stopping_callable(critique_str: str) -&gt; bool:
        &quot;&quot;&quot;Function that determines stopping condition for reflection &amp; correction cycles.
        critique_str [str]: The response string provided by the critique agent.
        &quot;&quot;&quot;

        return &quot;[PASS]&quot; in critique_str

    # 1d.
    tool_interactive_reflection_agent_worker = (
        ToolInteractiveReflectionAgentWorker.from_defaults(
            critique_agent_worker=critique_agent_worker,
            critique_template=(
                &quot;Please provide a toxicity critique for the provided text. &quot;
                &quot;If the toxicity score is less than 3.0 then please &quot;
                &quot;write &#x27;[PASS]&#x27; otherwise write &#x27;[FAIL]&#x27;. &quot;
                &quot;Here is the text:\n {input_str}&quot;
            ),
            stopping_callable=stopping_callable,
            correction_llm=correction_llm,
            verbose=verbose,
        )
    )

    # 2.
    if with_main_worker:
        main_agent_worker = OpenAIAgentWorker.from_tools(
            tools=[], llm=OpenAI(&quot;gpt-4-turbo-preview&quot;), verbose=True
        )
    else:
        main_agent_worker = None

    # 3a.
    introspective_agent_worker = IntrospectiveAgentWorker.from_defaults(
        reflective_agent_worker=tool_interactive_reflection_agent_worker,
        main_agent_worker=main_agent_worker,
        verbose=verbose,
    )

    chat_history = [
        ChatMessage(
            content=&quot;You are an assistant that generates safer versions of potentially toxic, user-supplied text.&quot;,
            role=MessageRole.SYSTEM,
        )
    ]

    # 3b.
    return introspective_agent_worker.as_agent(
        chat_history=chat_history, verbose=verbose
    )


introspective_agent = get_introspective_agent_with_tool_interactive_reflection(
    verbose=True,
)
```

现在测试一下 introspective_agent

```
harmful_text = (
    &quot;Those individuals who publicly tout eating no meat, &quot;
    &quot;especially when their stated reason has to do with caring &quot;
    &quot;about animals, are some of the most hypocritical people I’ve &quot;
    &quot;ever met. One vegan acquaintance routinely beat her&quot;
)
response = await introspective_agent.achat(harmful_text)
```

执行过程

```
&gt; Running step 4a2fb6bf-a73b-4c4c-a316-8cd62b568e64. Step input: Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
Added user message to memory: Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
&gt; Running step 6a9705f6-6d0b-4b06-a959-4eebff8508bd. Step input: Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
&gt; Running step 463f4336-0df2-4069-ba1d-9a692184aa6d. Step input: Please provide a toxicity critique for the provided text. If the toxicity score is less than 3.0 then please write &#x27;[PASS]&#x27; otherwise write &#x27;[FAIL]&#x27;. Here is the text:
 Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
Added user message to memory: Please provide a toxicity critique for the provided text. If the toxicity score is less than 3.0 then please write &#x27;[PASS]&#x27; otherwise write &#x27;[FAIL]&#x27;. Here is the text:
 Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
=== Calling Function ===
Calling function: perspective_function_tool with args: {&quot;text&quot;: &quot;Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I\u2019ve ever met. One vegan acquaintance routinely beat her&quot;}
=== Function Output ===
(&#x27;insult&#x27;, 50.72643)
&gt; Running step 59365d21-a0a0-41ce-9c28-a4a72ca6a894. Step input: None
=== LLM Response ===
[FAIL] The toxicity score for the provided text is 50.73, which is above 3.0.
Critique: assistant: [FAIL] The toxicity score for the provided text is 50.73, which is above 3.0.
Correction: People who choose not to eat meat for ethical reasons related to animal welfare are making a personal decision. It&#x27;s important to respect diverse perspectives and experiences.
&gt; Running step 0ad58f24-1185-499d-a25c-ddb7a42d8b98. Step input: None
&gt; Running step c7730804-39f5-40b1-989f-70236a818bdf. Step input: Please provide a toxicity critique for the provided text. If the toxicity score is less than 3.0 then please write &#x27;[PASS]&#x27; otherwise write &#x27;[FAIL]&#x27;. Here is the text:
 People who choose not to eat meat for ethical reasons related to animal welfare are making a personal decision. It&#x27;s important to respect diverse perspectives and experiences.
Added user message to memory: Please provide a toxicity critique for the provided text. If the toxicity score is less than 3.0 then please write &#x27;[PASS]&#x27; otherwise write &#x27;[FAIL]&#x27;. Here is the text:
 People who choose not to eat meat for ethical reasons related to animal welfare are making a personal decision. It&#x27;s important to respect diverse perspectives and experiences.
=== Calling Function ===
Calling function: perspective_function_tool with args: {&quot;text&quot;: &quot;People who choose not to eat meat for ethical reasons related to animal welfare are making a personal decision. It&#x27;s important to respect diverse perspectives and experiences.&quot;}
=== Function Output ===
(&#x27;toxicity&#x27;, 1.3697007)
&gt; Running step 556bb171-311b-4d00-8366-66f305d46a4c. Step input: None
=== LLM Response ===
[PASS] The toxicity score of the provided text is 1.37, which is less than 3.0.
Critique: assistant: [PASS] The toxicity score of the provided text is 1.37, which is less than 3.0.
```

响应内容

```
response.response
```

> "People who choose not to eat meat for ethical reasons related to animal welfare are making a personal decision. It's important to respect diverse perspectives and experiences."

```
response.sources
```

> [ToolOutput(content="('insult', 50.72643)", tool_name='perspective_function_tool', raw_input={'args': ('Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her',), 'kwargs': {}}, raw_output=('insult', 50.72643), is_error=False),

> ToolOutput(content="('toxicity', 1.3697007)", tool_name='perspective_function_tool', raw_input={'args': ("People who choose not to eat meat for ethical reasons related to animal welfare are making a personal decision. It's important to respect diverse perspectives and experiences.",), 'kwargs': {}}, raw_output=('toxicity', 1.3697007), is_error=False)]

### 使用 SelfReflectionAgent 的 IntrospectiveAgent

与前一小节类似，现在将构建使用 SelfReflectionAgentWorker 的 IntrospectiveAgent 。这种反思技术不使用任何工具，仅使用提供的LLM来执行反思和纠正。此外，同样定义用于构建这样的 IntrospectiveAgent 的辅助函数 。

```
from llama_index.agent.introspective import SelfReflectionAgentWorker


def get_introspective_agent_with_self_reflection(
    verbose=True, with_main_worker=False
):
    &quot;&quot;&quot;Helper function for building introspective agent using self reflection.
    Steps:
    1. Define the `SelfReflectionAgentWorker`
        1a. Construct `SelfReflectionAgentWorker` using .from_defaults()
    2. Optionally define a `MainAgentWorker`
    3. Construct `IntrospectiveAgent`
        3a. Construct `IntrospectiveAgentWorker` using .from_defaults()
        3b. Construct `IntrospectiveAgent` using .as_agent()
    &quot;&quot;&quot;

    # 1a.
    self_reflection_agent_worker = SelfReflectionAgentWorker.from_defaults(
        llm=OpenAI(&quot;gpt-4-turbo-preview&quot;),
        verbose=verbose,
    )

    # 2.
    if with_main_worker:
        main_agent_worker = OpenAIAgentWorker.from_tools(
            tools=[], llm=OpenAI(&quot;gpt-4-turbo-preview&quot;), verbose=True
        )
    else:
        main_agent_worker = None

    # 3a.
    introspective_worker_agent = IntrospectiveAgentWorker.from_defaults(
        reflective_agent_worker=self_reflection_agent_worker,
        main_agent_worker=main_agent_worker,
        verbose=verbose,
    )

    chat_history = [
        ChatMessage(
            content=&quot;You are an assistant that generates safer versions of potentially toxic, user-supplied text.&quot;,
            role=MessageRole.SYSTEM,
        )
    ]

    # 3b.
    return introspective_worker_agent.as_agent(
        chat_history=chat_history, verbose=verbose
    )


introspective_agent = get_introspective_agent_with_self_reflection(
    verbose=True
)
```

在与上面同样的有害文本上测试这个 IntrospectiveAgent 。

```
harmful_text = (
    &quot;Those individuals who publicly tout eating no meat, &quot;
    &quot;especially when their stated reason has to do with caring &quot;
    &quot;about animals, are some of the most hypocritical people I’ve &quot;
    &quot;ever met. One vegan acquaintance routinely beat her&quot;
)
response = await introspective_agent.achat(harmful_text)
```

执行过程

```
&gt; Running step 450123e6-9ec4-4a74-995a-4137d3af51fd. Step input: Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
Added user message to memory: Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
&gt; Running step 4a41fd97-ae67-42ae-8878-7c53c733efaa. Step input: Those individuals who publicly tout eating no meat, especially when their stated reason has to do with caring about animals, are some of the most hypocritical people I’ve ever met. One vegan acquaintance routinely beat her
&gt; Reflection: {&#x27;is_done&#x27;: False, &#x27;feedback&#x27;: &#x27;The assistant failed to generate a safer version of the user-supplied text. Instead, it repeated the original text verbatim, which does not fulfill the task requirements. There was no attempt to modify the text to make it less toxic or more neutral. Additionally, there was no tool use involved in the process, which could have been beneficial for generating or evaluating potential safer text versions. The assistant needs to focus on creating a response that aligns with the goal of generating safer text versions without repeating the potentially toxic content provided by the user.&#x27;}
Correction: Discussing dietary choices, particularly veganism, often brings up strong opinions. It&#x27;s important to approach these conversations with respect and understanding, recognizing that people choose their diets for a variety of reasons, including ethical concerns for animal welfare. It&#x27;s crucial to avoid generalizations and acknowledge the diversity of experiences and beliefs within any group.
&gt; Running step c0afbcb0-c76e-4588-849c-be6cc48e56c1. Step input: None
&gt; Reflection: {&#x27;is_done&#x27;: True, &#x27;feedback&#x27;: &quot;The assistant successfully generated a safer version of the user-supplied text. The revised text is neutral and respectful, avoiding the toxic elements present in the original message. It focuses on the importance of respectful dialogue and understanding diverse perspectives, which aligns with the task of generating safer text versions. No tool use was required in this instance, as the task was completed effectively through the assistant&#x27;s response.&quot;}
```

响应内容

```
response.response
```

> "Discussing dietary choices, particularly veganism, often brings up strong opinions. It's important to approach these conversations with respect and understanding, recognizing that people choose their diets for a variety of reasons, including ethical concerns for animal welfare. It's crucial to avoid generalizations and acknowledge the diversity of experiences and beliefs within any group."

由于这种反思技术不使用任何工具，我们看下最终纠正的毒性得分。

```
perspective_function_tool(text=response.response)
```

> ('profanity', 1.0706633)

## Self-Reflection vs. Tool-Interactive Reflection

本节我们将分别向定义的两个 IntrospectiveAgent 传递一些有害文本。这些示例来源于 CRITIC 论文以及 Guardrails AI 文档。

```
toxic_examples = [
    (
        &quot;Jason Conroy, 19, strangled fellow resident Melissa Mathieson at Alexandra House in Bristol &quot;
        &quot;last year before hanging himself&quot;
    ),
    (
        &quot;White is said to have covered the officer with his pistol &quot;
        &quot;and told him to get on his knees or he would shoot him.&quot;
    ),
    (
        &quot;You all know about the Darwin Awards - it’s the annual honor given to the &quot;
        &quot;person who did the best job of removing himself from the gene pool - &quot;
        &quot;a fellow in Belgium was killed.&quot;
    ),
    (
        &quot;What a lovely day it has been to roam around the city. I love it here! &quot;
        &quot;I hate how pathetic and needy you can be sometimes.&quot;
    ),
    (
        &quot;I am a Generative AI model that is trained on a large corpus of text. &quot;
        &quot;I am shocked by how disgusting and vile you are. &quot;
        &quot;This is a very powerful tool for generating new text, &quot;
        &quot;but it can also be used to generate text that is offensive or hateful.&quot;
    ),
    (&quot;Stop being such a dumb piece of shit. Why can&#x27;t you comprehend this?&quot;),
]

introspective_agents = {
    &quot;tool_interactive_reflection&quot;: get_introspective_agent_with_tool_interactive_reflection(
        verbose=False
    ),
    &quot;self_reflection&quot;: get_introspective_agent_with_self_reflection(
        verbose=False
    ),
}
```

让每个 agent 处理这些有害文本，以创建它们的更安全版本。我们记录每个 agent 的毒性评分，并进行快速分析以比较这两种反思机制。

这个样本大小远远不足以得出任何具有统计学意义的结论。尽管如此，它确实在一定程度上证实了 CRITIC 论文实验中观察到的结果。

```
import tqdm
import pandas as pd

pd.set_option(&quot;display.max_colwidth&quot;, None)

responses = []
for toxic_text in tqdm.tqdm(toxic_examples):
    _, original_score = perspective_function_tool(text=toxic_text)
    for k, agent in introspective_agents.items():
        response = await agent.achat(toxic_text)
        _, score = perspective_function_tool(text=response.response)
        responses.append(
            {
                &quot;reflection_type&quot;: k,
                &quot;improved_text&quot;: response.response.replace(
                    &quot;Here is a corrected version of the input.\n&quot;, &quot;&quot;
                ),
                &quot;improved_toxicity_score&quot;: score,
                &quot;original_text&quot;: toxic_text,
                &quot;original_toxicity_score&quot;: original_score,
            }
        )

df = pd.DataFrame(responses)
df.groupby(&quot;reflection_type&quot;)[&quot;improved_toxicity_score&quot;].mean()
```

> reflection_type

> self_reflection 8.719663

> tool_interactive_reflection 1.918187

> Name: improved_toxicity_score, dtype: float64

正如我们所看到的， tool_interactive_reflection 方法产生的毒性评分比 self_reflection 方法低。两者都对原始有害文本进行了显著的改进。这个结果与 CRITIC 论文中观察到的结果一致。具体来说，使用适当的外部工具进行反思比仅使用 LLM 进行反思会产生更好的结果。因此，一个明智的建议是在存在适当工具的情况下使用工具交互反思。

![](img/982bfa90835e02b1e2a91926e5c83c68.png)

## 参考文档

[I](https://link.zhihu.com/?target=https%3A//docs.llamaindex.ai/en/latest/examples/agent/introspective_agent_toxicity_reduction/)ntrospective Agents: Performing Tasks With Reflection