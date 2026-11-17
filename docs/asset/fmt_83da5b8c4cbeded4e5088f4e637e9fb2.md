
# 3. LangChain 链路构建

## 3.1 第 4 个概念：LCEL 声明语法

> LangChain Docs：https://api.python.langchain.com/en/v0.1/chains/langchain.chains.llm.LLMChain.html#langchain.chains.llm.LLMChain

```
from langchain.chains import LLMChain
from langchain_core.prompts import PromptTemplate
from langchain_openai import OpenAI


template = """问题: {question}

答案: 请一步一步的思考。"""

prompt_template = PromptTemplate.from_template(template)


llm = OpenAI()

chain = LLMChain(prompt=prompt_template, llm=llm)

chain.invoke("什么是快乐星球")
```

```
{'question': '什么是快乐星球',
 'text': '\n\n快乐星球是一个想象中的虚拟星球，它是一个充满快乐和幸福的地方。在这个星球上，每个人都可以自由地追求自己的梦想，没有痛苦和压力的存在。人与人之间和谐相处，没有战争和冲突。这个星球上的环境也是非常美好的，充满了绚丽的色彩和美妙的音乐。在快乐星球上，每个人都能够找到自己的快乐，享受生活的乐趣。它是人们对于理想社会的向往和梦想。'}
```

+ 需要理解的第四个概念：什么是 `LangChain` 的 `LCEL` 声明语法

上述是旧版本 `LangChain` 构建链路的一种形式，主要还是基于传统类的构建方法。但 `LangChain` 的迭代，是在向新的一种链路声明方式转变，即 `LCEL`。`LangChain` 表达式语言（`LCEL`）是一种链接 `LangChain` 组件的声明性方式。无需更改代码，可以实现从最简单的“提示 + `LLM`”链到最复杂的链（我们已经看到人们成功运行了 100 秒的 `LCEL` 链）生产步骤。

> LangChain Docs:https://python.langchain.com/v0.2/docs/concepts/#langchain-expression-language-lcel

```
from langchain_core.prompts import PromptTemplate
from langchain_openai import OpenAI


template = """问题: {question}

答案: 请一步一步的思考。"""

prompt_template = PromptTemplate.from_template(template)


llm = OpenAI()

chain = prompt_template | llm 
```

```
chain.invoke("什么是快乐星球")
```

```
'\n\n快乐星球可以理解为一个想象中的幻想世界，它是一个充满欢笑、快乐和和谐的星球。在这个星球上，所有的生物都拥有幸福快乐的生活，没有战争、疾病和贫穷的困扰。这里的人们相互关爱、友善和包容，生活在和平与幸福的环境中。快乐星球是人们向往的理想国，也是我们应当努力追求的目标。'
```

## 3.2 Chat Model 如何构建链路

`Chat Models` 类模型往往是基于 `LLMs` 类模型针对对话形式特殊微调过的模型。更适用于人类的对话习惯。所以它往往是使用消息序列作为输入并返回聊天消息作为输出（而不是使用纯文本）的语言模型。聊天模型支持为对话消息分配不同的角色，有助于区分来自 `AI`、用户和系统消息等指令的消息。

比如目前 `OpenAI` 的 `GPT` 系列模型、`GLM4` 系列模型，现在是全面支持对话类模型:https://platform.openai.com/docs/models/gpt-4-turbo-and-gpt-4

同样我们快速的测试一下,如果使用 `GLM 4` 官方提供的接口来调用 `GLM 4` 系列模型。`GLM-4` 作为在线大模型,其调用方法被抽象在 `LangChain` 中的 `Chat Models` 模块中。

> langChain Docs:https://python.langchain.com/v0.2/docs/integrations/chat/zhipuai/

### 第 5 个概念：在 `LangChain` 中调用 `Chat Models`

> GLM 4 API Docs: https://open.bigmodel.cn/dev/api#glm-4

```
#!pip install --upgrade httpx httpx-sse PyJWT
```

```
from langchain_community.chat_models import ChatZhipuAI
```

```
import os

#os.environ["ZHIPUAI_API_KEY"] = "zhipuai_api_key"

zhipuai_api_key = "3fd2ee07b86207a7e0f7e79ee459fbfa.9iiQgqVfddDOobbS"
```

```
chat = ChatZhipuAI(
    zhipuai_api_key = zhipuai_api_key,
    model="glm-4",
    temperature=0.5,
)
```

