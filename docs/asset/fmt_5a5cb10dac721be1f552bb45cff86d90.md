
```
# 计算相似度
similarity_1 = cosine_similarity(query[0], answer_1[0])
similarity_2 = cosine_similarity(query[0], answer_2[0])
similarity_3 = cosine_similarity(query[0], answer_3[0])

# 打印相似度结果
print(f"Query和Answer 1的相似度: {similarity_1:.4f}")
print(f"Query和Answer 2的相似度: {similarity_2:.4f}")
print(f"Query和Answer 3的相似度: {similarity_3:.4f}")
```

```
Query和Answer 1的相似度: 0.5054
Query和Answer 2的相似度: 0.3453
Query和Answer 3的相似度: 0.2463
```

那我们来计算一个`Query`和输入的`PDF`的`Chunks`的相似度。

```
# 生成Query的Embedding
query_text = "什么是LangChain？"
query_embedding = get_embeddings(query_text)

# 假设result已经是包含多个字典的列表，每个字典中包含p
```

```
# 计算相似度并存储结果
similarity_results = []
for item in results:
    similarity = cosine_similarity(query_embedding, item["embedding"][0])
    similarity_results.append((similarity, item["page_content"], item["chunks"]))

# 根据相似度排序
similarity_results.sort(reverse=True, key=lambda x: x[0])

# # 输出结果
# for score, text, chunk_id in similarity_results:
#     print(f"相似度分数: {score:.4f}, Chunk {chunk_id}: {text}")
```

```
similarity_results[0:5]
```

```
[(array([0.65413659]),
  '重点在于理解该模型的一些使
用原理，以及如何调整和构建tools与Schema表示。因此，整个开发过程中花费的大部分时间并非直接
与模型调用相关，而是在于深入理解和应用这些底层原理和技术细节。
换言之，在构建大模型应用的过程中，虽然大模型是核心，但实际针对大模型编写的代码量相对较
小。而在这些有限的代码中，提示工程又往往会占据主要的工作量。这进一步引出一个关键问题：除了
大模型本身，其他各环节如何高效串联，以及这些环节的代码开发和工作量如何分配和执行，将在很大
程度上影响最终应用的用户体验。
在这种情况下，解决这一窘境且广受认可和高度实用的解决方案，便是LangChain。
1. 什么是LangChain
LangChain现在归属于LangChain AI公司，LangChain作为其中的一个核心项目，开源发布在
Gitub上：https://github.com/langchain-ai/langchain
从LangChain的GitHub版本迭代历史上看，从2023年1月16日起已经经历了320个大小版本的迭
代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度',
  4),
 (array([0.62798402]),
  '象，设计出统一接
口以适配各种大模型。到目前为止，LangChain抽象出最重要的核心模块如下：
1. Model I/O ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；
2. Retrieval ：检索外部数据，然后在执行生成步骤时将其传递到 LLM，包括文档加载、切割、
Embedding等；
3. Chains ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多
功能的高级抽象；
4. Memory ： 记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；
5. Agents ： 目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；
6. Callbacks ：回调系统，允许连接到 LLM 应用程序的各个阶段。用于日志记录、监控、流传输和
其他任务；
从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文
记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的"Chain"高级抽象，定义了
不同形式下标准的链接方法，这就能够允许开发者',
  7),
 (array([0.59819352]),
  '采用Runnable协议。Runnable 协议是
编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可
以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了
Runnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。
LangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch
ain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable
而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解
的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。
如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI',
  46),
 (array([0.58787774]),
  'l I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如
OpenAI、ChatGLM、Qwen等，按照既定规范执行模型推理。其二通过输入和输出的模板化处理，使
其更贴合于应用开发的最佳实践。接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行
集成和操作的。
3 安装LangChain
LangChain的安装过程非常简单，可以通过常用的Python包管理工具，如pip或conda，直接进行
安装。稍复杂一点的还可以通过源码进行安装。但有一点大家一定要明确：LangChain的真正价值在于
它能够与多种模型提供商、数据存储解决方案等进行集成。默认情况下，使用上述两种安装方式中的任
意一种来进行LangChain安装后，安装的仅仅是LangChain的默认功能，并不包括这些集成所需的额外
依赖项。也就是说，如果我们想要使用特定的集成功能，还需要额外安装这些特定的依赖。以调用
OpenAI的API为例，我们首先需要通过运行命令pip install langchain-openai安装OpenAI的合作
伙伴包，安装此依赖包后，LangChain才能够与Ope',
  15),
 (array([0.58292596]),
  '形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整
的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。
也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和
使用方法，那么如何去搭这个“积木”，就需要我们对其每个核心模块都要有一个比较清楚的认知。所以
我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实
操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业
务情况，选择合适的构造模块和构造方法。
在整体上理解了LangChain之后，我们首先从Model I/O模块进行深入的探讨和实践。
2. LangChain核心模块：Model I/O
LangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的
Model I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就
是通过该模块，我们可以快速与某个大模型进行对话交互，整个',
  8)]
```

定义成一个函数：

