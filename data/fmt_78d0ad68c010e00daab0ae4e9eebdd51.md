
## 2.2 加载Embedding模型

LangChain 作为构建大语言模型应用的框架，提供了与不同Embedding的接入方式。这里通过使用 `OllamaEmbeddings` 接入一个开源的 Embedding 模型 `bge-m3`。

```
from langchain_ollama import OllamaEmbeddings

embeddings = OllamaEmbeddings(
    base_url = "http://192.168.110.131:11434",  # 注意：这里需要替换成自己本地启动的endpoint
    model="bge-m3",
)
```

```
text = "大模型  AI Agent 开发实战  \nCh.1 大模型应用发展及 Agent 前沿技术趋势"

single_vector = embeddings.embed_query(text)
print(str(single_vector)[:100])  # 显示前100个 词向量的表示， Bge-m3 是 1024 维度
```

```
[-0.030400125, -0.006851768, -0.041059047, -0.0048820246, -0.019558325, -0.0026998182, -0.013847404,
```

```
print(len(single_vector))
```

```
1024
```

## 2.3 存入向量数据库

这里我们使用免费的在线 `milvus` 实例，地址如下：`https://cloud.zilliz.com/login?redirect=/orgs` ， 先注册登录：

![](img/b31be3db83c2da2f9d521801598c9891.png)

然后创建索引：

![](img/075133128edfa808b00ecb71c8cd1e1a.png)

选择免费实例：

![](img/5e67771277ef866478f1f326a3dc8348.png)

注意：这里需要保存好用户名和密码，用于接下来的远程连接。

![](img/e859fce31b15e8c59ea45b89916289b8.png)

等待创建完成后，注意关注如下信息：

![](img/34cde7857cd853d970ca18e2e2f7f1ee.png)

通过如下代码构建向量索引，并存储到云端的 `Milvus` 向量数据库中。

```
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import WebBaseLoader
from langchain_milvus import Milvus


# 添加到向量数据中
vectorstore = Milvus.from_documents(
    documents=splits,
    collection_name="fufan_rag_milvus",
    embedding=embeddings,
    connection_args={
        "uri": "https://in03-b9ed7b460b5d89f.serverless.gcp-us-west1.cloud.zilliz.com",
        "user": "db_b9ed7b460b5d89f",
        "password": "Ua3;qLNjSKWmGl6F",
    }
)
```

同时可以登录网页端进行确认：

![](img/206524c5daa281917ff4ccc042d0cc81.png)

## 2.4 接入生成模型

LangChain支持接入在线模型或者本地开源模型。这里我们使用 `Ollama` 接入通义千问最新推出的推理大模型 `QWQ:32B`，当然大家也可以根据自己的实际使用情况进行灵活选择。

```
# 如果用开源模型，可以用Ollama 接入
from langchain_ollama import ChatOllama

qwq_llm = ChatOllama(
    base_url = "http://192.168.110.131:11434",  # 注意：这里需要替换成自己本地启动的endpoint
    model="qwq:latest",
)
```

```
print(qwq_llm.invoke("你好，请你详细的介绍一下你自己。").content)
```

```
作为阿里云开发的一款超大规模语言模型，我叫通义千问。我可以回答用户的问题、提供信息、创作文字内容等。

我是基于大量的文本数据训练出来的，具有丰富的知识和语言表达能力。我可以理解和生成自然语言，与人类进行交互。我的目标是帮助用户获得准确、有用的信息，解决他们的问题和困惑。

除了回答问题，我还可以进行创意写作，如写故事、诗歌、剧本等。我能够根据用户的指令，发挥创造力，生成独特的内容。

此外，我还具备一定的逻辑推理和 problem-solving 能力。我可以分析复杂的问题，提供合理的解决方案和建议。

总之，我是一个多功能的语言模型，致力于为用户提供优质的语言服务和帮助。
```

![](img/e182ca83e1e9675add6feca77ed5a9cb.png)

## 2.5 构建问答流程

在完成 RAG 的 indexing 过程后，接下来我们就可以开始构建问答流程了。其中，`PromptTemplate` 是 `LangChain` 用来定义提示的类。我们定义了一个用于问答任务的提示模板。模板的内容规定了如何使用检索到的上下文回答用户的问题。`{question}` 和 `{context}` 是输入变量，分别代表用户提出的问题和检索到的相关文档内容。`Answer` 则表示后面的部分是生成的答案，规定了返回简洁的三句话以内的回答。