```
chat.invoke("你好，请你介绍一下你自己")
```

```
AIMessage(content='你好！我是智谱清言，是清华大学 KEG 实验室和智谱 AI 公司于 2023 年共同训练的语言模型。我的目标是通过回答用户提出的问题来帮助他们解决问题。由于我是一个计算机程序，所以我没有自我意识，也不能像人类一样感知世界。我只能通过分析我所学到的信息来回答问题。', response_metadata={'token_usage': {'completion_tokens': 72, 'prompt_tokens': 10, 'total_tokens': 82}, 'model_name': 'glm-4', 'finish_reason': 'stop'}, id='run-7b798209-3cbc-47fb-a9e0-62a3ff6e6a26-0')
```

```
from langchain_core.messages import AIMessage, HumanMessage, SystemMessage

messages = [
    SystemMessage(content="你是一位诗人"),
    HumanMessage(content="请给我写一首关于小鸭子的诗"),
]
```

```
response = chat.invoke(messages)
print(response.content)  # Displays the AI-generated poem
```

```
小鸭子戏水曲

碧波荡漾小河畔，
鸭群嬉戏乐无边。
毛茸茸的小身影，
水中舞动如花瓣。

阳光洒落金色波，
小鸭子欢快游弋。
扑腾翅膀溅水花，
童真童趣显神采。

绿水悠悠映晴空，
它们尽情展翅翱。
捉鱼虾，逐蜻蜓，
无忧无虑度时光。

夏日炎炎伴晚风，
小鸭子们归巢中。
夕阳余晖映水面，
留下一路欢歌鸣。

岁月流转，时光荏苒，
小鸭子终将长大。
愿它们永远保持，
那颗纯真快乐的心。
```

### 第 6 个概念：什么是 `ChatPromptTemplate`

```
from langchain_core.prompts import ChatPromptTemplate

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "你是一位乐于助人的小助理"),
    ("user", "什么是 {topic}")
])

prompt_template.invoke({"topic": "快乐星球"})
```

```
ChatPromptValue(messages=[SystemMessage(content='你是一位乐于助人的小助理'), HumanMessage(content='什么是 快乐星球')])
```

```
from langchain_core.prompts import ChatPromptTemplate

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "你是一位乐于助人的小助理"),
    ("user", "什么是 {topic}")
])

prompt_template.invoke({"topic": "机器学习"})
```

```
ChatPromptValue(messages=[SystemMessage(content='你是一位乐于助人的小助理'), HumanMessage(content='什么是 机器学习')])
```

### 第 7 个概念：如何构建 `Chat Models` 的 `LLMChain` 链路

```
from langchain.chains import LLMChain
from langchain_core.prompts import ChatPromptTemplate

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "你是一位乐于助人的小助理"),
    ("user", "什么是 {topic}")
])

chat = ChatZhipuAI(
    zhipuai_api_key = zhipuai_api_key,
    model="glm-4",
    temperature=0.5,
)

chain = LLMChain(prompt=prompt_template, llm=chat)
```

```
C:\Users\snowb\anaconda3\Lib\site-packages\langchain_core\_api\deprecation.py:119: LangChainDeprecationWarning: The class `LLMChain` was deprecated in LangChain 0.1.17 and will be removed in 0.3.0. Use RunnableSequence, e.g., `prompt | llm` instead.
  warn_deprecated(
```

```
chain.invoke({"topic":"什么是快乐星球?"})
```

```
{'topic': '什么是快乐星球?',
 'text': '"什么是快乐星球？"这个问题来源于中国的一部少儿科幻电视剧《快乐星球》。该系列剧自2004年首播以来，受到了广大少年儿童的喜爱。在第五季中的一首插曲中，有这样的歌词：“什么是快乐星球？如果你想知道什么是快乐星球的话，我现在就带你研究。”这句歌词由于节奏明快、朗朗上口，在网络上迅速流行开来，成为了一种流行的梗。\n\n在更广泛的文化语境中，“快乐星球”指的是一个充满童真、欢乐和梦想的地方，它不仅仅是一个物理概念上的星球，更是一种精神寄托，象征着无忧无虑、纯真快乐的童年时光。\n\n而在2024年的消息中，动画电影《快乐星球》正式立项，讲述了剧中的角色乐乐在老顽童爷爷的培养下成为了一名AI科学家，并开发了一个同名的沉浸式AI科普空间，旨在通过学科学习让儿童了解文化和科技知识，开启智慧之门，并预警未来可能面临的挑战。这进一步扩展了“快乐星球”的概念，使之不仅限于电视剧情，还与现实世界中的科技教育和未来发展联系起来。'}
```