```
def find_similar_chunks(query_text, results, top_n=5, print_content=True):
    """
    计算查询文本与文档片段的相似度，并返回/打印最相似的几个片段。

    参数:
    query_text (str): 查询文本。
    results (list of dict): 包含文档片段和相应嵌入向量的列表。
    top_n (int): 要返回的最相似片段的数量。
    print_content (bool): 是否打印每个片段的文本内容。

    返回:
    list: 包含最相似片段的相似度和文本内容的列表。
    """
    query_embedding = get_embeddings(query_text)[0]
    similarity_results = []
    
    # 计算相似度
    for item in results:
        similarity = cosine_similarity(query_embedding, item['embedding'][0])
        similarity_results.append((similarity, item['page_content'], item['chunks']))

    # 排序并选取最相似的top_n个结果
    similarity_results.sort(reverse=True, key=lambda x: x[0])
    top_results = similarity_results[:top_n]

    # 根据选择打印结果
    if print_content:
        for score, text, chunk_id in top_results:
            print(f"相似度分数: {score:.4f}, Chunk {chunk_id}: {text}")

    return top_results
```

```
# 调用函数，输入查询文本，获取相似度最高的3个结果，选择打印文本内容
similar_chunks = find_similar_chunks("请帮我解释一下Model I/O模块", results, top_n=3, print_content=True)
```

```
相似度分数: 0.5076, Chunk 14: te(
model="gpt-3.5-turbo",
messages=[
{"role": "system", "content": "你是一位乐于助人的AI智能小助手"},
{"role": "user", "content": "你好，请你介绍一下你自己。"}
]
)
LLMs是简化的大语言模型抽象，即基于给定的Prompt提供内容生成的功能。而Chat Models则专
注于聊天API的抽象，需要维护上下文的记忆（聊天记录），呈现出更接近对话或聊天形式的交互。
Parse
我们知道，大模型的输出是不稳定的，同样的输入Prompt往往会得到不同形式的输出。在自然语
言交互中，不同的语言表达方式通常不会造成理解上的障碍。但在应用开发中，大模型的输出可能是下
一步逻辑处理的关键输入。因此，在这种情况下，规范化输出是必须要做的任务，以确保应用能够顺利
进行后续的逻辑处理。
输出解析器 Output Parser就是一个帮助结构化语言模型响应的抽象，可以获取格式指令或者进行
更深层次的解析。这我们会在后面的实践中直观的体验到。
整体而言，在Model I/O模块的抽象中，其一能够让开发者
相似度分数: 0.4992, Chunk 8: 形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整
的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。
也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和
使用方法，那么如何去搭这个“积木”，就需要我们对其每个核心模块都要有一个比较清楚的认知。所以
我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实
操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业
务情况，选择合适的构造模块和构造方法。
在整体上理解了LangChain之后，我们首先从Model I/O模块进行深入的探讨和实践。
2. LangChain核心模块：Model I/O
LangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的
Model I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就
是通过该模块，我们可以快速与某个大模型进行对话交互，整个
相似度分数: 0.4564, Chunk 31: 在LangChain的虚拟
环境下启动Jupyter Lab，才能正常执行接下来的实践。
4.Models I/O模块实战
任何语言模型应用的核心都是大语言模型（LLMs），无论涉及到提示模板（Prompt Template）
还是输出解析（OutPut Parse），其核心还是在于大语言模型（LLMs）。因此，在讨论和实践Model
I/O模块时，首先应当关注如何集成这些大模型。只有成功集成后才能深入探讨和实践如何有效地设计
提示模板和解析模型输出。所以我们先要实践的是：如何借助LangChain框架使用不同的大模型。
4.1 LangChain接入大模型的方法
LangChain 提供了一套与任何大语言模型进行交互的标准构建模块。所以需要明确的一点是：虽然
LLMs 是 LangChain 的核心元素，但 LangChain 本身不提供 LLMs，它仅仅是为多种不同的 LLMs 进
行交互提供了一个统一的接口。简单理解：以OpenAI的GPT系列模型为例，如果我们想通过
LangChain 接入 OpenAI 的 GPT 模型，我们需要在LangChain框架下先定义相关的类和方法
```

## 8. RAG的Generation过程

```
user_input = "请帮我解释一下Model I/O模块"
```

```
from openai import OpenAI

#client = OpenAI(api_key='sk-xxxxxxx')

client = OpenAI()

completion = client.chat.completions.create(
  model="gpt-4",
  messages=[
    {"role": "user", "content": user_input}
  ]
)

print(completion.choices[0].message.content)
```

```
Model I/O是Apple开发的一个框架，主要用于管理3D模型和相关的纹理、灯光和动画资源。它可以在一系列不同的3D资产类型之间进行转换，包括但不限于.obj, .abc, 和 .usd(通用场景描述)格式。这个框架也涉及物理原理上的一些内容，比如材质和光照模型。

总的来说，Model I/O让开发者不必关注这些底层操作，而能够专注于他们3D内容的创建和编辑，从而提升整体的工作效率。它常被用在iOS, macOS, tvOS, 和 watchOS上的游戏开发或者AR应用开发之中。
```

```
user_input = "请帮我解释一下Model I/O模块"
context = similar_chunks
```

```
from openai import OpenAI

#client = OpenAI(api_key='sk-xxxxxxx')

client = OpenAI()

completion = client.chat.completions.create(
  model="gpt-4",
  messages=[
    {"role": "system", "content": "你是一位文档问答助手，你基于`文档内容`回答用户提出的问题。文档内容如下: %s" % context},
    {"role": "user", "content": user_input}
  ]
)

print(completion.choices[0].message.content)
```