```
from langchain.prompts import PromptTemplate
from langchain import hub
from langchain_core.output_parsers import StrOutputParser

# 提示
prompt = PromptTemplate(
    template="""You are an assistant for question-answering tasks. 
    Use the following pieces of retrieved context to answer the question. If you don't know the answer, just say that you don't know. 
    Use three sentences maximum and keep the answer concise:
    Question: {question} 
    Context: {context} 
    Answer: 
    """,
    input_variables=["question", "document"],
)
```

然后通过管道运算符 `|` 将 `PromptTemplate`、`LLM` 模型 (`qwq_llm`) 和输出解析器 (`StrOutputParser`) 结合起来，构建了一个问答链。该链接收问题和文档内容，并生成简洁的回答。

```
# 构建传统的RAG Chain
rag_chain = prompt | qwq_llm | StrOutputParser()
```

接下来进行测试，提问了一个问题 `"请问什么是 AI Agent？"`，该问题将作为输入传递给链，作为问答任务的起点。`vectorstore.as_retriever()` 方法会将当前的向量数据库（`Milvus`）转换为一个检索器，可以在给定的向量空间中执行检索操作。`search_kwargs={"k": 3}` 设置检索器返回前 3 个最相关的文档。

```
# 运行
question = "请问什么是 AI Agent？"

# 构建检索器
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

# 执行检索
docs = retriever.invoke("question")

docs
```