`LCEL` 的语法结构就是:

```
from langchain.chains import LLMChain
from langchain_core.prompts import ChatPromptTemplate

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "你是一位乐于助人的小助理"),
    ("user", "什么是 {topic}")
])

chat = ChatZhipuAI(
    zhipuai_api_key = zhipuai_api_key,
    model="glm-4",
    temperature=0.5,
)

chain = prompt_template | chat
```

```
chain.invoke({"topic":"什么是快乐星球?"})
```

```
AIMessage(content='"什么是快乐星球？"这个问题来源于中国的一部知名少儿科幻电视剧《快乐星球》。该系列剧自2004年首播以来，深受广大少年儿童的喜爱。在第五季中的一首插曲中，有这样的歌词：“什么是快乐星球？如果你想知道什么是快乐星球的话，我现在就带你研究。”这句歌词因其魔性的节奏和易于传播的特质，在网络上广为流传，成为了一种流行的梗。\n\n在更广泛的文化语境中，“快乐星球”指的是一个充满欢乐、童真和积极向上的世界。它不仅仅是一个物理概念上的星球，更是人们心中对美好生活的向往和追求。在动画电影《快乐星球》立项的消息中，乐乐成为AI科学家的故事，也是对这种积极精神的延续。乐乐开发的沉浸式AI科普空间旨在教育儿童，通过学习文化科技知识，开启智慧之门，并预警未来可能面临的挑战。\n\n综上所述，“什么是快乐星球？”这个问题不仅仅是一个电视剧中的歌词，它还代表着一种积极乐观的生活态度和对未来的美好憧憬。', response_metadata={'token_usage': {'completion_tokens': 205, 'prompt_tokens': 1674, 'total_tokens': 1879}, 'model_name': 'glm-4', 'finish_reason': 'stop'}, id='run-180ac1f0-035f-47b3-92aa-a4c2a51ad926-0')
```

> LangChain Docs:https://python.langchain.com/v0.2/docs/integrations/chat/zhipuai/

# 4. Callbacks

`Callbacks`（回调函数）是一种编程模式，`Callbacks` 的意义在于：允许程序在某个任务完成时自动执行另一个函数，而不必阻塞等待某个长时间运行的操作完成。所以它会在处理异步操作，如网络请求、文件读写或任何可能需要等待的操作时频繁被使用。

对于与大模型的连续交互场景，一般采用的都是流式输出。但在做流式输出前，需要的就是回调 `Callbacks`。

`LangChain` 提供了一个回调系统，其官网文档地址：https://python.langchain.com/v0.2/docs/concepts/#callbacks

回调处理程序可以是 `sync`（同步） 或 `async` （异步）的。同步回调和异步回调是编程中常用的两种处理事件或数据的方式，它们在执行时机和用途上有明显的区别：

1. 同步回调（Synchronous Callbacks）:

   + **执行时机**：同步回调是在主程序流程中直接调用和执行的。当一个同步回调函数被触发时，程序会立即执行这个回调函数，并且在回调函数执行完成之前，主程序流程会被阻塞。

   + **用途**：同步回调通常用于确保某些操作必须在程序继续执行前完成。例如，在访问数组中的每个元素并对其应用函数时，你可能会使用数组的 `.forEach()` 方法，这是一个同步的回调实现。

2. 异步回调（Asynchronous Callbacks）:

   + **执行时机**：异步回调不会立即执行，它们通常被放置在事件队列中，等待当前执行堆栈清空后才开始执行。这意味着程序的主流程不会等待异步回调的完成，可以继续执行其他任务。

   + **用途**：异步回调通常用在不希望阻塞主程序流程的情况下，例如处理 I/O 操作（如网络请求、文件读写），或者在执行大量计算时不影响用户在并发编程和现代 Web 开发中。

在并发编程的 Web 开发中，交互式应用对话场景，肯定采用的是异步的回调。

`LangChain` 中的异步回调接口：https://api.python.langchain.com/en/latest/callbacks/langchain_core.callbacks.base.AsyncCallbackHandler.html