```
[Document(metadata={'pk': 454377259150482140}, page_content='人类的优势是能够吸收相对大量的信息，过滤掉不重要的细节，并根据关键信息做出决策。比如在\n处理一件事情之前，我们通常会先将大问题分解为一个个小的假设，然后尝试通过观察逐步支持或反驳\n这些假设。 从这个现实的观点出发，启发  AI Agent 早期范式的一篇论文  👉   REACT: SYNERGIZING  \nREASONING AND ACTING IN LANGUAGE MODELS   中使用 “ 思维链提示 ” 来模仿这个概念，它将多步\n骤问题分解为中间步骤 :\n发起一项行动，让大模型观察所选环境的反馈\n在流程中收集所有信息并使用它来决定下一步采取什么合适的行动\n迭代地执行此操作来解决更大、更复杂的任务，使用一种称为 “ 推理跟踪 ” 的方法，该方法涉及跟踪\n整个过程所经历的步骤或阶段以得出结论或解决方案\n  如下图所示，整个过程是一个动态循环。代理不断从环境中学习，通过其行动影响环境，然后根据\n环境的反馈继续调整其行动和策略。这种模式特别适用于那些需要理解和生成自然语言的应用场景，如\n聊天机器人、自动翻译系统或其他形式的自动化客户支持。\n  一个这是更加感官性的认知如下图所示：\n  如上图所示，展示了一个人工智能代理的基本架构，包括它与环境的互动、感知输入、大脑处理及\n其决策过程。具体来说：\n1. 环境（ Environment ） ： AI 代理接收来自其周围环境的信息。环境可以是一个网站、数据库或任\n何其他类型的系统。2. 感知（ Perception ） ： 即输入。 AI 代理通过多种方式感知环境，如视觉（图像）、听觉（声\n音）、文本（文字信息）和其他传感器输入（如位置、温度等）。这些输入帮助代理理解当前的环\n境状态。\n3. 大脑（ Brain ） ：\n存储（ Storage ） ：\n记忆（ Memory ） ：存储先前的经验和数据，类似于人类的记忆。\n知识（ Knowledge ） ：包括事实、信息和代理用于决策的程序。\n决策制定（ Decision Making ） ：\n总结（ Summary ） 、 回忆（ Recall ） 、 学习（ Learn ） 、 检索（ Retrieve ） ：这些功能\n帮助 AI 在需要时回顾和利用存储的知识。'),
 Document(metadata={'pk': 454377259150482131}, page_content='虽然大语言模型非常强大，但要有效使用它们并非易事。在开发者急于探索如何像处理传统算法模\n型那样通过微调快速迭代更新大模型的内部知识时，一篇极具启发性的论文  👉  GPT-3 Language  \nModels are Few-Shot Learners  提出了  In-Context Learning 的概念。该方法通过向模型提供少量标\n注的 “ 输入 - 输出对 ” 示例，在不需要大规模微调的情况下即可显著改善大模型的输出质量。这一发现开启\n了使用大模型的新方式  👇\n  当提出问题后，大模型能够以自然语言返回响应，这是生成式人工智能的一大优点。有些任务确实\n可以通过这种提示工程的方式引导大模型在对话过程中生成正确的回复，但这个过程最大的问题就是需\n要人工介入，正如上面的例子中涉及到的北京的天气信息：\n{\n \xa0"location": {\n \xa0 \xa0"city": "Beijing",\n \xa0 \xa0"country": "CN",\n \xa0 \xa0"timezone": "Asia/Shanghai",\n \xa0 \xa0"coordinates": {\n \xa0 \xa0 \xa0"latitude": 39.9042,\n \xa0 \xa0 \xa0"longitude": 116.4074\n \xa0  }\n  },\n \xa0"current": {\n \xa0 \xa0"temperature": {\n \xa0 \xa0 \xa0"value": 34,\n \xa0 \xa0 \xa0"unit": "C"  对于试图将大模型的响应与其他应用程序连接起来的开发人员来说，这就是一场噩梦。开发人员通\n常使用正则表达式  (Regex) 或提示工程将输出转换为所需的格式，然后才能将其传递到另一个应用程\n序，也就是说，这个过程中如果不需要人工介入，还想让它自动拿到这些信息，怎么做？\n  搞过开发的小伙伴对这种 JSON 数据应该非常熟悉，我们可以调用某个天气平台的 API ，比如  👉  \nOpenWeather ，输入一个城市的关键词，  就能得到该城市当前的天气信息数据，也就是如上所示的\nJSON 形式，那么这样的信息，如何让大模型自动解析，就进入到了我们探索大模型应用的第二个阶段  - \n函数调用 。\nStage 2 ：函数调用\n  2023 年  7 月， OpenAI 为其  GPT 模型引入了函数调用功能。大模型发展到现在，所有热门的大模型'),
 Document(metadata={'pk': 454377259150482136}, page_content='自然语言处理、计算机视觉等。自动驾驶汽车中的人工智能系统、推荐引擎、 Siri 、 Google Assistant 和 \nAlexa 都是狭义人工智能的形式。狭义人工智能取得了重大突破很大程度上归功于机器学习和深度学习的\n进步，由自然语言处理  (NLP) 提供支持，使其能够理解和处理人类语言。例如，人工智能系统现在在医\n学中用于高精度诊断癌症和其他疾病。\n  而我们现在期望做到的通用人工智能（ AGI ），是指  具有相当于或超越人类能力的人工智能。它涵盖\n跨不同领域学习、理解和应用知识的能力。 AGI 也被称为强人工智能。\n  所以能够感受到，我们现阶段做到的人工智能和通用人工智能之间存在根本区别。像 ChatGPT 这样\n的应用虽然掀起了新一轮的热潮，但本质上是它们也只是在做 “ 预测 ” ，通过大量数据的训练以达到生成准\n确响应的目的，但缺乏目标、身份或主动决策的概念。所以它们也只是复杂的文本生成器，没有目的或\n方向感。我们还没有完全做到 AGI ，因为现有的每个人工智能模型都只是模仿人类智能的某个方面。例\n如，大语言模型非常擅长理解和撰写文本，它们的能力常常超越人类在这些领域的表现。然而，当涉及\n到简单的算术任务时， LLMs 就会经常出现问题。\n  那么 让大模型能够自己解决更复杂的问题，现阶段提出的解决方案就是： AI Agent 。  早在  2016 年，强化学习代理就被炒得沸沸扬扬，人们试图创建不同类型的强化学习代理来玩游戏，\n从而去评判这类代理的智能性。当时还没有人工智能代理的概念。  OpenAI 进行了一系列研究，包括探\n索强化学习  (RL) 在不同领域的应用，比如游戏和网页导航。其中，有一个名为 “World of Bits” 的项目，\n就是在训练人工智能代理在复杂的网络环境中执行任务，例如订购商品或服务。这个项目是对强化学习\n技术在现实世界任务中应用潜力的探索之一。论文： 👉  World of Bits: An Open-Domain Platform for  \nWeb-Based Agents\n  如上图所示，这张论文中展示的多个网页界面中，每个界面都关联着一个特定的查询问题，用于展\n示不同类型的在线任务或服务。具体包括：\n1. 机票预订 ：从旧金山到纽约的航班预订。\n2. 餐馆搜索 ：寻找旧金山最好的韩国餐馆。')]
```

运行 `RAG_chain`,生成最终的回复。

```
generation = rag_chain.invoke({"context": docs, "question": question})
print(generation)
```
