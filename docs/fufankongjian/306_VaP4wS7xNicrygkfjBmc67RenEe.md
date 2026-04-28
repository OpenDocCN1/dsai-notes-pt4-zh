# 从0到1构建RAG-checkpoint

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/DejedJ2F3oO1bLxkPNgcmZRLnzY](https://kq4b3vgg5b.feishu.cn/docx/DejedJ2F3oO1bLxkPNgcmZRLnzY)

## 从0到1构建RAG

我们把在一个持久化的附加知识库中可以快速检索出相关的有效信息，并提供给Agents作为处理复杂任务的某一阶段的上下文这一过程称为RAG，所以能够理解，RAG的本质就是一种用附加数据来增强LLM知识的技术。从宏观的角度来看，LLMs 可以推理广泛的主题，但它们的知识仅限于它们接受训练的特定时间点的公共数据。如果我们想要构建能够推理私有数据及使用模型所需的特定信息来增强模型的知识，引入适当的信息并将其插入模型提示中的过程，称为检索增强生成 (RAG)。

![](img/eb785276c1ff498ea9b11b027a6ff974.png)

其核心构建流程如下：

![](img/687309269c39da896d78d6a90e474e96.png)

接下来的内容我们逐步实现这个过程。

---

🍻现开设了大**模型学习交流群，**扫描下👇码，来遇见更多志同道合的小伙伴~

![](img/6b463cad7d24287bf72bf1a153ac89a6.png)

海量硬核独家技术**干货内容**+无门槛**技术交流****+不定期开设****硬核干货&前沿技术公开课****，****扫码**👆即刻入群！

---

# 1. 安装最新的OpenAI依赖包

```
! pip install --upgrade openai
```

```
Requirement already satisfied: openai in d:\software\anaconda3\lib\site-packages (1.30.2)
Requirement already satisfied: anyio&lt;5,&gt;=3.5.0 in d:\software\anaconda3\lib\site-packages (from openai) (3.5.0)
Requirement already satisfied: distro&lt;2,&gt;=1.7.0 in d:\software\anaconda3\lib\site-packages (from openai) (1.8.0)
Requirement already satisfied: httpx&lt;1,&gt;=0.23.0 in d:\software\anaconda3\lib\site-packages (from openai) (0.25.1)
Requirement already satisfied: pydantic&lt;3,&gt;=1.9.0 in d:\software\anaconda3\lib\site-packages (from openai) (2.6.0)
Requirement already satisfied: sniffio in d:\software\anaconda3\lib\site-packages (from openai) (1.2.0)
Requirement already satisfied: tqdm&gt;4 in d:\software\anaconda3\lib\site-packages (from openai) (4.66.4)
Requirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in d:\software\anaconda3\lib\site-packages (from openai) (4.10.0)
Requirement already satisfied: idna&gt;=2.8 in d:\software\anaconda3\lib\site-packages (from anyio&lt;5,&gt;=3.5.0-&gt;openai) (3.4)
Requirement already satisfied: certifi in d:\software\anaconda3\lib\site-packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai) (2024.2.2)
Requirement already satisfied: httpcore in d:\software\anaconda3\lib\site-packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai) (1.0.1)
Requirement already satisfied: annotated-types&gt;=0.4.0 in d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1.9.0-&gt;openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1.9.0-&gt;openai) (2.16.1)
Requirement already satisfied: colorama in d:\software\anaconda3\lib\site-packages (from tqdm&gt;4-&gt;openai) (0.4.6)
Requirement already satisfied: h11&lt;0.15,&gt;=0.13 in d:\software\anaconda3\lib\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-&gt;openai) (0.14.0)
```

```
import openai

print(openai.__version__)
```

```
1.30.3
```

# 2. 设置全局代理

对于国内用户，是无法直接访问OpenAI的，需要让Jupyter在代理环境下启动，即需要令Jupyter可以通过代理来访问网络。这就需要大家找到自己所使用魔法的代理端口是多少。不同的魔法软件默认配置是不一样的，所以需要大家自行查看。以我个人使用的魔法软件为示例：

```
import os
os.environ[&#x27;http_proxy&#x27;] = &quot;http://127.0.0.1:15732&quot;
os.environ[&#x27;https_proxy&#x27;] =&quot;http://127.0.0.1:15732&quot;
```

+   测试能否调用OpenAI API

```
from openai import OpenAI

#client = OpenAI(api_key=&#x27;sk-xxxxxxx&#x27;)

client = OpenAI()

completion = client.chat.completions.create(
  model=&quot;gpt-4&quot;,
  messages=[
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}
  ]
)

print(completion.choices[0].message.content)
```

```
你好，我是OpenAI的人工智能助手，也叫做GPT-3。我是一个强大的预测模型，可理解和生成人类的文本。我可以用来回答问题、编写文章，还可以帮助设计者、开发者、创意人员等完成各种任务。但是，我并不具有情感、意识或理解能力，我只是通过我的编程和算法来处理输入和生成输出。
```

如果能正常得到输出，说明环境正常，可以继续执行接下来的操作。

# 3. Indexing过程

## 3.1 文档加载

这里我们以PDF格式的文档作为示例演示RAG从0到1的构建流程。对于PDF文件，需要安装一个pdfplumber第三方库。

pdfplumber 是一个用于提取 PDF 文件中信息的 Python 库。它非常适合那些需要从 PDF 中获取文本、表格和元数据的项目。其核心特点和常用方法可以总结如下：

+   **提取文本**：pdfplumber 可以从 PDF 文件中提取页面上的文本。

+   **读取表格**：这个库可以识别 PDF 中的表格布局，并能够将表格内容转换为 Python 的数据结构，如 Pandas 的 DataFrame。

+   **处理页面**：用户可以对 PDF 的每一页单独操作，获取具体页面的文本、表格等信息。

pdfplumber 的功能非常适合于那些需要从 PDF 文档中自动提取和处理数据的应用场景。它能够处理多种布局和复杂的文档结构，尽管有时候需要对提取逻辑进行一些调整和优化，以适应特定的文件格式和内容布局。

+   Step 1. 安装pdfplumber

```
# ! pdfplumber==0.10.3
```

```
import pdfplumber

print(pdfplumber.__version__)
```

```
0.11.0
```

```
pdf_path = &#x27;../04_data/LangChain整体项目介绍与核心模块Model IO详解.pdf&#x27;
pdf_name = &#x27;LangChain整体项目介绍与核心模块Model IO详解.pdf&#x27;
```

```
with pdfplumber.open(pdf_path) as pdf:
    # 依次遍历PDF文档每一页
    for i, page in enumerate(pdf.pages):
        # 提取每一页的文本信息
        page_text = page.extract_text()
        if page_text is not None:
            print(f&quot;-------- Page {i+1} --------&quot;)
            print(page_text)
```

```
-------- Page 1 --------
开源大模型应用开发实战
Ch 18 LangChain整体项目介绍与核心模块Model I/O详
解
注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情
况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，
将迅速更新教材内容以确保信息的准确性和实用性。
本期课件更新时间：2024年3月7日
在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地
私有化部署开源大模型。在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家
关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部
署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部
署。
但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也
是最简单的一步。熟练掌握基于大模型的上层应用开发还有非常长的路需要走。目前，大模型技术开发
总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API的应用开发，而另一类，就是先私有化
部署开源大模型，再借助开发框架构建大模型应用。当然了，目前主流的大模型应用开发框架也支持在
线模型API的调用，这正是我们之前一直强调学习掌握开源或在线API调用技巧的原因，因为这些方法和
技巧直接对应于应用开发的最佳实践。
而不论采用哪种开发形式，其基座和所依赖的，核心还是大模型本身。对于在线大模型，以
OpenAI为例，开发者需要配置科学上网的环境，在其官网创建 API Key，最后遵循API的使用说明去进
行应用配置。这一过程中涉及到的网络环境适配，数据隐私安全，成本管理及如何去定制化自己需求的
开发流程，往往都需要遵循官方提供的功能来实现。也正是因为这些复杂的因素使得尽管目前公认最强
的大语言模型GPT 4开放给开发者使用，还是会有非常多的公司和个人去拥抱开源，选择性能可能略逊
一筹的开源大模型，因为开源方案在某些方面提供了更多的灵活性和自主控制能力。
大家也能够发现，在我们的课程中每当涉及到大模型的私有化部署，我们都会向大家展示如何通过
命令行、网页端访问和API调用等三种方式与部署的大模型进行交互。此外，还通过text-generate-
webui这样的开源项目，这一切应用是帮助大家能够以最简单、最直接的方式与开源大模型进行对话，
了解大模型的运行模型。但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶
段肯定无法满足实际的需求。虽然目前的大模型基本都可以按照OpenAI的API规范形式进行调用，但如
果进行实操我们会发现，在《大模型技术实战》的课程中，借助OpenAI的GPT系列模型开发的项目，
如智能邮件处理、全自动AI开发流程，以及MateGen Agents等，尽管在开源模型上能够被复现，但这
背后的成功关键并非仅仅在于模型调用。例如，在《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进
行外部工具API调用开发实践》中，我们通过Qwen模型复现了《Ch.10 借助Function calling调用外部
工具API方法》中的天气信息调用过程。这一复现过程揭示了，对在线模型来说，我们需要学习的是其
API使用方法。而在开源大模型的技术领域下，要想实现某些特定功能，重点在于理解该模型的一些使
用原理，以及如何调整和构建tools与Schema表示。因此，整个开发过程中花费的大部分时间并非直接
与模型调用相关，而是在于深入理解和应用这些底层原理和技术细节。
换言之，在构建大模型应用的过程中，虽然大模型是核心，但实际针对大模型编写的代码量相对较
小。而在这些有限的代码中，提示工程又往往会占据主要的工作量。这进一步引出一个关键问题：除了
大模型本身，其他各环节如何高效串联，以及这些环节的代码开发和工作量如何分配和执行，将在很大
程度上影响最终应用的用户体验。
在这种情况下，解决这一窘境且广受认可和高度实用的解决方案，便是LangChain。
-------- Page 2 --------
1. 什么是LangChain
LangChain现在归属于LangChain AI公司，LangChain作为其中的一个核心项目，开源发布在
Gitub上：https://github.com/langchain-ai/langchain
从LangChain的GitHub版本迭代历史上看，从2023年1月16日起已经经历了320个大小版本的迭
代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高
的。LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架。它的做法是：通过提
供标准化且丰富的模块抽象，构建大语言模型的输入输入规范，利用其核心概念chains，灵活地连接
整个应用开发流程。而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出
来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发
领域内被普遍高度认可的通用框架。
为什么需要这样做？
首先，我们需考虑当前大模型的发展态势。尽管OpenAI的GPT系列模型作为大模型领域的领军人
物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大
模型间的使用方式完全相同。例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不
适用。因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是
十分低下的。
其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据
连接方式以及输出结果的不稳定性等问题。在应用开发中，如何找到这些问题的有效解决策略？
上述提到的每个限制都紧密关联于大模型本身的特性。尽管理论上可以通过重新训练、微调来增强
模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资
源、时间和财力，选择这条路径一定会导致方向越来越偏离目标。我们之前讨论的OpenAI的Function
Calling和通过ReAct流程接入第三方API能够提供一些解决方案，但这每一步都需大量的研发投入，而且
最终实现后的应用效果，也取决于研发人员的个人技术能力。在这种背景下，既然大家都有不同的想法
和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的
快速搭建，这就是LangChain一直想要做到，且正在做的事情。
LangChain的做法
-------- Page 3 --------
从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对
大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接
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
记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的&quot;Chain&quot;高级抽象，定义了
不同形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整
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
是通过该模块，我们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过
程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API
调用规范：
response = openai.ChatCompletion.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是机器学习？&quot;}
]
)
-------- Page 4 --------
而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话
交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象
出一个chain，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核
心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和
Output Parser（对应下图中的Parse部分）。
Format：即指代Prompts Template，通过模板化来管理大模型的输入；
Predict：即指代Models，使用通用接口调用不同的大语言模型；
Parse：即指代Output部分，用来从模型的推理中提取信息，并按照预先设定好的模版来规范化
输出。
整个Model I/O工作流如下图所示：
Format
对于Prompt Template第一部分，传统上我们创建提示词是通过手工编写来实现的，在这个过程中
会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能。然而，
在应用开发中，一个关键的考量是提示词不能是一成不变的。其原因在于，应用开发需要适应多变的用
户需求和场景。固定的提示词限制了模型的灵活性和适用范围。例如，如果我们正在开发一个天气查询
应用，用户可能会以多种方式提出查询，如“今天的天气怎么样？”或“明天纽约的温度是多少度？”。如
果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求。
而Prompt Template，就像我们在介绍《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进行外
部工具API调用开发实践》中不断优化出来的ReAct模版，将API的使用、问题解答过程等复杂逻辑封装
成了一套结构化的格式。我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引
导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：
# 将一个插件的关键信息拼接成一段文本的模版。
TOOL_DESC = &quot;&quot;&quot;{name_for_model}: Call this tool to interact with the
{name_for_human} API. What is the {name_for_human} API useful for?
{description_for_model} Parameters: {parameters}&quot;&quot;&quot;
# ReAct prompting 的 instruction 模版，将包含插件的详细信息。
PROMPT_REACT = &quot;&quot;&quot;Answer the following questions as best you can. You have
access to the following APIs:
{tool_descs}
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
-------- Page 5 --------
... (this Thought/Action/Action Input/Observation can be repeated zero or more
times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question
Begin!
Question: {query}&quot;&quot;&quot;
因此，引入Prompt Template可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输
入动态调整提示词，从而更好地响应用户的具体需求。LangChain通过这种方式来提高应用的通用性和
用户体验。
Predict
在Predict部分，实质上是处理模型从接收输入到执行推理的整个过程。考虑到存在两种主要类型的
大模型——Base类模型和Chat类模型，LangChain在其Model I/O模块中对这两种模型都进行了抽象，
分别归类为LLMs（Large Language Models）和Chat Models。我们还是以OpenAI 的 Completion 和
Chatcompletions方法为例：
# Base类模型
client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=&quot;Say this is a test&quot;,
)
# 聊天模型
client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
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
整体而言，在Model I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如
OpenAI、ChatGLM、Qwen等，按照既定规范执行模型推理。其二通过输入和输出的模板化处理，使
其更贴合于应用开发的最佳实践。接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行
集成和操作的。
3 安装LangChain
-------- Page 6 --------
LangChain的安装过程非常简单，可以通过常用的Python包管理工具，如pip或conda，直接进行
安装。稍复杂一点的还可以通过源码进行安装。但有一点大家一定要明确：LangChain的真正价值在于
它能够与多种模型提供商、数据存储解决方案等进行集成。默认情况下，使用上述两种安装方式中的任
意一种来进行LangChain安装后，安装的仅仅是LangChain的默认功能，并不包括这些集成所需的额外
依赖项。也就是说，如果我们想要使用特定的集成功能，还需要额外安装这些特定的依赖。以调用
OpenAI的API为例，我们首先需要通过运行命令pip install langchain-openai安装OpenAI的合作
伙伴包，安装此依赖包后，LangChain才能够与OpenAI的API进行交互。后续我们在使用相关功能的时
候，会提供额外的说明。
LangChain安装官方说明文档：https://python.langchain.com/docs/get_started/installation
3.1 使用包版本管理工具安装
LangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建
过程。这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成
LangChain的安装。具体操作如下：
! pip install langchain
Requirement already satisfied: langchain in d:\software\anaconda3\lib\site-
packages (0.1.10)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain) (6.0)
Requirement already satisfied: SQLAlchemy&lt;3,&gt;=1.4 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.4.39)
Requirement already satisfied: aiohttp&lt;4.0.0,&gt;=3.8.3 in
d:\software\anaconda3\lib\site-packages (from langchain) (3.8.5)
Requirement already satisfied: dataclasses-json&lt;0.7,&gt;=0.5.7 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.6.4)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.33)
Requirement already satisfied: langchain-community&lt;0.1,&gt;=0.0.25 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.25)
Requirement already satisfied: langchain-core&lt;0.2,&gt;=0.1.28 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.28)
Requirement already satisfied: langchain-text-splitters&lt;0.1,&gt;=0.0.1 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.1)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.14)
Requirement already satisfied: numpy&lt;2,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (1.24.3)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (8.2.2)
Requirement already satisfied: attrs&gt;=17.3.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (22.1.0)
Requirement already satisfied: charset-normalizer&lt;4.0,&gt;=2.0 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(2.0.4)
Requirement already satisfied: multidict&lt;7.0,&gt;=4.5 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(6.0.2)
-------- Page 7 --------
Requirement already satisfied: async-timeout&lt;5.0,&gt;=4.0.0a3 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(4.0.2)
Requirement already satisfied: yarl&lt;2.0,&gt;=1.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (1.8.1)
Requirement already satisfied: frozenlist&gt;=1.1.1 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.3.3)
Requirement already satisfied: aiosignal&gt;=1.1.2 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.2.0)
Requirement already satisfied: marshmallow&lt;4.0.0,&gt;=3.18.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (3.21.0)
Requirement already satisfied: typing-inspect&lt;1,&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (0.9.0)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain)
(2.1)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (3.5.0)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2,&gt;=0.1.28-
&gt;langchain) (23.2)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain)
(2.16.1)
Requirement already satisfied: typing-extensions&gt;=4.6.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (4.7.1)
Requirement already satisfied: idna&lt;4,&gt;=2.5 in d:\software\anaconda3\lib\site-
packages (from requests&lt;3,&gt;=2-&gt;langchain) (3.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(1.26.16)
Requirement already satisfied: certifi&gt;=2017.4.17 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(2023.11.17)
Requirement already satisfied: greenlet!=0.4.17 in
d:\software\anaconda3\lib\site-packages (from SQLAlchemy&lt;3,&gt;=1.4-&gt;langchain)
(2.0.1)
Requirement already satisfied: sniffio&gt;=1.1 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (1.2.0)
Requirement already satisfied: mypy-extensions&gt;=0.3.0 in
d:\software\anaconda3\lib\site-packages (from typing-inspect&lt;1,&gt;=0.4.0-
&gt;dataclasses-json&lt;0.7,&gt;=0.5.7-&gt;langchain) (1.0.0)
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：
-------- Page 8 --------
import langchain
print(langchain.__version__)
0.1.10
如果能正常输出LangChain的版本，说明在当前环境下的安装成功。
3.2 源码安装
除了通过pip安装外，还有一种通过源码安装的方法。这需要使用git拉取远程仓库，然后进入项目
文件夹并执行pip install -e .命令。这种方法不仅会安装必要的依赖，同时也将程序的源代码保存
在本地。通常来说，除非大家打算作为LangChain的协作开发者参与到项目的功能更新中，否则没必要
采用这种安装方式，直接使用pip安装更为简便。
对于课程学习而言，我们推荐采用源码安装方式，这将非常有助于在后续的LangChain功能探索
中，通过源码分析深入理解框架的构建原理和详细机制。
接下来我们以Ubuntu操作系统为例，介绍源码安装LangChain的详细步骤：
Step 1. 安装Anaconda
如果本地服务器是Ubuntu系统，请参考：《Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B
模型》中的2.3 安装Anaconda环境小节；而如果是Windows操作系统，请参考：《Ch.6 在Windows
系统下部署运行ChatGLM3-6B模型》中的3.2.1 方式一：使用Anaconda创建项目依赖环境（推荐）小节
中，执行完Step 6. 验证conda步骤，回到本文继续执行后续的操作：
注：如果部署环境是租赁的云服务器，Anaconda环境已经预设好，并不需要我们再次手动配置。
按照对应的教程内容配置好Anaconda后，可以通过如下命令验证当前环境的Conda：
conda --version
如果可以正常输出Conda的版本，则说明Anaconda安装成功。
Step 2. 使用Conda创建LangChain的Python虚拟环境
安装好Anaconda后，我们需要借助Conda包版本工具，为LangChain项目创建一个新的Python虚
拟运行环境，执行代码如下：
conda create --name langchain python==3.11
-------- Page 9 --------
如上所示，新创建了一个名为langchain的Python虚拟环境，其Python版本为3.11。创建完成后，
通过如下命令进入该虚拟环境，执行后续的操作：
conda activate langchain
Step 3. 下载LangChain的项目文件
进入LangChain的官方Github，地址：https://github.com/langchain-ai/langchain ， 在 GitHub
上将项目文件下载到有两种方式：克隆 (Clone) 和 下载 ZIP 压缩包。推荐使用克隆 (Clone)的方式。我
们首先在GitHub上找到其仓库的URL。
在执行命令之前，需要先安装git软件包，执行命令如下：
sudo apt install git
-------- Page 10 --------
执行克隆命令，将LangChain Github上的项目文件下载至本地的当前路径下，如下：
git clone https://github.com/langchain-ai/langchain.git
Step 4. 升级pip版本
建议在执行项目的依赖安装之前升级 pip 的版本，如果使用的是旧版本的 pip，可能无法安装一些
最新的包，或者可能无法正确解析依赖关系。升级 pip 很简单，只需要运行命令如下命令：
python -m pip install --upgrade pip
Step 5. 源码安装项目依赖
不同于我们之前一直使用的pip install -r requirements.txt方式，这种方法用于批量安装多
个依赖包，是在部署项目或确保开发环境与其他开发者/环境一致时的常用方式。而对于LangChain，我
们需要使用pip install -e的方式，以可编辑模式安装包。这种方式主要用于开发过程中。当以可编
辑模式安装一个包时，依赖包会被直接从源代码所在位置安装，而不是复制到Python的site-packages
目录，是开发模式下用于安装并实时反映对本地包更改的方法。需要执行的步骤如下：
cd langchain/libs/langchain/
pip install -e .
-------- Page 11 --------
如在安装过程未发生任何报错，则说明安装成功。在安装完依赖后，我们就正式进入LangChain的
Model I/O模块的实践。
注：如果采用的是源码安装，需要参考《Ch.11 大模型开发工具Transformers库的使用及配置大
模型开发环境》中远程服务器部署大模型，本地Jupyter NoteBook加载小节，在LangChain的虚拟
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
LangChain 接入 OpenAI 的 GPT 模型，我们需要在LangChain框架下先定义相关的类和方法来规定如
何与模型进行交互，包括数据的输入和输出格式以及如何连接到模型本身。然后按照 OpenAI GPT 模型
的接口规范来集成这些功能。通过这种方式，LangChain 充当一个桥梁，使我们能够按照统一的标准来
接入和使用多种不同的大语言模型。
在LangChain的Model I/O模块中，集成了两类模型，分别是LLMs和聊天模型，这两种模型在输
入和输出的数据类型上有所区别。
接下来我们就以OpenAI的API为示例，分别实践一下在LangChain框架中如何使用这两种不同类型
的大语言模型。首先来看LLMs类模型。
4.1.1 LLMs类模型接入方法
LLMs指的是纯文本补全模型，其封装的 API 输入接收的是字符串，输出也是字符串。 OpenAI 的
GPT-3 是作为 LLM 实现的。最简单的理解：LLMs指代的就是Base类模型，这类模型仅仅可以理解并
生成自然语言或代码，并未按照特定指令进行训练。截止到2024年3月07日，OpenAI可支持调用的
Base类模型仅有2个，分别是babbage-002和davinci-002，除此之外，还可以使用gpt-3.5-turbo-
instruct调用OpenAI 的 Completion方法。
首先需要安装OpenAI的集成依赖包langchain-openai，执行如下命令：
! pip install langchain-openai
Requirement already satisfied: langchain-openai in
d:\software\anaconda3\lib\site-packages (0.0.8)
-------- Page 12 --------
Requirement already satisfied: langchain-core&lt;0.2.0,&gt;=0.1.27 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.1.28)
Requirement already satisfied: openai&lt;2.0.0,&gt;=1.10.0 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (1.13.3)
Requirement already satisfied: tiktoken&lt;1,&gt;=0.5.2 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.6.0)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (6.0)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.5.0)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (1.33)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (0.1.14)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (23.2)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (8.2.2)
Requirement already satisfied: distro&lt;2,&gt;=1.7.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (1.8.0)
Requirement already satisfied: httpx&lt;1,&gt;=0.23.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (0.25.1)
Requirement already satisfied: sniffio in d:\software\anaconda3\lib\site-
packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (1.2.0)
Requirement already satisfied: tqdm&gt;4 in d:\software\anaconda3\lib\site-packages
(from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (4.65.0)
Requirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (4.7.1)
Requirement already satisfied: regex&gt;=2022.1.18 in
d:\software\anaconda3\lib\site-packages (from tiktoken&lt;1,&gt;=0.5.2-&gt;langchain-
openai) (2022.7.9)
Requirement already satisfied: idna&gt;=2.8 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai)
(3.4)
Requirement already satisfied: certifi in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(2023.11.17)
Requirement already satisfied: httpcore in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(1.0.1)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.1)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.9.15)
-------- Page 13 --------
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.16.1)
Requirement already satisfied: charset-normalizer&lt;4,&gt;=2 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.0.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (1.26.16)
Requirement already satisfied: colorama in d:\software\anaconda3\lib\site-
packages (from tqdm&gt;4-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.4.6)
Requirement already satisfied: h11&lt;0.15,&gt;=0.13 in
d:\software\anaconda3\lib\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-
&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.14.0)
使用OpenAI的 GPT 系列模型的API有两个前提条件：首先，需要配置科学上网以确保可以访问
OpenAI服务；其次，必须拥有一个有效的API Key。这两项要求的详细配置在《Ch.1 本地调用OpenAI
API流程及OpenAI官网使用指南》中有详细说明，因此我们在此不再赘述，直接进入实操过程。
查看OpenAI的依赖包版本
OpenAI 的 API 在 0.28 版本和 1.xx 版本之间的调用方式存在差异。我们这里使用的是最新的调用
方法，即采用 1.13.XX 版本。因此，需要确认当前 Python 虚拟环境中安装的 openai 包版本是 1.xx。
可以通过执行以下命令来检查当前安装的版本：
import openai
print(openai.__version__)
0.28.0
如果如上显示是0.28.0版本，执行如下命令将openai包升级到最新版本：
# ! pip install --upgrade openai
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
import openai
print(openai.__version__)
1.13.3
设置科学上网环境
根据所使用的科学上网软件，查找相应的代理端口，并按照以下代码进行网络环境配置：
# 这里的端口，请修改为实际使用科学上网软件配置的代理端口
!set HTTP_PROXY=http://127.0.0.1:15732
!set HTTPS_PROXY=http://127.0.0.1:15732
-------- Page 14 --------
新版本的OpenAI API调用方法不允许使用openai.api_key= &quot;sk-xxx&quot;这种老版本的显式指定API
Key，所以如果没有在系统变量中进行配置的话，使用下面的命令设置环境变量：
# 将your- API -key-here替换为实际的API密钥:
# ! setx OPENAI_API_KEY &quot;sk-xxxx&quot;
成功: 指定的值已得到保存。
注意：完成环境变量的初次配置后，需重启电脑以激活设置。这样，在下次使用时就可以直接调用
配置好的环境。为验证环境变量中的API Key是否已正确设置为自己的，可以执行以下命令进行检查。
# ! echo %OPENAI_API_KEY%
完成了科学上网环境和API Key的设置后，在接入LangChain之前，我们先使用如下测试代码检查
当前环境及API的运行状态是否正常，执行代码如下：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
]
)
print(completion.choices[0].message.content)
你好，我是一位可以回答问题、提供信息和与您交流的AI助手。我可以帮助解决问题，回答疑惑，提供建议，
而且乐意与您分享知识和信息。有什么我可以帮助你的吗？
若测试代码能正常输出结果，表明OpenAI的环境配置无误。需要注意的是，LangChain接入
OpenAI GPT模型只是利用其定义的标准模型接入框架来整合OpenAI API，因此进行上述连通性测试
是必要的。
OpenAI GPT Base模型官方说明：https://platform.openai.com/docs/models/moderation
LangChain为了使开发者可以轻松地创建自定义链，整体采用Runnable协议。Runnable 协议是
编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可
以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了
Runnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。
-------- Page 15 --------
LangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch
ain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable
而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解
的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。
如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI：
LangChain Interface说明：https://python.langchain.com/docs/modules/model_io/llms/quic
k_start
LangChain已集成LLMs说明：https://python.langchain.com/docs/integrations/llms/
首先，我们来了解常规的调用方法。具体支持的调用方式如下所示：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
接下来我们对上述方法依次进行尝试。在LangChain框架中，为了规范地接入LLMs类型的大模
型，首先需要导入OpenAI的依赖包。导入命令如下：
from langchain_openai import OpenAI
字符串形式的输入
调用invoke方法，并提供字符串格式的Prompt，便可触发指定模型按照所给Prompt进行续写操
作。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
llm.invoke(
&quot;你好呀，请你介绍一下你自己&quot;
)
-------- Page 16 --------
&#x27;吧。”韩寒摇头，说：“我下次再来，你们自己给我介绍好了。”这一次，韩寒的风趣让陈航漂了一下，他说：
“还是先看看大家的房间吧。”陈航带着韩寒到了楼下，这时连小李都已经来到了楼下，他对韩寒说：“你看，
这是我们的宿舍。”陈航的语气是那么的平淡，既没有过多的炫耀，也没有明显的拒绝，只是那么的平淡，这
让韩寒很有点奇怪，他说：“你们是在这里住吗？”小李说：“是啊，我们是这里的住宿生。”韩寒对小李说：
“那我是来参加考试的，你&#x27;
从上述模型的返回输出，我们可以看到它本质上作为一个补全模型，通过使用大模型原生的
.generate() 方法来对输入的Prompt进行续写。这部分的详细内容已在《Ch.2 Completions模型API
使用指南》中进行了深入讲解，因此此处不再进行赘述。
上述调用方法相当于使用我们熟悉的OpenAI的Completion方法来调用GPT Base类型模型的过
程：
from openai import OpenAI
client = OpenAI()
completion = client.completions.create(
model=&quot;davinci-002&quot;,
prompt=&quot;你好呀，请你介绍一下你自己&quot;,
max_tokens=1024,
)
print(completion.choices[0].text)
。» 她把咳嗽调整成百米冲刺那种负重颤抖。»不，我们在上面，请你告诉会场。哪一地方过去你都要做朋友
呢？」Larsen一转眼注意到卡萨姆旁站着一位古老的男人。他身着粗布制服，最左边有一种圆盏形的供奶
头。透映着那个老的质朴魅力和吓坏人恶心的样子。»LarsenKeberichTaylor.Ch02 3/5/07 5:27 PM
Page 39LEARNING WHERE WECAME FROM»喂主持人。自从我给你们滴剂的时候，我就耐心等待了。请为
快点谈到刚才提的内容。»正赞成的木质松枝变苍白了。既然他是最耐烦的老爷，我们劝劝他这样三小时也
烦。时光久了会累人的人，他更应该知道这些。»好，每个人都听到。»那些选手的名字有老项目拯救人」，
Brickman说。AnderMartin、Taylor, här nere – efter en minut eller så. Likdant för
LarsenKeberich, rätt så bra ger du tillbaka vätsken, yeah, and the eighth
man.»»Eighth man?」Brickman坏坏垂下眼帘扬扬眉头，头盔上的唇影随着他挺起了脸颊。好，一位是
我。不，我不是。Just a moment.»六记地雷爆炸。嫣红的色泽弥漫在鼻子周围极其耀眼之下，淀粉饼糊塞
了全脸，剔饭一般的巧克力混合在一起。»»Would you all please proceed into the next
building and make sure that you use the turnstiles.»伤口破口流血，簇瓣的血色俗套，但去
还是要用。套房好美啊、清生。套房饲養下只要一个；_ 户功能装置就。»艰难地活着足以获得一天的欢乐的
人确实算够奇特的。咳嗽的木质松枝_cancel6 3/5/07 5:27 PM Page 40IT IS NOT ENOUGH
TOMAKE MANKINDHAPPY,LARSEN KEBERICH向报纸走来，她在走廊哪一个厅卡萨姆等待着;在这个保证已
经到达那种别有心碎的正式屋子里。她穿过镶有玫瑰花的拉链的抢洗衣区和单人舔饭的厕所区圆朝圣地的道。
先是葡萄干牛油，透过臭风从供应箱中飘来，刚开始时她便一直寻找那一种苏查尔的酸味。那种味美常是发出
着从薄饼里流发出来的所以一圈难找到对不起，小姐，我们之前选择做了决定，就算更改好一个时刻，也无法
改变决定。拨动键盘-找麻烦你的，怎么做了、哦，听起来好像你一突然卸了山猫带来了 Asktohell你疼
吗。杨虎大吼着，冲向对方。丁
流式输出
要进行流式调用，可以调用stream方法。这种方式允许在接收到模型响应的同时，逐块输出结
果，实现即时展示而无需等待全部内容处理完毕。
-------- Page 17 --------
from langchain_openai import OpenAI
for chunk in llm.stream(
&quot;你好呀，请你介绍一下你自己&quot;
):
print(chunk, end=&quot;&quot;, flush=True)
吧，你是谁，你在哪里，你是什么学问，你喜欢什么，你不喜欢什么，你不会什么，你会什么，你梦想什么，
你的优点是什么，你的缺点是什么，你的性格是什么，你的兴趣是什么，你喜欢的食物是什么，你不喜欢的食
物是什么，你的爱好是什么，你的愿望是什么。介绍一下你自己好的。
[ 编辑 xiaofan1 在 07-03-18 23:49 ]
批量生成
通过调用batch方法，可以同时对多个不同的Prompt进行调用处理。在这个例子中，输入包括“你
好”、“什么是机器学习”和“什么是深度学习”三个独立的Prompt。这种批量处理方式允许一次性提交多个
Prompt，从而提高推理效率。
from langchain_openai import OpenAI
llm.batch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
-------- Page 18 --------
[&#x27;，习惯了你的成熟，可是这次你的决定让我感到非常惊讶，我不知道你的这次决定是不是真的，我不知道你
是否真的那么爱我，虽然我知道你非常爱我，可是我不知道你是不是真的爱我，假如你为了我不想去上学，我
也可以放弃一切，去读书，去上大学，但是你为什么要这样做呢，我不清楚你的想法，我不知道你的想法，我
只知道我爱你，我只知道我不想失去你，我只知道我不想让你失去你的希望，我只知道我不想让你失去你的未
来，可是我想知道你为什么要这样做呢，我想知道你的想法，你是&#x27;,
&#x27; 机器学习的原理 机器学习的技术 机器学习的应用机器学习在生产过程中的作用机器学习在生产过程中的
作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产
过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学
习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作
用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学&#x27;,
&#x27;？ 2. Why is the industry so interested in deep learning? 3. How will deep
learning transform the financial industry? 4. How can you use deep learning to
build better AI? 5. What are the ingredients of a deep learning model?\n\nA
study of the best deep learning algorithms and how they work, the techniques
involved in building them, and the results they can bring to the table: 1.
Unsupervised learning 2. Convolutional Neural Networks (CNN) 3. Recurrent Neural
Networks (RNN) 4. Deep Reinforcement Learning (RL) 5. Generative Adversarial
Networks (GAN)\n\nA step-by-step guide to building a real-world, deep learning
app: 1. What is a deep learning app? 2. How do you build a deep learning app? 3.
The various components involved in building a deep learning app 4. How do you
deploy a deep learning app? 5. The future of deep learning apps\n\nHow to build
a deep learning app: A guide to the various tools, frameworks, and libraries
that you can use to build deep learning apps: 1. Deep Learning Frameworks 2.
Deep Learning Libraries 3. Deep Learning Tools 4&#x27;]
异步调用
llm.invoke(...)本质上是一个同步调用。在这种情况下，程序会在调用返回结果之前停止执行
任何后续代码。这意味着如果invoke操作耗时较长，它会导致程序暂时挂起，直到操作完成。我们可
以通过这样一个测试代码来直观的理解同步调用：
import time
def call_model():
# 模拟同步API调用
print(&quot;开始调用模型...&quot;)
time.sleep(3) # 模拟调用等待
print(&quot;模型调用完成。&quot;)
def perform_other_tasks():
# 模拟执行其他任务
for i in range(3):
print(f&quot;执行其他任务 {i + 1}&quot;)
time.sleep(1)
def main():
start_time = time.time()
call_model()
perform_other_tasks()
end_time = time.time()
total_time = end_time - start_time
return f&quot;总共耗时：{total_time}秒&quot;
# 运行同步任务并打印完成时间
main_time = main()
-------- Page 19 --------
main_time
开始调用模型...
模型调用完成。
执行其他任务 1
执行其他任务 2
执行其他任务 3
&#x27;总共耗时：6.001835107803345秒&#x27;
这段同步调用的程序先模拟了一个耗时3秒的模型调用，随后执行了三个其他任务，每个任务耗时1
秒。实际的执行时间为约6.00秒。这体现了同步执行的特点：每个操作依次执行，直到当前操作完成后
才开始下一个操作，从而导致总的执行时间是各个操作时间的总和。
而异步调用，允许程序在等待某些操作完成时继续执行其他任务，而不是阻塞等待。这在处理I/O
操作（如网络请求、文件读写等）时特别有用，可以显著提高程序的效率和响应性。
我们可以设计这样如下情景：使用async和await关键字， 通过async def定义异步函数
async_call和perform_other_tasks。await asyncio.sleep(3)模拟了一个耗时3秒的异步操
作。这里的await表明程序在这里“等待”这个操作完成，但与同步操作不同，这个等待是非阻塞的。这
意味着程序可以在这3秒内执行其他异步代码。这种方式不会阻塞整个程序的运行，而是让出控制权，
允许事件循环继续运行其他任务。
! pip install asyncio
Collecting asyncio
Obtaining dependency information for asyncio from
https://files.pythonhosted.org/packages/22/74/07679c5b9f98a7cb0fc147b1ef1cc1853b
c07a4eb9cb5731e24732c5f773/asyncio-3.4.3-py3-none-any.whl.metadata
Downloading asyncio-3.4.3-py3-none-any.whl.metadata (1.7 kB)
Downloading asyncio-3.4.3-py3-none-any.whl (101 kB)
---------------------------------------- 0.0/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
----------- --------------------------- 30.7/101.8 kB 163.8 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
----------------------- --------------- 61.4/101.8 kB 113.0 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
-------------------------------------- 101.8/101.8 kB 136.2 kB/s eta 0:00:00
Installing collected packages: asyncio
-------- Page 20 --------
Successfully installed asyncio-3.4.3
import asyncio
import time
async def async_call(llm):
await asyncio.sleep(3) # 模拟异步操作
print(&quot;异步调用完成&quot;)
async def perform_other_tasks():
await asyncio.sleep(3) # 模拟异步操作
print(&quot;其他任务完成&quot;)
async def run_async_tasks():
start_time = time.time()
await asyncio.gather(
async_call(None), # 示例调用，替换None为模拟的LLM对象
perform_other_tasks()
)
end_time = time.time()
return f&quot;总共耗时：{end_time - start_time}秒&quot;
# 运行异步任务并打印完成时间
await run_async_tasks()
异步调用完成
其他任务完成
&#x27;总共耗时：3.009281873703003秒&#x27;
使用asyncio.gather()并行执行时，理想情况下，因为两个任务几乎同时开始，它们的执行时间
将重叠。如果两个任务的执行时间相同（这里都是3秒），那么总执行时间应该接近单个任务的执行时
间（3秒左右），而不是两者时间之和。
理解了同步调用和异步调用的区别后，我们接下来看一下LangChain中集成的异步调用链使用方
法。
要实现异步调用并优化处理多个请求的场景，在LangChain中，可以使用ainvoke方法来达成。这
种方法允许同时启动多个请求，而不必等待每个请求依次完成。这对于提高应用程序的响应性和效率尤
其有用，特别是在处理需要与外部服务交互的任务时。
-------- Page 21 --------
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;这次您知道了吗？这是鸡肋大战，您是来玩的吧，我也是。我来纠结你的，看看你敢不敢。我一般不来这种
鸡肋大战，因为我很懒，这次我还是来了，因为我懒惯了，其实我很想快点解脱了，我很想走。你的目的在哪
里？我想敌对你，我还是要意识到了，你想不想敌对我呢？你敢不敢这样问？我不敢，因为我懒。你的目的是
什么？我很想快点解脱，我不想你，我只想快点走，早点走。你敢不敢这样说？我敢，因为我懒。你的目的是
什么？我&#x27;
异步流式响应
from langchain_openai import OpenAI
async for chunk in llm.astream(
&quot;你好，&quot;,
):
print(chunk, end=&quot;&quot;, flush=True)
我是来投诉的 [Image]
你现在没有抱怨，说的确实很有道理，如果他们为了你不让你上，你能不上吗，我想大多数人应该会上，所以
他们是不会因为你不上而阻止你上的，但是如果你上了，你的优惠和你的一切都是不可能了，你上了，他们是
不能为你划上你的学费的，所以我觉得就算你上了也没有用，他们不会为你划学费，因为你上了从而给了他们
非常大的麻烦，你上了，他们是不能有一点好处的，他们还要为你上的学费等等付出，所以他们会说你上不
了，就是为了不让你上，不让你上，他们不会为你划学费，所以我觉得你如果上了，你就没有任何
异步批量处理
from langchain_openai import OpenAI
await llm.abatch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
-------- Page 22 --------
[&#x27;，前来亲自拜访的，因为我们这里很少有人来拜访，你就打算在这里待上几个礼拜了吧？”“我是回来找你，
我是到桐谷这里来的。”李晓婷冷冷的回道。这一次很明显的不是她想让他留下来的，而是让他走了。众人看
着李晓婷，这里的她就这么冷了，竟然连郑晓东都不看一眼，郑晓东是她的男朋友，他们是一对，她怎么可能
不看一眼？“那你为什么不和我说，你的事情怎么样了？”郑晓东看向李晓婷的脸，她的表情很冷，她怎么不看
一眼自己&#x27;,
&#x27;？\n\n2:王景仪 2021-01-07 23:12:57\n\nannounced\n\n3:李桂林 2021-01-01
23:12:57\n\nMy Sunshine is a TV drama adapted from Gu Man’s novel of the same
title. It stars Wallace Huo and Tang Yan, premiering in January, 2015. The drama
depicts a man and a woman’s sweet love story.\n\n4:王威 2020-12-27
23:12:57\n\nThe new data released yesterday represent the first snapshot of
global trade for 2015. And the figures suggest that 2015 was not a good year for
world trade, as it had been hoped.\n\n5:苏晓波 2021-01-01 23:12:57\n\nMark Rutte,
the Dutch prime minister whose People’s Party for Freedom and Democracy (VVD) is
slightly ahead of the PVV, has seen his poll numbers rise in recent days because
of his hardline stance with Ankara. On Tuesday he called Mr Erdogan’s comments
“a disgusting distortion of history”, adding: “We will not lower ourselves to
this level. It is totally unacceptable.”\n\n&#x27;,
&#x27;？深度学习是一种模仿大脑思维的机器学习方法，深度学习方法被广泛用于图像识别、语音识别、自然语言
处理等领域。深度学习过程中，模型经常需要大量的训练数据，因此大量的数据获取和数据刷新是有必要。.
1) Find a free server to host the application in. I have been trying to create a
simple neural network by using the TensorFlow Object Detection API to run an
object detection model. Cloud TPU is a Google Cloud service that accelerates
machine learning workloads. Major cloud providers offer free tier of services to
help you get started. Google Cloud Storage is a RESTful web service for storing
and accessing data on Google Cloud Platform. In this video, you will learn about
Cloud TPU, a high-performance, scalable, and cost-effective machine learning
service. This IP address 157. 0, a neural network framework. Contribute.
TensorFlow Lite for mobile and embedded devices For Production TensorFlow
Extended for end-to-end ML components. The Google Cloud Platform&#x27;]
除此之外，在Jupyter Notebook或Python交互式解释器中进行操作的话，可以使用?来查看关于
llm中内置的方法和帮助文档，执行如下代码：
llm?
•[1;31mSignature:•[0m
•[0mllm•[0m•[1;33m(•[0m•[1;33m
•[0m •[0mprompt•[0m•[1;33m:•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mstop•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mcallbacks•[0m•[1;33m:•[0m •[1;34m&#x27;Callbacks&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m*•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mtags•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mmetadata•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[Dict[str, Any]]&#x27;•[0m
•[1;33m=•[0m •[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m**•[0m•[0mkwargs•[0m•[1;33m:•[0m
•[1;34m&#x27;Any&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m•[1;33m)•[0m •[1;33m-&gt;•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m•[0m•[1;33m•[0m•[0m
•[1;31mType:•[0m OpenAI
•[1;31mString form:•[0m
•[1mOpenAI•[0m
-------- Page 23 --------
Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {},
&#x27;max_tokens&#x27;: 256}
•[1;31mFile:•[0m d:\software\anaconda3\lib\site-
packages\langchain_openai\llms\base.py
•[1;31mDocstring:•[0m
OpenAI large language models.
To use, you should have the ``openai`` python package installed, and the
environment variable ``OPENAI_API_KEY`` set with your API key.
Any parameters that are valid to be passed to the openai.create call can be
passed
in, even if not explicitly saved on this class.
Example:
.. code-block:: python
from langchain_community.llms import OpenAI
openai = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
•[1;31mInit docstring:•[0m
Create a new model by parsing and validating input data from keyword arguments.
Raises ValidationError if the input data cannot be parsed to form a valid model.
•[1;31mCall docstring:•[0m
[*Deprecated*] Check Cache and run the LLM on the given prompt and input.
Notes
-----
.. deprecated:: 0.1.7
Use invoke instead.
这里能看到非常多的关键信息，比如：
默认参数：Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {}, &#x27;max_tokens&#x27;: 256}
当不输入model_name时，默认会调用davinci-002模型：
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;我是小影。很高兴认识你，有什么可以帮到你的吗？&#x27;
同时，也可以调整温度temperature:
-------- Page 24 --------
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
temperature=0
)
&#x27;我是一个人工智能助手，很高兴认识你！有什么可以帮到你的吗？&#x27;
model：必选参数，具体调用的Completions模型名称，可以调用的模型包括text-davinci-003、
text-davinci-002、text-curie-001、text-babbage-001、text-ada-001等，不同模型参数规模不
同；这里需要注意，大模型领域不同于机器学习领域，后者哪怕是简单模型在某些场景下可能也会
拥有比复杂模型更好的表现。在大模型领域，（就OpenAI提供的A、B、C、D四大模型来看）参
数规模越大、越新版本的模型效果更好（当然费用也更高），因此课程中主要以text-davinci-003
使用为例进行讲解；
prompt：必选参数，提示词；
suffix：可选参数，默认为空，具体指模型返回结果的后缀；
max_tokens：可选参数，默认为16，代表返回结果的token数量；
temperature：可选参数，取值范围为0-2，默认值为1。参数代表采样温度，数值越小，则模型会
倾向于选择概率较高的词汇，生成的文本会更加保守；而当temperature值较高时，模型会更多地
选择概率较低的词汇，生成的文本会更加多样；
top_p：可选参数，取值范围为0-1，默认值为1，和temperature作用类似，用于控制输出文本的
随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节
文本随机性，top_p和temperature两个参数选择一个进行调整即可；这里更推荐使用
temperature参数进行文本随机性调整；
n：可选参数，默认值为1，表示一个提示返回几个Completion；
stream：可选参数，默认值为False，表示回复响应的方式，当为False时，模型会等待返回结果
全部生成后一次性返回全部结果，而为True时，则会逐个字进行返回；
logprobs：可选参数，默认为null，该参数用于指定模型返回前N个概率最高的token及其对数概
率。例如，如果logprobs设为10，那么对于生成的每个token，API会返回模型预测的前10个
token及其对数概率；
echo：可选参数，默认为False，该参数用于控制模型是否应该简单地复述用户的输入。如果设为
True，模型的响应会尽可能地复述用户的输入；
stop：可选参数，默认为null，该参数接受一个或多个字符串，用于指定生成文本的停止信号。当
模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成。这可以用来控制模型的输出长
度或格式；
presence_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容
（例如新的概念或主题）的倾向性。较高的值会使模型更倾向于生成新内容，而较低的值则会使模
型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取
值；
frequency_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾
向性。较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果
篇幅较大并且存在前后语言重复时，可以提高该参数的取值；
best_of：该参数用于控制模型的生成过程。它会让模型进行多次尝试（例如，生成5个不同的响
应），然后选择这些响应中得分最高的一个；
logit_bias：该参数接受一个字典，用于调整特定token的概率。字典的键是token的ID，值是应用
于该token的对数概率的偏置；在GPT中我们可以使用tokenizer tool查看文本Token的标记。一般
-------- Page 25 --------
不建议修改；
user：可选参数，使用用户的身份标记，可以通过人为设置标记，来注明当前使用者身份。需要
注意的是，Completion.create函数中的user和后续介绍的对话类模型的user参数含义并不相同，
需要注意区分；
更详细的参数说明和使用细节可在《Ch.2 Completions模型API使用指南》中找到。此处不在展开
重复说明。
目前在LangChain中已经集成的LLMs有82个，均已经实现了对异步、流式处理和批处理的基本支
持，查看地址如下：https://python.langchain.com/docs/integrations/llms/
4.1.2 Chat Models接入方法
所谓聊天模型，简单理解就是我们经常使用的xxx-chat类模型，比如Baichuan2-7b-chat，
Qwen-14b-chat，这类模型是基于Base模型经过针对特定数据格式的微调得到，以更贴近人类对话的
方式进行输入和输出。所以这类模型不会使用“文本输入、文本输出”的形式，而是采用聊天消息作为输
入并返回聊天消息作为输出（非纯文本）。LangChain框架与OpenAI、Cohere、Hugging Face等多家
模型提供商已经实现集成，提供了一个标准化的接口，以便与这些不同的模型进行交互。
我们先回顾下OpenAI规范下使用Chat模型进行一次交互需遵循的代码执行方式：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo-0125&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一个乐于助人的智能AI小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}
]
)
print(completion.choices[0].message.content)
-------- Page 26 --------
您好，我是一个智能AI助手，可以帮助您回答问题、提供信息、解决问题等。如果您有任何疑问或需要帮助的
话，都可以随时向我提问。我会尽力帮助您解决问题。
这里不得不再次提到，LangChain作为一个应用开发框架，需要集成各种不同的大模型，如上述
OpenAI的GPT系列模型调用示例，通过Message数据输入规范，定义不同的role，即system、user和
assistant来区分对话过程，但对于其他大模型，并不意味这一定会遵守这种输入输出及角色的定义，所
以LangChain的做法是，因为Chat Model基于消息而不是原始文本，LangChain目前就抽象出来的消息
类型有 AIMessage 、 HumanMessage 、 SystemMessage 、 FunctionMessage 和 ChatMessage ，
但大多时候我们只需要处理 HumanMessage 、 AIMessage 和 SystemMessage，即：
SystemMessage ：用于启动 AI 行为，作为输入消息序列中的第一个传入。
HumanMessage ：表示来自与聊天模型交互的人的消息。
AIMessage ：表示来自聊天模型的消息。这可以是文本，也可以是调用工具的请求。
同样，Chat Models实现了 Runnable 接口，也支持 invoke 、 ainvoke 、 stream 、 astream 、
batch 、 abatch 等调用。即：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
所以对于LangChain中调用OpenAI的GPT模型，首先需要导入对应的模块，代码如下：
from langchain_core.messages import HumanMessage, SystemMessage
消息形式输入调用
定义消息对象：
messages = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;你好，请你介绍一下你自己&quot;),]
messages
[SystemMessage(content=&#x27;你是一位乐于助人的智能小助手&#x27;), HumanMessage(content=&#x27;你好，请
你介绍一下你自己&#x27;)]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
-------- Page 27 --------
执行推理：
chat.invoke(messages)
AIMessage(content=&#x27;你好，我是一位可以回答问题、提供帮助的智能小助手。无论是关于知识、技能还是
日常生活中的问题，我都会尽力为您提供准确和有用的答案。有什么可以帮助您的吗？&#x27;)
如果只想获取到纯净的输出，可以通过.content来格式化。
reponse.content
&#x27;你好！我是一位智能助手，可以回答各种问题，提供信息和帮助。无论是关于知识、技能、娱乐、生活等方
面，我都会尽力为你提供帮助。有什么问题需要我帮忙的吗？&#x27;
流式调用
for chunk in chat.stream(messages):
print(chunk.content, end=&quot;&quot;, flush=True)
你好！我是一位智能助手，专门为用户提供信息咨询、问题解答和帮助服务。无论你有什么疑问或需求，我都
会尽力帮助你，让你更方便地获取所需的信息和支持。有什么我可以帮助你的吗？
批量调用
先定义三个不同的消息对象：
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
messages2 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是深度学习&quot;),]
messages3 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是大模型技术&quot;),]
将上述三个消息对象放在一个列表中，使用.batch方法执行批量调用。
reponse = chat.batch([messages1,
messages2,
messages3,])
reponse
-------- Page 28 --------
[AIMessage(content=&#x27;机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来
改善自身的性能，而无需明确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规
律、模式以及知识的技术。通过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预
测、分类、识别等任务。机器学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测
等领域。&#x27;),
AIMessage(content=&#x27;深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神
经网络层来学习和理解复杂的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识
别、语音识别、自然语言处理等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对
新数据进行准确预测和分类。深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金
融、交通等各个领域。&#x27;),
AIMessage(content=&#x27;大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部
署复杂的深度学习模型。这些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优
化。\n\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模
型技术，研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。\n\n然
而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员和
工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。&#x27;)]
格式化输出：
# 使用列表生成式打印每一个消息的内容
contents = [msg.content for msg in reponse]
# 打印出内容
for content in contents:
print(content, &quot;\n---\n&quot;)
机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来改善自身的性能，而无需明
确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规律、模式以及知识的技术。通
过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预测、分类、识别等任务。机器
学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测等领域。
---
深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神经网络层来学习和理解复杂
的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识别、语音识别、自然语言处理
等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对新数据进行准确预测和分类。
深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金融、交通等各个领域。
---
大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部署复杂的深度学习模型。这
些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优化。
大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模型技术，
研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。
然而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员
和工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。
---
异步调用
-------- Page 29 --------
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
reponse = await chat.ainvoke(messages1)
reponse
AIMessage(content=&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并
改进性能，而无需显式地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断
优化自身的表现。机器学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常
见的机器学习方法包括监督学习、无监督学习、强化学习等。&#x27;)
reponse.content
&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并改进性能，而无需显式
地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断优化自身的表现。机器
学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常见的机器学习方法包括
监督学习、无监督学习、强化学习等。&#x27;
流式调用
async for chunk in chat.astream(messages1):
print(chunk.content, end=&quot;&quot;, flush=True)
机器学习是一种人工智能的分支，它致力于让计算机系统通过学习数据和模式，从而改进自身的性能。简而言
之，机器学习让计算机具有从经验中学习的能力，而不需要明确地编程指令。
在机器学习中，计算机系统利用大量数据进行训练，通过识别数据中的模式和规律来建立模型。这些模型可以
用来进行预测、分类、识别等任务。机器学习在各个领域中都有广泛的应用，如自然语言处理、图像识别、医
疗诊断、金融预测等。
常见的机器学习算法包括监督学习、无监督学习、强化学习等。监督学习是通过已标记的数据进行训练，无监
督学习则是在没有标记的数据中发现模式，强化学习则通过试错的方式学习最优策略。机器学习技术的发展使
得计算机系统能够更加智能地处理复杂任务，为人们的生活和工作带来了许多便利。
通过上述描述，我们展示了在LangChain中使用LLMs类模型和Chat Model类模型的不同方法，其
核心区别在于输入Prompt的格式。除此之外其他的工作可以直接利用统一的抽象接口，实现与模型交
互的快速过程。而针对不同的模型，LangChain也提供个对应的接入方法，其相关说明文档地址：http
s://python.langchain.com/docs/integrations/chat/
-------- Page 30 --------
比如我们以Baichuan2的在线API模型为例快速接入一下：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;我日薪8块钱，请问在闰年的二月，我月薪多少&quot;)])
response
AIMessage(content=&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所
以，你在闰年的二月月薪是232元。&#x27;)
response.content
&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所以，你在闰年的二月月
薪是232元。&#x27;
如未实践过Baichuan2的在线API，请查看《APICh.16 BaiChuan 2 大模型生态介绍及本地私有化
部署方案》中1. Baichuan在线大模型生态介绍小节
可以看出，LangChain能够迅速接入不同的模型API。至于接入本地私有部署的开源大模型的方
法，我们将在后续课程中进行详细介绍。
-------- Page 31 --------
4.2 LangChain中如何使用Prompt Template
提示工程（Prompt Engineering）大家应该比较熟悉，这个概念是指在与大语言模型（LLMs），
如GPT-3、Qwen等模型进行交互时，精心设计输入文本（即提示）的过程，以获得更精准、相关或有
创造性的输出。正如我们在大模型技术应用课程中介绍的，通过采用Few-Shot、Chain of Thought
(CoT)等高级提示技巧，可以显著提高大模型在推理任务上的表现。目前，提示工程已经发展成为一个
专业领域，非常多的公司设立了专门的职位，负责为特定任务编写精确且具有创造力的提示。
以使用ChatGPT等网页端对话交互应用中，大部分人常见的做法是将提示（Prompt）做硬编码，
例如将一段提示文本固定在System Messages中。而在应用开发领域，开发者往往无法预知用户的具体
输入内容，同时又希望大模型能够根据不同的应用任务以一种较为统一的逻辑来处理用户输入。所以，
LangChain通过提供Prompt Templates功能，优雅地解决了这个问题。Prompt Templates将用户输入
到完整格式化提示的转换逻辑进行封装，使得模型能够更灵活、高效地处理各种输入。
LangChain 提供了创建和使用提示模板的工具，其使用PromptTemplate 方法创建字符串提示模
板，因为目前为止，语言模型接收的提示基本都是字符串或聊天消息列表。
4.2.1 使用str.format语言构建模版
在LangChain的默认设置下， PromptTemplate 使用 Python 的 str.format() 方法进行模板
化。
Python的str.format()方法是一种字符串格式化的手段，允许我们在字符串中插入变量。使用这
种方法，可以创建包含占位符的字符串模板，占位符由花括号{}标识。调用format()方法时，可以传入
一个或多个参数，这些参数将被顺序替换进占位符中。str.format()提供了灵活的方式来构造字符串，支
持多种格式化选项，包括数字格式化、对齐、填充、宽度设置等。
基本用法
# 简单示例，直接替换
greeting = &quot;Hello, {}!&quot;.format(&quot;Alice&quot;)
print(greeting)
# 输出: Hello, Alice!
Hello, Alice!
带有位置参数的用法
# 使用位置参数
info = &quot;Name: {0}, Age: {1}&quot;.format(&quot;Bob&quot;, 30)
print(info)
# 输出: Name: Bob, Age: 30
Name: Bob, Age: 30
带有关键字参数的用法
# 使用关键字参数
info = &quot;Name: {name}, Age: {age}&quot;.format(name=&quot;Charlie&quot;, age=25)
print(info)
# 输出: Name: Charlie, Age: 25
-------- Page 32 --------
Name: Charlie, Age: 25
使用字典解包的方式：
# 使用字典解包
person = {&quot;name&quot;: &quot;David&quot;, &quot;age&quot;: 40}
info = &quot;Name: {name}, Age: {age}&quot;.format(**person)
print(info)
# 输出: Name: David, Age: 40
Name: David, Age: 40
在LangChain中，基本采用了Python的原生str.format()方法对输入数据进行格式化，这样在模
型接收输入前，可以根据需要对数据进行预处理和结构化，以此来引导大模型进行更准确的推理。
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(
&quot;请给我一个关于{topic}的{type}解释。&quot;
)
prompt= prompt_template.format(type=&quot;详细&quot;, topic=&quot;量子力学&quot;)
prompt
&#x27;请给我一个关于量子力学的详细解释。&#x27;
如上所示，可以使用PromptTemplate的from_template方法创建一个提示模板实例，这个模板
包含了两个占位符：{topic} 和 {type}，这些占位符在实际调用时可以被实际的值替换。
调用LLMs模型
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
我觉得我已经对量子力学的基本理解了，但是我还是很想听听有关量子力学的详细解释，比如：量子力学是怎
么起作用的。
-------- Page 33 --------
谢谢了.
量子力学是在克里姆林宫试验时的实验结果发现.一开始是用熟悉的物理学法则去解释它.比如说,量子力学是
量子力学的结果,而不是量子力学的原因.玻尔兹曼方程就是克里姆林宫试验结论的表示.后来人们发现,等效
性,柏格效应等现象.然后才发现,原来我们的光学和电磁学等理论是有问题的.这些现象的实现是在离子束中.
所以量子力学是很实际的.
[ 编辑 stuartm 在 06-07-08 12:11 ]
prompt= prompt_template.format(type=&quot;简短&quot;, topic=&quot;机器学习&quot;)
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
2)目前的时代，机器学习已经发展到了什么程度，也有什么应用场景。 3)目前的研究方向是什么。 4)什么
时候会用到这些技术。 5)最后一个问题是，对于我自己来说，是不是还有其他的方向比如说算法之类的，还
有在个人的技术知识里面是不是需要这些东西，不是特别需要。 也许这些都是我自己的猜测，如果你有什么
好的建议，我也非常感谢。 最后，我想问一下，你是在哪里学的，是在哪个大学，我就是在这里问问。 谢
谢。 2019.6.16
电影《伪装者》中的重要概念
原理：1. 无线电波的能量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无
线电波的方向不会发生改变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改
变，传播途中，波的方向不会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途
中，波的方向会发生改变（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变
（这就是遮挡）3. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）4. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）5. 无线电波的方向会发生改变，
传播途中，波的方向不会发生改变（这就是遮挡）6. 无线电波的方向会发生改变，传播途中，波的方向不会
发生改变（这就是遮挡）7. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）
8. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）原理：1. 无线电波的能
量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无线电波的方向不会发生改
变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改变，传播途中，波的方向不
会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途中，波的方向会发生改变
（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）3. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这
除此之外，这种模板生成方式支持任意数量的变量，包括无变量，可以做如下测试：
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(&quot;给我讲一个笑话&quot;)
prompt_template.format()
-------- Page 34 --------
&#x27;给我讲一个笑话&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, )
result = llm.invoke(prompt)
print(result)
如果我是一个机器，会被称为什么？
谢谢
调用Chat Model
上述示例展示了如何使用字符串模板生成和调用LLMs类模型。对于聊天模型，也需要使用 Python
的 str.format() 方法进行模板化，但其构造的输入形式则有所区别，应采用聊天消息列表的形式，
每条消息都包含内容和一个名为role的附加参数。例如，在OpenAI的Chat Completions API中，聊天
消息需要包含为assistant、user或system。在这种情况下，应使用ChatPromptTemplate来构建对话
模板。
from langchain_core.prompts import ChatPromptTemplate
chat_template = ChatPromptTemplate.from_messages(
[
(&quot;system&quot;, &quot;你是一个有帮助的AI机器人，你的名字是{name}。&quot;),
(&quot;human&quot;, &quot;你好，最近怎么样？&quot;),
(&quot;ai&quot;, &quot;我很好，谢谢！&quot;),
(&quot;human&quot;, &quot;{user_input}&quot;),
]
)
messages = chat_template.format_messages(name=&quot;小明&quot;, user_input=&quot;你叫什么名字？&quot;)
messages
[SystemMessage(content=&#x27;你是一个有帮助的AI机器人，你的名字是小明。&#x27;),
HumanMessage(content=&#x27;你好，最近怎么样？&#x27;),
AIMessage(content=&#x27;我很好，谢谢！&#x27;),
HumanMessage(content=&#x27;你叫什么名字？&#x27;)]
print(chat_template)
-------- Page 35 --------
input_variables=[&#x27;name&#x27;, &#x27;user_input&#x27;] messages=
[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;name&#x27;],
template=&#x27;你是一个有帮助的AI机器人，你的名字是{name}。&#x27;)),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[],
template=&#x27;你好，最近怎么样？&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;我很
好，谢谢！&#x27;)), HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=
[&#x27;user_input&#x27;], template=&#x27;{user_input}&#x27;))]
从输出上看，其构造函数在实例化prompt_template时，主要由两个关键参数进行指定：
input_variables：这是一个列表，包含模板中需要动态填充的变量名。这些变量名在模板字符串
中以花括号（如{name}）标记。通过指定这些变量，可以在后续过程中动态地替换这些占位符。
template：这是定义具体提示文本的模板字符串。它可以包含静态文本和input_variables列表中
指定的变量占位符。当调用format方法时，这些占位符会被实际的变量值替换，生成最终的提示
文本。
调用Chat Model，这里我们选择GPT 3.5。代码如下：
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
我叫小明，很高兴为您提供帮助！有什么问题可以帮到您？
messages = chat_template.format_messages(name=&quot;大熊&quot;, user_input=&quot;你要去哪里玩？&quot;)
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
作为一个AI机器人，我没有身体，所以无法去任何地方玩耍。但我可以帮助你规划旅行或提供有关旅游目的地
的信息。有什么我可以帮助你的吗？
通过上述流程，我们演示了如何在LangChain框架内使用ChatPromptTemplate来构建与聊天模型
交互的对话模板。通过此方法，能够创建一个包含不同参与者角色和定制化消息内容的对话流程，这对
于开发复杂的聊天应用或增强AI助手的交互能力至关重要。
4.2.2 构造Few-Shot模版
在LangChain中，很多的功能抽象、链路抽象本质上都是在对大模型的“涌现能力”能够应用落地的
一种具体实现方法，而其推理的不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度
依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。最为简单的
提示工程的方法就是通过输入一些类似问题和问题答案，让模型参考学习，并在同一个prompt的末尾
提出新的问题，依次提升模型的推理能力。
比如我们在《Ch.3 基于思维链的进阶提示工程》中测试的经典推理问题：
-------- Page 36 --------
import os
import openai
openai.api_key = os.getenv(&quot;OPENAI_API_KEY&quot;)
prompt = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次？&#x27;
prompt
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n在关闭之前，艾米能滑14次。&#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n\n15分钟 = 900秒\n\n每次滑下来需要1分钟，所以在15分钟内，可以滑下900秒/（1分钟+4分钟）=
900/5 = 180次。&#x27;
可以看到多次执行，其推理均是错误的。正确的计算过程应该是先计算艾米一次爬上爬下总共需要
5分钟，然后滑梯还有15分钟关闭，因此关闭之前能够再滑15/5=3次。
-------- Page 37 --------
而我们当时在OpenAI的API规范下，是通过这种方式来进行Few-Shot的提示：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
在Few-Shot的提示下，再次尝试进行提问：
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt_Few_shot_CoT4,
max_tokens=1000,
)
response1.choices[0].text
&#x27;”艾米花了1分钟滑下来，滑一次上去需要4分钟，因此在15分钟关闭之前，她能滑15分钟-1分钟=14分钟，
14分钟除以4分钟每次，她能滑14÷4=3次，答案是3次。“ &#x27;
-------- Page 38 --------
从输出结果上看，通过Few-Shot提示可以激发大模型的涌现能力从而回答对复杂推理问题，在应用
开发中同样需要这种能力来提升大模型的推理稳定性。所以LangChain抽象了
FewShotPromptTemplate方法来实现。我们可以先测试下在LangChain的接口下直接提问：
直接提问
prompt1 = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在
关闭之前她能滑多少次？&#x27;
prompt1
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在关闭之前，艾米能滑(15-4-1)/4=2次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在15分钟内，她能滑15次。&#x27;
其实也很容易想到，LangChain的工作仅仅是集成并接入OpenAI的GPT模型，因此其本质上与直接
调用GPT模型的效果相同。出现错误回答也属于正常情况。接下来，我们根据LangChain对Few-Shot模
板的定义规范，进行提示词的构建。
在LangChain中，需要使用 PromptTemplate 创建字符串提示模板。模板可以包括说明、少量示
例以及适合给定任务的特定上下文和问题。因此，我们要创建一个少量示例的列表。每个示例都是一个
字典，其中键是输入变量，值是这些输入变量的值。
适合LLMs模型的模版构造形式
from langchain.prompts.few_shot import FewShotPromptTemplate
-------- Page 39 --------
from langchain.prompts.prompt import PromptTemplate
examples = [
{
&quot;question&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个
网球？&quot;,
&quot;answer&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现
在总共由5+6=11个网球。因此答案是11。&quot;
},
{
&quot;question&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在
食堂总共有多少个苹果？&quot;,
&quot;answer&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答
案是9。&quot;
},
{
&quot;question&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？&quot;,
&quot;answer&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4
个，答案是4个。&quot;
},
]
接下来需要实例化一个 PromptTemplate 对象，将少量示例格式化为字符串。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[0]))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[1]))
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
最后，创建一个 FewShotPromptTemplate 对象。该对象接受经过上一步格式化后的少数样本示
例。
-------- Page 40 --------
# 导入 FewShotPromptTemplate 类
from langchain.prompts.few_shot import FewShotPromptTemplate
# 创建一个 FewShotPromptTemplate 对象
few_shot_prompt = FewShotPromptTemplate(
examples=examples, # 最开始定义的 examples 示例
example_prompt=example_prompt, # 上一步定义的 example_prompt 作为提示模板
suffix=&quot;Question: {input}&quot;, # 后缀模板，其中 {input} 会被替换为实际输入
input_variables=[&quot;input&quot;] # 定义输入变量的列表
)
# 使用给定的输入格式化 prompt，并打印结果
print(few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，
水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Question: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有
多少个蓝色高尔夫球？
总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
Question: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次？
final_input = few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟
才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)
final_input
&#x27;Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰
一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此
答案是11。\n\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现
在食堂总共有多少个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹
果，答案是9。\n\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是
蓝色的。请问总共有多少个蓝色高尔夫球？\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是
蓝色的，也就是4个，答案是4个。\n\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&#x27;
将带有Few-Shot提示的输入，输入到大模型中执行推理。
-------- Page 41 --------
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米需要4分钟爬到顶部，1分钟滑下来，一共花费5分钟。在15分钟内，她能滑3次，因为15÷5=3。因此
答案是3次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米花了5分钟（4分钟爬上去+1分钟滑下来），因此在15分钟内可以滑3次（15÷5=3）。答案是3次。&#x27;
从输出结果来看，显然受到了提示工程的正面影响，通过问题的拆解过程显著提高了回答的准确
性。
适合Chat Model的提示模版构造形式
我们在前面解释过LLMs和Chat Model 的区别，因其接受的输入形式不同，所以构造的提示模版也
是不一样的，对于Chat Model来说，它接收的是聊天形式的对话格式，所以对于其对应的Few-Shot，
在LangChain中的基本结构如下：
examples ：要包含在最终提示中的字典示例列表。
example_prompt ：通过其 format_messages 方法将每个示例转换为 1 条或多条消息。一个常
见的示例是将每个示例转换为一条人工消息和一条人工智能消息响应，或者一条人工消息后跟一条
函数调用消息。
我们还是使用四个数学推理题来进行测试：
-------- Page 42 --------
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
直接调用
对于Chat Model，需要将输入转化成对话聊天格式：
from langchain_core.messages import HumanMessage, SystemMessage
messages = [SystemMessage(content=&quot;你是一个擅长数学推理的专家&quot;),
HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟
后关闭，请问在关闭之前她能滑多少次？&quot;),]
实例化模型进行推理：
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
resonse = chat.invoke(messages)
resonse.content
-------- Page 43 --------
&#x27;首先，艾米花了1分钟滑下来，所以她每次滑下来后需要等待3分钟（4分钟上去 + 1分钟下来 = 5分钟，15
分钟总时间 - 5分钟 = 10分钟，10分钟 / 3分钟 = 3次）。\n\n在水滑梯关闭之前，艾米可以滑3次。&#x27;
&amp;esmp; 实际上，在使用GPT-3.5时，我们可以观察到，即使不采用Few-Shot提示，模型也能以很高
的概率正确回答问题，这归功于模型本身已经非常强大的能力。所以我们改为Baichuan2模型来测试：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟滑下来，所以她在滑梯关闭前可以滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟从滑梯顶部滑下来，所以她可以在滑梯关闭前滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，所以每次滑行需要4+1=5分钟。\n\n水滑梯将在
15分钟后关闭，所以她能在关闭之前滑行15÷5=3次。&#x27;
可以看到，在切换到Baichuan模型进行测试时，三次测试中出现了两次错误回答。因此，我们就
使用Baichuan2模型，尝试通过添加Few-Shot来探索是否能增强其推理能力。
首先，安装LangChain的规范，导入生成示例的模块：
-------- Page 44 --------
from langchain.prompts import (
ChatPromptTemplate,
FewShotChatMessagePromptTemplate,
)
接下来以input-output对的形式构建Few-Shot提示模板。在这里，“input”可以被视作人类发出的
Promot，而“output”则是大模型给出的回答。
examples = [
{&quot;input&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网
球？&quot;,
&quot;output&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总
共由5+6=11个网球。因此答案是11。&quot;},
{&quot;input&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共
有多少个苹果？&quot;,
&quot;output&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&quot;},
{&quot;input&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问
总共有多少个蓝色高尔夫球？&quot;,
&quot;output&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答
案是4个。&quot;},
]
然后，使用from_messages方法进行模版构造。
# This is a prompt template used to format each individual example.
example_prompt = ChatPromptTemplate.from_messages(
[
(&quot;human&quot;, &quot;{input}&quot;),
(&quot;ai&quot;, &quot;{output}&quot;),
]
)
few_shot_prompt = FewShotChatMessagePromptTemplate(
example_prompt=example_prompt,
examples=examples,
)
print(few_shot_prompt.format())
Human: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
AI: 罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网
球。因此答案是11。
Human: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹
果？
AI: 食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Human: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少
个蓝色高尔夫球？
AI: 总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
生成最终的输入模版，设置 input 作为动态变量：
-------- Page 45 --------
final_prompt = ChatPromptTemplate.from_messages(
[
few_shot_prompt,
(&quot;human&quot;, &quot;{input}&quot;),
]
)
final_prompt
ChatPromptTemplate(input_variables=[&#x27;input&#x27;], messages=
[FewShotChatMessagePromptTemplate(examples=[{&#x27;input&#x27;: &#x27;罗杰有五个网球，他又买了两盒网
球，每盒有3个网球，请问他现在总共有多少个网球？&#x27;, &#x27;output&#x27;: &#x27;罗杰一开始有五个网球，又购买了两
盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此答案是11。&#x27;}, {&#x27;input&#x27;:
&#x27;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？&#x27;,
&#x27;output&#x27;: &#x27;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&#x27;}, {&#x27;input&#x27;: &#x27;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。
请问总共有多少个蓝色高尔夫球？&#x27;, &#x27;output&#x27;: &#x27;总共有16个球，其中一半是高尔夫球，也就是8个，其中
一半是蓝色的，也就是4个，答案是4个。&#x27;}],
example_prompt=ChatPromptTemplate(input_variables=[&#x27;input&#x27;, &#x27;output&#x27;], messages=
[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;output&#x27;],
template=&#x27;{output}&#x27;))])),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;))])
实例化baichuan2模型：
最后，使用LangChain中的chain的抽象，合并最终的提示和大模型共同执行。
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-e9&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米需要4分钟才能爬上去，1分钟滑下来，总共需要4+1=5分钟。15分钟可以滑15÷5=3次，所以答案是3
次。&#x27;
-------- Page 46 --------
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米爬上去需要4分钟，滑下来只需要1分钟，所以她可以在15分钟内完成4次滑行，因为每次滑行需要
4+1=5分钟。&#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54695e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，然后1分钟滑下来，总共需要4+1=5分钟。因此，在滑梯关闭之前，她可
以滑15÷5=3次。&#x27;
4.3 LangChain中的Output Parsers
Output Parsers，即输出解析器，这个概念非常好理解，就是负责获取大模型的输出并将其转换为
更合适的格式。这在应用开发中及其重要。在大多数复杂应用场景中，处理逻辑往往环环相扣，执行某
项业务逻辑可能需要多次调用大模型，其中上一次的调用结果将被用于指导下一次调用的逻辑。在这种
情况下，结构化的信息会比纯文本又有价值，同时这也是输出解析器的价值所在。
LangChain构造的输出解释器必须实现两个主要方法：
Get format instructions：该方法会返回一个字符串，其中包含有关如何格式化语言模型输出的指
令。
Parse：该方法会接收字符串，并将其解析为某种结构
目前已经支持的解析格式已经包括Json、Xml、Csv以及OpenAI的Tools和Functions等多种格式，
具体可看：https://python.langchain.com/docs/modules/model_io/output_parsers/
-------- Page 47 --------
简而言之，这个过程本质上是对文本的格式化处理。但与传统的后处理不同，LangChain采取的实
现策略是通过在Prompt上增加输出解析，直接引导大模型输出的格式化方式。
我们以格式化日期来看一下LangChain的构造思路，首先导入相关的模块：
from langchain.output_parsers import DatetimeOutputParser
from langchain.prompts import PromptTemplate
from langchain_openai import OpenAI
实例化日期解析器：
output_parser = DatetimeOutputParser()
通过.get_format_instructions方法，可以查看定义的解析格式：
output_parser.get_format_instructions()
&quot;Write a datetime string that matches the following pattern: &#x27;%Y-%m-
%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1181-01-05T02:20:33.675873Z, 1938-12-
13T09:46:36.791229Z, 1468-01-17T07:34:51.108688Z\n\nReturn ONLY this string, no
other words!&quot;
构造输入模版，这里的区别是：在输入的Prompt Template中，加入了OutPut Parse的内容：
template = &quot;&quot;&quot;用户发起的提问:
{question}
{format_instructions}&quot;&quot;&quot;
生成最终实际输入大模型的Promot内容：
-------- Page 48 --------
prompt = PromptTemplate.from_template(
template,
# 预定义的变量，这里我们传入格式化指令
partial_variables={&quot;format_instructions&quot;:
output_parser.get_format_instructions()},
)
prompt
PromptTemplate(input_variables=[&#x27;question&#x27;], partial_variables=
{&#x27;format_instructions&#x27;: &quot;Write a datetime string that matches the following
pattern: &#x27;%Y-%m-%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1472-10-23T17:39:56.199188Z,
1643-01-14T10:55:11.903091Z, 0631-12-11T23:04:04.345333Z\n\nReturn ONLY this
string, no other words!&quot;}, template=&#x27;用户发起的提
问:\n\n{question}\n\n{format_instructions}&#x27;)
最后，使用LangChain中的chain的抽象，合并最终的提示、大模型实例及OutPut Parse共同执
行。
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请问你叫什么？&quot;})
print(output)
2021-10-07 12:02:13.345333
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是机器学习&quot;})
print(output)
2018-01-23 09:42:32.123456
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是深度学习&quot;})
print(output)
2019-08-07 18:30:00
-------- Page 49 --------
通过测试可以明显观察到，以此方式定义的输出解析器极大地规范了模型的输出。当然，这也直接
受到大模型实际能力和模板定义情况的影响。
LangChain中不同模版的使用方式，除了可以在官方文档上看到，在其源码上看会更加直观，而且
可以自定义修改，其所有的OutPut Parse的模版存储路径为：
langchain/libs/langchain/langchain/output_parsers ， 大家可以自行尝试。比如我们上面使
用的日期解析的源码对应如下：
到此为止，我们已经非常详尽地介绍了LangChain的核心概念。如果有学习过大模型技术实战课程
的同学来说，会发现LangChain的Model I/O模块中讨论的所有概念都是之前接触过的，需要学习的主
要是LangChain对大模型进行抽象的方法以及在LangChain框架下的应用开发技巧。
而从LangChain的整体开源情况看，其内容丰富且迭代非常快，通过模块化的方式介绍，可以帮助
大家快速构建起对LangChain以及应用开发的思维框架。尽管在Model I/O模块中还有许多更高级的内
容，例如如何接入本地部署的开源大模型，关于函数调用、Memory缓存等更深入的主题，这些内容我
们将在后续章节中一一接触并进行详细的探讨。
```

由于 PDF 文件通常包含丰富的数据，如文本和表格，且每个页面可能有不同的布局和内容类型，处理这些文件时通常需要逐页进行。这使得精细化处理具有一定的复杂性。在此情况下，为简化操作，我们暂不考虑特定格式的数据处理，而是选择将所有内容作为一个整体进行加载。

```
import pdfplumber
import os

def extract_all_text(pdf_path):
    &quot;&quot;&quot;从指定的PDF文件中提取所有页面的文本，并将其作为一个字符串返回。&quot;&quot;&quot;
    if not os.path.exists(pdf_path):
        return &quot;指定的文件不存在，请检查路径。&quot;
    
    all_text = &quot;&quot;  # 初始化一个空字符串，用于存储所有文本
    try:
        with pdfplumber.open(pdf_path) as pdf:
            for page in pdf.pages:
                page_text = page.extract_text()
                if page_text is not None:
                    all_text += page_text + &quot;\n&quot;
                else:
                    all_text += &quot;此页面没有可提取的文本。\n&quot;
    except Exception as e:
        return f&quot;处理 PDF 时出现错误：{e}&quot;
    
    return all_text
```

```
# 执行加载过程
extracted_text = extract_all_text(pdf_path)
print(extracted_text)
```

```
开源大模型应用开发实战
Ch 18 LangChain整体项目介绍与核心模块Model I/O详
解
注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情
况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，
将迅速更新教材内容以确保信息的准确性和实用性。
本期课件更新时间：2024年3月7日
在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地
私有化部署开源大模型。在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家
关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部
署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部
署。
但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也
是最简单的一步。熟练掌握基于大模型的上层应用开发还有非常长的路需要走。目前，大模型技术开发
总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API的应用开发，而另一类，就是先私有化
部署开源大模型，再借助开发框架构建大模型应用。当然了，目前主流的大模型应用开发框架也支持在
线模型API的调用，这正是我们之前一直强调学习掌握开源或在线API调用技巧的原因，因为这些方法和
技巧直接对应于应用开发的最佳实践。
而不论采用哪种开发形式，其基座和所依赖的，核心还是大模型本身。对于在线大模型，以
OpenAI为例，开发者需要配置科学上网的环境，在其官网创建 API Key，最后遵循API的使用说明去进
行应用配置。这一过程中涉及到的网络环境适配，数据隐私安全，成本管理及如何去定制化自己需求的
开发流程，往往都需要遵循官方提供的功能来实现。也正是因为这些复杂的因素使得尽管目前公认最强
的大语言模型GPT 4开放给开发者使用，还是会有非常多的公司和个人去拥抱开源，选择性能可能略逊
一筹的开源大模型，因为开源方案在某些方面提供了更多的灵活性和自主控制能力。
大家也能够发现，在我们的课程中每当涉及到大模型的私有化部署，我们都会向大家展示如何通过
命令行、网页端访问和API调用等三种方式与部署的大模型进行交互。此外，还通过text-generate-
webui这样的开源项目，这一切应用是帮助大家能够以最简单、最直接的方式与开源大模型进行对话，
了解大模型的运行模型。但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶
段肯定无法满足实际的需求。虽然目前的大模型基本都可以按照OpenAI的API规范形式进行调用，但如
果进行实操我们会发现，在《大模型技术实战》的课程中，借助OpenAI的GPT系列模型开发的项目，
如智能邮件处理、全自动AI开发流程，以及MateGen Agents等，尽管在开源模型上能够被复现，但这
背后的成功关键并非仅仅在于模型调用。例如，在《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进
行外部工具API调用开发实践》中，我们通过Qwen模型复现了《Ch.10 借助Function calling调用外部
工具API方法》中的天气信息调用过程。这一复现过程揭示了，对在线模型来说，我们需要学习的是其
API使用方法。而在开源大模型的技术领域下，要想实现某些特定功能，重点在于理解该模型的一些使
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
代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高
的。LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架。它的做法是：通过提
供标准化且丰富的模块抽象，构建大语言模型的输入输入规范，利用其核心概念chains，灵活地连接
整个应用开发流程。而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出
来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发
领域内被普遍高度认可的通用框架。
为什么需要这样做？
首先，我们需考虑当前大模型的发展态势。尽管OpenAI的GPT系列模型作为大模型领域的领军人
物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大
模型间的使用方式完全相同。例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不
适用。因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是
十分低下的。
其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据
连接方式以及输出结果的不稳定性等问题。在应用开发中，如何找到这些问题的有效解决策略？
上述提到的每个限制都紧密关联于大模型本身的特性。尽管理论上可以通过重新训练、微调来增强
模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资
源、时间和财力，选择这条路径一定会导致方向越来越偏离目标。我们之前讨论的OpenAI的Function
Calling和通过ReAct流程接入第三方API能够提供一些解决方案，但这每一步都需大量的研发投入，而且
最终实现后的应用效果，也取决于研发人员的个人技术能力。在这种背景下，既然大家都有不同的想法
和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的
快速搭建，这就是LangChain一直想要做到，且正在做的事情。
LangChain的做法
从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对
大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接
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
记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的&quot;Chain&quot;高级抽象，定义了
不同形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整
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
是通过该模块，我们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过
程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API
调用规范：
response = openai.ChatCompletion.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是机器学习？&quot;}
]
)
而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话
交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象
出一个chain，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核
心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和
Output Parser（对应下图中的Parse部分）。
Format：即指代Prompts Template，通过模板化来管理大模型的输入；
Predict：即指代Models，使用通用接口调用不同的大语言模型；
Parse：即指代Output部分，用来从模型的推理中提取信息，并按照预先设定好的模版来规范化
输出。
整个Model I/O工作流如下图所示：
Format
对于Prompt Template第一部分，传统上我们创建提示词是通过手工编写来实现的，在这个过程中
会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能。然而，
在应用开发中，一个关键的考量是提示词不能是一成不变的。其原因在于，应用开发需要适应多变的用
户需求和场景。固定的提示词限制了模型的灵活性和适用范围。例如，如果我们正在开发一个天气查询
应用，用户可能会以多种方式提出查询，如“今天的天气怎么样？”或“明天纽约的温度是多少度？”。如
果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求。
而Prompt Template，就像我们在介绍《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进行外
部工具API调用开发实践》中不断优化出来的ReAct模版，将API的使用、问题解答过程等复杂逻辑封装
成了一套结构化的格式。我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引
导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：
# 将一个插件的关键信息拼接成一段文本的模版。
TOOL_DESC = &quot;&quot;&quot;{name_for_model}: Call this tool to interact with the
{name_for_human} API. What is the {name_for_human} API useful for?
{description_for_model} Parameters: {parameters}&quot;&quot;&quot;
# ReAct prompting 的 instruction 模版，将包含插件的详细信息。
PROMPT_REACT = &quot;&quot;&quot;Answer the following questions as best you can. You have
access to the following APIs:
{tool_descs}
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can be repeated zero or more
times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question
Begin!
Question: {query}&quot;&quot;&quot;
因此，引入Prompt Template可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输
入动态调整提示词，从而更好地响应用户的具体需求。LangChain通过这种方式来提高应用的通用性和
用户体验。
Predict
在Predict部分，实质上是处理模型从接收输入到执行推理的整个过程。考虑到存在两种主要类型的
大模型——Base类模型和Chat类模型，LangChain在其Model I/O模块中对这两种模型都进行了抽象，
分别归类为LLMs（Large Language Models）和Chat Models。我们还是以OpenAI 的 Completion 和
Chatcompletions方法为例：
# Base类模型
client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=&quot;Say this is a test&quot;,
)
# 聊天模型
client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
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
整体而言，在Model I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如
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
伙伴包，安装此依赖包后，LangChain才能够与OpenAI的API进行交互。后续我们在使用相关功能的时
候，会提供额外的说明。
LangChain安装官方说明文档：https://python.langchain.com/docs/get_started/installation
3.1 使用包版本管理工具安装
LangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建
过程。这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成
LangChain的安装。具体操作如下：
! pip install langchain
Requirement already satisfied: langchain in d:\software\anaconda3\lib\site-
packages (0.1.10)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain) (6.0)
Requirement already satisfied: SQLAlchemy&lt;3,&gt;=1.4 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.4.39)
Requirement already satisfied: aiohttp&lt;4.0.0,&gt;=3.8.3 in
d:\software\anaconda3\lib\site-packages (from langchain) (3.8.5)
Requirement already satisfied: dataclasses-json&lt;0.7,&gt;=0.5.7 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.6.4)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.33)
Requirement already satisfied: langchain-community&lt;0.1,&gt;=0.0.25 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.25)
Requirement already satisfied: langchain-core&lt;0.2,&gt;=0.1.28 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.28)
Requirement already satisfied: langchain-text-splitters&lt;0.1,&gt;=0.0.1 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.1)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.14)
Requirement already satisfied: numpy&lt;2,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (1.24.3)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (8.2.2)
Requirement already satisfied: attrs&gt;=17.3.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (22.1.0)
Requirement already satisfied: charset-normalizer&lt;4.0,&gt;=2.0 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(2.0.4)
Requirement already satisfied: multidict&lt;7.0,&gt;=4.5 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(6.0.2)
Requirement already satisfied: async-timeout&lt;5.0,&gt;=4.0.0a3 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(4.0.2)
Requirement already satisfied: yarl&lt;2.0,&gt;=1.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (1.8.1)
Requirement already satisfied: frozenlist&gt;=1.1.1 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.3.3)
Requirement already satisfied: aiosignal&gt;=1.1.2 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.2.0)
Requirement already satisfied: marshmallow&lt;4.0.0,&gt;=3.18.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (3.21.0)
Requirement already satisfied: typing-inspect&lt;1,&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (0.9.0)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain)
(2.1)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (3.5.0)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2,&gt;=0.1.28-
&gt;langchain) (23.2)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain)
(2.16.1)
Requirement already satisfied: typing-extensions&gt;=4.6.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (4.7.1)
Requirement already satisfied: idna&lt;4,&gt;=2.5 in d:\software\anaconda3\lib\site-
packages (from requests&lt;3,&gt;=2-&gt;langchain) (3.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(1.26.16)
Requirement already satisfied: certifi&gt;=2017.4.17 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(2023.11.17)
Requirement already satisfied: greenlet!=0.4.17 in
d:\software\anaconda3\lib\site-packages (from SQLAlchemy&lt;3,&gt;=1.4-&gt;langchain)
(2.0.1)
Requirement already satisfied: sniffio&gt;=1.1 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (1.2.0)
Requirement already satisfied: mypy-extensions&gt;=0.3.0 in
d:\software\anaconda3\lib\site-packages (from typing-inspect&lt;1,&gt;=0.4.0-
&gt;dataclasses-json&lt;0.7,&gt;=0.5.7-&gt;langchain) (1.0.0)
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：
import langchain
print(langchain.__version__)
0.1.10
如果能正常输出LangChain的版本，说明在当前环境下的安装成功。
3.2 源码安装
除了通过pip安装外，还有一种通过源码安装的方法。这需要使用git拉取远程仓库，然后进入项目
文件夹并执行pip install -e .命令。这种方法不仅会安装必要的依赖，同时也将程序的源代码保存
在本地。通常来说，除非大家打算作为LangChain的协作开发者参与到项目的功能更新中，否则没必要
采用这种安装方式，直接使用pip安装更为简便。
对于课程学习而言，我们推荐采用源码安装方式，这将非常有助于在后续的LangChain功能探索
中，通过源码分析深入理解框架的构建原理和详细机制。
接下来我们以Ubuntu操作系统为例，介绍源码安装LangChain的详细步骤：
Step 1. 安装Anaconda
如果本地服务器是Ubuntu系统，请参考：《Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B
模型》中的2.3 安装Anaconda环境小节；而如果是Windows操作系统，请参考：《Ch.6 在Windows
系统下部署运行ChatGLM3-6B模型》中的3.2.1 方式一：使用Anaconda创建项目依赖环境（推荐）小节
中，执行完Step 6. 验证conda步骤，回到本文继续执行后续的操作：
注：如果部署环境是租赁的云服务器，Anaconda环境已经预设好，并不需要我们再次手动配置。
按照对应的教程内容配置好Anaconda后，可以通过如下命令验证当前环境的Conda：
conda --version
如果可以正常输出Conda的版本，则说明Anaconda安装成功。
Step 2. 使用Conda创建LangChain的Python虚拟环境
安装好Anaconda后，我们需要借助Conda包版本工具，为LangChain项目创建一个新的Python虚
拟运行环境，执行代码如下：
conda create --name langchain python==3.11
如上所示，新创建了一个名为langchain的Python虚拟环境，其Python版本为3.11。创建完成后，
通过如下命令进入该虚拟环境，执行后续的操作：
conda activate langchain
Step 3. 下载LangChain的项目文件
进入LangChain的官方Github，地址：https://github.com/langchain-ai/langchain ， 在 GitHub
上将项目文件下载到有两种方式：克隆 (Clone) 和 下载 ZIP 压缩包。推荐使用克隆 (Clone)的方式。我
们首先在GitHub上找到其仓库的URL。
在执行命令之前，需要先安装git软件包，执行命令如下：
sudo apt install git
执行克隆命令，将LangChain Github上的项目文件下载至本地的当前路径下，如下：
git clone https://github.com/langchain-ai/langchain.git
Step 4. 升级pip版本
建议在执行项目的依赖安装之前升级 pip 的版本，如果使用的是旧版本的 pip，可能无法安装一些
最新的包，或者可能无法正确解析依赖关系。升级 pip 很简单，只需要运行命令如下命令：
python -m pip install --upgrade pip
Step 5. 源码安装项目依赖
不同于我们之前一直使用的pip install -r requirements.txt方式，这种方法用于批量安装多
个依赖包，是在部署项目或确保开发环境与其他开发者/环境一致时的常用方式。而对于LangChain，我
们需要使用pip install -e的方式，以可编辑模式安装包。这种方式主要用于开发过程中。当以可编
辑模式安装一个包时，依赖包会被直接从源代码所在位置安装，而不是复制到Python的site-packages
目录，是开发模式下用于安装并实时反映对本地包更改的方法。需要执行的步骤如下：
cd langchain/libs/langchain/
pip install -e .
如在安装过程未发生任何报错，则说明安装成功。在安装完依赖后，我们就正式进入LangChain的
Model I/O模块的实践。
注：如果采用的是源码安装，需要参考《Ch.11 大模型开发工具Transformers库的使用及配置大
模型开发环境》中远程服务器部署大模型，本地Jupyter NoteBook加载小节，在LangChain的虚拟
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
LangChain 接入 OpenAI 的 GPT 模型，我们需要在LangChain框架下先定义相关的类和方法来规定如
何与模型进行交互，包括数据的输入和输出格式以及如何连接到模型本身。然后按照 OpenAI GPT 模型
的接口规范来集成这些功能。通过这种方式，LangChain 充当一个桥梁，使我们能够按照统一的标准来
接入和使用多种不同的大语言模型。
在LangChain的Model I/O模块中，集成了两类模型，分别是LLMs和聊天模型，这两种模型在输
入和输出的数据类型上有所区别。
接下来我们就以OpenAI的API为示例，分别实践一下在LangChain框架中如何使用这两种不同类型
的大语言模型。首先来看LLMs类模型。
4.1.1 LLMs类模型接入方法
LLMs指的是纯文本补全模型，其封装的 API 输入接收的是字符串，输出也是字符串。 OpenAI 的
GPT-3 是作为 LLM 实现的。最简单的理解：LLMs指代的就是Base类模型，这类模型仅仅可以理解并
生成自然语言或代码，并未按照特定指令进行训练。截止到2024年3月07日，OpenAI可支持调用的
Base类模型仅有2个，分别是babbage-002和davinci-002，除此之外，还可以使用gpt-3.5-turbo-
instruct调用OpenAI 的 Completion方法。
首先需要安装OpenAI的集成依赖包langchain-openai，执行如下命令：
! pip install langchain-openai
Requirement already satisfied: langchain-openai in
d:\software\anaconda3\lib\site-packages (0.0.8)
Requirement already satisfied: langchain-core&lt;0.2.0,&gt;=0.1.27 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.1.28)
Requirement already satisfied: openai&lt;2.0.0,&gt;=1.10.0 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (1.13.3)
Requirement already satisfied: tiktoken&lt;1,&gt;=0.5.2 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.6.0)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (6.0)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.5.0)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (1.33)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (0.1.14)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (23.2)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (8.2.2)
Requirement already satisfied: distro&lt;2,&gt;=1.7.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (1.8.0)
Requirement already satisfied: httpx&lt;1,&gt;=0.23.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (0.25.1)
Requirement already satisfied: sniffio in d:\software\anaconda3\lib\site-
packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (1.2.0)
Requirement already satisfied: tqdm&gt;4 in d:\software\anaconda3\lib\site-packages
(from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (4.65.0)
Requirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (4.7.1)
Requirement already satisfied: regex&gt;=2022.1.18 in
d:\software\anaconda3\lib\site-packages (from tiktoken&lt;1,&gt;=0.5.2-&gt;langchain-
openai) (2022.7.9)
Requirement already satisfied: idna&gt;=2.8 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai)
(3.4)
Requirement already satisfied: certifi in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(2023.11.17)
Requirement already satisfied: httpcore in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(1.0.1)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.1)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.16.1)
Requirement already satisfied: charset-normalizer&lt;4,&gt;=2 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.0.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (1.26.16)
Requirement already satisfied: colorama in d:\software\anaconda3\lib\site-
packages (from tqdm&gt;4-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.4.6)
Requirement already satisfied: h11&lt;0.15,&gt;=0.13 in
d:\software\anaconda3\lib\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-
&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.14.0)
使用OpenAI的 GPT 系列模型的API有两个前提条件：首先，需要配置科学上网以确保可以访问
OpenAI服务；其次，必须拥有一个有效的API Key。这两项要求的详细配置在《Ch.1 本地调用OpenAI
API流程及OpenAI官网使用指南》中有详细说明，因此我们在此不再赘述，直接进入实操过程。
查看OpenAI的依赖包版本
OpenAI 的 API 在 0.28 版本和 1.xx 版本之间的调用方式存在差异。我们这里使用的是最新的调用
方法，即采用 1.13.XX 版本。因此，需要确认当前 Python 虚拟环境中安装的 openai 包版本是 1.xx。
可以通过执行以下命令来检查当前安装的版本：
import openai
print(openai.__version__)
0.28.0
如果如上显示是0.28.0版本，执行如下命令将openai包升级到最新版本：
# ! pip install --upgrade openai
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
import openai
print(openai.__version__)
1.13.3
设置科学上网环境
根据所使用的科学上网软件，查找相应的代理端口，并按照以下代码进行网络环境配置：
# 这里的端口，请修改为实际使用科学上网软件配置的代理端口
!set HTTP_PROXY=http://127.0.0.1:15732
!set HTTPS_PROXY=http://127.0.0.1:15732
新版本的OpenAI API调用方法不允许使用openai.api_key= &quot;sk-xxx&quot;这种老版本的显式指定API
Key，所以如果没有在系统变量中进行配置的话，使用下面的命令设置环境变量：
# 将your- API -key-here替换为实际的API密钥:
# ! setx OPENAI_API_KEY &quot;sk-xxxx&quot;
成功: 指定的值已得到保存。
注意：完成环境变量的初次配置后，需重启电脑以激活设置。这样，在下次使用时就可以直接调用
配置好的环境。为验证环境变量中的API Key是否已正确设置为自己的，可以执行以下命令进行检查。
# ! echo %OPENAI_API_KEY%
完成了科学上网环境和API Key的设置后，在接入LangChain之前，我们先使用如下测试代码检查
当前环境及API的运行状态是否正常，执行代码如下：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
]
)
print(completion.choices[0].message.content)
你好，我是一位可以回答问题、提供信息和与您交流的AI助手。我可以帮助解决问题，回答疑惑，提供建议，
而且乐意与您分享知识和信息。有什么我可以帮助你的吗？
若测试代码能正常输出结果，表明OpenAI的环境配置无误。需要注意的是，LangChain接入
OpenAI GPT模型只是利用其定义的标准模型接入框架来整合OpenAI API，因此进行上述连通性测试
是必要的。
OpenAI GPT Base模型官方说明：https://platform.openai.com/docs/models/moderation
LangChain为了使开发者可以轻松地创建自定义链，整体采用Runnable协议。Runnable 协议是
编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可
以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了
Runnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。
LangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch
ain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable
而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解
的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。
如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI：
LangChain Interface说明：https://python.langchain.com/docs/modules/model_io/llms/quic
k_start
LangChain已集成LLMs说明：https://python.langchain.com/docs/integrations/llms/
首先，我们来了解常规的调用方法。具体支持的调用方式如下所示：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
接下来我们对上述方法依次进行尝试。在LangChain框架中，为了规范地接入LLMs类型的大模
型，首先需要导入OpenAI的依赖包。导入命令如下：
from langchain_openai import OpenAI
字符串形式的输入
调用invoke方法，并提供字符串格式的Prompt，便可触发指定模型按照所给Prompt进行续写操
作。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
llm.invoke(
&quot;你好呀，请你介绍一下你自己&quot;
)
&#x27;吧。”韩寒摇头，说：“我下次再来，你们自己给我介绍好了。”这一次，韩寒的风趣让陈航漂了一下，他说：
“还是先看看大家的房间吧。”陈航带着韩寒到了楼下，这时连小李都已经来到了楼下，他对韩寒说：“你看，
这是我们的宿舍。”陈航的语气是那么的平淡，既没有过多的炫耀，也没有明显的拒绝，只是那么的平淡，这
让韩寒很有点奇怪，他说：“你们是在这里住吗？”小李说：“是啊，我们是这里的住宿生。”韩寒对小李说：
“那我是来参加考试的，你&#x27;
从上述模型的返回输出，我们可以看到它本质上作为一个补全模型，通过使用大模型原生的
.generate() 方法来对输入的Prompt进行续写。这部分的详细内容已在《Ch.2 Completions模型API
使用指南》中进行了深入讲解，因此此处不再进行赘述。
上述调用方法相当于使用我们熟悉的OpenAI的Completion方法来调用GPT Base类型模型的过
程：
from openai import OpenAI
client = OpenAI()
completion = client.completions.create(
model=&quot;davinci-002&quot;,
prompt=&quot;你好呀，请你介绍一下你自己&quot;,
max_tokens=1024,
)
print(completion.choices[0].text)
。» 她把咳嗽调整成百米冲刺那种负重颤抖。»不，我们在上面，请你告诉会场。哪一地方过去你都要做朋友
呢？」Larsen一转眼注意到卡萨姆旁站着一位古老的男人。他身着粗布制服，最左边有一种圆盏形的供奶
头。透映着那个老的质朴魅力和吓坏人恶心的样子。»LarsenKeberichTaylor.Ch02 3/5/07 5:27 PM
Page 39LEARNING WHERE WECAME FROM»喂主持人。自从我给你们滴剂的时候，我就耐心等待了。请为
快点谈到刚才提的内容。»正赞成的木质松枝变苍白了。既然他是最耐烦的老爷，我们劝劝他这样三小时也
烦。时光久了会累人的人，他更应该知道这些。»好，每个人都听到。»那些选手的名字有老项目拯救人」，
Brickman说。AnderMartin、Taylor, här nere – efter en minut eller så. Likdant för
LarsenKeberich, rätt så bra ger du tillbaka vätsken, yeah, and the eighth
man.»»Eighth man?」Brickman坏坏垂下眼帘扬扬眉头，头盔上的唇影随着他挺起了脸颊。好，一位是
我。不，我不是。Just a moment.»六记地雷爆炸。嫣红的色泽弥漫在鼻子周围极其耀眼之下，淀粉饼糊塞
了全脸，剔饭一般的巧克力混合在一起。»»Would you all please proceed into the next
building and make sure that you use the turnstiles.»伤口破口流血，簇瓣的血色俗套，但去
还是要用。套房好美啊、清生。套房饲養下只要一个；_ 户功能装置就。»艰难地活着足以获得一天的欢乐的
人确实算够奇特的。咳嗽的木质松枝_cancel6 3/5/07 5:27 PM Page 40IT IS NOT ENOUGH
TOMAKE MANKINDHAPPY,LARSEN KEBERICH向报纸走来，她在走廊哪一个厅卡萨姆等待着;在这个保证已
经到达那种别有心碎的正式屋子里。她穿过镶有玫瑰花的拉链的抢洗衣区和单人舔饭的厕所区圆朝圣地的道。
先是葡萄干牛油，透过臭风从供应箱中飘来，刚开始时她便一直寻找那一种苏查尔的酸味。那种味美常是发出
着从薄饼里流发出来的所以一圈难找到对不起，小姐，我们之前选择做了决定，就算更改好一个时刻，也无法
改变决定。拨动键盘-找麻烦你的，怎么做了、哦，听起来好像你一突然卸了山猫带来了 Asktohell你疼
吗。杨虎大吼着，冲向对方。丁
流式输出
要进行流式调用，可以调用stream方法。这种方式允许在接收到模型响应的同时，逐块输出结
果，实现即时展示而无需等待全部内容处理完毕。
from langchain_openai import OpenAI
for chunk in llm.stream(
&quot;你好呀，请你介绍一下你自己&quot;
):
print(chunk, end=&quot;&quot;, flush=True)
吧，你是谁，你在哪里，你是什么学问，你喜欢什么，你不喜欢什么，你不会什么，你会什么，你梦想什么，
你的优点是什么，你的缺点是什么，你的性格是什么，你的兴趣是什么，你喜欢的食物是什么，你不喜欢的食
物是什么，你的爱好是什么，你的愿望是什么。介绍一下你自己好的。
[ 编辑 xiaofan1 在 07-03-18 23:49 ]
批量生成
通过调用batch方法，可以同时对多个不同的Prompt进行调用处理。在这个例子中，输入包括“你
好”、“什么是机器学习”和“什么是深度学习”三个独立的Prompt。这种批量处理方式允许一次性提交多个
Prompt，从而提高推理效率。
from langchain_openai import OpenAI
llm.batch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，习惯了你的成熟，可是这次你的决定让我感到非常惊讶，我不知道你的这次决定是不是真的，我不知道你
是否真的那么爱我，虽然我知道你非常爱我，可是我不知道你是不是真的爱我，假如你为了我不想去上学，我
也可以放弃一切，去读书，去上大学，但是你为什么要这样做呢，我不清楚你的想法，我不知道你的想法，我
只知道我爱你，我只知道我不想失去你，我只知道我不想让你失去你的希望，我只知道我不想让你失去你的未
来，可是我想知道你为什么要这样做呢，我想知道你的想法，你是&#x27;,
&#x27; 机器学习的原理 机器学习的技术 机器学习的应用机器学习在生产过程中的作用机器学习在生产过程中的
作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产
过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学
习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作
用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学&#x27;,
&#x27;？ 2. Why is the industry so interested in deep learning? 3. How will deep
learning transform the financial industry? 4. How can you use deep learning to
build better AI? 5. What are the ingredients of a deep learning model?\n\nA
study of the best deep learning algorithms and how they work, the techniques
involved in building them, and the results they can bring to the table: 1.
Unsupervised learning 2. Convolutional Neural Networks (CNN) 3. Recurrent Neural
Networks (RNN) 4. Deep Reinforcement Learning (RL) 5. Generative Adversarial
Networks (GAN)\n\nA step-by-step guide to building a real-world, deep learning
app: 1. What is a deep learning app? 2. How do you build a deep learning app? 3.
The various components involved in building a deep learning app 4. How do you
deploy a deep learning app? 5. The future of deep learning apps\n\nHow to build
a deep learning app: A guide to the various tools, frameworks, and libraries
that you can use to build deep learning apps: 1. Deep Learning Frameworks 2.
Deep Learning Libraries 3. Deep Learning Tools 4&#x27;]
异步调用
llm.invoke(...)本质上是一个同步调用。在这种情况下，程序会在调用返回结果之前停止执行
任何后续代码。这意味着如果invoke操作耗时较长，它会导致程序暂时挂起，直到操作完成。我们可
以通过这样一个测试代码来直观的理解同步调用：
import time
def call_model():
# 模拟同步API调用
print(&quot;开始调用模型...&quot;)
time.sleep(3) # 模拟调用等待
print(&quot;模型调用完成。&quot;)
def perform_other_tasks():
# 模拟执行其他任务
for i in range(3):
print(f&quot;执行其他任务 {i + 1}&quot;)
time.sleep(1)
def main():
start_time = time.time()
call_model()
perform_other_tasks()
end_time = time.time()
total_time = end_time - start_time
return f&quot;总共耗时：{total_time}秒&quot;
# 运行同步任务并打印完成时间
main_time = main()
main_time
开始调用模型...
模型调用完成。
执行其他任务 1
执行其他任务 2
执行其他任务 3
&#x27;总共耗时：6.001835107803345秒&#x27;
这段同步调用的程序先模拟了一个耗时3秒的模型调用，随后执行了三个其他任务，每个任务耗时1
秒。实际的执行时间为约6.00秒。这体现了同步执行的特点：每个操作依次执行，直到当前操作完成后
才开始下一个操作，从而导致总的执行时间是各个操作时间的总和。
而异步调用，允许程序在等待某些操作完成时继续执行其他任务，而不是阻塞等待。这在处理I/O
操作（如网络请求、文件读写等）时特别有用，可以显著提高程序的效率和响应性。
我们可以设计这样如下情景：使用async和await关键字， 通过async def定义异步函数
async_call和perform_other_tasks。await asyncio.sleep(3)模拟了一个耗时3秒的异步操
作。这里的await表明程序在这里“等待”这个操作完成，但与同步操作不同，这个等待是非阻塞的。这
意味着程序可以在这3秒内执行其他异步代码。这种方式不会阻塞整个程序的运行，而是让出控制权，
允许事件循环继续运行其他任务。
! pip install asyncio
Collecting asyncio
Obtaining dependency information for asyncio from
https://files.pythonhosted.org/packages/22/74/07679c5b9f98a7cb0fc147b1ef1cc1853b
c07a4eb9cb5731e24732c5f773/asyncio-3.4.3-py3-none-any.whl.metadata
Downloading asyncio-3.4.3-py3-none-any.whl.metadata (1.7 kB)
Downloading asyncio-3.4.3-py3-none-any.whl (101 kB)
---------------------------------------- 0.0/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
----------- --------------------------- 30.7/101.8 kB 163.8 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
----------------------- --------------- 61.4/101.8 kB 113.0 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
-------------------------------------- 101.8/101.8 kB 136.2 kB/s eta 0:00:00
Installing collected packages: asyncio
Successfully installed asyncio-3.4.3
import asyncio
import time
async def async_call(llm):
await asyncio.sleep(3) # 模拟异步操作
print(&quot;异步调用完成&quot;)
async def perform_other_tasks():
await asyncio.sleep(3) # 模拟异步操作
print(&quot;其他任务完成&quot;)
async def run_async_tasks():
start_time = time.time()
await asyncio.gather(
async_call(None), # 示例调用，替换None为模拟的LLM对象
perform_other_tasks()
)
end_time = time.time()
return f&quot;总共耗时：{end_time - start_time}秒&quot;
# 运行异步任务并打印完成时间
await run_async_tasks()
异步调用完成
其他任务完成
&#x27;总共耗时：3.009281873703003秒&#x27;
使用asyncio.gather()并行执行时，理想情况下，因为两个任务几乎同时开始，它们的执行时间
将重叠。如果两个任务的执行时间相同（这里都是3秒），那么总执行时间应该接近单个任务的执行时
间（3秒左右），而不是两者时间之和。
理解了同步调用和异步调用的区别后，我们接下来看一下LangChain中集成的异步调用链使用方
法。
要实现异步调用并优化处理多个请求的场景，在LangChain中，可以使用ainvoke方法来达成。这
种方法允许同时启动多个请求，而不必等待每个请求依次完成。这对于提高应用程序的响应性和效率尤
其有用，特别是在处理需要与外部服务交互的任务时。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;这次您知道了吗？这是鸡肋大战，您是来玩的吧，我也是。我来纠结你的，看看你敢不敢。我一般不来这种
鸡肋大战，因为我很懒，这次我还是来了，因为我懒惯了，其实我很想快点解脱了，我很想走。你的目的在哪
里？我想敌对你，我还是要意识到了，你想不想敌对我呢？你敢不敢这样问？我不敢，因为我懒。你的目的是
什么？我很想快点解脱，我不想你，我只想快点走，早点走。你敢不敢这样说？我敢，因为我懒。你的目的是
什么？我&#x27;
异步流式响应
from langchain_openai import OpenAI
async for chunk in llm.astream(
&quot;你好，&quot;,
):
print(chunk, end=&quot;&quot;, flush=True)
我是来投诉的 [Image]
你现在没有抱怨，说的确实很有道理，如果他们为了你不让你上，你能不上吗，我想大多数人应该会上，所以
他们是不会因为你不上而阻止你上的，但是如果你上了，你的优惠和你的一切都是不可能了，你上了，他们是
不能为你划上你的学费的，所以我觉得就算你上了也没有用，他们不会为你划学费，因为你上了从而给了他们
非常大的麻烦，你上了，他们是不能有一点好处的，他们还要为你上的学费等等付出，所以他们会说你上不
了，就是为了不让你上，不让你上，他们不会为你划学费，所以我觉得你如果上了，你就没有任何
异步批量处理
from langchain_openai import OpenAI
await llm.abatch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，前来亲自拜访的，因为我们这里很少有人来拜访，你就打算在这里待上几个礼拜了吧？”“我是回来找你，
我是到桐谷这里来的。”李晓婷冷冷的回道。这一次很明显的不是她想让他留下来的，而是让他走了。众人看
着李晓婷，这里的她就这么冷了，竟然连郑晓东都不看一眼，郑晓东是她的男朋友，他们是一对，她怎么可能
不看一眼？“那你为什么不和我说，你的事情怎么样了？”郑晓东看向李晓婷的脸，她的表情很冷，她怎么不看
一眼自己&#x27;,
&#x27;？\n\n2:王景仪 2021-01-07 23:12:57\n\nannounced\n\n3:李桂林 2021-01-01
23:12:57\n\nMy Sunshine is a TV drama adapted from Gu Man’s novel of the same
title. It stars Wallace Huo and Tang Yan, premiering in January, 2015. The drama
depicts a man and a woman’s sweet love story.\n\n4:王威 2020-12-27
23:12:57\n\nThe new data released yesterday represent the first snapshot of
global trade for 2015. And the figures suggest that 2015 was not a good year for
world trade, as it had been hoped.\n\n5:苏晓波 2021-01-01 23:12:57\n\nMark Rutte,
the Dutch prime minister whose People’s Party for Freedom and Democracy (VVD) is
slightly ahead of the PVV, has seen his poll numbers rise in recent days because
of his hardline stance with Ankara. On Tuesday he called Mr Erdogan’s comments
“a disgusting distortion of history”, adding: “We will not lower ourselves to
this level. It is totally unacceptable.”\n\n&#x27;,
&#x27;？深度学习是一种模仿大脑思维的机器学习方法，深度学习方法被广泛用于图像识别、语音识别、自然语言
处理等领域。深度学习过程中，模型经常需要大量的训练数据，因此大量的数据获取和数据刷新是有必要。.
1) Find a free server to host the application in. I have been trying to create a
simple neural network by using the TensorFlow Object Detection API to run an
object detection model. Cloud TPU is a Google Cloud service that accelerates
machine learning workloads. Major cloud providers offer free tier of services to
help you get started. Google Cloud Storage is a RESTful web service for storing
and accessing data on Google Cloud Platform. In this video, you will learn about
Cloud TPU, a high-performance, scalable, and cost-effective machine learning
service. This IP address 157. 0, a neural network framework. Contribute.
TensorFlow Lite for mobile and embedded devices For Production TensorFlow
Extended for end-to-end ML components. The Google Cloud Platform&#x27;]
除此之外，在Jupyter Notebook或Python交互式解释器中进行操作的话，可以使用?来查看关于
llm中内置的方法和帮助文档，执行如下代码：
llm?
•[1;31mSignature:•[0m
•[0mllm•[0m•[1;33m(•[0m•[1;33m
•[0m •[0mprompt•[0m•[1;33m:•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mstop•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mcallbacks•[0m•[1;33m:•[0m •[1;34m&#x27;Callbacks&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m*•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mtags•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mmetadata•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[Dict[str, Any]]&#x27;•[0m
•[1;33m=•[0m •[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m**•[0m•[0mkwargs•[0m•[1;33m:•[0m
•[1;34m&#x27;Any&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m•[1;33m)•[0m •[1;33m-&gt;•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m•[0m•[1;33m•[0m•[0m
•[1;31mType:•[0m OpenAI
•[1;31mString form:•[0m
•[1mOpenAI•[0m
Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {},
&#x27;max_tokens&#x27;: 256}
•[1;31mFile:•[0m d:\software\anaconda3\lib\site-
packages\langchain_openai\llms\base.py
•[1;31mDocstring:•[0m
OpenAI large language models.
To use, you should have the ``openai`` python package installed, and the
environment variable ``OPENAI_API_KEY`` set with your API key.
Any parameters that are valid to be passed to the openai.create call can be
passed
in, even if not explicitly saved on this class.
Example:
.. code-block:: python
from langchain_community.llms import OpenAI
openai = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
•[1;31mInit docstring:•[0m
Create a new model by parsing and validating input data from keyword arguments.
Raises ValidationError if the input data cannot be parsed to form a valid model.
•[1;31mCall docstring:•[0m
[*Deprecated*] Check Cache and run the LLM on the given prompt and input.
Notes
-----
.. deprecated:: 0.1.7
Use invoke instead.
这里能看到非常多的关键信息，比如：
默认参数：Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {}, &#x27;max_tokens&#x27;: 256}
当不输入model_name时，默认会调用davinci-002模型：
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;我是小影。很高兴认识你，有什么可以帮到你的吗？&#x27;
同时，也可以调整温度temperature:
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
temperature=0
)
&#x27;我是一个人工智能助手，很高兴认识你！有什么可以帮到你的吗？&#x27;
model：必选参数，具体调用的Completions模型名称，可以调用的模型包括text-davinci-003、
text-davinci-002、text-curie-001、text-babbage-001、text-ada-001等，不同模型参数规模不
同；这里需要注意，大模型领域不同于机器学习领域，后者哪怕是简单模型在某些场景下可能也会
拥有比复杂模型更好的表现。在大模型领域，（就OpenAI提供的A、B、C、D四大模型来看）参
数规模越大、越新版本的模型效果更好（当然费用也更高），因此课程中主要以text-davinci-003
使用为例进行讲解；
prompt：必选参数，提示词；
suffix：可选参数，默认为空，具体指模型返回结果的后缀；
max_tokens：可选参数，默认为16，代表返回结果的token数量；
temperature：可选参数，取值范围为0-2，默认值为1。参数代表采样温度，数值越小，则模型会
倾向于选择概率较高的词汇，生成的文本会更加保守；而当temperature值较高时，模型会更多地
选择概率较低的词汇，生成的文本会更加多样；
top_p：可选参数，取值范围为0-1，默认值为1，和temperature作用类似，用于控制输出文本的
随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节
文本随机性，top_p和temperature两个参数选择一个进行调整即可；这里更推荐使用
temperature参数进行文本随机性调整；
n：可选参数，默认值为1，表示一个提示返回几个Completion；
stream：可选参数，默认值为False，表示回复响应的方式，当为False时，模型会等待返回结果
全部生成后一次性返回全部结果，而为True时，则会逐个字进行返回；
logprobs：可选参数，默认为null，该参数用于指定模型返回前N个概率最高的token及其对数概
率。例如，如果logprobs设为10，那么对于生成的每个token，API会返回模型预测的前10个
token及其对数概率；
echo：可选参数，默认为False，该参数用于控制模型是否应该简单地复述用户的输入。如果设为
True，模型的响应会尽可能地复述用户的输入；
stop：可选参数，默认为null，该参数接受一个或多个字符串，用于指定生成文本的停止信号。当
模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成。这可以用来控制模型的输出长
度或格式；
presence_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容
（例如新的概念或主题）的倾向性。较高的值会使模型更倾向于生成新内容，而较低的值则会使模
型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取
值；
frequency_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾
向性。较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果
篇幅较大并且存在前后语言重复时，可以提高该参数的取值；
best_of：该参数用于控制模型的生成过程。它会让模型进行多次尝试（例如，生成5个不同的响
应），然后选择这些响应中得分最高的一个；
logit_bias：该参数接受一个字典，用于调整特定token的概率。字典的键是token的ID，值是应用
于该token的对数概率的偏置；在GPT中我们可以使用tokenizer tool查看文本Token的标记。一般
不建议修改；
user：可选参数，使用用户的身份标记，可以通过人为设置标记，来注明当前使用者身份。需要
注意的是，Completion.create函数中的user和后续介绍的对话类模型的user参数含义并不相同，
需要注意区分；
更详细的参数说明和使用细节可在《Ch.2 Completions模型API使用指南》中找到。此处不在展开
重复说明。
目前在LangChain中已经集成的LLMs有82个，均已经实现了对异步、流式处理和批处理的基本支
持，查看地址如下：https://python.langchain.com/docs/integrations/llms/
4.1.2 Chat Models接入方法
所谓聊天模型，简单理解就是我们经常使用的xxx-chat类模型，比如Baichuan2-7b-chat，
Qwen-14b-chat，这类模型是基于Base模型经过针对特定数据格式的微调得到，以更贴近人类对话的
方式进行输入和输出。所以这类模型不会使用“文本输入、文本输出”的形式，而是采用聊天消息作为输
入并返回聊天消息作为输出（非纯文本）。LangChain框架与OpenAI、Cohere、Hugging Face等多家
模型提供商已经实现集成，提供了一个标准化的接口，以便与这些不同的模型进行交互。
我们先回顾下OpenAI规范下使用Chat模型进行一次交互需遵循的代码执行方式：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo-0125&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一个乐于助人的智能AI小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}
]
)
print(completion.choices[0].message.content)
您好，我是一个智能AI助手，可以帮助您回答问题、提供信息、解决问题等。如果您有任何疑问或需要帮助的
话，都可以随时向我提问。我会尽力帮助您解决问题。
这里不得不再次提到，LangChain作为一个应用开发框架，需要集成各种不同的大模型，如上述
OpenAI的GPT系列模型调用示例，通过Message数据输入规范，定义不同的role，即system、user和
assistant来区分对话过程，但对于其他大模型，并不意味这一定会遵守这种输入输出及角色的定义，所
以LangChain的做法是，因为Chat Model基于消息而不是原始文本，LangChain目前就抽象出来的消息
类型有 AIMessage 、 HumanMessage 、 SystemMessage 、 FunctionMessage 和 ChatMessage ，
但大多时候我们只需要处理 HumanMessage 、 AIMessage 和 SystemMessage，即：
SystemMessage ：用于启动 AI 行为，作为输入消息序列中的第一个传入。
HumanMessage ：表示来自与聊天模型交互的人的消息。
AIMessage ：表示来自聊天模型的消息。这可以是文本，也可以是调用工具的请求。
同样，Chat Models实现了 Runnable 接口，也支持 invoke 、 ainvoke 、 stream 、 astream 、
batch 、 abatch 等调用。即：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
所以对于LangChain中调用OpenAI的GPT模型，首先需要导入对应的模块，代码如下：
from langchain_core.messages import HumanMessage, SystemMessage
消息形式输入调用
定义消息对象：
messages = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;你好，请你介绍一下你自己&quot;),]
messages
[SystemMessage(content=&#x27;你是一位乐于助人的智能小助手&#x27;), HumanMessage(content=&#x27;你好，请
你介绍一下你自己&#x27;)]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
执行推理：
chat.invoke(messages)
AIMessage(content=&#x27;你好，我是一位可以回答问题、提供帮助的智能小助手。无论是关于知识、技能还是
日常生活中的问题，我都会尽力为您提供准确和有用的答案。有什么可以帮助您的吗？&#x27;)
如果只想获取到纯净的输出，可以通过.content来格式化。
reponse.content
&#x27;你好！我是一位智能助手，可以回答各种问题，提供信息和帮助。无论是关于知识、技能、娱乐、生活等方
面，我都会尽力为你提供帮助。有什么问题需要我帮忙的吗？&#x27;
流式调用
for chunk in chat.stream(messages):
print(chunk.content, end=&quot;&quot;, flush=True)
你好！我是一位智能助手，专门为用户提供信息咨询、问题解答和帮助服务。无论你有什么疑问或需求，我都
会尽力帮助你，让你更方便地获取所需的信息和支持。有什么我可以帮助你的吗？
批量调用
先定义三个不同的消息对象：
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
messages2 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是深度学习&quot;),]
messages3 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是大模型技术&quot;),]
将上述三个消息对象放在一个列表中，使用.batch方法执行批量调用。
reponse = chat.batch([messages1,
messages2,
messages3,])
reponse
[AIMessage(content=&#x27;机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来
改善自身的性能，而无需明确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规
律、模式以及知识的技术。通过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预
测、分类、识别等任务。机器学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测
等领域。&#x27;),
AIMessage(content=&#x27;深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神
经网络层来学习和理解复杂的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识
别、语音识别、自然语言处理等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对
新数据进行准确预测和分类。深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金
融、交通等各个领域。&#x27;),
AIMessage(content=&#x27;大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部
署复杂的深度学习模型。这些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优
化。\n\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模
型技术，研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。\n\n然
而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员和
工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。&#x27;)]
格式化输出：
# 使用列表生成式打印每一个消息的内容
contents = [msg.content for msg in reponse]
# 打印出内容
for content in contents:
print(content, &quot;\n---\n&quot;)
机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来改善自身的性能，而无需明
确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规律、模式以及知识的技术。通
过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预测、分类、识别等任务。机器
学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测等领域。
---
深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神经网络层来学习和理解复杂
的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识别、语音识别、自然语言处理
等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对新数据进行准确预测和分类。
深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金融、交通等各个领域。
---
大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部署复杂的深度学习模型。这
些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优化。
大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模型技术，
研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。
然而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员
和工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。
---
异步调用
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
reponse = await chat.ainvoke(messages1)
reponse
AIMessage(content=&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并
改进性能，而无需显式地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断
优化自身的表现。机器学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常
见的机器学习方法包括监督学习、无监督学习、强化学习等。&#x27;)
reponse.content
&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并改进性能，而无需显式
地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断优化自身的表现。机器
学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常见的机器学习方法包括
监督学习、无监督学习、强化学习等。&#x27;
流式调用
async for chunk in chat.astream(messages1):
print(chunk.content, end=&quot;&quot;, flush=True)
机器学习是一种人工智能的分支，它致力于让计算机系统通过学习数据和模式，从而改进自身的性能。简而言
之，机器学习让计算机具有从经验中学习的能力，而不需要明确地编程指令。
在机器学习中，计算机系统利用大量数据进行训练，通过识别数据中的模式和规律来建立模型。这些模型可以
用来进行预测、分类、识别等任务。机器学习在各个领域中都有广泛的应用，如自然语言处理、图像识别、医
疗诊断、金融预测等。
常见的机器学习算法包括监督学习、无监督学习、强化学习等。监督学习是通过已标记的数据进行训练，无监
督学习则是在没有标记的数据中发现模式，强化学习则通过试错的方式学习最优策略。机器学习技术的发展使
得计算机系统能够更加智能地处理复杂任务，为人们的生活和工作带来了许多便利。
通过上述描述，我们展示了在LangChain中使用LLMs类模型和Chat Model类模型的不同方法，其
核心区别在于输入Prompt的格式。除此之外其他的工作可以直接利用统一的抽象接口，实现与模型交
互的快速过程。而针对不同的模型，LangChain也提供个对应的接入方法，其相关说明文档地址：http
s://python.langchain.com/docs/integrations/chat/
比如我们以Baichuan2的在线API模型为例快速接入一下：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;我日薪8块钱，请问在闰年的二月，我月薪多少&quot;)])
response
AIMessage(content=&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所
以，你在闰年的二月月薪是232元。&#x27;)
response.content
&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所以，你在闰年的二月月
薪是232元。&#x27;
如未实践过Baichuan2的在线API，请查看《APICh.16 BaiChuan 2 大模型生态介绍及本地私有化
部署方案》中1. Baichuan在线大模型生态介绍小节
可以看出，LangChain能够迅速接入不同的模型API。至于接入本地私有部署的开源大模型的方
法，我们将在后续课程中进行详细介绍。
4.2 LangChain中如何使用Prompt Template
提示工程（Prompt Engineering）大家应该比较熟悉，这个概念是指在与大语言模型（LLMs），
如GPT-3、Qwen等模型进行交互时，精心设计输入文本（即提示）的过程，以获得更精准、相关或有
创造性的输出。正如我们在大模型技术应用课程中介绍的，通过采用Few-Shot、Chain of Thought
(CoT)等高级提示技巧，可以显著提高大模型在推理任务上的表现。目前，提示工程已经发展成为一个
专业领域，非常多的公司设立了专门的职位，负责为特定任务编写精确且具有创造力的提示。
以使用ChatGPT等网页端对话交互应用中，大部分人常见的做法是将提示（Prompt）做硬编码，
例如将一段提示文本固定在System Messages中。而在应用开发领域，开发者往往无法预知用户的具体
输入内容，同时又希望大模型能够根据不同的应用任务以一种较为统一的逻辑来处理用户输入。所以，
LangChain通过提供Prompt Templates功能，优雅地解决了这个问题。Prompt Templates将用户输入
到完整格式化提示的转换逻辑进行封装，使得模型能够更灵活、高效地处理各种输入。
LangChain 提供了创建和使用提示模板的工具，其使用PromptTemplate 方法创建字符串提示模
板，因为目前为止，语言模型接收的提示基本都是字符串或聊天消息列表。
4.2.1 使用str.format语言构建模版
在LangChain的默认设置下， PromptTemplate 使用 Python 的 str.format() 方法进行模板
化。
Python的str.format()方法是一种字符串格式化的手段，允许我们在字符串中插入变量。使用这
种方法，可以创建包含占位符的字符串模板，占位符由花括号{}标识。调用format()方法时，可以传入
一个或多个参数，这些参数将被顺序替换进占位符中。str.format()提供了灵活的方式来构造字符串，支
持多种格式化选项，包括数字格式化、对齐、填充、宽度设置等。
基本用法
# 简单示例，直接替换
greeting = &quot;Hello, {}!&quot;.format(&quot;Alice&quot;)
print(greeting)
# 输出: Hello, Alice!
Hello, Alice!
带有位置参数的用法
# 使用位置参数
info = &quot;Name: {0}, Age: {1}&quot;.format(&quot;Bob&quot;, 30)
print(info)
# 输出: Name: Bob, Age: 30
Name: Bob, Age: 30
带有关键字参数的用法
# 使用关键字参数
info = &quot;Name: {name}, Age: {age}&quot;.format(name=&quot;Charlie&quot;, age=25)
print(info)
# 输出: Name: Charlie, Age: 25
Name: Charlie, Age: 25
使用字典解包的方式：
# 使用字典解包
person = {&quot;name&quot;: &quot;David&quot;, &quot;age&quot;: 40}
info = &quot;Name: {name}, Age: {age}&quot;.format(**person)
print(info)
# 输出: Name: David, Age: 40
Name: David, Age: 40
在LangChain中，基本采用了Python的原生str.format()方法对输入数据进行格式化，这样在模
型接收输入前，可以根据需要对数据进行预处理和结构化，以此来引导大模型进行更准确的推理。
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(
&quot;请给我一个关于{topic}的{type}解释。&quot;
)
prompt= prompt_template.format(type=&quot;详细&quot;, topic=&quot;量子力学&quot;)
prompt
&#x27;请给我一个关于量子力学的详细解释。&#x27;
如上所示，可以使用PromptTemplate的from_template方法创建一个提示模板实例，这个模板
包含了两个占位符：{topic} 和 {type}，这些占位符在实际调用时可以被实际的值替换。
调用LLMs模型
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
我觉得我已经对量子力学的基本理解了，但是我还是很想听听有关量子力学的详细解释，比如：量子力学是怎
么起作用的。
谢谢了.
量子力学是在克里姆林宫试验时的实验结果发现.一开始是用熟悉的物理学法则去解释它.比如说,量子力学是
量子力学的结果,而不是量子力学的原因.玻尔兹曼方程就是克里姆林宫试验结论的表示.后来人们发现,等效
性,柏格效应等现象.然后才发现,原来我们的光学和电磁学等理论是有问题的.这些现象的实现是在离子束中.
所以量子力学是很实际的.
[ 编辑 stuartm 在 06-07-08 12:11 ]
prompt= prompt_template.format(type=&quot;简短&quot;, topic=&quot;机器学习&quot;)
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
2)目前的时代，机器学习已经发展到了什么程度，也有什么应用场景。 3)目前的研究方向是什么。 4)什么
时候会用到这些技术。 5)最后一个问题是，对于我自己来说，是不是还有其他的方向比如说算法之类的，还
有在个人的技术知识里面是不是需要这些东西，不是特别需要。 也许这些都是我自己的猜测，如果你有什么
好的建议，我也非常感谢。 最后，我想问一下，你是在哪里学的，是在哪个大学，我就是在这里问问。 谢
谢。 2019.6.16
电影《伪装者》中的重要概念
原理：1. 无线电波的能量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无
线电波的方向不会发生改变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改
变，传播途中，波的方向不会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途
中，波的方向会发生改变（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变
（这就是遮挡）3. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）4. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）5. 无线电波的方向会发生改变，
传播途中，波的方向不会发生改变（这就是遮挡）6. 无线电波的方向会发生改变，传播途中，波的方向不会
发生改变（这就是遮挡）7. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）
8. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）原理：1. 无线电波的能
量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无线电波的方向不会发生改
变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改变，传播途中，波的方向不
会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途中，波的方向会发生改变
（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）3. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这
除此之外，这种模板生成方式支持任意数量的变量，包括无变量，可以做如下测试：
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(&quot;给我讲一个笑话&quot;)
prompt_template.format()
&#x27;给我讲一个笑话&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, )
result = llm.invoke(prompt)
print(result)
如果我是一个机器，会被称为什么？
谢谢
调用Chat Model
上述示例展示了如何使用字符串模板生成和调用LLMs类模型。对于聊天模型，也需要使用 Python
的 str.format() 方法进行模板化，但其构造的输入形式则有所区别，应采用聊天消息列表的形式，
每条消息都包含内容和一个名为role的附加参数。例如，在OpenAI的Chat Completions API中，聊天
消息需要包含为assistant、user或system。在这种情况下，应使用ChatPromptTemplate来构建对话
模板。
from langchain_core.prompts import ChatPromptTemplate
chat_template = ChatPromptTemplate.from_messages(
[
(&quot;system&quot;, &quot;你是一个有帮助的AI机器人，你的名字是{name}。&quot;),
(&quot;human&quot;, &quot;你好，最近怎么样？&quot;),
(&quot;ai&quot;, &quot;我很好，谢谢！&quot;),
(&quot;human&quot;, &quot;{user_input}&quot;),
]
)
messages = chat_template.format_messages(name=&quot;小明&quot;, user_input=&quot;你叫什么名字？&quot;)
messages
[SystemMessage(content=&#x27;你是一个有帮助的AI机器人，你的名字是小明。&#x27;),
HumanMessage(content=&#x27;你好，最近怎么样？&#x27;),
AIMessage(content=&#x27;我很好，谢谢！&#x27;),
HumanMessage(content=&#x27;你叫什么名字？&#x27;)]
print(chat_template)
input_variables=[&#x27;name&#x27;, &#x27;user_input&#x27;] messages=
[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;name&#x27;],
template=&#x27;你是一个有帮助的AI机器人，你的名字是{name}。&#x27;)),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[],
template=&#x27;你好，最近怎么样？&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;我很
好，谢谢！&#x27;)), HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=
[&#x27;user_input&#x27;], template=&#x27;{user_input}&#x27;))]
从输出上看，其构造函数在实例化prompt_template时，主要由两个关键参数进行指定：
input_variables：这是一个列表，包含模板中需要动态填充的变量名。这些变量名在模板字符串
中以花括号（如{name}）标记。通过指定这些变量，可以在后续过程中动态地替换这些占位符。
template：这是定义具体提示文本的模板字符串。它可以包含静态文本和input_variables列表中
指定的变量占位符。当调用format方法时，这些占位符会被实际的变量值替换，生成最终的提示
文本。
调用Chat Model，这里我们选择GPT 3.5。代码如下：
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
我叫小明，很高兴为您提供帮助！有什么问题可以帮到您？
messages = chat_template.format_messages(name=&quot;大熊&quot;, user_input=&quot;你要去哪里玩？&quot;)
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
作为一个AI机器人，我没有身体，所以无法去任何地方玩耍。但我可以帮助你规划旅行或提供有关旅游目的地
的信息。有什么我可以帮助你的吗？
通过上述流程，我们演示了如何在LangChain框架内使用ChatPromptTemplate来构建与聊天模型
交互的对话模板。通过此方法，能够创建一个包含不同参与者角色和定制化消息内容的对话流程，这对
于开发复杂的聊天应用或增强AI助手的交互能力至关重要。
4.2.2 构造Few-Shot模版
在LangChain中，很多的功能抽象、链路抽象本质上都是在对大模型的“涌现能力”能够应用落地的
一种具体实现方法，而其推理的不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度
依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。最为简单的
提示工程的方法就是通过输入一些类似问题和问题答案，让模型参考学习，并在同一个prompt的末尾
提出新的问题，依次提升模型的推理能力。
比如我们在《Ch.3 基于思维链的进阶提示工程》中测试的经典推理问题：
import os
import openai
openai.api_key = os.getenv(&quot;OPENAI_API_KEY&quot;)
prompt = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次？&#x27;
prompt
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n在关闭之前，艾米能滑14次。&#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n\n15分钟 = 900秒\n\n每次滑下来需要1分钟，所以在15分钟内，可以滑下900秒/（1分钟+4分钟）=
900/5 = 180次。&#x27;
可以看到多次执行，其推理均是错误的。正确的计算过程应该是先计算艾米一次爬上爬下总共需要
5分钟，然后滑梯还有15分钟关闭，因此关闭之前能够再滑15/5=3次。
而我们当时在OpenAI的API规范下，是通过这种方式来进行Few-Shot的提示：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
在Few-Shot的提示下，再次尝试进行提问：
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt_Few_shot_CoT4,
max_tokens=1000,
)
response1.choices[0].text
&#x27;”艾米花了1分钟滑下来，滑一次上去需要4分钟，因此在15分钟关闭之前，她能滑15分钟-1分钟=14分钟，
14分钟除以4分钟每次，她能滑14÷4=3次，答案是3次。“ &#x27;
从输出结果上看，通过Few-Shot提示可以激发大模型的涌现能力从而回答对复杂推理问题，在应用
开发中同样需要这种能力来提升大模型的推理稳定性。所以LangChain抽象了
FewShotPromptTemplate方法来实现。我们可以先测试下在LangChain的接口下直接提问：
直接提问
prompt1 = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在
关闭之前她能滑多少次？&#x27;
prompt1
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在关闭之前，艾米能滑(15-4-1)/4=2次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在15分钟内，她能滑15次。&#x27;
其实也很容易想到，LangChain的工作仅仅是集成并接入OpenAI的GPT模型，因此其本质上与直接
调用GPT模型的效果相同。出现错误回答也属于正常情况。接下来，我们根据LangChain对Few-Shot模
板的定义规范，进行提示词的构建。
在LangChain中，需要使用 PromptTemplate 创建字符串提示模板。模板可以包括说明、少量示
例以及适合给定任务的特定上下文和问题。因此，我们要创建一个少量示例的列表。每个示例都是一个
字典，其中键是输入变量，值是这些输入变量的值。
适合LLMs模型的模版构造形式
from langchain.prompts.few_shot import FewShotPromptTemplate
from langchain.prompts.prompt import PromptTemplate
examples = [
{
&quot;question&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个
网球？&quot;,
&quot;answer&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现
在总共由5+6=11个网球。因此答案是11。&quot;
},
{
&quot;question&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在
食堂总共有多少个苹果？&quot;,
&quot;answer&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答
案是9。&quot;
},
{
&quot;question&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？&quot;,
&quot;answer&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4
个，答案是4个。&quot;
},
]
接下来需要实例化一个 PromptTemplate 对象，将少量示例格式化为字符串。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[0]))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[1]))
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
最后，创建一个 FewShotPromptTemplate 对象。该对象接受经过上一步格式化后的少数样本示
例。
# 导入 FewShotPromptTemplate 类
from langchain.prompts.few_shot import FewShotPromptTemplate
# 创建一个 FewShotPromptTemplate 对象
few_shot_prompt = FewShotPromptTemplate(
examples=examples, # 最开始定义的 examples 示例
example_prompt=example_prompt, # 上一步定义的 example_prompt 作为提示模板
suffix=&quot;Question: {input}&quot;, # 后缀模板，其中 {input} 会被替换为实际输入
input_variables=[&quot;input&quot;] # 定义输入变量的列表
)
# 使用给定的输入格式化 prompt，并打印结果
print(few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，
水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Question: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有
多少个蓝色高尔夫球？
总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
Question: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次？
final_input = few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟
才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)
final_input
&#x27;Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰
一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此
答案是11。\n\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现
在食堂总共有多少个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹
果，答案是9。\n\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是
蓝色的。请问总共有多少个蓝色高尔夫球？\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是
蓝色的，也就是4个，答案是4个。\n\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&#x27;
将带有Few-Shot提示的输入，输入到大模型中执行推理。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米需要4分钟爬到顶部，1分钟滑下来，一共花费5分钟。在15分钟内，她能滑3次，因为15÷5=3。因此
答案是3次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米花了5分钟（4分钟爬上去+1分钟滑下来），因此在15分钟内可以滑3次（15÷5=3）。答案是3次。&#x27;
从输出结果来看，显然受到了提示工程的正面影响，通过问题的拆解过程显著提高了回答的准确
性。
适合Chat Model的提示模版构造形式
我们在前面解释过LLMs和Chat Model 的区别，因其接受的输入形式不同，所以构造的提示模版也
是不一样的，对于Chat Model来说，它接收的是聊天形式的对话格式，所以对于其对应的Few-Shot，
在LangChain中的基本结构如下：
examples ：要包含在最终提示中的字典示例列表。
example_prompt ：通过其 format_messages 方法将每个示例转换为 1 条或多条消息。一个常
见的示例是将每个示例转换为一条人工消息和一条人工智能消息响应，或者一条人工消息后跟一条
函数调用消息。
我们还是使用四个数学推理题来进行测试：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
直接调用
对于Chat Model，需要将输入转化成对话聊天格式：
from langchain_core.messages import HumanMessage, SystemMessage
messages = [SystemMessage(content=&quot;你是一个擅长数学推理的专家&quot;),
HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟
后关闭，请问在关闭之前她能滑多少次？&quot;),]
实例化模型进行推理：
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
resonse = chat.invoke(messages)
resonse.content
&#x27;首先，艾米花了1分钟滑下来，所以她每次滑下来后需要等待3分钟（4分钟上去 + 1分钟下来 = 5分钟，15
分钟总时间 - 5分钟 = 10分钟，10分钟 / 3分钟 = 3次）。\n\n在水滑梯关闭之前，艾米可以滑3次。&#x27;
&amp;esmp; 实际上，在使用GPT-3.5时，我们可以观察到，即使不采用Few-Shot提示，模型也能以很高
的概率正确回答问题，这归功于模型本身已经非常强大的能力。所以我们改为Baichuan2模型来测试：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟滑下来，所以她在滑梯关闭前可以滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟从滑梯顶部滑下来，所以她可以在滑梯关闭前滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，所以每次滑行需要4+1=5分钟。\n\n水滑梯将在
15分钟后关闭，所以她能在关闭之前滑行15÷5=3次。&#x27;
可以看到，在切换到Baichuan模型进行测试时，三次测试中出现了两次错误回答。因此，我们就
使用Baichuan2模型，尝试通过添加Few-Shot来探索是否能增强其推理能力。
首先，安装LangChain的规范，导入生成示例的模块：
from langchain.prompts import (
ChatPromptTemplate,
FewShotChatMessagePromptTemplate,
)
接下来以input-output对的形式构建Few-Shot提示模板。在这里，“input”可以被视作人类发出的
Promot，而“output”则是大模型给出的回答。
examples = [
{&quot;input&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网
球？&quot;,
&quot;output&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总
共由5+6=11个网球。因此答案是11。&quot;},
{&quot;input&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共
有多少个苹果？&quot;,
&quot;output&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&quot;},
{&quot;input&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问
总共有多少个蓝色高尔夫球？&quot;,
&quot;output&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答
案是4个。&quot;},
]
然后，使用from_messages方法进行模版构造。
# This is a prompt template used to format each individual example.
example_prompt = ChatPromptTemplate.from_messages(
[
(&quot;human&quot;, &quot;{input}&quot;),
(&quot;ai&quot;, &quot;{output}&quot;),
]
)
few_shot_prompt = FewShotChatMessagePromptTemplate(
example_prompt=example_prompt,
examples=examples,
)
print(few_shot_prompt.format())
Human: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
AI: 罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网
球。因此答案是11。
Human: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹
果？
AI: 食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Human: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少
个蓝色高尔夫球？
AI: 总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
生成最终的输入模版，设置 input 作为动态变量：
final_prompt = ChatPromptTemplate.from_messages(
[
few_shot_prompt,
(&quot;human&quot;, &quot;{input}&quot;),
]
)
final_prompt
ChatPromptTemplate(input_variables=[&#x27;input&#x27;], messages=
[FewShotChatMessagePromptTemplate(examples=[{&#x27;input&#x27;: &#x27;罗杰有五个网球，他又买了两盒网
球，每盒有3个网球，请问他现在总共有多少个网球？&#x27;, &#x27;output&#x27;: &#x27;罗杰一开始有五个网球，又购买了两
盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此答案是11。&#x27;}, {&#x27;input&#x27;:
&#x27;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？&#x27;,
&#x27;output&#x27;: &#x27;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&#x27;}, {&#x27;input&#x27;: &#x27;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。
请问总共有多少个蓝色高尔夫球？&#x27;, &#x27;output&#x27;: &#x27;总共有16个球，其中一半是高尔夫球，也就是8个，其中
一半是蓝色的，也就是4个，答案是4个。&#x27;}],
example_prompt=ChatPromptTemplate(input_variables=[&#x27;input&#x27;, &#x27;output&#x27;], messages=
[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;output&#x27;],
template=&#x27;{output}&#x27;))])),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;))])
实例化baichuan2模型：
最后，使用LangChain中的chain的抽象，合并最终的提示和大模型共同执行。
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-e9&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米需要4分钟才能爬上去，1分钟滑下来，总共需要4+1=5分钟。15分钟可以滑15÷5=3次，所以答案是3
次。&#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米爬上去需要4分钟，滑下来只需要1分钟，所以她可以在15分钟内完成4次滑行，因为每次滑行需要
4+1=5分钟。&#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54695e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，然后1分钟滑下来，总共需要4+1=5分钟。因此，在滑梯关闭之前，她可
以滑15÷5=3次。&#x27;
4.3 LangChain中的Output Parsers
Output Parsers，即输出解析器，这个概念非常好理解，就是负责获取大模型的输出并将其转换为
更合适的格式。这在应用开发中及其重要。在大多数复杂应用场景中，处理逻辑往往环环相扣，执行某
项业务逻辑可能需要多次调用大模型，其中上一次的调用结果将被用于指导下一次调用的逻辑。在这种
情况下，结构化的信息会比纯文本又有价值，同时这也是输出解析器的价值所在。
LangChain构造的输出解释器必须实现两个主要方法：
Get format instructions：该方法会返回一个字符串，其中包含有关如何格式化语言模型输出的指
令。
Parse：该方法会接收字符串，并将其解析为某种结构
目前已经支持的解析格式已经包括Json、Xml、Csv以及OpenAI的Tools和Functions等多种格式，
具体可看：https://python.langchain.com/docs/modules/model_io/output_parsers/
简而言之，这个过程本质上是对文本的格式化处理。但与传统的后处理不同，LangChain采取的实
现策略是通过在Prompt上增加输出解析，直接引导大模型输出的格式化方式。
我们以格式化日期来看一下LangChain的构造思路，首先导入相关的模块：
from langchain.output_parsers import DatetimeOutputParser
from langchain.prompts import PromptTemplate
from langchain_openai import OpenAI
实例化日期解析器：
output_parser = DatetimeOutputParser()
通过.get_format_instructions方法，可以查看定义的解析格式：
output_parser.get_format_instructions()
&quot;Write a datetime string that matches the following pattern: &#x27;%Y-%m-
%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1181-01-05T02:20:33.675873Z, 1938-12-
13T09:46:36.791229Z, 1468-01-17T07:34:51.108688Z\n\nReturn ONLY this string, no
other words!&quot;
构造输入模版，这里的区别是：在输入的Prompt Template中，加入了OutPut Parse的内容：
template = &quot;&quot;&quot;用户发起的提问:
{question}
{format_instructions}&quot;&quot;&quot;
生成最终实际输入大模型的Promot内容：
prompt = PromptTemplate.from_template(
template,
# 预定义的变量，这里我们传入格式化指令
partial_variables={&quot;format_instructions&quot;:
output_parser.get_format_instructions()},
)
prompt
PromptTemplate(input_variables=[&#x27;question&#x27;], partial_variables=
{&#x27;format_instructions&#x27;: &quot;Write a datetime string that matches the following
pattern: &#x27;%Y-%m-%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1472-10-23T17:39:56.199188Z,
1643-01-14T10:55:11.903091Z, 0631-12-11T23:04:04.345333Z\n\nReturn ONLY this
string, no other words!&quot;}, template=&#x27;用户发起的提
问:\n\n{question}\n\n{format_instructions}&#x27;)
最后，使用LangChain中的chain的抽象，合并最终的提示、大模型实例及OutPut Parse共同执
行。
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请问你叫什么？&quot;})
print(output)
2021-10-07 12:02:13.345333
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是机器学习&quot;})
print(output)
2018-01-23 09:42:32.123456
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是深度学习&quot;})
print(output)
2019-08-07 18:30:00
通过测试可以明显观察到，以此方式定义的输出解析器极大地规范了模型的输出。当然，这也直接
受到大模型实际能力和模板定义情况的影响。
LangChain中不同模版的使用方式，除了可以在官方文档上看到，在其源码上看会更加直观，而且
可以自定义修改，其所有的OutPut Parse的模版存储路径为：
langchain/libs/langchain/langchain/output_parsers ， 大家可以自行尝试。比如我们上面使
用的日期解析的源码对应如下：
到此为止，我们已经非常详尽地介绍了LangChain的核心概念。如果有学习过大模型技术实战课程
的同学来说，会发现LangChain的Model I/O模块中讨论的所有概念都是之前接触过的，需要学习的主
要是LangChain对大模型进行抽象的方法以及在LangChain框架下的应用开发技巧。
而从LangChain的整体开源情况看，其内容丰富且迭代非常快，通过模块化的方式介绍，可以帮助
大家快速构建起对LangChain以及应用开发的思维框架。尽管在Model I/O模块中还有许多更高级的内
容，例如如何接入本地部署的开源大模型，关于函数调用、Memory缓存等更深入的主题，这些内容我
们将在后续章节中一一接触并进行详细的探讨。
```

至此，我们就完成了文档的加载工作。接下来要考虑的是，如何把这一整份文档切分成一个个小的（Chunks）。

## 3.2 文档切分

分块（Chunking），其实现形式上是将长文档拆分为较小的块的过程，目的是在检索时能够准确地找到最直接和最相关的段落。由于文章通常包含大量不相关信息，在进行分块之前，也常常需要进行一些预处理工作，如文本清洗、停用词处理等，因为数据形式和需求不同，其文本清洗方法大相径庭。

转回到核心内容来看，一个有效的分块策略，可以确保搜索结果精确地反映用户查询的实际需求。如果分块过小或过大，都可能导致搜索结果不准确或提取不到最相关的内容。理想的文本块应尽可能语义独立，即不过度依赖上下文，这样的文本是语言模型最易于理解的。因此，为文档确定最佳的块大小是确保搜索结果准确性和相关性的关键。这涉及多个决策因素，如块的大小；如果句子太短，模型可能难以理解其意义，且句子越短，包含的有效信息就越少。比较常用的有如下五种不同的方法来优化分块策略：

1.  **根据句子切分**：这种方法按照自然句子边界进行切分，以保持语义完整性。

1.  **按照固定字符数来切分**：这种策略根据特定的字符数量来划分文本，但可能会在不适当的位置切断句子。

1.  **按固定字符数来切分，结合重叠窗口（**overlapping windows）：此方法与按字符数切分相似，但通过重叠窗口技术避免切分关键内容，确保信息连贯性。

1.  **递归方法**：通过递归方式动态确定切分点，这种方法可以根据文档的复杂性和内容密度来调整块的大小。

1.  **根据语义切割**：这种高级策略依据文本的语义内容来划分块，旨在保持相关信息的集中和完整，适用于需要高度语义保持的应用场景。

第二种方法（按照字符数切分）和第三种方法（按固定字符数切分结合重叠窗口）主要基于字符进行文本的切分，而不考虑文章的实际内容和语义。这种方式虽简单，但可能会导致主题或语义上的断裂。相对而言，递归方法更加灵活和高效，它结合了固定长度切分和语义分析。通常是首选策略，因为它能够更好地确保每个段落包含一个完整的主题。而最后一项，基于语义的分割虽然能精确地切分出完整的主题段落，但这种方法效率较低。它需要运行复杂的分段算法（segmentation algorithm），处理速度较慢，并且段落长度可能极不均匀——有的主题段落可能很长，而有的则较短。因此，尽管它在某些需要高精度语义保持的场景下有其应用价值，但并不适合所有情况。

这些方法各有优势和局限，选择适当的分块策略取决于具体的应用需求和预期的检索效果。接下来我们依次尝试用常规手段应该如何实现上述几种方法的文本切分。

这里我们初次尝试，选择手动实现三种的形式，分别为按照句子切分、按照Token切分和按照滑动窗口的固定字符切分。

### 3.2.1 按照句子切分

按照句子切分，其实就是通过标点符号来进行文本切分（分割），这可以直接使用Python的标准库来完成这个任务。一种简单的方法是使用re模块，它提供了正则表达式的支持，可以方便地根据标点符号来分割文本。如下示例中，展示了如何使用re.split()函数来根据中文和英文的标点符号进行文本切分。代码如下：

```
import re

def split_text_by_punctuation(text):
    # 定义一个正则表达式，包括常见的中英文标点
    # pattern = r&quot;[。！？｡＂＃＄％＆＇（）＊＋，－／：；＜＝＞＠［＼］＾＿｀｛｜｝～\s、]+&quot;
    pattern = r&quot;[。！？｡]+&quot;
    # 使用正则表达式进行分割
    segments = re.split(pattern, text)
    # 过滤掉空字符串
    return [segment for segment in segments if segment]
```

这个函数会根据中文和英文的标点符号来分割文本，并移除空字符串。定义好分割函数后，我们可以尝试进行功能测试：

```
# 文本
text = &quot;春节的脚步越来越近，大街小巷都布满了节日的气氛。商店门口挂满了红灯笼和春联，家家户户都在忙着打扫卫生，准备迎接新的一年。\
小明回到家乡，感受到了浓浓的过年氛围。他在街上走着，看到小朋友们手持烟花棒，欢笑声此起彼伏。\
夜幕降临，整个城市亮起了五彩缤纷的灯光，映照着人们脸上的喜悦与期待。老人们聚在一起，回忆过去，展望未来。\
而年轻人则在夜市享受美食，放松心情。这是一个充满希望和喜悦的时刻，每个人都在以自己的方式庆祝这个特殊的节日。&quot;

# 调用函数进行分割
segments = split_text_by_punctuation(text)

# 使用循环来打印每个chunk
for i, segment in enumerate(segments):
    print(&quot;Chunk {}: {}&quot;.format(i + 1, segment))
```

```
Chunk 1: 春节的脚步越来越近，大街小巷都布满了节日的气氛
Chunk 2: 商店门口挂满了红灯笼和春联，家家户户都在忙着打扫卫生，准备迎接新的一年
Chunk 3: 小明回到家乡，感受到了浓浓的过年氛围
Chunk 4: 他在街上走着，看到小朋友们手持烟花棒，欢笑声此起彼伏
Chunk 5: 夜幕降临，整个城市亮起了五彩缤纷的灯光，映照着人们脸上的喜悦与期待
Chunk 6: 老人们聚在一起，回忆过去，展望未来
Chunk 7: 而年轻人则在夜市享受美食，放松心情
Chunk 8: 这是一个充满希望和喜悦的时刻，每个人都在以自己的方式庆祝这个特殊的节日
```

接下来，我们切分刚才的加载的PDF文档的全部内容，其存储在变量extracted_text中。

```
# 调用函数进行分割
segments = split_text_by_punctuation(extracted_text)

# 使用循环来打印每个chunk
for i, segment in enumerate(segments):
    print(&quot;Chunk {}: {}&quot;.format(i + 1, segment))
```

```
Chunk 1: 开源大模型应用开发实战
Ch 18 LangChain整体项目介绍与核心模块Model I/O详
解
注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情
况
Chunk 2: 如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系
Chunk 3: 我们在接到相关反馈后，
将迅速更新教材内容以确保信息的准确性和实用性
Chunk 4: 
本期课件更新时间：2024年3月7日
在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地
私有化部署开源大模型
Chunk 5: 在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家
关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部
署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部
署
Chunk 6: 
但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也
是最简单的一步
Chunk 7: 熟练掌握基于大模型的上层应用开发还有非常长的路需要走
Chunk 8: 目前，大模型技术开发
总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API的应用开发，而另一类，就是先私有化
部署开源大模型，再借助开发框架构建大模型应用
Chunk 9: 当然了，目前主流的大模型应用开发框架也支持在
线模型API的调用，这正是我们之前一直强调学习掌握开源或在线API调用技巧的原因，因为这些方法和
技巧直接对应于应用开发的最佳实践
Chunk 10: 
而不论采用哪种开发形式，其基座和所依赖的，核心还是大模型本身
Chunk 11: 对于在线大模型，以
OpenAI为例，开发者需要配置科学上网的环境，在其官网创建 API Key，最后遵循API的使用说明去进
行应用配置
Chunk 12: 这一过程中涉及到的网络环境适配，数据隐私安全，成本管理及如何去定制化自己需求的
开发流程，往往都需要遵循官方提供的功能来实现
Chunk 13: 也正是因为这些复杂的因素使得尽管目前公认最强
的大语言模型GPT 4开放给开发者使用，还是会有非常多的公司和个人去拥抱开源，选择性能可能略逊
一筹的开源大模型，因为开源方案在某些方面提供了更多的灵活性和自主控制能力
Chunk 14: 
大家也能够发现，在我们的课程中每当涉及到大模型的私有化部署，我们都会向大家展示如何通过
命令行、网页端访问和API调用等三种方式与部署的大模型进行交互
Chunk 15: 此外，还通过text-generate-
webui这样的开源项目，这一切应用是帮助大家能够以最简单、最直接的方式与开源大模型进行对话，
了解大模型的运行模型
Chunk 16: 但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶
段肯定无法满足实际的需求
Chunk 17: 虽然目前的大模型基本都可以按照OpenAI的API规范形式进行调用，但如
果进行实操我们会发现，在《大模型技术实战》的课程中，借助OpenAI的GPT系列模型开发的项目，
如智能邮件处理、全自动AI开发流程，以及MateGen Agents等，尽管在开源模型上能够被复现，但这
背后的成功关键并非仅仅在于模型调用
Chunk 18: 例如，在《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进
行外部工具API调用开发实践》中，我们通过Qwen模型复现了《Ch.10 借助Function calling调用外部
工具API方法》中的天气信息调用过程
Chunk 19: 这一复现过程揭示了，对在线模型来说，我们需要学习的是其
API使用方法
Chunk 20: 而在开源大模型的技术领域下，要想实现某些特定功能，重点在于理解该模型的一些使
用原理，以及如何调整和构建tools与Schema表示
Chunk 21: 因此，整个开发过程中花费的大部分时间并非直接
与模型调用相关，而是在于深入理解和应用这些底层原理和技术细节
Chunk 22: 
换言之，在构建大模型应用的过程中，虽然大模型是核心，但实际针对大模型编写的代码量相对较
小
Chunk 23: 而在这些有限的代码中，提示工程又往往会占据主要的工作量
Chunk 24: 这进一步引出一个关键问题：除了
大模型本身，其他各环节如何高效串联，以及这些环节的代码开发和工作量如何分配和执行，将在很大
程度上影响最终应用的用户体验
Chunk 25: 
在这种情况下，解决这一窘境且广受认可和高度实用的解决方案，便是LangChain
Chunk 26: 
1. 什么是LangChain
LangChain现在归属于LangChain AI公司，LangChain作为其中的一个核心项目，开源发布在
Gitub上：https://github.com/langchain-ai/langchain
从LangChain的GitHub版本迭代历史上看，从2023年1月16日起已经经历了320个大小版本的迭
代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高
的
Chunk 27: LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架
Chunk 28: 它的做法是：通过提
供标准化且丰富的模块抽象，构建大语言模型的输入输入规范，利用其核心概念chains，灵活地连接
整个应用开发流程
Chunk 29: 而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出
来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发
领域内被普遍高度认可的通用框架
Chunk 30: 
为什么需要这样做
Chunk 31: 
首先，我们需考虑当前大模型的发展态势
Chunk 32: 尽管OpenAI的GPT系列模型作为大模型领域的领军人
物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大
模型间的使用方式完全相同
Chunk 33: 例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不
适用
Chunk 34: 因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是
十分低下的
Chunk 35: 
其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据
连接方式以及输出结果的不稳定性等问题
Chunk 36: 在应用开发中，如何找到这些问题的有效解决策略
Chunk 37: 
上述提到的每个限制都紧密关联于大模型本身的特性
Chunk 38: 尽管理论上可以通过重新训练、微调来增强
模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资
源、时间和财力，选择这条路径一定会导致方向越来越偏离目标
Chunk 39: 我们之前讨论的OpenAI的Function
Calling和通过ReAct流程接入第三方API能够提供一些解决方案，但这每一步都需大量的研发投入，而且
最终实现后的应用效果，也取决于研发人员的个人技术能力
Chunk 40: 在这种背景下，既然大家都有不同的想法
和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的
快速搭建，这就是LangChain一直想要做到，且正在做的事情
Chunk 41: 
LangChain的做法
从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对
大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接
口以适配各种大模型
Chunk 42: 到目前为止，LangChain抽象出最重要的核心模块如下：
1. Model I/O ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；
2. Retrieval ：检索外部数据，然后在执行生成步骤时将其传递到 LLM，包括文档加载、切割、
Embedding等；
3. Chains ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多
功能的高级抽象；
4. Memory ： 记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；
5. Agents ： 目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；
6. Callbacks ：回调系统，允许连接到 LLM 应用程序的各个阶段
Chunk 43: 用于日志记录、监控、流传输和
其他任务；
从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文
记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准
Chunk 44: 通过核心的&quot;Chain&quot;高级抽象，定义了
不同形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整
的应用程序
Chunk 45: 这个过程类似于搭建积木，可以灵活适应不同的任务需求
Chunk 46: 
也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和
使用方法，那么如何去搭这个“积木”，就需要我们对其每个核心模块都要有一个比较清楚的认知
Chunk 47: 所以
我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实
操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业
务情况，选择合适的构造模块和构造方法
Chunk 48: 
在整体上理解了LangChain之后，我们首先从Model I/O模块进行深入的探讨和实践
Chunk 49: 
2. LangChain核心模块：Model I/O
LangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成
Chunk 50: 所谓的
Model I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就
是通过该模块，我们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过
程：输入Prompt，得到大模型针对该Prompt的推理结果
Chunk 51: 如下示例为OpenAI的 GPT 系列模型的API
调用规范：
response = openai.ChatCompletion.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是机器学习
Chunk 52: &quot;}
]
)
而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话
交互程度，那相较于直接使用OpenAI的API调用又有何异呢
Chunk 53: 所以在这个模块中，LangChain同样抽象
出一个chain，用于进一步简化和增强交互流程
Chunk 54: 在LangChain的Model I/O模块设计中，包含三个核
心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和
Output Parser（对应下图中的Parse部分）
Chunk 55: 
Format：即指代Prompts Template，通过模板化来管理大模型的输入；
Predict：即指代Models，使用通用接口调用不同的大语言模型；
Parse：即指代Output部分，用来从模型的推理中提取信息，并按照预先设定好的模版来规范化
输出
Chunk 56: 
整个Model I/O工作流如下图所示：
Format
对于Prompt Template第一部分，传统上我们创建提示词是通过手工编写来实现的，在这个过程中
会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能
Chunk 57: 然而，
在应用开发中，一个关键的考量是提示词不能是一成不变的
Chunk 58: 其原因在于，应用开发需要适应多变的用
户需求和场景
Chunk 59: 固定的提示词限制了模型的灵活性和适用范围
Chunk 60: 例如，如果我们正在开发一个天气查询
应用，用户可能会以多种方式提出查询，如“今天的天气怎么样
Chunk 61: ”或“明天纽约的温度是多少度
Chunk 62: ”
Chunk 63: 如
果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求
Chunk 64: 
而Prompt Template，就像我们在介绍《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进行外
部工具API调用开发实践》中不断优化出来的ReAct模版，将API的使用、问题解答过程等复杂逻辑封装
成了一套结构化的格式
Chunk 65: 我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引
导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：
# 将一个插件的关键信息拼接成一段文本的模版
Chunk 66: 
TOOL_DESC = &quot;&quot;&quot;{name_for_model}: Call this tool to interact with the
{name_for_human} API. What is the {name_for_human} API useful for?
{description_for_model} Parameters: {parameters}&quot;&quot;&quot;
# ReAct prompting 的 instruction 模版，将包含插件的详细信息
Chunk 67: 
PROMPT_REACT = &quot;&quot;&quot;Answer the following questions as best you can. You have
access to the following APIs:
{tool_descs}
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can be repeated zero or more
times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question
Begin!
Question: {query}&quot;&quot;&quot;
因此，引入Prompt Template可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输
入动态调整提示词，从而更好地响应用户的具体需求
Chunk 68: LangChain通过这种方式来提高应用的通用性和
用户体验
Chunk 69: 
Predict
在Predict部分，实质上是处理模型从接收输入到执行推理的整个过程
Chunk 70: 考虑到存在两种主要类型的
大模型——Base类模型和Chat类模型，LangChain在其Model I/O模块中对这两种模型都进行了抽象，
分别归类为LLMs（Large Language Models）和Chat Models
Chunk 71: 我们还是以OpenAI 的 Completion 和
Chatcompletions方法为例：
# Base类模型
client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=&quot;Say this is a test&quot;,
)
# 聊天模型
client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己
Chunk 72: &quot;}
]
)
LLMs是简化的大语言模型抽象，即基于给定的Prompt提供内容生成的功能
Chunk 73: 而Chat Models则专
注于聊天API的抽象，需要维护上下文的记忆（聊天记录），呈现出更接近对话或聊天形式的交互
Chunk 74: 
Parse
我们知道，大模型的输出是不稳定的，同样的输入Prompt往往会得到不同形式的输出
Chunk 75: 在自然语
言交互中，不同的语言表达方式通常不会造成理解上的障碍
Chunk 76: 但在应用开发中，大模型的输出可能是下
一步逻辑处理的关键输入
Chunk 77: 因此，在这种情况下，规范化输出是必须要做的任务，以确保应用能够顺利
进行后续的逻辑处理
Chunk 78: 
输出解析器 Output Parser就是一个帮助结构化语言模型响应的抽象，可以获取格式指令或者进行
更深层次的解析
Chunk 79: 这我们会在后面的实践中直观的体验到
Chunk 80: 
整体而言，在Model I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如
OpenAI、ChatGLM、Qwen等，按照既定规范执行模型推理
Chunk 81: 其二通过输入和输出的模板化处理，使
其更贴合于应用开发的最佳实践
Chunk 82: 接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行
集成和操作的
Chunk 83: 
3 安装LangChain
LangChain的安装过程非常简单，可以通过常用的Python包管理工具，如pip或conda，直接进行
安装
Chunk 84: 稍复杂一点的还可以通过源码进行安装
Chunk 85: 但有一点大家一定要明确：LangChain的真正价值在于
它能够与多种模型提供商、数据存储解决方案等进行集成
Chunk 86: 默认情况下，使用上述两种安装方式中的任
意一种来进行LangChain安装后，安装的仅仅是LangChain的默认功能，并不包括这些集成所需的额外
依赖项
Chunk 87: 也就是说，如果我们想要使用特定的集成功能，还需要额外安装这些特定的依赖
Chunk 88: 以调用
OpenAI的API为例，我们首先需要通过运行命令pip install langchain-openai安装OpenAI的合作
伙伴包，安装此依赖包后，LangChain才能够与OpenAI的API进行交互
Chunk 89: 后续我们在使用相关功能的时
候，会提供额外的说明
Chunk 90: 
LangChain安装官方说明文档：https://python.langchain.com/docs/get_started/installation
3.1 使用包版本管理工具安装
LangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建
过程
Chunk 91: 这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成
LangChain的安装
Chunk 92: 具体操作如下：
! pip install langchain
Requirement already satisfied: langchain in d:\software\anaconda3\lib\site-
packages (0.1.10)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain) (6.0)
Requirement already satisfied: SQLAlchemy&lt;3,&gt;=1.4 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.4.39)
Requirement already satisfied: aiohttp&lt;4.0.0,&gt;=3.8.3 in
d:\software\anaconda3\lib\site-packages (from langchain) (3.8.5)
Requirement already satisfied: dataclasses-json&lt;0.7,&gt;=0.5.7 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.6.4)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.33)
Requirement already satisfied: langchain-community&lt;0.1,&gt;=0.0.25 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.25)
Requirement already satisfied: langchain-core&lt;0.2,&gt;=0.1.28 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.28)
Requirement already satisfied: langchain-text-splitters&lt;0.1,&gt;=0.0.1 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.1)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.14)
Requirement already satisfied: numpy&lt;2,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (1.24.3)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (8.2.2)
Requirement already satisfied: attrs&gt;=17.3.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (22.1.0)
Requirement already satisfied: charset-normalizer&lt;4.0,&gt;=2.0 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(2.0.4)
Requirement already satisfied: multidict&lt;7.0,&gt;=4.5 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(6.0.2)
Requirement already satisfied: async-timeout&lt;5.0,&gt;=4.0.0a3 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(4.0.2)
Requirement already satisfied: yarl&lt;2.0,&gt;=1.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (1.8.1)
Requirement already satisfied: frozenlist&gt;=1.1.1 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.3.3)
Requirement already satisfied: aiosignal&gt;=1.1.2 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.2.0)
Requirement already satisfied: marshmallow&lt;4.0.0,&gt;=3.18.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (3.21.0)
Requirement already satisfied: typing-inspect&lt;1,&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (0.9.0)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain)
(2.1)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (3.5.0)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2,&gt;=0.1.28-
&gt;langchain) (23.2)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain)
(2.16.1)
Requirement already satisfied: typing-extensions&gt;=4.6.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (4.7.1)
Requirement already satisfied: idna&lt;4,&gt;=2.5 in d:\software\anaconda3\lib\site-
packages (from requests&lt;3,&gt;=2-&gt;langchain) (3.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(1.26.16)
Requirement already satisfied: certifi&gt;=2017.4.17 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(2023.11.17)
Requirement already satisfied: greenlet!=0.4.17 in
d:\software\anaconda3\lib\site-packages (from SQLAlchemy&lt;3,&gt;=1.4-&gt;langchain)
(2.0.1)
Requirement already satisfied: sniffio&gt;=1.1 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (1.2.0)
Requirement already satisfied: mypy-extensions&gt;=0.3.0 in
d:\software\anaconda3\lib\site-packages (from typing-inspect&lt;1,&gt;=0.4.0-
&gt;dataclasses-json&lt;0.7,&gt;=0.5.7-&gt;langchain) (1.0.0)
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效
Chunk 93: 
重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：
import langchain
print(langchain.__version__)
0.1.10
如果能正常输出LangChain的版本，说明在当前环境下的安装成功
Chunk 94: 
3.2 源码安装
除了通过pip安装外，还有一种通过源码安装的方法
Chunk 95: 这需要使用git拉取远程仓库，然后进入项目
文件夹并执行pip install -e .命令
Chunk 96: 这种方法不仅会安装必要的依赖，同时也将程序的源代码保存
在本地
Chunk 97: 通常来说，除非大家打算作为LangChain的协作开发者参与到项目的功能更新中，否则没必要
采用这种安装方式，直接使用pip安装更为简便
Chunk 98: 
对于课程学习而言，我们推荐采用源码安装方式，这将非常有助于在后续的LangChain功能探索
中，通过源码分析深入理解框架的构建原理和详细机制
Chunk 99: 
接下来我们以Ubuntu操作系统为例，介绍源码安装LangChain的详细步骤：
Step 1. 安装Anaconda
如果本地服务器是Ubuntu系统，请参考：《Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B
模型》中的2.3 安装Anaconda环境小节；而如果是Windows操作系统，请参考：《Ch.6 在Windows
系统下部署运行ChatGLM3-6B模型》中的3.2.1 方式一：使用Anaconda创建项目依赖环境（推荐）小节
中，执行完Step 6. 验证conda步骤，回到本文继续执行后续的操作：
注：如果部署环境是租赁的云服务器，Anaconda环境已经预设好，并不需要我们再次手动配置
Chunk 100: 
按照对应的教程内容配置好Anaconda后，可以通过如下命令验证当前环境的Conda：
conda --version
如果可以正常输出Conda的版本，则说明Anaconda安装成功
Chunk 101: 
Step 2. 使用Conda创建LangChain的Python虚拟环境
安装好Anaconda后，我们需要借助Conda包版本工具，为LangChain项目创建一个新的Python虚
拟运行环境，执行代码如下：
conda create --name langchain python==3.11
如上所示，新创建了一个名为langchain的Python虚拟环境，其Python版本为3.11
Chunk 102: 创建完成后，
通过如下命令进入该虚拟环境，执行后续的操作：
conda activate langchain
Step 3. 下载LangChain的项目文件
进入LangChain的官方Github，地址：https://github.com/langchain-ai/langchain ， 在 GitHub
上将项目文件下载到有两种方式：克隆 (Clone) 和 下载 ZIP 压缩包
Chunk 103: 推荐使用克隆 (Clone)的方式
Chunk 104: 我
们首先在GitHub上找到其仓库的URL
Chunk 105: 
在执行命令之前，需要先安装git软件包，执行命令如下：
sudo apt install git
执行克隆命令，将LangChain Github上的项目文件下载至本地的当前路径下，如下：
git clone https://github.com/langchain-ai/langchain.git
Step 4. 升级pip版本
建议在执行项目的依赖安装之前升级 pip 的版本，如果使用的是旧版本的 pip，可能无法安装一些
最新的包，或者可能无法正确解析依赖关系
Chunk 106: 升级 pip 很简单，只需要运行命令如下命令：
python -m pip install --upgrade pip
Step 5. 源码安装项目依赖
不同于我们之前一直使用的pip install -r requirements.txt方式，这种方法用于批量安装多
个依赖包，是在部署项目或确保开发环境与其他开发者/环境一致时的常用方式
Chunk 107: 而对于LangChain，我
们需要使用pip install -e的方式，以可编辑模式安装包
Chunk 108: 这种方式主要用于开发过程中
Chunk 109: 当以可编
辑模式安装一个包时，依赖包会被直接从源代码所在位置安装，而不是复制到Python的site-packages
目录，是开发模式下用于安装并实时反映对本地包更改的方法
Chunk 110: 需要执行的步骤如下：
cd langchain/libs/langchain/
pip install -e .
如在安装过程未发生任何报错，则说明安装成功
Chunk 111: 在安装完依赖后，我们就正式进入LangChain的
Model I/O模块的实践
Chunk 112: 
注：如果采用的是源码安装，需要参考《Ch.11 大模型开发工具Transformers库的使用及配置大
模型开发环境》中远程服务器部署大模型，本地Jupyter NoteBook加载小节，在LangChain的虚拟
环境下启动Jupyter Lab，才能正常执行接下来的实践
Chunk 113: 
4.Models I/O模块实战
任何语言模型应用的核心都是大语言模型（LLMs），无论涉及到提示模板（Prompt Template）
还是输出解析（OutPut Parse），其核心还是在于大语言模型（LLMs）
Chunk 114: 因此，在讨论和实践Model
I/O模块时，首先应当关注如何集成这些大模型
Chunk 115: 只有成功集成后才能深入探讨和实践如何有效地设计
提示模板和解析模型输出
Chunk 116: 所以我们先要实践的是：如何借助LangChain框架使用不同的大模型
Chunk 117: 
4.1 LangChain接入大模型的方法
LangChain 提供了一套与任何大语言模型进行交互的标准构建模块
Chunk 118: 所以需要明确的一点是：虽然
LLMs 是 LangChain 的核心元素，但 LangChain 本身不提供 LLMs，它仅仅是为多种不同的 LLMs 进
行交互提供了一个统一的接口
Chunk 119: 简单理解：以OpenAI的GPT系列模型为例，如果我们想通过
LangChain 接入 OpenAI 的 GPT 模型，我们需要在LangChain框架下先定义相关的类和方法来规定如
何与模型进行交互，包括数据的输入和输出格式以及如何连接到模型本身
Chunk 120: 然后按照 OpenAI GPT 模型
的接口规范来集成这些功能
Chunk 121: 通过这种方式，LangChain 充当一个桥梁，使我们能够按照统一的标准来
接入和使用多种不同的大语言模型
Chunk 122: 
在LangChain的Model I/O模块中，集成了两类模型，分别是LLMs和聊天模型，这两种模型在输
入和输出的数据类型上有所区别
Chunk 123: 
接下来我们就以OpenAI的API为示例，分别实践一下在LangChain框架中如何使用这两种不同类型
的大语言模型
Chunk 124: 首先来看LLMs类模型
Chunk 125: 
4.1.1 LLMs类模型接入方法
LLMs指的是纯文本补全模型，其封装的 API 输入接收的是字符串，输出也是字符串
Chunk 126:  OpenAI 的
GPT-3 是作为 LLM 实现的
Chunk 127: 最简单的理解：LLMs指代的就是Base类模型，这类模型仅仅可以理解并
生成自然语言或代码，并未按照特定指令进行训练
Chunk 128: 截止到2024年3月07日，OpenAI可支持调用的
Base类模型仅有2个，分别是babbage-002和davinci-002，除此之外，还可以使用gpt-3.5-turbo-
instruct调用OpenAI 的 Completion方法
Chunk 129: 
首先需要安装OpenAI的集成依赖包langchain-openai，执行如下命令：
! pip install langchain-openai
Requirement already satisfied: langchain-openai in
d:\software\anaconda3\lib\site-packages (0.0.8)
Requirement already satisfied: langchain-core&lt;0.2.0,&gt;=0.1.27 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.1.28)
Requirement already satisfied: openai&lt;2.0.0,&gt;=1.10.0 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (1.13.3)
Requirement already satisfied: tiktoken&lt;1,&gt;=0.5.2 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.6.0)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (6.0)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.5.0)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (1.33)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (0.1.14)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (23.2)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (8.2.2)
Requirement already satisfied: distro&lt;2,&gt;=1.7.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (1.8.0)
Requirement already satisfied: httpx&lt;1,&gt;=0.23.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (0.25.1)
Requirement already satisfied: sniffio in d:\software\anaconda3\lib\site-
packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (1.2.0)
Requirement already satisfied: tqdm&gt;4 in d:\software\anaconda3\lib\site-packages
(from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (4.65.0)
Requirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (4.7.1)
Requirement already satisfied: regex&gt;=2022.1.18 in
d:\software\anaconda3\lib\site-packages (from tiktoken&lt;1,&gt;=0.5.2-&gt;langchain-
openai) (2022.7.9)
Requirement already satisfied: idna&gt;=2.8 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai)
(3.4)
Requirement already satisfied: certifi in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(2023.11.17)
Requirement already satisfied: httpcore in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(1.0.1)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.1)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.16.1)
Requirement already satisfied: charset-normalizer&lt;4,&gt;=2 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.0.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (1.26.16)
Requirement already satisfied: colorama in d:\software\anaconda3\lib\site-
packages (from tqdm&gt;4-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.4.6)
Requirement already satisfied: h11&lt;0.15,&gt;=0.13 in
d:\software\anaconda3\lib\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-
&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.14.0)
使用OpenAI的 GPT 系列模型的API有两个前提条件：首先，需要配置科学上网以确保可以访问
OpenAI服务；其次，必须拥有一个有效的API Key
Chunk 130: 这两项要求的详细配置在《Ch.1 本地调用OpenAI
API流程及OpenAI官网使用指南》中有详细说明，因此我们在此不再赘述，直接进入实操过程
Chunk 131: 
查看OpenAI的依赖包版本
OpenAI 的 API 在 0.28 版本和 1.xx 版本之间的调用方式存在差异
Chunk 132: 我们这里使用的是最新的调用
方法，即采用 1.13.XX 版本
Chunk 133: 因此，需要确认当前 Python 虚拟环境中安装的 openai 包版本是 1.xx
Chunk 134: 
可以通过执行以下命令来检查当前安装的版本：
import openai
print(openai.__version__)
0.28.0
如果如上显示是0.28.0版本，执行如下命令将openai包升级到最新版本：
# ! pip install --upgrade openai
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效
Chunk 135: 
import openai
print(openai.__version__)
1.13.3
设置科学上网环境
根据所使用的科学上网软件，查找相应的代理端口，并按照以下代码进行网络环境配置：
# 这里的端口，请修改为实际使用科学上网软件配置的代理端口
!set HTTP_PROXY=http://127.0.0.1:15732
!set HTTPS_PROXY=http://127.0.0.1:15732
新版本的OpenAI API调用方法不允许使用openai.api_key= &quot;sk-xxx&quot;这种老版本的显式指定API
Key，所以如果没有在系统变量中进行配置的话，使用下面的命令设置环境变量：
# 将your- API -key-here替换为实际的API密钥:
# ! setx OPENAI_API_KEY &quot;sk-xxxx&quot;
成功: 指定的值已得到保存
Chunk 136: 
注意：完成环境变量的初次配置后，需重启电脑以激活设置
Chunk 137: 这样，在下次使用时就可以直接调用
配置好的环境
Chunk 138: 为验证环境变量中的API Key是否已正确设置为自己的，可以执行以下命令进行检查
Chunk 139: 
# ! echo %OPENAI_API_KEY%
完成了科学上网环境和API Key的设置后，在接入LangChain之前，我们先使用如下测试代码检查
当前环境及API的运行状态是否正常，执行代码如下：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己
Chunk 140: &quot;}
]
)
print(completion.choices[0].message.content)
你好，我是一位可以回答问题、提供信息和与您交流的AI助手
Chunk 141: 我可以帮助解决问题，回答疑惑，提供建议，
而且乐意与您分享知识和信息
Chunk 142: 有什么我可以帮助你的吗
Chunk 143: 
若测试代码能正常输出结果，表明OpenAI的环境配置无误
Chunk 144: 需要注意的是，LangChain接入
OpenAI GPT模型只是利用其定义的标准模型接入框架来整合OpenAI API，因此进行上述连通性测试
是必要的
Chunk 145: 
OpenAI GPT Base模型官方说明：https://platform.openai.com/docs/models/moderation
LangChain为了使开发者可以轻松地创建自定义链，整体采用Runnable协议
Chunk 146: Runnable 协议是
编程中一种常见的设计模式，用于定义可以执行的任务或行为
Chunk 147: 在LangChain中通过构建标准接口，可
以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了
Runnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用
Chunk 148: 
LangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch
ain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable
而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解
的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下
Chunk 149: 
如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑
Chunk 150: 比如OpenAI：
LangChain Interface说明：https://python.langchain.com/docs/modules/model_io/llms/quic
k_start
LangChain已集成LLMs说明：https://python.langchain.com/docs/integrations/llms/
首先，我们来了解常规的调用方法
Chunk 151: 具体支持的调用方式如下所示：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
接下来我们对上述方法依次进行尝试
Chunk 152: 在LangChain框架中，为了规范地接入LLMs类型的大模
型，首先需要导入OpenAI的依赖包
Chunk 153: 导入命令如下：
from langchain_openai import OpenAI
字符串形式的输入
调用invoke方法，并提供字符串格式的Prompt，便可触发指定模型按照所给Prompt进行续写操
作
Chunk 154: 
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
llm.invoke(
&quot;你好呀，请你介绍一下你自己&quot;
)
&#x27;吧
Chunk 155: ”韩寒摇头，说：“我下次再来，你们自己给我介绍好了
Chunk 156: ”这一次，韩寒的风趣让陈航漂了一下，他说：
“还是先看看大家的房间吧
Chunk 157: ”陈航带着韩寒到了楼下，这时连小李都已经来到了楼下，他对韩寒说：“你看，
这是我们的宿舍
Chunk 158: ”陈航的语气是那么的平淡，既没有过多的炫耀，也没有明显的拒绝，只是那么的平淡，这
让韩寒很有点奇怪，他说：“你们是在这里住吗
Chunk 159: ”小李说：“是啊，我们是这里的住宿生
Chunk 160: ”韩寒对小李说：
“那我是来参加考试的，你&#x27;
从上述模型的返回输出，我们可以看到它本质上作为一个补全模型，通过使用大模型原生的
.generate() 方法来对输入的Prompt进行续写
Chunk 161: 这部分的详细内容已在《Ch.2 Completions模型API
使用指南》中进行了深入讲解，因此此处不再进行赘述
Chunk 162: 
上述调用方法相当于使用我们熟悉的OpenAI的Completion方法来调用GPT Base类型模型的过
程：
from openai import OpenAI
client = OpenAI()
completion = client.completions.create(
model=&quot;davinci-002&quot;,
prompt=&quot;你好呀，请你介绍一下你自己&quot;,
max_tokens=1024,
)
print(completion.choices[0].text)

Chunk 163: » 她把咳嗽调整成百米冲刺那种负重颤抖
Chunk 164: »不，我们在上面，请你告诉会场
Chunk 165: 哪一地方过去你都要做朋友
呢
Chunk 166: 」Larsen一转眼注意到卡萨姆旁站着一位古老的男人
Chunk 167: 他身着粗布制服，最左边有一种圆盏形的供奶
头
Chunk 168: 透映着那个老的质朴魅力和吓坏人恶心的样子
Chunk 169: »LarsenKeberichTaylor.Ch02 3/5/07 5:27 PM
Page 39LEARNING WHERE WECAME FROM»喂主持人
Chunk 170: 自从我给你们滴剂的时候，我就耐心等待了
Chunk 171: 请为
快点谈到刚才提的内容
Chunk 172: »正赞成的木质松枝变苍白了
Chunk 173: 既然他是最耐烦的老爷，我们劝劝他这样三小时也
烦
Chunk 174: 时光久了会累人的人，他更应该知道这些
Chunk 175: »好，每个人都听到
Chunk 176: »那些选手的名字有老项目拯救人」，
Brickman说
Chunk 177: AnderMartin、Taylor, här nere – efter en minut eller så. Likdant för
LarsenKeberich, rätt så bra ger du tillbaka vätsken, yeah, and the eighth
man.»»Eighth man?」Brickman坏坏垂下眼帘扬扬眉头，头盔上的唇影随着他挺起了脸颊
Chunk 178: 好，一位是
我
Chunk 179: 不，我不是
Chunk 180: Just a moment.»六记地雷爆炸
Chunk 181: 嫣红的色泽弥漫在鼻子周围极其耀眼之下，淀粉饼糊塞
了全脸，剔饭一般的巧克力混合在一起
Chunk 182: »»Would you all please proceed into the next
building and make sure that you use the turnstiles.»伤口破口流血，簇瓣的血色俗套，但去
还是要用
Chunk 183: 套房好美啊、清生
Chunk 184: 套房饲養下只要一个；_ 户功能装置就
Chunk 185: »艰难地活着足以获得一天的欢乐的
人确实算够奇特的
Chunk 186: 咳嗽的木质松枝_cancel6 3/5/07 5:27 PM Page 40IT IS NOT ENOUGH
TOMAKE MANKINDHAPPY,LARSEN KEBERICH向报纸走来，她在走廊哪一个厅卡萨姆等待着;在这个保证已
经到达那种别有心碎的正式屋子里
Chunk 187: 她穿过镶有玫瑰花的拉链的抢洗衣区和单人舔饭的厕所区圆朝圣地的道
Chunk 188: 
先是葡萄干牛油，透过臭风从供应箱中飘来，刚开始时她便一直寻找那一种苏查尔的酸味
Chunk 189: 那种味美常是发出
着从薄饼里流发出来的所以一圈难找到对不起，小姐，我们之前选择做了决定，就算更改好一个时刻，也无法
改变决定
Chunk 190: 拨动键盘-找麻烦你的，怎么做了、哦，听起来好像你一突然卸了山猫带来了 Asktohell你疼
吗
Chunk 191: 杨虎大吼着，冲向对方
Chunk 192: 丁
流式输出
要进行流式调用，可以调用stream方法
Chunk 193: 这种方式允许在接收到模型响应的同时，逐块输出结
果，实现即时展示而无需等待全部内容处理完毕
Chunk 194: 
from langchain_openai import OpenAI
for chunk in llm.stream(
&quot;你好呀，请你介绍一下你自己&quot;
):
print(chunk, end=&quot;&quot;, flush=True)
吧，你是谁，你在哪里，你是什么学问，你喜欢什么，你不喜欢什么，你不会什么，你会什么，你梦想什么，
你的优点是什么，你的缺点是什么，你的性格是什么，你的兴趣是什么，你喜欢的食物是什么，你不喜欢的食
物是什么，你的爱好是什么，你的愿望是什么
Chunk 195: 介绍一下你自己好的
Chunk 196: 
[ 编辑 xiaofan1 在 07-03-18 23:49 ]
批量生成
通过调用batch方法，可以同时对多个不同的Prompt进行调用处理
Chunk 197: 在这个例子中，输入包括“你
好”、“什么是机器学习”和“什么是深度学习”三个独立的Prompt
Chunk 198: 这种批量处理方式允许一次性提交多个
Prompt，从而提高推理效率
Chunk 199: 
from langchain_openai import OpenAI
llm.batch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，习惯了你的成熟，可是这次你的决定让我感到非常惊讶，我不知道你的这次决定是不是真的，我不知道你
是否真的那么爱我，虽然我知道你非常爱我，可是我不知道你是不是真的爱我，假如你为了我不想去上学，我
也可以放弃一切，去读书，去上大学，但是你为什么要这样做呢，我不清楚你的想法，我不知道你的想法，我
只知道我爱你，我只知道我不想失去你，我只知道我不想让你失去你的希望，我只知道我不想让你失去你的未
来，可是我想知道你为什么要这样做呢，我想知道你的想法，你是&#x27;,
&#x27; 机器学习的原理 机器学习的技术 机器学习的应用机器学习在生产过程中的作用机器学习在生产过程中的
作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产
过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学
习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作
用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学&#x27;,
&#x27;
Chunk 200:  2. Why is the industry so interested in deep learning? 3. How will deep
learning transform the financial industry? 4. How can you use deep learning to
build better AI? 5. What are the ingredients of a deep learning model?\n\nA
study of the best deep learning algorithms and how they work, the techniques
involved in building them, and the results they can bring to the table: 1.
Unsupervised learning 2. Convolutional Neural Networks (CNN) 3. Recurrent Neural
Networks (RNN) 4. Deep Reinforcement Learning (RL) 5. Generative Adversarial
Networks (GAN)\n\nA step-by-step guide to building a real-world, deep learning
app: 1. What is a deep learning app? 2. How do you build a deep learning app? 3.
The various components involved in building a deep learning app 4. How do you
deploy a deep learning app? 5. The future of deep learning apps\n\nHow to build
a deep learning app: A guide to the various tools, frameworks, and libraries
that you can use to build deep learning apps: 1. Deep Learning Frameworks 2.
Deep Learning Libraries 3. Deep Learning Tools 4&#x27;]
异步调用
llm.invoke(...)本质上是一个同步调用
Chunk 201: 在这种情况下，程序会在调用返回结果之前停止执行
任何后续代码
Chunk 202: 这意味着如果invoke操作耗时较长，它会导致程序暂时挂起，直到操作完成
Chunk 203: 我们可
以通过这样一个测试代码来直观的理解同步调用：
import time
def call_model():
# 模拟同步API调用
print(&quot;开始调用模型...&quot;)
time.sleep(3) # 模拟调用等待
print(&quot;模型调用完成
Chunk 204: &quot;)
def perform_other_tasks():
# 模拟执行其他任务
for i in range(3):
print(f&quot;执行其他任务 {i + 1}&quot;)
time.sleep(1)
def main():
start_time = time.time()
call_model()
perform_other_tasks()
end_time = time.time()
total_time = end_time - start_time
return f&quot;总共耗时：{total_time}秒&quot;
# 运行同步任务并打印完成时间
main_time = main()
main_time
开始调用模型...
模型调用完成
Chunk 205: 
执行其他任务 1
执行其他任务 2
执行其他任务 3
&#x27;总共耗时：6.001835107803345秒&#x27;
这段同步调用的程序先模拟了一个耗时3秒的模型调用，随后执行了三个其他任务，每个任务耗时1
秒
Chunk 206: 实际的执行时间为约6.00秒
Chunk 207: 这体现了同步执行的特点：每个操作依次执行，直到当前操作完成后
才开始下一个操作，从而导致总的执行时间是各个操作时间的总和
Chunk 208: 
而异步调用，允许程序在等待某些操作完成时继续执行其他任务，而不是阻塞等待
Chunk 209: 这在处理I/O
操作（如网络请求、文件读写等）时特别有用，可以显著提高程序的效率和响应性
Chunk 210: 
我们可以设计这样如下情景：使用async和await关键字， 通过async def定义异步函数
async_call和perform_other_tasks
Chunk 211: await asyncio.sleep(3)模拟了一个耗时3秒的异步操
作
Chunk 212: 这里的await表明程序在这里“等待”这个操作完成，但与同步操作不同，这个等待是非阻塞的
Chunk 213: 这
意味着程序可以在这3秒内执行其他异步代码
Chunk 214: 这种方式不会阻塞整个程序的运行，而是让出控制权，
允许事件循环继续运行其他任务
Chunk 215: 
! pip install asyncio
Collecting asyncio
Obtaining dependency information for asyncio from
https://files.pythonhosted.org/packages/22/74/07679c5b9f98a7cb0fc147b1ef1cc1853b
c07a4eb9cb5731e24732c5f773/asyncio-3.4.3-py3-none-any.whl.metadata
Downloading asyncio-3.4.3-py3-none-any.whl.metadata (1.7 kB)
Downloading asyncio-3.4.3-py3-none-any.whl (101 kB)
---------------------------------------- 0.0/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
----------- --------------------------- 30.7/101.8 kB 163.8 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
----------------------- --------------- 61.4/101.8 kB 113.0 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
-------------------------------------- 101.8/101.8 kB 136.2 kB/s eta 0:00:00
Installing collected packages: asyncio
Successfully installed asyncio-3.4.3
import asyncio
import time
async def async_call(llm):
await asyncio.sleep(3) # 模拟异步操作
print(&quot;异步调用完成&quot;)
async def perform_other_tasks():
await asyncio.sleep(3) # 模拟异步操作
print(&quot;其他任务完成&quot;)
async def run_async_tasks():
start_time = time.time()
await asyncio.gather(
async_call(None), # 示例调用，替换None为模拟的LLM对象
perform_other_tasks()
)
end_time = time.time()
return f&quot;总共耗时：{end_time - start_time}秒&quot;
# 运行异步任务并打印完成时间
await run_async_tasks()
异步调用完成
其他任务完成
&#x27;总共耗时：3.009281873703003秒&#x27;
使用asyncio.gather()并行执行时，理想情况下，因为两个任务几乎同时开始，它们的执行时间
将重叠
Chunk 216: 如果两个任务的执行时间相同（这里都是3秒），那么总执行时间应该接近单个任务的执行时
间（3秒左右），而不是两者时间之和
Chunk 217: 
理解了同步调用和异步调用的区别后，我们接下来看一下LangChain中集成的异步调用链使用方
法
Chunk 218: 
要实现异步调用并优化处理多个请求的场景，在LangChain中，可以使用ainvoke方法来达成
Chunk 219: 这
种方法允许同时启动多个请求，而不必等待每个请求依次完成
Chunk 220: 这对于提高应用程序的响应性和效率尤
其有用，特别是在处理需要与外部服务交互的任务时
Chunk 221: 
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;这次您知道了吗
Chunk 222: 这是鸡肋大战，您是来玩的吧，我也是
Chunk 223: 我来纠结你的，看看你敢不敢
Chunk 224: 我一般不来这种
鸡肋大战，因为我很懒，这次我还是来了，因为我懒惯了，其实我很想快点解脱了，我很想走
Chunk 225: 你的目的在哪
里
Chunk 226: 我想敌对你，我还是要意识到了，你想不想敌对我呢
Chunk 227: 你敢不敢这样问
Chunk 228: 我不敢，因为我懒
Chunk 229: 你的目的是
什么
Chunk 230: 我很想快点解脱，我不想你，我只想快点走，早点走
Chunk 231: 你敢不敢这样说
Chunk 232: 我敢，因为我懒
Chunk 233: 你的目的是
什么
Chunk 234: 我&#x27;
异步流式响应
from langchain_openai import OpenAI
async for chunk in llm.astream(
&quot;你好，&quot;,
):
print(chunk, end=&quot;&quot;, flush=True)
我是来投诉的 [Image]
你现在没有抱怨，说的确实很有道理，如果他们为了你不让你上，你能不上吗，我想大多数人应该会上，所以
他们是不会因为你不上而阻止你上的，但是如果你上了，你的优惠和你的一切都是不可能了，你上了，他们是
不能为你划上你的学费的，所以我觉得就算你上了也没有用，他们不会为你划学费，因为你上了从而给了他们
非常大的麻烦，你上了，他们是不能有一点好处的，他们还要为你上的学费等等付出，所以他们会说你上不
了，就是为了不让你上，不让你上，他们不会为你划学费，所以我觉得你如果上了，你就没有任何
异步批量处理
from langchain_openai import OpenAI
await llm.abatch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，前来亲自拜访的，因为我们这里很少有人来拜访，你就打算在这里待上几个礼拜了吧
Chunk 235: ”“我是回来找你，
我是到桐谷这里来的
Chunk 236: ”李晓婷冷冷的回道
Chunk 237: 这一次很明显的不是她想让他留下来的，而是让他走了
Chunk 238: 众人看
着李晓婷，这里的她就这么冷了，竟然连郑晓东都不看一眼，郑晓东是她的男朋友，他们是一对，她怎么可能
不看一眼
Chunk 239: “那你为什么不和我说，你的事情怎么样了
Chunk 240: ”郑晓东看向李晓婷的脸，她的表情很冷，她怎么不看
一眼自己&#x27;,
&#x27;
Chunk 241: \n\n2:王景仪 2021-01-07 23:12:57\n\nannounced\n\n3:李桂林 2021-01-01
23:12:57\n\nMy Sunshine is a TV drama adapted from Gu Man’s novel of the same
title. It stars Wallace Huo and Tang Yan, premiering in January, 2015. The drama
depicts a man and a woman’s sweet love story.\n\n4:王威 2020-12-27
23:12:57\n\nThe new data released yesterday represent the first snapshot of
global trade for 2015. And the figures suggest that 2015 was not a good year for
world trade, as it had been hoped.\n\n5:苏晓波 2021-01-01 23:12:57\n\nMark Rutte,
the Dutch prime minister whose People’s Party for Freedom and Democracy (VVD) is
slightly ahead of the PVV, has seen his poll numbers rise in recent days because
of his hardline stance with Ankara. On Tuesday he called Mr Erdogan’s comments
“a disgusting distortion of history”, adding: “We will not lower ourselves to
this level. It is totally unacceptable.”\n\n&#x27;,
&#x27;
Chunk 242: 深度学习是一种模仿大脑思维的机器学习方法，深度学习方法被广泛用于图像识别、语音识别、自然语言
处理等领域
Chunk 243: 深度学习过程中，模型经常需要大量的训练数据，因此大量的数据获取和数据刷新是有必要
Chunk 244: .
1) Find a free server to host the application in. I have been trying to create a
simple neural network by using the TensorFlow Object Detection API to run an
object detection model. Cloud TPU is a Google Cloud service that accelerates
machine learning workloads. Major cloud providers offer free tier of services to
help you get started. Google Cloud Storage is a RESTful web service for storing
and accessing data on Google Cloud Platform. In this video, you will learn about
Cloud TPU, a high-performance, scalable, and cost-effective machine learning
service. This IP address 157. 0, a neural network framework. Contribute.
TensorFlow Lite for mobile and embedded devices For Production TensorFlow
Extended for end-to-end ML components. The Google Cloud Platform&#x27;]
除此之外，在Jupyter Notebook或Python交互式解释器中进行操作的话，可以使用?来查看关于
llm中内置的方法和帮助文档，执行如下代码：
llm?
•[1;31mSignature:•[0m
•[0mllm•[0m•[1;33m(•[0m•[1;33m
•[0m •[0mprompt•[0m•[1;33m:•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mstop•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mcallbacks•[0m•[1;33m:•[0m •[1;34m&#x27;Callbacks&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m*•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mtags•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mmetadata•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[Dict[str, Any]]&#x27;•[0m
•[1;33m=•[0m •[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m**•[0m•[0mkwargs•[0m•[1;33m:•[0m
•[1;34m&#x27;Any&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m•[1;33m)•[0m •[1;33m-&gt;•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m•[0m•[1;33m•[0m•[0m
•[1;31mType:•[0m OpenAI
•[1;31mString form:•[0m
•[1mOpenAI•[0m
Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {},
&#x27;max_tokens&#x27;: 256}
•[1;31mFile:•[0m d:\software\anaconda3\lib\site-
packages\langchain_openai\llms\base.py
•[1;31mDocstring:•[0m
OpenAI large language models.
To use, you should have the ``openai`` python package installed, and the
environment variable ``OPENAI_API_KEY`` set with your API key.
Any parameters that are valid to be passed to the openai.create call can be
passed
in, even if not explicitly saved on this class.
Example:
.. code-block:: python
from langchain_community.llms import OpenAI
openai = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
•[1;31mInit docstring:•[0m
Create a new model by parsing and validating input data from keyword arguments.
Raises ValidationError if the input data cannot be parsed to form a valid model.
•[1;31mCall docstring:•[0m
[*Deprecated*] Check Cache and run the LLM on the given prompt and input.
Notes
-----
.. deprecated:: 0.1.7
Use invoke instead.
这里能看到非常多的关键信息，比如：
默认参数：Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {}, &#x27;max_tokens&#x27;: 256}
当不输入model_name时，默认会调用davinci-002模型：
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;我是小影
Chunk 245: 很高兴认识你，有什么可以帮到你的吗
Chunk 246: &#x27;
同时，也可以调整温度temperature:
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
temperature=0
)
&#x27;我是一个人工智能助手，很高兴认识你
Chunk 247: 有什么可以帮到你的吗
Chunk 248: &#x27;
model：必选参数，具体调用的Completions模型名称，可以调用的模型包括text-davinci-003、
text-davinci-002、text-curie-001、text-babbage-001、text-ada-001等，不同模型参数规模不
同；这里需要注意，大模型领域不同于机器学习领域，后者哪怕是简单模型在某些场景下可能也会
拥有比复杂模型更好的表现
Chunk 249: 在大模型领域，（就OpenAI提供的A、B、C、D四大模型来看）参
数规模越大、越新版本的模型效果更好（当然费用也更高），因此课程中主要以text-davinci-003
使用为例进行讲解；
prompt：必选参数，提示词；
suffix：可选参数，默认为空，具体指模型返回结果的后缀；
max_tokens：可选参数，默认为16，代表返回结果的token数量；
temperature：可选参数，取值范围为0-2，默认值为1
Chunk 250: 参数代表采样温度，数值越小，则模型会
倾向于选择概率较高的词汇，生成的文本会更加保守；而当temperature值较高时，模型会更多地
选择概率较低的词汇，生成的文本会更加多样；
top_p：可选参数，取值范围为0-1，默认值为1，和temperature作用类似，用于控制输出文本的
随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节
文本随机性，top_p和temperature两个参数选择一个进行调整即可；这里更推荐使用
temperature参数进行文本随机性调整；
n：可选参数，默认值为1，表示一个提示返回几个Completion；
stream：可选参数，默认值为False，表示回复响应的方式，当为False时，模型会等待返回结果
全部生成后一次性返回全部结果，而为True时，则会逐个字进行返回；
logprobs：可选参数，默认为null，该参数用于指定模型返回前N个概率最高的token及其对数概
率
Chunk 251: 例如，如果logprobs设为10，那么对于生成的每个token，API会返回模型预测的前10个
token及其对数概率；
echo：可选参数，默认为False，该参数用于控制模型是否应该简单地复述用户的输入
Chunk 252: 如果设为
True，模型的响应会尽可能地复述用户的输入；
stop：可选参数，默认为null，该参数接受一个或多个字符串，用于指定生成文本的停止信号
Chunk 253: 当
模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成
Chunk 254: 这可以用来控制模型的输出长
度或格式；
presence_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容
（例如新的概念或主题）的倾向性
Chunk 255: 较高的值会使模型更倾向于生成新内容，而较低的值则会使模
型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取
值；
frequency_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾
向性
Chunk 256: 较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果
篇幅较大并且存在前后语言重复时，可以提高该参数的取值；
best_of：该参数用于控制模型的生成过程
Chunk 257: 它会让模型进行多次尝试（例如，生成5个不同的响
应），然后选择这些响应中得分最高的一个；
logit_bias：该参数接受一个字典，用于调整特定token的概率
Chunk 258: 字典的键是token的ID，值是应用
于该token的对数概率的偏置；在GPT中我们可以使用tokenizer tool查看文本Token的标记
Chunk 259: 一般
不建议修改；
user：可选参数，使用用户的身份标记，可以通过人为设置标记，来注明当前使用者身份
Chunk 260: 需要
注意的是，Completion.create函数中的user和后续介绍的对话类模型的user参数含义并不相同，
需要注意区分；
更详细的参数说明和使用细节可在《Ch.2 Completions模型API使用指南》中找到
Chunk 261: 此处不在展开
重复说明
Chunk 262: 
目前在LangChain中已经集成的LLMs有82个，均已经实现了对异步、流式处理和批处理的基本支
持，查看地址如下：https://python.langchain.com/docs/integrations/llms/
4.1.2 Chat Models接入方法
所谓聊天模型，简单理解就是我们经常使用的xxx-chat类模型，比如Baichuan2-7b-chat，
Qwen-14b-chat，这类模型是基于Base模型经过针对特定数据格式的微调得到，以更贴近人类对话的
方式进行输入和输出
Chunk 263: 所以这类模型不会使用“文本输入、文本输出”的形式，而是采用聊天消息作为输
入并返回聊天消息作为输出（非纯文本）
Chunk 264: LangChain框架与OpenAI、Cohere、Hugging Face等多家
模型提供商已经实现集成，提供了一个标准化的接口，以便与这些不同的模型进行交互
Chunk 265: 
我们先回顾下OpenAI规范下使用Chat模型进行一次交互需遵循的代码执行方式：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo-0125&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一个乐于助人的智能AI小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}
]
)
print(completion.choices[0].message.content)
您好，我是一个智能AI助手，可以帮助您回答问题、提供信息、解决问题等
Chunk 266: 如果您有任何疑问或需要帮助的
话，都可以随时向我提问
Chunk 267: 我会尽力帮助您解决问题
Chunk 268: 
这里不得不再次提到，LangChain作为一个应用开发框架，需要集成各种不同的大模型，如上述
OpenAI的GPT系列模型调用示例，通过Message数据输入规范，定义不同的role，即system、user和
assistant来区分对话过程，但对于其他大模型，并不意味这一定会遵守这种输入输出及角色的定义，所
以LangChain的做法是，因为Chat Model基于消息而不是原始文本，LangChain目前就抽象出来的消息
类型有 AIMessage 、 HumanMessage 、 SystemMessage 、 FunctionMessage 和 ChatMessage ，
但大多时候我们只需要处理 HumanMessage 、 AIMessage 和 SystemMessage，即：
SystemMessage ：用于启动 AI 行为，作为输入消息序列中的第一个传入
Chunk 269: 
HumanMessage ：表示来自与聊天模型交互的人的消息
Chunk 270: 
AIMessage ：表示来自聊天模型的消息
Chunk 271: 这可以是文本，也可以是调用工具的请求
Chunk 272: 
同样，Chat Models实现了 Runnable 接口，也支持 invoke 、 ainvoke 、 stream 、 astream 、
batch 、 abatch 等调用
Chunk 273: 即：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
所以对于LangChain中调用OpenAI的GPT模型，首先需要导入对应的模块，代码如下：
from langchain_core.messages import HumanMessage, SystemMessage
消息形式输入调用
定义消息对象：
messages = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;你好，请你介绍一下你自己&quot;),]
messages
[SystemMessage(content=&#x27;你是一位乐于助人的智能小助手&#x27;), HumanMessage(content=&#x27;你好，请
你介绍一下你自己&#x27;)]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
执行推理：
chat.invoke(messages)
AIMessage(content=&#x27;你好，我是一位可以回答问题、提供帮助的智能小助手
Chunk 274: 无论是关于知识、技能还是
日常生活中的问题，我都会尽力为您提供准确和有用的答案
Chunk 275: 有什么可以帮助您的吗
Chunk 276: &#x27;)
如果只想获取到纯净的输出，可以通过.content来格式化
Chunk 277: 
reponse.content
&#x27;你好
Chunk 278: 我是一位智能助手，可以回答各种问题，提供信息和帮助
Chunk 279: 无论是关于知识、技能、娱乐、生活等方
面，我都会尽力为你提供帮助
Chunk 280: 有什么问题需要我帮忙的吗
Chunk 281: &#x27;
流式调用
for chunk in chat.stream(messages):
print(chunk.content, end=&quot;&quot;, flush=True)
你好
Chunk 282: 我是一位智能助手，专门为用户提供信息咨询、问题解答和帮助服务
Chunk 283: 无论你有什么疑问或需求，我都
会尽力帮助你，让你更方便地获取所需的信息和支持
Chunk 284: 有什么我可以帮助你的吗
Chunk 285: 
批量调用
先定义三个不同的消息对象：
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
messages2 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是深度学习&quot;),]
messages3 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是大模型技术&quot;),]
将上述三个消息对象放在一个列表中，使用.batch方法执行批量调用
Chunk 286: 
reponse = chat.batch([messages1,
messages2,
messages3,])
reponse
[AIMessage(content=&#x27;机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来
改善自身的性能，而无需明确地进行编程
Chunk 287: 简而言之，机器学习是一种让计算机系统从数据中学习和提取规
律、模式以及知识的技术
Chunk 288: 通过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预
测、分类、识别等任务
Chunk 289: 机器学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测
等领域
Chunk 290: &#x27;),
AIMessage(content=&#x27;深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神
经网络层来学习和理解复杂的数据
Chunk 291: 深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识
别、语音识别、自然语言处理等任务
Chunk 292: 深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对
新数据进行准确预测和分类
Chunk 293: 深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金
融、交通等各个领域
Chunk 294: &#x27;),
AIMessage(content=&#x27;大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部
署复杂的深度学习模型
Chunk 295: 这些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优
化
Chunk 296: \n\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等
Chunk 297: 通过使用大模
型技术，研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果
Chunk 298: \n\n然
而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等
Chunk 299: 因此，研究人员和
工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性
Chunk 300: &#x27;)]
格式化输出：
# 使用列表生成式打印每一个消息的内容
contents = [msg.content for msg in reponse]
# 打印出内容
for content in contents:
print(content, &quot;\n---\n&quot;)
机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来改善自身的性能，而无需明
确地进行编程
Chunk 301: 简而言之，机器学习是一种让计算机系统从数据中学习和提取规律、模式以及知识的技术
Chunk 302: 通
过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预测、分类、识别等任务
Chunk 303: 机器
学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测等领域
Chunk 304: 
---
深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神经网络层来学习和理解复杂
的数据
Chunk 305: 深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识别、语音识别、自然语言处理
等任务
Chunk 306: 深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对新数据进行准确预测和分类
Chunk 307: 
深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金融、交通等各个领域
Chunk 308: 
---
大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部署复杂的深度学习模型
Chunk 309: 这
些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优化
Chunk 310: 
大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等
Chunk 311: 通过使用大模型技术，
研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果
Chunk 312: 
然而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等
Chunk 313: 因此，研究人员
和工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性
Chunk 314: 
---
异步调用
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
reponse = await chat.ainvoke(messages1)
reponse
AIMessage(content=&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并
改进性能，而无需显式地进行编程
Chunk 315: 通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断
优化自身的表现
Chunk 316: 机器学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域
Chunk 317: 常
见的机器学习方法包括监督学习、无监督学习、强化学习等
Chunk 318: &#x27;)
reponse.content
&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并改进性能，而无需显式
地进行编程
Chunk 319: 通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断优化自身的表现
Chunk 320: 机器
学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域
Chunk 321: 常见的机器学习方法包括
监督学习、无监督学习、强化学习等
Chunk 322: &#x27;
流式调用
async for chunk in chat.astream(messages1):
print(chunk.content, end=&quot;&quot;, flush=True)
机器学习是一种人工智能的分支，它致力于让计算机系统通过学习数据和模式，从而改进自身的性能
Chunk 323: 简而言
之，机器学习让计算机具有从经验中学习的能力，而不需要明确地编程指令
Chunk 324: 
在机器学习中，计算机系统利用大量数据进行训练，通过识别数据中的模式和规律来建立模型
Chunk 325: 这些模型可以
用来进行预测、分类、识别等任务
Chunk 326: 机器学习在各个领域中都有广泛的应用，如自然语言处理、图像识别、医
疗诊断、金融预测等
Chunk 327: 
常见的机器学习算法包括监督学习、无监督学习、强化学习等
Chunk 328: 监督学习是通过已标记的数据进行训练，无监
督学习则是在没有标记的数据中发现模式，强化学习则通过试错的方式学习最优策略
Chunk 329: 机器学习技术的发展使
得计算机系统能够更加智能地处理复杂任务，为人们的生活和工作带来了许多便利
Chunk 330: 
通过上述描述，我们展示了在LangChain中使用LLMs类模型和Chat Model类模型的不同方法，其
核心区别在于输入Prompt的格式
Chunk 331: 除此之外其他的工作可以直接利用统一的抽象接口，实现与模型交
互的快速过程
Chunk 332: 而针对不同的模型，LangChain也提供个对应的接入方法，其相关说明文档地址：http
s://python.langchain.com/docs/integrations/chat/
比如我们以Baichuan2的在线API模型为例快速接入一下：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;我日薪8块钱，请问在闰年的二月，我月薪多少&quot;)])
response
AIMessage(content=&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所
以，你在闰年的二月月薪是232元
Chunk 333: &#x27;)
response.content
&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所以，你在闰年的二月月
薪是232元
Chunk 334: &#x27;
如未实践过Baichuan2的在线API，请查看《APICh.16 BaiChuan 2 大模型生态介绍及本地私有化
部署方案》中1. Baichuan在线大模型生态介绍小节
可以看出，LangChain能够迅速接入不同的模型API
Chunk 335: 至于接入本地私有部署的开源大模型的方
法，我们将在后续课程中进行详细介绍
Chunk 336: 
4.2 LangChain中如何使用Prompt Template
提示工程（Prompt Engineering）大家应该比较熟悉，这个概念是指在与大语言模型（LLMs），
如GPT-3、Qwen等模型进行交互时，精心设计输入文本（即提示）的过程，以获得更精准、相关或有
创造性的输出
Chunk 337: 正如我们在大模型技术应用课程中介绍的，通过采用Few-Shot、Chain of Thought
(CoT)等高级提示技巧，可以显著提高大模型在推理任务上的表现
Chunk 338: 目前，提示工程已经发展成为一个
专业领域，非常多的公司设立了专门的职位，负责为特定任务编写精确且具有创造力的提示
Chunk 339: 
以使用ChatGPT等网页端对话交互应用中，大部分人常见的做法是将提示（Prompt）做硬编码，
例如将一段提示文本固定在System Messages中
Chunk 340: 而在应用开发领域，开发者往往无法预知用户的具体
输入内容，同时又希望大模型能够根据不同的应用任务以一种较为统一的逻辑来处理用户输入
Chunk 341: 所以，
LangChain通过提供Prompt Templates功能，优雅地解决了这个问题
Chunk 342: Prompt Templates将用户输入
到完整格式化提示的转换逻辑进行封装，使得模型能够更灵活、高效地处理各种输入
Chunk 343: 
LangChain 提供了创建和使用提示模板的工具，其使用PromptTemplate 方法创建字符串提示模
板，因为目前为止，语言模型接收的提示基本都是字符串或聊天消息列表
Chunk 344: 
4.2.1 使用str.format语言构建模版
在LangChain的默认设置下， PromptTemplate 使用 Python 的 str.format() 方法进行模板
化
Chunk 345: 
Python的str.format()方法是一种字符串格式化的手段，允许我们在字符串中插入变量
Chunk 346: 使用这
种方法，可以创建包含占位符的字符串模板，占位符由花括号{}标识
Chunk 347: 调用format()方法时，可以传入
一个或多个参数，这些参数将被顺序替换进占位符中
Chunk 348: str.format()提供了灵活的方式来构造字符串，支
持多种格式化选项，包括数字格式化、对齐、填充、宽度设置等
Chunk 349: 
基本用法
# 简单示例，直接替换
greeting = &quot;Hello, {}!&quot;.format(&quot;Alice&quot;)
print(greeting)
# 输出: Hello, Alice!
Hello, Alice!
带有位置参数的用法
# 使用位置参数
info = &quot;Name: {0}, Age: {1}&quot;.format(&quot;Bob&quot;, 30)
print(info)
# 输出: Name: Bob, Age: 30
Name: Bob, Age: 30
带有关键字参数的用法
# 使用关键字参数
info = &quot;Name: {name}, Age: {age}&quot;.format(name=&quot;Charlie&quot;, age=25)
print(info)
# 输出: Name: Charlie, Age: 25
Name: Charlie, Age: 25
使用字典解包的方式：
# 使用字典解包
person = {&quot;name&quot;: &quot;David&quot;, &quot;age&quot;: 40}
info = &quot;Name: {name}, Age: {age}&quot;.format(**person)
print(info)
# 输出: Name: David, Age: 40
Name: David, Age: 40
在LangChain中，基本采用了Python的原生str.format()方法对输入数据进行格式化，这样在模
型接收输入前，可以根据需要对数据进行预处理和结构化，以此来引导大模型进行更准确的推理
Chunk 350: 
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(
&quot;请给我一个关于{topic}的{type}解释
Chunk 351: &quot;
)
prompt= prompt_template.format(type=&quot;详细&quot;, topic=&quot;量子力学&quot;)
prompt
&#x27;请给我一个关于量子力学的详细解释
Chunk 352: &#x27;
如上所示，可以使用PromptTemplate的from_template方法创建一个提示模板实例，这个模板
包含了两个占位符：{topic} 和 {type}，这些占位符在实际调用时可以被实际的值替换
Chunk 353: 
调用LLMs模型
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
我觉得我已经对量子力学的基本理解了，但是我还是很想听听有关量子力学的详细解释，比如：量子力学是怎
么起作用的
Chunk 354: 
谢谢了.
量子力学是在克里姆林宫试验时的实验结果发现.一开始是用熟悉的物理学法则去解释它.比如说,量子力学是
量子力学的结果,而不是量子力学的原因.玻尔兹曼方程就是克里姆林宫试验结论的表示.后来人们发现,等效
性,柏格效应等现象.然后才发现,原来我们的光学和电磁学等理论是有问题的.这些现象的实现是在离子束中.
所以量子力学是很实际的.
[ 编辑 stuartm 在 06-07-08 12:11 ]
prompt= prompt_template.format(type=&quot;简短&quot;, topic=&quot;机器学习&quot;)
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
2)目前的时代，机器学习已经发展到了什么程度，也有什么应用场景
Chunk 355:  3)目前的研究方向是什么
Chunk 356:  4)什么
时候会用到这些技术
Chunk 357:  5)最后一个问题是，对于我自己来说，是不是还有其他的方向比如说算法之类的，还
有在个人的技术知识里面是不是需要这些东西，不是特别需要
Chunk 358:  也许这些都是我自己的猜测，如果你有什么
好的建议，我也非常感谢
Chunk 359:  最后，我想问一下，你是在哪里学的，是在哪个大学，我就是在这里问问
Chunk 360:  谢
谢
Chunk 361:  2019.6.16
电影《伪装者》中的重要概念
原理：1. 无线电波的能量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无
线电波的方向不会发生改变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改
变，传播途中，波的方向不会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途
中，波的方向会发生改变（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变
（这就是遮挡）3. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）4. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）5. 无线电波的方向会发生改变，
传播途中，波的方向不会发生改变（这就是遮挡）6. 无线电波的方向会发生改变，传播途中，波的方向不会
发生改变（这就是遮挡）7. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）
8. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）原理：1. 无线电波的能
量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无线电波的方向不会发生改
变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改变，传播途中，波的方向不
会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途中，波的方向会发生改变
（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）3. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这
除此之外，这种模板生成方式支持任意数量的变量，包括无变量，可以做如下测试：
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(&quot;给我讲一个笑话&quot;)
prompt_template.format()
&#x27;给我讲一个笑话&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, )
result = llm.invoke(prompt)
print(result)
如果我是一个机器，会被称为什么
Chunk 362: 
谢谢
调用Chat Model
上述示例展示了如何使用字符串模板生成和调用LLMs类模型
Chunk 363: 对于聊天模型，也需要使用 Python
的 str.format() 方法进行模板化，但其构造的输入形式则有所区别，应采用聊天消息列表的形式，
每条消息都包含内容和一个名为role的附加参数
Chunk 364: 例如，在OpenAI的Chat Completions API中，聊天
消息需要包含为assistant、user或system
Chunk 365: 在这种情况下，应使用ChatPromptTemplate来构建对话
模板
Chunk 366: 
from langchain_core.prompts import ChatPromptTemplate
chat_template = ChatPromptTemplate.from_messages(
[
(&quot;system&quot;, &quot;你是一个有帮助的AI机器人，你的名字是{name}
Chunk 367: &quot;),
(&quot;human&quot;, &quot;你好，最近怎么样
Chunk 368: &quot;),
(&quot;ai&quot;, &quot;我很好，谢谢
Chunk 369: &quot;),
(&quot;human&quot;, &quot;{user_input}&quot;),
]
)
messages = chat_template.format_messages(name=&quot;小明&quot;, user_input=&quot;你叫什么名字
Chunk 370: &quot;)
messages
[SystemMessage(content=&#x27;你是一个有帮助的AI机器人，你的名字是小明
Chunk 371: &#x27;),
HumanMessage(content=&#x27;你好，最近怎么样
Chunk 372: &#x27;),
AIMessage(content=&#x27;我很好，谢谢
Chunk 373: &#x27;),
HumanMessage(content=&#x27;你叫什么名字
Chunk 374: &#x27;)]
print(chat_template)
input_variables=[&#x27;name&#x27;, &#x27;user_input&#x27;] messages=
[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;name&#x27;],
template=&#x27;你是一个有帮助的AI机器人，你的名字是{name}
Chunk 375: &#x27;)),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[],
template=&#x27;你好，最近怎么样
Chunk 376: &#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;我很
好，谢谢
Chunk 377: &#x27;)), HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=
[&#x27;user_input&#x27;], template=&#x27;{user_input}&#x27;))]
从输出上看，其构造函数在实例化prompt_template时，主要由两个关键参数进行指定：
input_variables：这是一个列表，包含模板中需要动态填充的变量名
Chunk 378: 这些变量名在模板字符串
中以花括号（如{name}）标记
Chunk 379: 通过指定这些变量，可以在后续过程中动态地替换这些占位符
Chunk 380: 
template：这是定义具体提示文本的模板字符串
Chunk 381: 它可以包含静态文本和input_variables列表中
指定的变量占位符
Chunk 382: 当调用format方法时，这些占位符会被实际的变量值替换，生成最终的提示
文本
Chunk 383: 
调用Chat Model，这里我们选择GPT 3.5
Chunk 384: 代码如下：
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
我叫小明，很高兴为您提供帮助
Chunk 385: 有什么问题可以帮到您
Chunk 386: 
messages = chat_template.format_messages(name=&quot;大熊&quot;, user_input=&quot;你要去哪里玩
Chunk 387: &quot;)
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
作为一个AI机器人，我没有身体，所以无法去任何地方玩耍
Chunk 388: 但我可以帮助你规划旅行或提供有关旅游目的地
的信息
Chunk 389: 有什么我可以帮助你的吗
Chunk 390: 
通过上述流程，我们演示了如何在LangChain框架内使用ChatPromptTemplate来构建与聊天模型
交互的对话模板
Chunk 391: 通过此方法，能够创建一个包含不同参与者角色和定制化消息内容的对话流程，这对
于开发复杂的聊天应用或增强AI助手的交互能力至关重要
Chunk 392: 
4.2.2 构造Few-Shot模版
在LangChain中，很多的功能抽象、链路抽象本质上都是在对大模型的“涌现能力”能够应用落地的
一种具体实现方法，而其推理的不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度
依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果
Chunk 393: 最为简单的
提示工程的方法就是通过输入一些类似问题和问题答案，让模型参考学习，并在同一个prompt的末尾
提出新的问题，依次提升模型的推理能力
Chunk 394: 
比如我们在《Ch.3 基于思维链的进阶提示工程》中测试的经典推理问题：
import os
import openai
openai.api_key = os.getenv(&quot;OPENAI_API_KEY&quot;)
prompt = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次
Chunk 395: &#x27;
prompt
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次
Chunk 396: &#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n在关闭之前，艾米能滑14次
Chunk 397: &#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n\n15分钟 = 900秒\n\n每次滑下来需要1分钟，所以在15分钟内，可以滑下900秒/（1分钟+4分钟）=
900/5 = 180次
Chunk 398: &#x27;
可以看到多次执行，其推理均是错误的
Chunk 399: 正确的计算过程应该是先计算艾米一次爬上爬下总共需要
5分钟，然后滑梯还有15分钟关闭，因此关闭之前能够再滑15/5=3次
Chunk 400: 
而我们当时在OpenAI的API规范下，是通过这种方式来进行Few-Shot的提示：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球
Chunk 401: ” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球
Chunk 402: 因此答案是11
Chunk 403: ” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果
Chunk 404: ” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9
Chunk 405: ” \
Q：“杂耍者可以杂耍16个球
Chunk 406: 其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的
Chunk 407: 请问总共有多少个蓝色高尔夫球
Chunk 408: ” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个
Chunk 409: ” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 410: ” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球
Chunk 411: ”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球
Chunk 412: 因此答案是11
Chunk 413: ” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果
Chunk 414: ”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9
Chunk 415: ”
Q：“杂耍者可以杂耍16个球
Chunk 416: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的
Chunk 417: 请问总共有多少个蓝色高尔夫球
Chunk 418: ” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个
Chunk 419: ” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次
Chunk 420: ” A：&#x27;
在Few-Shot的提示下，再次尝试进行提问：
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt_Few_shot_CoT4,
max_tokens=1000,
)
response1.choices[0].text
&#x27;”艾米花了1分钟滑下来，滑一次上去需要4分钟，因此在15分钟关闭之前，她能滑15分钟-1分钟=14分钟，
14分钟除以4分钟每次，她能滑14÷4=3次，答案是3次
Chunk 421: “ &#x27;
从输出结果上看，通过Few-Shot提示可以激发大模型的涌现能力从而回答对复杂推理问题，在应用
开发中同样需要这种能力来提升大模型的推理稳定性
Chunk 422: 所以LangChain抽象了
FewShotPromptTemplate方法来实现
Chunk 423: 我们可以先测试下在LangChain的接口下直接提问：
直接提问
prompt1 = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在
关闭之前她能滑多少次
Chunk 424: &#x27;
prompt1
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次
Chunk 425: &#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在关闭之前，艾米能滑(15-4-1)/4=2次
Chunk 426: &#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在15分钟内，她能滑15次
Chunk 427: &#x27;
其实也很容易想到，LangChain的工作仅仅是集成并接入OpenAI的GPT模型，因此其本质上与直接
调用GPT模型的效果相同
Chunk 428: 出现错误回答也属于正常情况
Chunk 429: 接下来，我们根据LangChain对Few-Shot模
板的定义规范，进行提示词的构建
Chunk 430: 
在LangChain中，需要使用 PromptTemplate 创建字符串提示模板
Chunk 431: 模板可以包括说明、少量示
例以及适合给定任务的特定上下文和问题
Chunk 432: 因此，我们要创建一个少量示例的列表
Chunk 433: 每个示例都是一个
字典，其中键是输入变量，值是这些输入变量的值
Chunk 434: 
适合LLMs模型的模版构造形式
from langchain.prompts.few_shot import FewShotPromptTemplate
from langchain.prompts.prompt import PromptTemplate
examples = [
{
&quot;question&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个
网球
Chunk 435: &quot;,
&quot;answer&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现
在总共由5+6=11个网球
Chunk 436: 因此答案是11
Chunk 437: &quot;
},
{
&quot;question&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在
食堂总共有多少个苹果
Chunk 438: &quot;,
&quot;answer&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答
案是9
Chunk 439: &quot;
},
{
&quot;question&quot;: &quot;杂耍者可以杂耍16个球
Chunk 440: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的
Chunk 441: 请问总共有多少个蓝色高尔夫球
Chunk 442: &quot;,
&quot;answer&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4
个，答案是4个
Chunk 443: &quot;
},
]
接下来需要实例化一个 PromptTemplate 对象，将少量示例格式化为字符串
Chunk 444: 
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[0]))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球
Chunk 445: 
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球
Chunk 446: 
因此答案是11
Chunk 447: 
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[1]))
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果
Chunk 448: 
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9
Chunk 449: 
最后，创建一个 FewShotPromptTemplate 对象
Chunk 450: 该对象接受经过上一步格式化后的少数样本示
例
Chunk 451: 
# 导入 FewShotPromptTemplate 类
from langchain.prompts.few_shot import FewShotPromptTemplate
# 创建一个 FewShotPromptTemplate 对象
few_shot_prompt = FewShotPromptTemplate(
examples=examples, # 最开始定义的 examples 示例
example_prompt=example_prompt, # 上一步定义的 example_prompt 作为提示模板
suffix=&quot;Question: {input}&quot;, # 后缀模板，其中 {input} 会被替换为实际输入
input_variables=[&quot;input&quot;] # 定义输入变量的列表
)
# 使用给定的输入格式化 prompt，并打印结果
print(few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，
水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 452: &quot;))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球
Chunk 453: 
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球
Chunk 454: 
因此答案是11
Chunk 455: 
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果
Chunk 456: 
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9
Chunk 457: 
Question: 杂耍者可以杂耍16个球
Chunk 458: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的
Chunk 459: 请问总共有
多少个蓝色高尔夫球
Chunk 460: 
总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个
Chunk 461: 
Question: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次
Chunk 462: 
final_input = few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟
才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 463: &quot;)
final_input
&#x27;Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球
Chunk 464: \n罗杰
一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球
Chunk 465: 因此
答案是11
Chunk 466: \n\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现
在食堂总共有多少个苹果
Chunk 467: \n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹
果，答案是9
Chunk 468: \n\nQuestion: 杂耍者可以杂耍16个球
Chunk 469: 其中一半的球是高尔夫球，其中一半的高尔夫球是
蓝色的
Chunk 470: 请问总共有多少个蓝色高尔夫球
Chunk 471: \n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是
蓝色的，也就是4个，答案是4个
Chunk 472: \n\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 473: &#x27;
将带有Few-Shot提示的输入，输入到大模型中执行推理
Chunk 474: 
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米需要4分钟爬到顶部，1分钟滑下来，一共花费5分钟
Chunk 475: 在15分钟内，她能滑3次，因为15÷5=3
Chunk 476: 因此
答案是3次
Chunk 477: &#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米花了5分钟（4分钟爬上去+1分钟滑下来），因此在15分钟内可以滑3次（15÷5=3）
Chunk 478: 答案是3次
Chunk 479: &#x27;
从输出结果来看，显然受到了提示工程的正面影响，通过问题的拆解过程显著提高了回答的准确
性
Chunk 480: 
适合Chat Model的提示模版构造形式
我们在前面解释过LLMs和Chat Model 的区别，因其接受的输入形式不同，所以构造的提示模版也
是不一样的，对于Chat Model来说，它接收的是聊天形式的对话格式，所以对于其对应的Few-Shot，
在LangChain中的基本结构如下：
examples ：要包含在最终提示中的字典示例列表
Chunk 481: 
example_prompt ：通过其 format_messages 方法将每个示例转换为 1 条或多条消息
Chunk 482: 一个常
见的示例是将每个示例转换为一条人工消息和一条人工智能消息响应，或者一条人工消息后跟一条
函数调用消息
Chunk 483: 
我们还是使用四个数学推理题来进行测试：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球
Chunk 484: ” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球
Chunk 485: 因此答案是11
Chunk 486: ” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果
Chunk 487: ” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9
Chunk 488: ” \
Q：“杂耍者可以杂耍16个球
Chunk 489: 其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的
Chunk 490: 请问总共有多少个蓝色高尔夫球
Chunk 491: ” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个
Chunk 492: ” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 493: ” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球
Chunk 494: ”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球
Chunk 495: 因此答案是11
Chunk 496: ” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果
Chunk 497: ”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9
Chunk 498: ”
Q：“杂耍者可以杂耍16个球
Chunk 499: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的
Chunk 500: 请问总共有多少个蓝色高尔夫球
Chunk 501: ” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个
Chunk 502: ” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次
Chunk 503: ” A：&#x27;
直接调用
对于Chat Model，需要将输入转化成对话聊天格式：
from langchain_core.messages import HumanMessage, SystemMessage
messages = [SystemMessage(content=&quot;你是一个擅长数学推理的专家&quot;),
HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟
后关闭，请问在关闭之前她能滑多少次
Chunk 504: &quot;),]
实例化模型进行推理：
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
resonse = chat.invoke(messages)
resonse.content
&#x27;首先，艾米花了1分钟滑下来，所以她每次滑下来后需要等待3分钟（4分钟上去 + 1分钟下来 = 5分钟，15
分钟总时间 - 5分钟 = 10分钟，10分钟 / 3分钟 = 3次）
Chunk 505: \n\n在水滑梯关闭之前，艾米可以滑3次
Chunk 506: &#x27;
&amp;esmp; 实际上，在使用GPT-3.5时，我们可以观察到，即使不采用Few-Shot提示，模型也能以很高
的概率正确回答问题，这归功于模型本身已经非常强大的能力
Chunk 507: 所以我们改为Baichuan2模型来测试：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 508: &quot;)])
response.content
&#x27;艾米需要1分钟滑下来，所以她在滑梯关闭前可以滑15次
Chunk 509: &#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 510: &quot;)])
response.content
&#x27;艾米需要1分钟从滑梯顶部滑下来，所以她可以在滑梯关闭前滑15次
Chunk 511: &#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 512: &quot;)])
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，所以每次滑行需要4+1=5分钟
Chunk 513: \n\n水滑梯将在
15分钟后关闭，所以她能在关闭之前滑行15÷5=3次
Chunk 514: &#x27;
可以看到，在切换到Baichuan模型进行测试时，三次测试中出现了两次错误回答
Chunk 515: 因此，我们就
使用Baichuan2模型，尝试通过添加Few-Shot来探索是否能增强其推理能力
Chunk 516: 
首先，安装LangChain的规范，导入生成示例的模块：
from langchain.prompts import (
ChatPromptTemplate,
FewShotChatMessagePromptTemplate,
)
接下来以input-output对的形式构建Few-Shot提示模板
Chunk 517: 在这里，“input”可以被视作人类发出的
Promot，而“output”则是大模型给出的回答
Chunk 518: 
examples = [
{&quot;input&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网
球
Chunk 519: &quot;,
&quot;output&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总
共由5+6=11个网球
Chunk 520: 因此答案是11
Chunk 521: &quot;},
{&quot;input&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共
有多少个苹果
Chunk 522: &quot;,
&quot;output&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9
Chunk 523: &quot;},
{&quot;input&quot;: &quot;杂耍者可以杂耍16个球
Chunk 524: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的
Chunk 525: 请问
总共有多少个蓝色高尔夫球
Chunk 526: &quot;,
&quot;output&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答
案是4个
Chunk 527: &quot;},
]
然后，使用from_messages方法进行模版构造
Chunk 528: 
# This is a prompt template used to format each individual example.
example_prompt = ChatPromptTemplate.from_messages(
[
(&quot;human&quot;, &quot;{input}&quot;),
(&quot;ai&quot;, &quot;{output}&quot;),
]
)
few_shot_prompt = FewShotChatMessagePromptTemplate(
example_prompt=example_prompt,
examples=examples,
)
print(few_shot_prompt.format())
Human: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球
Chunk 529: 
AI: 罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网
球
Chunk 530: 因此答案是11
Chunk 531: 
Human: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹
果
Chunk 532: 
AI: 食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9
Chunk 533: 
Human: 杂耍者可以杂耍16个球
Chunk 534: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的
Chunk 535: 请问总共有多少
个蓝色高尔夫球
Chunk 536: 
AI: 总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个
Chunk 537: 
生成最终的输入模版，设置 input 作为动态变量：
final_prompt = ChatPromptTemplate.from_messages(
[
few_shot_prompt,
(&quot;human&quot;, &quot;{input}&quot;),
]
)
final_prompt
ChatPromptTemplate(input_variables=[&#x27;input&#x27;], messages=
[FewShotChatMessagePromptTemplate(examples=[{&#x27;input&#x27;: &#x27;罗杰有五个网球，他又买了两盒网
球，每盒有3个网球，请问他现在总共有多少个网球
Chunk 538: &#x27;, &#x27;output&#x27;: &#x27;罗杰一开始有五个网球，又购买了两
盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球
Chunk 539: 因此答案是11
Chunk 540: &#x27;}, {&#x27;input&#x27;:
&#x27;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果
Chunk 541: &#x27;,
&#x27;output&#x27;: &#x27;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9
Chunk 542: &#x27;}, {&#x27;input&#x27;: &#x27;杂耍者可以杂耍16个球
Chunk 543: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的
Chunk 544: 
请问总共有多少个蓝色高尔夫球
Chunk 545: &#x27;, &#x27;output&#x27;: &#x27;总共有16个球，其中一半是高尔夫球，也就是8个，其中
一半是蓝色的，也就是4个，答案是4个
Chunk 546: &#x27;}],
example_prompt=ChatPromptTemplate(input_variables=[&#x27;input&#x27;, &#x27;output&#x27;], messages=
[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;output&#x27;],
template=&#x27;{output}&#x27;))])),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;))])
实例化baichuan2模型：
最后，使用LangChain中的chain的抽象，合并最终的提示和大模型共同执行
Chunk 547: 
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-e9&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 548: &quot;})
response.content
&#x27;艾米需要4分钟才能爬上去，1分钟滑下来，总共需要4+1=5分钟
Chunk 549: 15分钟可以滑15÷5=3次，所以答案是3
次
Chunk 550: &#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 551: &quot;})
response.content
&#x27;艾米爬上去需要4分钟，滑下来只需要1分钟，所以她可以在15分钟内完成4次滑行，因为每次滑行需要
4+1=5分钟
Chunk 552: &#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54695e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次
Chunk 553: &quot;})
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，然后1分钟滑下来，总共需要4+1=5分钟
Chunk 554: 因此，在滑梯关闭之前，她可
以滑15÷5=3次
Chunk 555: &#x27;
4.3 LangChain中的Output Parsers
Output Parsers，即输出解析器，这个概念非常好理解，就是负责获取大模型的输出并将其转换为
更合适的格式
Chunk 556: 这在应用开发中及其重要
Chunk 557: 在大多数复杂应用场景中，处理逻辑往往环环相扣，执行某
项业务逻辑可能需要多次调用大模型，其中上一次的调用结果将被用于指导下一次调用的逻辑
Chunk 558: 在这种
情况下，结构化的信息会比纯文本又有价值，同时这也是输出解析器的价值所在
Chunk 559: 
LangChain构造的输出解释器必须实现两个主要方法：
Get format instructions：该方法会返回一个字符串，其中包含有关如何格式化语言模型输出的指
令
Chunk 560: 
Parse：该方法会接收字符串，并将其解析为某种结构
目前已经支持的解析格式已经包括Json、Xml、Csv以及OpenAI的Tools和Functions等多种格式，
具体可看：https://python.langchain.com/docs/modules/model_io/output_parsers/
简而言之，这个过程本质上是对文本的格式化处理
Chunk 561: 但与传统的后处理不同，LangChain采取的实
现策略是通过在Prompt上增加输出解析，直接引导大模型输出的格式化方式
Chunk 562: 
我们以格式化日期来看一下LangChain的构造思路，首先导入相关的模块：
from langchain.output_parsers import DatetimeOutputParser
from langchain.prompts import PromptTemplate
from langchain_openai import OpenAI
实例化日期解析器：
output_parser = DatetimeOutputParser()
通过.get_format_instructions方法，可以查看定义的解析格式：
output_parser.get_format_instructions()
&quot;Write a datetime string that matches the following pattern: &#x27;%Y-%m-
%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1181-01-05T02:20:33.675873Z, 1938-12-
13T09:46:36.791229Z, 1468-01-17T07:34:51.108688Z\n\nReturn ONLY this string, no
other words!&quot;
构造输入模版，这里的区别是：在输入的Prompt Template中，加入了OutPut Parse的内容：
template = &quot;&quot;&quot;用户发起的提问:
{question}
{format_instructions}&quot;&quot;&quot;
生成最终实际输入大模型的Promot内容：
prompt = PromptTemplate.from_template(
template,
# 预定义的变量，这里我们传入格式化指令
partial_variables={&quot;format_instructions&quot;:
output_parser.get_format_instructions()},
)
prompt
PromptTemplate(input_variables=[&#x27;question&#x27;], partial_variables=
{&#x27;format_instructions&#x27;: &quot;Write a datetime string that matches the following
pattern: &#x27;%Y-%m-%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1472-10-23T17:39:56.199188Z,
1643-01-14T10:55:11.903091Z, 0631-12-11T23:04:04.345333Z\n\nReturn ONLY this
string, no other words!&quot;}, template=&#x27;用户发起的提
问:\n\n{question}\n\n{format_instructions}&#x27;)
最后，使用LangChain中的chain的抽象，合并最终的提示、大模型实例及OutPut Parse共同执
行
Chunk 563: 
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请问你叫什么
Chunk 564: &quot;})
print(output)
2021-10-07 12:02:13.345333
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是机器学习&quot;})
print(output)
2018-01-23 09:42:32.123456
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是深度学习&quot;})
print(output)
2019-08-07 18:30:00
通过测试可以明显观察到，以此方式定义的输出解析器极大地规范了模型的输出
Chunk 565: 当然，这也直接
受到大模型实际能力和模板定义情况的影响
Chunk 566: 
LangChain中不同模版的使用方式，除了可以在官方文档上看到，在其源码上看会更加直观，而且
可以自定义修改，其所有的OutPut Parse的模版存储路径为：
langchain/libs/langchain/langchain/output_parsers ， 大家可以自行尝试
Chunk 567: 比如我们上面使
用的日期解析的源码对应如下：
到此为止，我们已经非常详尽地介绍了LangChain的核心概念
Chunk 568: 如果有学习过大模型技术实战课程
的同学来说，会发现LangChain的Model I/O模块中讨论的所有概念都是之前接触过的，需要学习的主
要是LangChain对大模型进行抽象的方法以及在LangChain框架下的应用开发技巧
Chunk 569: 
而从LangChain的整体开源情况看，其内容丰富且迭代非常快，通过模块化的方式介绍，可以帮助
大家快速构建起对LangChain以及应用开发的思维框架
Chunk 570: 尽管在Model I/O模块中还有许多更高级的内
容，例如如何接入本地部署的开源大模型，关于函数调用、Memory缓存等更深入的主题，这些内容我
们将在后续章节中一一接触并进行详细的探讨
Chunk 571: 
```

如上所示，一整段text会根据设定的标点符号被分割为多个chunks，当然如果有特定的分割需求（比如保留某些特定的标点符号），可以调整正则表达式来灵活的调整。

### 3.2.2 按照固定字符数切分

如果想按照固定字符数来切分文本，这种方法就不再依赖于标点符号，而是简单地按照给定的字符数来切分文本。我们可以编写一个函数，用来将文本分割成指定长度的片段。代码如下：

```
def split_text_by_fixed_length(text, length):
    # 使用列表推导式按固定长度切分文本
    return [text[i:i + length] for i in range(0, len(text), length)]
```

这个函数的作用是根据指定的长度（在这个例子中为100个字符）来切分文本。我们可以根据具体需要调整这个长度。

```
# 文本
text = &quot;春节的脚步越来越近，大街小巷都布满了节日的气氛。商店门口挂满了红灯笼和春联，家家户户都在忙着打扫卫生，准备迎接新的一年。\
小明回到家乡，感受到了浓浓的过年氛围。他在街上走着，看到小朋友们手持烟花棒，欢笑声此起彼伏。\
夜幕降临，整个城市亮起了五彩缤纷的灯光，映照着人们脸上的喜悦与期待。老人们聚在一起，回忆过去，展望未来。\
而年轻人则在夜市享受美食，放松心情。这是一个充满希望和喜悦的时刻，每个人都在以自己的方式庆祝这个特殊的节日。&quot;

# 定义每个片段的长度
chunk_length = 100

# 调用函数进行分割
result = split_text_by_fixed_length(text, chunk_length)

# 打印结果
for i, segment in enumerate(result):
    print(f&quot;Chunk {i+1}: {segment}&quot;)
```

```
Chunk 1: 春节的脚步越来越近，大街小巷都布满了节日的气氛。商店门口挂满了红灯笼和春联，家家户户都在忙着打扫卫生，准备迎接新的一年。小明回到家乡，感受到了浓浓的过年氛围。他在街上走着，看到小朋友们手持烟花棒，欢笑
Chunk 2: 声此起彼伏。夜幕降临，整个城市亮起了五彩缤纷的灯光，映照着人们脸上的喜悦与期待。老人们聚在一起，回忆过去，展望未来。而年轻人则在夜市享受美食，放松心情。这是一个充满希望和喜悦的时刻，每个人都在以自己的
Chunk 3: 方式庆祝这个特殊的节日。
```

接下来，我们切分刚才的加载的PDF文档的全部内容，其存储在变量extracted_text中。

```
# 定义每个片段的长度
chunk_length = 500

# 调用函数进行分割
result = split_text_by_fixed_length(extracted_text, chunk_length)

# 打印结果
for i, segment in enumerate(result):
    print(f&quot;Chunk {i+1}: {segment}&quot;)
```

```
Chunk 1: 开源大模型应用开发实战
Ch 18 LangChain整体项目介绍与核心模块Model I/O详
解
注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情
况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，
将迅速更新教材内容以确保信息的准确性和实用性。
本期课件更新时间：2024年3月7日
在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地
私有化部署开源大模型。在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家
关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部
署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部
署。
但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也
是最简单的一步。熟练掌握基于大模型的上层应用开发还有非常长的路需要走。目前，大模型技术开发
总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API
Chunk 2: 的应用开发，而另一类，就是先私有化
部署开源大模型，再借助开发框架构建大模型应用。当然了，目前主流的大模型应用开发框架也支持在
线模型API的调用，这正是我们之前一直强调学习掌握开源或在线API调用技巧的原因，因为这些方法和
技巧直接对应于应用开发的最佳实践。
而不论采用哪种开发形式，其基座和所依赖的，核心还是大模型本身。对于在线大模型，以
OpenAI为例，开发者需要配置科学上网的环境，在其官网创建 API Key，最后遵循API的使用说明去进
行应用配置。这一过程中涉及到的网络环境适配，数据隐私安全，成本管理及如何去定制化自己需求的
开发流程，往往都需要遵循官方提供的功能来实现。也正是因为这些复杂的因素使得尽管目前公认最强
的大语言模型GPT 4开放给开发者使用，还是会有非常多的公司和个人去拥抱开源，选择性能可能略逊
一筹的开源大模型，因为开源方案在某些方面提供了更多的灵活性和自主控制能力。
大家也能够发现，在我们的课程中每当涉及到大模型的私有化部署，我们都会向大家展示如何通过
命令行、网页端访问和API调用等三种方式与部署的大模型进行交互。此外，还通过text-generate-
Chunk 3: 
webui这样的开源项目，这一切应用是帮助大家能够以最简单、最直接的方式与开源大模型进行对话，
了解大模型的运行模型。但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶
段肯定无法满足实际的需求。虽然目前的大模型基本都可以按照OpenAI的API规范形式进行调用，但如
果进行实操我们会发现，在《大模型技术实战》的课程中，借助OpenAI的GPT系列模型开发的项目，
如智能邮件处理、全自动AI开发流程，以及MateGen Agents等，尽管在开源模型上能够被复现，但这
背后的成功关键并非仅仅在于模型调用。例如，在《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进
行外部工具API调用开发实践》中，我们通过Qwen模型复现了《Ch.10 借助Function calling调用外部
工具API方法》中的天气信息调用过程。这一复现过程揭示了，对在线模型来说，我们需要学习的是其
API使用方法。而在开源大模型的技术领域下，要想实现某些特定功能，重点在于理解该模型的一些使
用原理，以及如何调整和构建tools与Schema表示。因此，整个开发过程中花费的大部分时间
Chunk 4: 并非直接
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
代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高
的。LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架。它的做法是：通过提
Chunk 5: 
供标准化且丰富的模块抽象，构建大语言模型的输入输入规范，利用其核心概念chains，灵活地连接
整个应用开发流程。而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出
来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发
领域内被普遍高度认可的通用框架。
为什么需要这样做？
首先，我们需考虑当前大模型的发展态势。尽管OpenAI的GPT系列模型作为大模型领域的领军人
物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大
模型间的使用方式完全相同。例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不
适用。因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是
十分低下的。
其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据
连接方式以及输出结果的不稳定性等问题。在应用开发中，如何找到这些问题的有效解决策略？
上述提到的每个限制都紧密关联于大模型本身的特性。尽管理论上可以通过重新训练、微调来增
Chunk 6: 强
模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资
源、时间和财力，选择这条路径一定会导致方向越来越偏离目标。我们之前讨论的OpenAI的Function
Calling和通过ReAct流程接入第三方API能够提供一些解决方案，但这每一步都需大量的研发投入，而且
最终实现后的应用效果，也取决于研发人员的个人技术能力。在这种背景下，既然大家都有不同的想法
和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的
快速搭建，这就是LangChain一直想要做到，且正在做的事情。
LangChain的做法
从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对
大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接
口以适配各种大模型。到目前为止，LangChain抽象出最重要的核心模块如下：
1. Model I/O ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；
2. Retrieval ：检索外部数据，然后
Chunk 7: 在执行生成步骤时将其传递到 LLM，包括文档加载、切割、
Embedding等；
3. Chains ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多
功能的高级抽象；
4. Memory ： 记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；
5. Agents ： 目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；
6. Callbacks ：回调系统，允许连接到 LLM 应用程序的各个阶段。用于日志记录、监控、流传输和
其他任务；
从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文
记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的&quot;Chain&quot;高级抽象，定义了
不同形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整
的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。
也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和
使用方法，那么如何去搭这个“积
Chunk 8: 木”，就需要我们对其每个核心模块都要有一个比较清楚的认知。所以
我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实
操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业
务情况，选择合适的构造模块和构造方法。
在整体上理解了LangChain之后，我们首先从Model I/O模块进行深入的探讨和实践。
2. LangChain核心模块：Model I/O
LangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的
Model I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就
是通过该模块，我们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过
程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API
调用规范：
response = openai.ChatCompletion.create(
model=&quot;gpt-3.5-tur
Chunk 9: bo&quot;,
messages=[
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是机器学习？&quot;}
]
)
而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话
交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象
出一个chain，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核
心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和
Output Parser（对应下图中的Parse部分）。
Format：即指代Prompts Template，通过模板化来管理大模型的输入；
Predict：即指代Models，使用通用接口调用不同的大语言模型；
Parse：即指代Output部分，用来从模型的推理中提取信息，并按照预先设定好的模版来规范化
输出。
整个Model I/O工作流如下图所示：
Format
对于Prompt Template第一部分，传统上我们
Chunk 10: 创建提示词是通过手工编写来实现的，在这个过程中
会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能。然而，
在应用开发中，一个关键的考量是提示词不能是一成不变的。其原因在于，应用开发需要适应多变的用
户需求和场景。固定的提示词限制了模型的灵活性和适用范围。例如，如果我们正在开发一个天气查询
应用，用户可能会以多种方式提出查询，如“今天的天气怎么样？”或“明天纽约的温度是多少度？”。如
果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求。
而Prompt Template，就像我们在介绍《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进行外
部工具API调用开发实践》中不断优化出来的ReAct模版，将API的使用、问题解答过程等复杂逻辑封装
成了一套结构化的格式。我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引
导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：
# 将一个插件的关键信息拼接成一段文本的模版。
TOOL_DESC = &quot;&quot;&quot;{name_for_model}:
Chunk 11:  Call this tool to interact with the
{name_for_human} API. What is the {name_for_human} API useful for?
{description_for_model} Parameters: {parameters}&quot;&quot;&quot;
# ReAct prompting 的 instruction 模版，将包含插件的详细信息。
PROMPT_REACT = &quot;&quot;&quot;Answer the following questions as best you can. You have
access to the following APIs:
{tool_descs}
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}
Chunk 12: ]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can be repeated zero or more
times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question
Begin!
Question: {query}&quot;&quot;&quot;
因此，引入Prompt Template可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输
入动态调整提示词，从而更好地响应用户的具体需求。LangChain通过这种方式来提高应用的通用性和
用户体验。
Predict
在Predict部分，实质上是处理模型从接收输入到执行推理的整个过程。考虑到存在两种主要类型的
大模型——Base类模型和Chat类模型，LangChain在其Model I/O模块中对这
Chunk 13: 两种模型都进行了抽象，
分别归类为LLMs（Large Language Models）和Chat Models。我们还是以OpenAI 的 Completion 和
Chatcompletions方法为例：
# Base类模型
client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=&quot;Say this is a test&quot;,
)
# 聊天模型
client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
]
)
LLMs是简化的大语言模型抽象，即基于给定的Prompt提供内容生成的功能。而Chat Models则专
注于聊天API的抽象，需要维护上下文的记忆（聊天记录），呈现出更接近对话或聊天形式的交互。
Parse
我们知道，大模型的输出是不稳定的，
Chunk 14: 同样的输入Prompt往往会得到不同形式的输出。在自然语
言交互中，不同的语言表达方式通常不会造成理解上的障碍。但在应用开发中，大模型的输出可能是下
一步逻辑处理的关键输入。因此，在这种情况下，规范化输出是必须要做的任务，以确保应用能够顺利
进行后续的逻辑处理。
输出解析器 Output Parser就是一个帮助结构化语言模型响应的抽象，可以获取格式指令或者进行
更深层次的解析。这我们会在后面的实践中直观的体验到。
整体而言，在Model I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如
OpenAI、ChatGLM、Qwen等，按照既定规范执行模型推理。其二通过输入和输出的模板化处理，使
其更贴合于应用开发的最佳实践。接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行
集成和操作的。
3 安装LangChain
LangChain的安装过程非常简单，可以通过常用的Python包管理工具，如pip或conda，直接进行
安装。稍复杂一点的还可以通过源码进行安装。但有一点大家一定要明确：LangChain的真正价值在于
它能够与多种模型提供商、数据存储解
Chunk 15: 决方案等进行集成。默认情况下，使用上述两种安装方式中的任
意一种来进行LangChain安装后，安装的仅仅是LangChain的默认功能，并不包括这些集成所需的额外
依赖项。也就是说，如果我们想要使用特定的集成功能，还需要额外安装这些特定的依赖。以调用
OpenAI的API为例，我们首先需要通过运行命令pip install langchain-openai安装OpenAI的合作
伙伴包，安装此依赖包后，LangChain才能够与OpenAI的API进行交互。后续我们在使用相关功能的时
候，会提供额外的说明。
LangChain安装官方说明文档：https://python.langchain.com/docs/get_started/installation
3.1 使用包版本管理工具安装
LangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建
过程。这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成
LangChain的安装。具体操作如下：
! pip install langchain
Requiremen
Chunk 16: t already satisfied: langchain in d:\software\anaconda3\lib\site-
packages (0.1.10)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain) (6.0)
Requirement already satisfied: SQLAlchemy&lt;3,&gt;=1.4 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.4.39)
Requirement already satisfied: aiohttp&lt;4.0.0,&gt;=3.8.3 in
d:\software\anaconda3\lib\site-packages (from langchain) (3.8.5)
Requirement already satisfied: dataclasses-json&lt;0.7,&gt;=0.5.7 in
d:\
Chunk 17: software\anaconda3\lib\site-packages (from langchain) (0.6.4)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.33)
Requirement already satisfied: langchain-community&lt;0.1,&gt;=0.0.25 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.25)
Requirement already satisfied: langchain-core&lt;0.2,&gt;=0.1.28 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.28)
Requirement already satisfied: langchain-text-splitters&lt;0.
Chunk 18: 1,&gt;=0.0.1 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.1)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.14)
Requirement already satisfied: numpy&lt;2,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (1.24.3)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3
Chunk 19: \lib\site-
packages (from langchain) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain) (8.2.2)
Requirement already satisfied: attrs&gt;=17.3.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (22.1.0)
Requirement already satisfied: charset-normalizer&lt;4.0,&gt;=2.0 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(2.0.4)
Requirement already satisfied: multidict&lt;7.
Chunk 20: 0,&gt;=4.5 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(6.0.2)
Requirement already satisfied: async-timeout&lt;5.0,&gt;=4.0.0a3 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(4.0.2)
Requirement already satisfied: yarl&lt;2.0,&gt;=1.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (1.8.1)
Requirement already satisfied: frozenlist&gt;=1.1.1 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;
Chunk 21: =3.8.3-&gt;langchain)
(1.3.3)
Requirement already satisfied: aiosignal&gt;=1.1.2 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.2.0)
Requirement already satisfied: marshmallow&lt;4.0.0,&gt;=3.18.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (3.21.0)
Requirement already satisfied: typing-inspect&lt;1,&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (0.9.0)
Requirement already s
Chunk 22: atisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain)
(2.1)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (3.5.0)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2,&gt;=0.1.28-
&gt;langchain) (23.2)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-package
Chunk 23: s (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain)
(2.16.1)
Requirement already satisfied: typing-extensions&gt;=4.6.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (4.7.1)
Requirement already satisfied:
Chunk 24:  idna&lt;4,&gt;=2.5 in d:\software\anaconda3\lib\site-
packages (from requests&lt;3,&gt;=2-&gt;langchain) (3.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(1.26.16)
Requirement already satisfied: certifi&gt;=2017.4.17 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(2023.11.17)
Requirement already satisfied: greenlet!=0.4.17 in
d:\software\anaconda3\lib\site-packages (from SQLAlchemy&lt;3,&gt;=1.4-&gt;langchain)

Chunk 25: (2.0.1)
Requirement already satisfied: sniffio&gt;=1.1 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (1.2.0)
Requirement already satisfied: mypy-extensions&gt;=0.3.0 in
d:\software\anaconda3\lib\site-packages (from typing-inspect&lt;1,&gt;=0.4.0-
&gt;dataclasses-json&lt;0.7,&gt;=0.5.7-&gt;langchain) (1.0.0)
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：
import langchain
print(langchain.__version__)
0.1.10
如果能正常输出LangChain
Chunk 26: 的版本，说明在当前环境下的安装成功。
3.2 源码安装
除了通过pip安装外，还有一种通过源码安装的方法。这需要使用git拉取远程仓库，然后进入项目
文件夹并执行pip install -e .命令。这种方法不仅会安装必要的依赖，同时也将程序的源代码保存
在本地。通常来说，除非大家打算作为LangChain的协作开发者参与到项目的功能更新中，否则没必要
采用这种安装方式，直接使用pip安装更为简便。
对于课程学习而言，我们推荐采用源码安装方式，这将非常有助于在后续的LangChain功能探索
中，通过源码分析深入理解框架的构建原理和详细机制。
接下来我们以Ubuntu操作系统为例，介绍源码安装LangChain的详细步骤：
Step 1. 安装Anaconda
如果本地服务器是Ubuntu系统，请参考：《Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B
模型》中的2.3 安装Anaconda环境小节；而如果是Windows操作系统，请参考：《Ch.6 在Windows
系统下部署运行ChatGLM3-6B模型》中的3.2.1 方式一：使用Anaconda创建项目
Chunk 27: 依赖环境（推荐）小节
中，执行完Step 6. 验证conda步骤，回到本文继续执行后续的操作：
注：如果部署环境是租赁的云服务器，Anaconda环境已经预设好，并不需要我们再次手动配置。
按照对应的教程内容配置好Anaconda后，可以通过如下命令验证当前环境的Conda：
conda --version
如果可以正常输出Conda的版本，则说明Anaconda安装成功。
Step 2. 使用Conda创建LangChain的Python虚拟环境
安装好Anaconda后，我们需要借助Conda包版本工具，为LangChain项目创建一个新的Python虚
拟运行环境，执行代码如下：
conda create --name langchain python==3.11
如上所示，新创建了一个名为langchain的Python虚拟环境，其Python版本为3.11。创建完成后，
通过如下命令进入该虚拟环境，执行后续的操作：
conda activate langchain
Step 3. 下载LangChain的项目文件
进入LangChain的官方Github，地址：https:
Chunk 28: //github.com/langchain-ai/langchain ， 在 GitHub
上将项目文件下载到有两种方式：克隆 (Clone) 和 下载 ZIP 压缩包。推荐使用克隆 (Clone)的方式。我
们首先在GitHub上找到其仓库的URL。
在执行命令之前，需要先安装git软件包，执行命令如下：
sudo apt install git
执行克隆命令，将LangChain Github上的项目文件下载至本地的当前路径下，如下：
git clone https://github.com/langchain-ai/langchain.git
Step 4. 升级pip版本
建议在执行项目的依赖安装之前升级 pip 的版本，如果使用的是旧版本的 pip，可能无法安装一些
最新的包，或者可能无法正确解析依赖关系。升级 pip 很简单，只需要运行命令如下命令：
python -m pip install --upgrade pip
Step 5. 源码安装项目依赖
不同于我们之前一直使用的pip install -r requirements.txt方式，这种方法用于批量安装多
个
Chunk 29: 依赖包，是在部署项目或确保开发环境与其他开发者/环境一致时的常用方式。而对于LangChain，我
们需要使用pip install -e的方式，以可编辑模式安装包。这种方式主要用于开发过程中。当以可编
辑模式安装一个包时，依赖包会被直接从源代码所在位置安装，而不是复制到Python的site-packages
目录，是开发模式下用于安装并实时反映对本地包更改的方法。需要执行的步骤如下：
cd langchain/libs/langchain/
pip install -e .
如在安装过程未发生任何报错，则说明安装成功。在安装完依赖后，我们就正式进入LangChain的
Model I/O模块的实践。
注：如果采用的是源码安装，需要参考《Ch.11 大模型开发工具Transformers库的使用及配置大
模型开发环境》中远程服务器部署大模型，本地Jupyter NoteBook加载小节，在LangChain的虚拟
环境下启动Jupyter Lab，才能正常执行接下来的实践。
4.Models I/O模块实战
任何语言模型应用的核心都是大语言模型（LLMs），无论涉及到提示模板（Pro
Chunk 30: mpt Template）
还是输出解析（OutPut Parse），其核心还是在于大语言模型（LLMs）。因此，在讨论和实践Model
I/O模块时，首先应当关注如何集成这些大模型。只有成功集成后才能深入探讨和实践如何有效地设计
提示模板和解析模型输出。所以我们先要实践的是：如何借助LangChain框架使用不同的大模型。
4.1 LangChain接入大模型的方法
LangChain 提供了一套与任何大语言模型进行交互的标准构建模块。所以需要明确的一点是：虽然
LLMs 是 LangChain 的核心元素，但 LangChain 本身不提供 LLMs，它仅仅是为多种不同的 LLMs 进
行交互提供了一个统一的接口。简单理解：以OpenAI的GPT系列模型为例，如果我们想通过
LangChain 接入 OpenAI 的 GPT 模型，我们需要在LangChain框架下先定义相关的类和方法来规定如
何与模型进行交互，包括数据的输入和输出格式以及如何连接到模型本身。然后按照 OpenAI GPT 模型
的接口规范来集成这些功能。通过这种方式，LangChain 充当一个桥梁，使我们能够按
Chunk 31: 照统一的标准来
接入和使用多种不同的大语言模型。
在LangChain的Model I/O模块中，集成了两类模型，分别是LLMs和聊天模型，这两种模型在输
入和输出的数据类型上有所区别。
接下来我们就以OpenAI的API为示例，分别实践一下在LangChain框架中如何使用这两种不同类型
的大语言模型。首先来看LLMs类模型。
4.1.1 LLMs类模型接入方法
LLMs指的是纯文本补全模型，其封装的 API 输入接收的是字符串，输出也是字符串。 OpenAI 的
GPT-3 是作为 LLM 实现的。最简单的理解：LLMs指代的就是Base类模型，这类模型仅仅可以理解并
生成自然语言或代码，并未按照特定指令进行训练。截止到2024年3月07日，OpenAI可支持调用的
Base类模型仅有2个，分别是babbage-002和davinci-002，除此之外，还可以使用gpt-3.5-turbo-
instruct调用OpenAI 的 Completion方法。
首先需要安装OpenAI的集成依赖包langchain-openai，执行如下命令：
! pip install langch
Chunk 32: ain-openai
Requirement already satisfied: langchain-openai in
d:\software\anaconda3\lib\site-packages (0.0.8)
Requirement already satisfied: langchain-core&lt;0.2.0,&gt;=0.1.27 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.1.28)
Requirement already satisfied: openai&lt;2.0.0,&gt;=1.10.0 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (1.13.3)
Requirement already satisfied: tiktoken&lt;1,&gt;=0.5.2 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.6.0)
Chunk 33: 
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (6.0)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.5.0)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (1.33)
Requirement already satisfied: langsmith&lt;0.
Chunk 34: 2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (0.1.14)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (23.2)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anacond
Chunk 35: a3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (8.2.2)
Requirement already satisfied: distro&lt;2,&gt;=1.7.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (1.8.0)
Requirement already satisfied: httpx&lt;1,&gt;=0.23.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.
Chunk 36: 0.0,&gt;=1.10.0-&gt;langchain-
openai) (0.25.1)
Requirement already satisfied: sniffio in d:\software\anaconda3\lib\site-
packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (1.2.0)
Requirement already satisfied: tqdm&gt;4 in d:\software\anaconda3\lib\site-packages
(from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (4.65.0)
Requirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (4.7.1)
Requirement already satisfi
Chunk 37: ed: regex&gt;=2022.1.18 in
d:\software\anaconda3\lib\site-packages (from tiktoken&lt;1,&gt;=0.5.2-&gt;langchain-
openai) (2022.7.9)
Requirement already satisfied: idna&gt;=2.8 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai)
(3.4)
Requirement already satisfied: certifi in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(2023.11.17)
Requirement already satisfied: httpcore in d:\software\anaco
Chunk 38: nda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(1.0.1)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.1)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.9.15)
Requirement already satisfied: annotated-t
Chunk 39: ypes&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.16.1)
Requirement already satisfied: charset-normalizer&lt;4,&gt;=2 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.0.4)
Requireme
Chunk 40: nt already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (1.26.16)
Requirement already satisfied: colorama in d:\software\anaconda3\lib\site-
packages (from tqdm&gt;4-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.4.6)
Requirement already satisfied: h11&lt;0.15,&gt;=0.13 in
d:\software\anaconda3\lib\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-
&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.14.0)
使用OpenAI的 GP
Chunk 41: T 系列模型的API有两个前提条件：首先，需要配置科学上网以确保可以访问
OpenAI服务；其次，必须拥有一个有效的API Key。这两项要求的详细配置在《Ch.1 本地调用OpenAI
API流程及OpenAI官网使用指南》中有详细说明，因此我们在此不再赘述，直接进入实操过程。
查看OpenAI的依赖包版本
OpenAI 的 API 在 0.28 版本和 1.xx 版本之间的调用方式存在差异。我们这里使用的是最新的调用
方法，即采用 1.13.XX 版本。因此，需要确认当前 Python 虚拟环境中安装的 openai 包版本是 1.xx。
可以通过执行以下命令来检查当前安装的版本：
import openai
print(openai.__version__)
0.28.0
如果如上显示是0.28.0版本，执行如下命令将openai包升级到最新版本：
# ! pip install --upgrade openai
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
import openai
print(openai.__version__)
Chunk 42: 
1.13.3
设置科学上网环境
根据所使用的科学上网软件，查找相应的代理端口，并按照以下代码进行网络环境配置：
# 这里的端口，请修改为实际使用科学上网软件配置的代理端口
!set HTTP_PROXY=http://127.0.0.1:15732
!set HTTPS_PROXY=http://127.0.0.1:15732
新版本的OpenAI API调用方法不允许使用openai.api_key= &quot;sk-xxx&quot;这种老版本的显式指定API
Key，所以如果没有在系统变量中进行配置的话，使用下面的命令设置环境变量：
# 将your- API -key-here替换为实际的API密钥:
# ! setx OPENAI_API_KEY &quot;sk-xxxx&quot;
成功: 指定的值已得到保存。
注意：完成环境变量的初次配置后，需重启电脑以激活设置。这样，在下次使用时就可以直接调用
配置好的环境。为验证环境变量中的API Key是否已正确设置为自己的，可以执行以下命令进行检查。
# ! echo %OPENAI_API_KEY%
完成了科学上网环境和API Key的设置后，在接入LangCha
Chunk 43: in之前，我们先使用如下测试代码检查
当前环境及API的运行状态是否正常，执行代码如下：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
]
)
print(completion.choices[0].message.content)
你好，我是一位可以回答问题、提供信息和与您交流的AI助手。我可以帮助解决问题，回答疑惑，提供建议，
而且乐意与您分享知识和信息。有什么我可以帮助你的吗？
若测试代码能正常输出结果，表明OpenAI的环境配置无误。需要注意的是，LangChain接入
OpenAI GPT模型只是利用其定义的标准模型接入框架来整合OpenAI API，因此进行上述连通性测试
是必要的。
Op
Chunk 44: enAI GPT Base模型官方说明：https://platform.openai.com/docs/models/moderation
LangChain为了使开发者可以轻松地创建自定义链，整体采用Runnable协议。Runnable 协议是
编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可
以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了
Runnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。
LangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch
ain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable
而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解
的LLMs
Chunk 45: 模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。
如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI：
LangChain Interface说明：https://python.langchain.com/docs/modules/model_io/llms/quic
k_start
LangChain已集成LLMs说明：https://python.langchain.com/docs/integrations/llms/
首先，我们来了解常规的调用方法。具体支持的调用方式如下所示：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
接下来我们对上述方法依次进行尝试。在LangChain框架中，为了规范地接入LLMs类型的大模
型，首先需要导入OpenAI的依赖包。导入命令如下：
from langchain_openai import OpenAI

Chunk 46: 字符串形式的输入
调用invoke方法，并提供字符串格式的Prompt，便可触发指定模型按照所给Prompt进行续写操
作。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
llm.invoke(
&quot;你好呀，请你介绍一下你自己&quot;
)
&#x27;吧。”韩寒摇头，说：“我下次再来，你们自己给我介绍好了。”这一次，韩寒的风趣让陈航漂了一下，他说：
“还是先看看大家的房间吧。”陈航带着韩寒到了楼下，这时连小李都已经来到了楼下，他对韩寒说：“你看，
这是我们的宿舍。”陈航的语气是那么的平淡，既没有过多的炫耀，也没有明显的拒绝，只是那么的平淡，这
让韩寒很有点奇怪，他说：“你们是在这里住吗？”小李说：“是啊，我们是这里的住宿生。”韩寒对小李说：
“那我是来参加考试的，你&#x27;
从上述模型的返回输出，我们可以看到它本质上作为一个补全模型，通过使用大模型原生的
.generate() 方法来对输入的Prompt进行续写。这部分的详细内容已在《Ch.2 Completions模型API
使用指南》中进行了深入讲解，
Chunk 47: 因此此处不再进行赘述。
上述调用方法相当于使用我们熟悉的OpenAI的Completion方法来调用GPT Base类型模型的过
程：
from openai import OpenAI
client = OpenAI()
completion = client.completions.create(
model=&quot;davinci-002&quot;,
prompt=&quot;你好呀，请你介绍一下你自己&quot;,
max_tokens=1024,
)
print(completion.choices[0].text)
。» 她把咳嗽调整成百米冲刺那种负重颤抖。»不，我们在上面，请你告诉会场。哪一地方过去你都要做朋友
呢？」Larsen一转眼注意到卡萨姆旁站着一位古老的男人。他身着粗布制服，最左边有一种圆盏形的供奶
头。透映着那个老的质朴魅力和吓坏人恶心的样子。»LarsenKeberichTaylor.Ch02 3/5/07 5:27 PM
Page 39LEARNING WHERE WECAME FROM»喂主持人。自从我给你们滴剂的时候，我就耐心等待了。请为
快点谈到刚才提的内容。»正赞成的木质松枝变苍白
Chunk 48: 了。既然他是最耐烦的老爷，我们劝劝他这样三小时也
烦。时光久了会累人的人，他更应该知道这些。»好，每个人都听到。»那些选手的名字有老项目拯救人」，
Brickman说。AnderMartin、Taylor, här nere – efter en minut eller så. Likdant för
LarsenKeberich, rätt så bra ger du tillbaka vätsken, yeah, and the eighth
man.»»Eighth man?」Brickman坏坏垂下眼帘扬扬眉头，头盔上的唇影随着他挺起了脸颊。好，一位是
我。不，我不是。Just a moment.»六记地雷爆炸。嫣红的色泽弥漫在鼻子周围极其耀眼之下，淀粉饼糊塞
了全脸，剔饭一般的巧克力混合在一起。»»Would you all please proceed into the next
building and make sure that you use the turnstiles.»伤口破口流血，簇瓣的血色俗套，但去
还是要用。套房好美啊、清生。套房饲養下只要一个；_ 户功
Chunk 49: 能装置就。»艰难地活着足以获得一天的欢乐的
人确实算够奇特的。咳嗽的木质松枝_cancel6 3/5/07 5:27 PM Page 40IT IS NOT ENOUGH
TOMAKE MANKINDHAPPY,LARSEN KEBERICH向报纸走来，她在走廊哪一个厅卡萨姆等待着;在这个保证已
经到达那种别有心碎的正式屋子里。她穿过镶有玫瑰花的拉链的抢洗衣区和单人舔饭的厕所区圆朝圣地的道。
先是葡萄干牛油，透过臭风从供应箱中飘来，刚开始时她便一直寻找那一种苏查尔的酸味。那种味美常是发出
着从薄饼里流发出来的所以一圈难找到对不起，小姐，我们之前选择做了决定，就算更改好一个时刻，也无法
改变决定。拨动键盘-找麻烦你的，怎么做了、哦，听起来好像你一突然卸了山猫带来了 Asktohell你疼
吗。杨虎大吼着，冲向对方。丁
流式输出
要进行流式调用，可以调用stream方法。这种方式允许在接收到模型响应的同时，逐块输出结
果，实现即时展示而无需等待全部内容处理完毕。
from langchain_openai import OpenAI
for chunk in llm.stream(
&quot;你好
Chunk 50: 呀，请你介绍一下你自己&quot;
):
print(chunk, end=&quot;&quot;, flush=True)
吧，你是谁，你在哪里，你是什么学问，你喜欢什么，你不喜欢什么，你不会什么，你会什么，你梦想什么，
你的优点是什么，你的缺点是什么，你的性格是什么，你的兴趣是什么，你喜欢的食物是什么，你不喜欢的食
物是什么，你的爱好是什么，你的愿望是什么。介绍一下你自己好的。
[ 编辑 xiaofan1 在 07-03-18 23:49 ]
批量生成
通过调用batch方法，可以同时对多个不同的Prompt进行调用处理。在这个例子中，输入包括“你
好”、“什么是机器学习”和“什么是深度学习”三个独立的Prompt。这种批量处理方式允许一次性提交多个
Prompt，从而提高推理效率。
from langchain_openai import OpenAI
llm.batch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，习惯了你的成熟，可是这次你的决定让我感到非常惊讶，我不知道你的这次决定是不是真的，我不知道你
是否真的那么爱我，虽然我知道你非常爱我，可是我不知道你是不是真的爱我
Chunk 51: ，假如你为了我不想去上学，我
也可以放弃一切，去读书，去上大学，但是你为什么要这样做呢，我不清楚你的想法，我不知道你的想法，我
只知道我爱你，我只知道我不想失去你，我只知道我不想让你失去你的希望，我只知道我不想让你失去你的未
来，可是我想知道你为什么要这样做呢，我想知道你的想法，你是&#x27;,
&#x27; 机器学习的原理 机器学习的技术 机器学习的应用机器学习在生产过程中的作用机器学习在生产过程中的
作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产
过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学
习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作
用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学&#x27;,
&#x27;？ 2. Why is the industry so interested in deep learning? 3. How will deep
learning transform the financial indu
Chunk 52: stry? 4. How can you use deep learning to
build better AI? 5. What are the ingredients of a deep learning model?\n\nA
study of the best deep learning algorithms and how they work, the techniques
involved in building them, and the results they can bring to the table: 1.
Unsupervised learning 2. Convolutional Neural Networks (CNN) 3. Recurrent Neural
Networks (RNN) 4. Deep Reinforcement Learning (RL) 5. Generative Adversarial
Networks (GAN)\n\nA step-by-step guide to building a real-world, deep le
Chunk 53: arning
app: 1. What is a deep learning app? 2. How do you build a deep learning app? 3.
The various components involved in building a deep learning app 4. How do you
deploy a deep learning app? 5. The future of deep learning apps\n\nHow to build
a deep learning app: A guide to the various tools, frameworks, and libraries
that you can use to build deep learning apps: 1. Deep Learning Frameworks 2.
Deep Learning Libraries 3. Deep Learning Tools 4&#x27;]
异步调用
llm.invoke(...)本质上是一个同步调用。在这种情况下，程序会在调用返回结果之
Chunk 54: 前停止执行
任何后续代码。这意味着如果invoke操作耗时较长，它会导致程序暂时挂起，直到操作完成。我们可
以通过这样一个测试代码来直观的理解同步调用：
import time
def call_model():
# 模拟同步API调用
print(&quot;开始调用模型...&quot;)
time.sleep(3) # 模拟调用等待
print(&quot;模型调用完成。&quot;)
def perform_other_tasks():
# 模拟执行其他任务
for i in range(3):
print(f&quot;执行其他任务 {i + 1}&quot;)
time.sleep(1)
def main():
start_time = time.time()
call_model()
perform_other_tasks()
end_time = time.time()
total_time = end_time - start_time
return f&quot;总共耗时：{total_time}秒&quot;
# 运行同步任务并打印完成时间
main_time = main()
main_time
开始调用模型...
模型调用完成。
执行其他
Chunk 55: 任务 1
执行其他任务 2
执行其他任务 3
&#x27;总共耗时：6.001835107803345秒&#x27;
这段同步调用的程序先模拟了一个耗时3秒的模型调用，随后执行了三个其他任务，每个任务耗时1
秒。实际的执行时间为约6.00秒。这体现了同步执行的特点：每个操作依次执行，直到当前操作完成后
才开始下一个操作，从而导致总的执行时间是各个操作时间的总和。
而异步调用，允许程序在等待某些操作完成时继续执行其他任务，而不是阻塞等待。这在处理I/O
操作（如网络请求、文件读写等）时特别有用，可以显著提高程序的效率和响应性。
我们可以设计这样如下情景：使用async和await关键字， 通过async def定义异步函数
async_call和perform_other_tasks。await asyncio.sleep(3)模拟了一个耗时3秒的异步操
作。这里的await表明程序在这里“等待”这个操作完成，但与同步操作不同，这个等待是非阻塞的。这
意味着程序可以在这3秒内执行其他异步代码。这种方式不会阻塞整个程序的运行，而是让出控制权，
允许事件循环继续运行其他任务。
! pip install asy
Chunk 56: ncio
Collecting asyncio
Obtaining dependency information for asyncio from
https://files.pythonhosted.org/packages/22/74/07679c5b9f98a7cb0fc147b1ef1cc1853b
c07a4eb9cb5731e24732c5f773/asyncio-3.4.3-py3-none-any.whl.metadata
Downloading asyncio-3.4.3-py3-none-any.whl.metadata (1.7 kB)
Downloading asyncio-3.4.3-py3-none-any.whl (101 kB)
---------------------------------------- 0.0/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- -----------------------
Chunk 57: ------------ 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
----------- --------------------------- 30.7/101.8 kB 163.8 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
-----
Chunk 58: ---------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
----------------------- --------------- 61.4/101.8 kB 113.0 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
-------------------------------------- 101.
Chunk 59: 8/101.8 kB 136.2 kB/s eta 0:00:00
Installing collected packages: asyncio
Successfully installed asyncio-3.4.3
import asyncio
import time
async def async_call(llm):
await asyncio.sleep(3) # 模拟异步操作
print(&quot;异步调用完成&quot;)
async def perform_other_tasks():
await asyncio.sleep(3) # 模拟异步操作
print(&quot;其他任务完成&quot;)
async def run_async_tasks():
start_time = time.time()
await asyncio.gather(
async_call(None), # 示例调用，替换None为模拟的LLM对象
perform_other_tasks()
)
end_time = time.time()
return f&quot;总共耗时：{end_time - start_time}秒&quot;
# 运
Chunk 60: 行异步任务并打印完成时间
await run_async_tasks()
异步调用完成
其他任务完成
&#x27;总共耗时：3.009281873703003秒&#x27;
使用asyncio.gather()并行执行时，理想情况下，因为两个任务几乎同时开始，它们的执行时间
将重叠。如果两个任务的执行时间相同（这里都是3秒），那么总执行时间应该接近单个任务的执行时
间（3秒左右），而不是两者时间之和。
理解了同步调用和异步调用的区别后，我们接下来看一下LangChain中集成的异步调用链使用方
法。
要实现异步调用并优化处理多个请求的场景，在LangChain中，可以使用ainvoke方法来达成。这
种方法允许同时启动多个请求，而不必等待每个请求依次完成。这对于提高应用程序的响应性和效率尤
其有用，特别是在处理需要与外部服务交互的任务时。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;这次您知道了吗？这是鸡肋大战，您是来玩的吧，我也是。我
Chunk 61: 来纠结你的，看看你敢不敢。我一般不来这种
鸡肋大战，因为我很懒，这次我还是来了，因为我懒惯了，其实我很想快点解脱了，我很想走。你的目的在哪
里？我想敌对你，我还是要意识到了，你想不想敌对我呢？你敢不敢这样问？我不敢，因为我懒。你的目的是
什么？我很想快点解脱，我不想你，我只想快点走，早点走。你敢不敢这样说？我敢，因为我懒。你的目的是
什么？我&#x27;
异步流式响应
from langchain_openai import OpenAI
async for chunk in llm.astream(
&quot;你好，&quot;,
):
print(chunk, end=&quot;&quot;, flush=True)
我是来投诉的 [Image]
你现在没有抱怨，说的确实很有道理，如果他们为了你不让你上，你能不上吗，我想大多数人应该会上，所以
他们是不会因为你不上而阻止你上的，但是如果你上了，你的优惠和你的一切都是不可能了，你上了，他们是
不能为你划上你的学费的，所以我觉得就算你上了也没有用，他们不会为你划学费，因为你上了从而给了他们
非常大的麻烦，你上了，他们是不能有一点好处的，他们还要为你上的学费等等付出，所以他们会说你上
Chunk 62: 不
了，就是为了不让你上，不让你上，他们不会为你划学费，所以我觉得你如果上了，你就没有任何
异步批量处理
from langchain_openai import OpenAI
await llm.abatch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，前来亲自拜访的，因为我们这里很少有人来拜访，你就打算在这里待上几个礼拜了吧？”“我是回来找你，
我是到桐谷这里来的。”李晓婷冷冷的回道。这一次很明显的不是她想让他留下来的，而是让他走了。众人看
着李晓婷，这里的她就这么冷了，竟然连郑晓东都不看一眼，郑晓东是她的男朋友，他们是一对，她怎么可能
不看一眼？“那你为什么不和我说，你的事情怎么样了？”郑晓东看向李晓婷的脸，她的表情很冷，她怎么不看
一眼自己&#x27;,
&#x27;？\n\n2:王景仪 2021-01-07 23:12:57\n\nannounced\n\n3:李桂林 2021-01-01
23:12:57\n\nMy Sunshine is a TV drama adapted from Gu Man’s novel of the same
title. It 
Chunk 63: stars Wallace Huo and Tang Yan, premiering in January, 2015. The drama
depicts a man and a woman’s sweet love story.\n\n4:王威 2020-12-27
23:12:57\n\nThe new data released yesterday represent the first snapshot of
global trade for 2015. And the figures suggest that 2015 was not a good year for
world trade, as it had been hoped.\n\n5:苏晓波 2021-01-01 23:12:57\n\nMark Rutte,
the Dutch prime minister whose People’s Party for Freedom and Democracy (VVD) is
slightly ahead of the PVV, has seen his poll nu
Chunk 64: mbers rise in recent days because
of his hardline stance with Ankara. On Tuesday he called Mr Erdogan’s comments
“a disgusting distortion of history”, adding: “We will not lower ourselves to
this level. It is totally unacceptable.”\n\n&#x27;,
&#x27;？深度学习是一种模仿大脑思维的机器学习方法，深度学习方法被广泛用于图像识别、语音识别、自然语言
处理等领域。深度学习过程中，模型经常需要大量的训练数据，因此大量的数据获取和数据刷新是有必要。.
1) Find a free server to host the application in. I have been trying to create a
simple neural network by using the TensorFlow Object Detection API to run an
object
Chunk 65:  detection model. Cloud TPU is a Google Cloud service that accelerates
machine learning workloads. Major cloud providers offer free tier of services to
help you get started. Google Cloud Storage is a RESTful web service for storing
and accessing data on Google Cloud Platform. In this video, you will learn about
Cloud TPU, a high-performance, scalable, and cost-effective machine learning
service. This IP address 157. 0, a neural network framework. Contribute.
TensorFlow Lite for mobile and embedd
Chunk 66: ed devices For Production TensorFlow
Extended for end-to-end ML components. The Google Cloud Platform&#x27;]
除此之外，在Jupyter Notebook或Python交互式解释器中进行操作的话，可以使用?来查看关于
llm中内置的方法和帮助文档，执行如下代码：
llm?
•[1;31mSignature:•[0m
•[0mllm•[0m•[1;33m(•[0m•[1;33m
•[0m •[0mprompt•[0m•[1;33m:•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mstop•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mcallbacks•[0m•[1;33m:•[0m •[1;34m&#x27;Callbacks&#x27;•[0m •[1;33m=•[0m
•[1;32mNone
Chunk 67: •[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m*•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mtags•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mmetadata•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[Dict[str, Any]]&#x27;•[0m
•[1;33m=•[0m •[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m**•[0m•[0mkwargs•[0m•[1;33m:•[0m
•[1;34m&#x27;Any&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m•[1;33m)•[0m •[1;33m-&gt;•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m•[0m•[1;33m•[0m•[0m
•[1;31mType:•[0m OpenAI
•[1;31mString form:•[0m
•[1mO
Chunk 68: penAI•[0m
Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {},
&#x27;max_tokens&#x27;: 256}
•[1;31mFile:•[0m d:\software\anaconda3\lib\site-
packages\langchain_openai\llms\base.py
•[1;31mDocstring:•[0m
OpenAI large language models.
To use, you should have the ``openai`` python package installed, and the
environment variable ``OPENAI_API_KEY`` set with your API key.
Any parameters that are valid to be passed to the op
Chunk 69: enai.create call can be
passed
in, even if not explicitly saved on this class.
Example:
.. code-block:: python
from langchain_community.llms import OpenAI
openai = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
•[1;31mInit docstring:•[0m
Create a new model by parsing and validating input data from keyword arguments.
Raises ValidationError if the input data cannot be parsed to form a valid model.
•[1;31mCall docstring:•[0m
[*Deprecated*] Check Cache and run the LLM on the given prompt and input.
Not
Chunk 70: es
-----
.. deprecated:: 0.1.7
Use invoke instead.
这里能看到非常多的关键信息，比如：
默认参数：Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {}, &#x27;max_tokens&#x27;: 256}
当不输入model_name时，默认会调用davinci-002模型：
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;我是小影。很高兴认识你，有什么可以帮到你的吗？&#x27;
同时，也可以调整温度temperature:
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
temperature=0
)
Chunk 71: 
&#x27;我是一个人工智能助手，很高兴认识你！有什么可以帮到你的吗？&#x27;
model：必选参数，具体调用的Completions模型名称，可以调用的模型包括text-davinci-003、
text-davinci-002、text-curie-001、text-babbage-001、text-ada-001等，不同模型参数规模不
同；这里需要注意，大模型领域不同于机器学习领域，后者哪怕是简单模型在某些场景下可能也会
拥有比复杂模型更好的表现。在大模型领域，（就OpenAI提供的A、B、C、D四大模型来看）参
数规模越大、越新版本的模型效果更好（当然费用也更高），因此课程中主要以text-davinci-003
使用为例进行讲解；
prompt：必选参数，提示词；
suffix：可选参数，默认为空，具体指模型返回结果的后缀；
max_tokens：可选参数，默认为16，代表返回结果的token数量；
temperature：可选参数，取值范围为0-2，默认值为1。参数代表采样温度，数值越小，则模型会
倾向于选择概率较高的词汇，生成的文本会更加保守；而当temperature值较高时，模型会
Chunk 72: 更多地
选择概率较低的词汇，生成的文本会更加多样；
top_p：可选参数，取值范围为0-1，默认值为1，和temperature作用类似，用于控制输出文本的
随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节
文本随机性，top_p和temperature两个参数选择一个进行调整即可；这里更推荐使用
temperature参数进行文本随机性调整；
n：可选参数，默认值为1，表示一个提示返回几个Completion；
stream：可选参数，默认值为False，表示回复响应的方式，当为False时，模型会等待返回结果
全部生成后一次性返回全部结果，而为True时，则会逐个字进行返回；
logprobs：可选参数，默认为null，该参数用于指定模型返回前N个概率最高的token及其对数概
率。例如，如果logprobs设为10，那么对于生成的每个token，API会返回模型预测的前10个
token及其对数概率；
echo：可选参数，默认为False，该参数用于控制模型是否应该简单地复述用户的输入。如果设为
True，模型的响应会尽可能地复述用户的输入；
Chunk 73: 
stop：可选参数，默认为null，该参数接受一个或多个字符串，用于指定生成文本的停止信号。当
模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成。这可以用来控制模型的输出长
度或格式；
presence_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容
（例如新的概念或主题）的倾向性。较高的值会使模型更倾向于生成新内容，而较低的值则会使模
型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取
值；
frequency_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾
向性。较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果
篇幅较大并且存在前后语言重复时，可以提高该参数的取值；
best_of：该参数用于控制模型的生成过程。它会让模型进行多次尝试（例如，生成5个不同的响
应），然后选择这些响应中得分最高的一个；
logit_bias：该参数接受一个字典，用于调整特定token的概率。字典的键是token的ID，值是应用
于该t
Chunk 74: oken的对数概率的偏置；在GPT中我们可以使用tokenizer tool查看文本Token的标记。一般
不建议修改；
user：可选参数，使用用户的身份标记，可以通过人为设置标记，来注明当前使用者身份。需要
注意的是，Completion.create函数中的user和后续介绍的对话类模型的user参数含义并不相同，
需要注意区分；
更详细的参数说明和使用细节可在《Ch.2 Completions模型API使用指南》中找到。此处不在展开
重复说明。
目前在LangChain中已经集成的LLMs有82个，均已经实现了对异步、流式处理和批处理的基本支
持，查看地址如下：https://python.langchain.com/docs/integrations/llms/
4.1.2 Chat Models接入方法
所谓聊天模型，简单理解就是我们经常使用的xxx-chat类模型，比如Baichuan2-7b-chat，
Qwen-14b-chat，这类模型是基于Base模型经过针对特定数据格式的微调得到，以更贴近人类对话的
方式进行输入和输出。所以这类模型不会使用“文本输入、文本输出”
Chunk 75: 的形式，而是采用聊天消息作为输
入并返回聊天消息作为输出（非纯文本）。LangChain框架与OpenAI、Cohere、Hugging Face等多家
模型提供商已经实现集成，提供了一个标准化的接口，以便与这些不同的模型进行交互。
我们先回顾下OpenAI规范下使用Chat模型进行一次交互需遵循的代码执行方式：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo-0125&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一个乐于助人的智能AI小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}
]
)
print(completion.choices[0].message.content)
您好，我是一个智能AI助手，可以帮助您回答问题、提供信息、解决问题等。如果您有任何疑问或需要帮助的
话，都可以随时向我提问。我会尽力帮助您解
Chunk 76: 决问题。
这里不得不再次提到，LangChain作为一个应用开发框架，需要集成各种不同的大模型，如上述
OpenAI的GPT系列模型调用示例，通过Message数据输入规范，定义不同的role，即system、user和
assistant来区分对话过程，但对于其他大模型，并不意味这一定会遵守这种输入输出及角色的定义，所
以LangChain的做法是，因为Chat Model基于消息而不是原始文本，LangChain目前就抽象出来的消息
类型有 AIMessage 、 HumanMessage 、 SystemMessage 、 FunctionMessage 和 ChatMessage ，
但大多时候我们只需要处理 HumanMessage 、 AIMessage 和 SystemMessage，即：
SystemMessage ：用于启动 AI 行为，作为输入消息序列中的第一个传入。
HumanMessage ：表示来自与聊天模型交互的人的消息。
AIMessage ：表示来自聊天模型的消息。这可以是文本，也可以是调用工具的请求。
同样，Chat Models实现了 Runnabl
Chunk 77: e 接口，也支持 invoke 、 ainvoke 、 stream 、 astream 、
batch 、 abatch 等调用。即：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
所以对于LangChain中调用OpenAI的GPT模型，首先需要导入对应的模块，代码如下：
from langchain_core.messages import HumanMessage, SystemMessage
消息形式输入调用
定义消息对象：
messages = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;你好，请你介绍一下你自己&quot;),]
messages
[SystemMessage(content=&#x27;你是一位乐于助人的智能小助手&#x27;), HumanMessage(content=&#x27;你好，请
你介绍一下你自己&#x27;)]
from langchain_openai import
Chunk 78:  ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
执行推理：
chat.invoke(messages)
AIMessage(content=&#x27;你好，我是一位可以回答问题、提供帮助的智能小助手。无论是关于知识、技能还是
日常生活中的问题，我都会尽力为您提供准确和有用的答案。有什么可以帮助您的吗？&#x27;)
如果只想获取到纯净的输出，可以通过.content来格式化。
reponse.content
&#x27;你好！我是一位智能助手，可以回答各种问题，提供信息和帮助。无论是关于知识、技能、娱乐、生活等方
面，我都会尽力为你提供帮助。有什么问题需要我帮忙的吗？&#x27;
流式调用
for chunk in chat.stream(messages):
print(chunk.content, end=&quot;&quot;, flush=True)
你好！我是一位智能助手，专门为用户提供信息咨询、问题解答和帮助服务。无论你有什么疑问或需求，我都
会尽力帮助你，让你更方便地获取所需的信息和支持。有什么我可以帮助你的吗？
批量调用
先定义三个不同的消息对象：
Chunk 79: 
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
messages2 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是深度学习&quot;),]
messages3 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是大模型技术&quot;),]
将上述三个消息对象放在一个列表中，使用.batch方法执行批量调用。
reponse = chat.batch([messages1,
messages2,
messages3,])
reponse
[AIMessage(content=&#x27;机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来
改善自身的性能，而无需明确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规
律、模式以及知识
Chunk 80: 的技术。通过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预
测、分类、识别等任务。机器学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测
等领域。&#x27;),
AIMessage(content=&#x27;深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神
经网络层来学习和理解复杂的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识
别、语音识别、自然语言处理等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对
新数据进行准确预测和分类。深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金
融、交通等各个领域。&#x27;),
AIMessage(content=&#x27;大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部
署复杂的深度学习模型。这些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优
化。\n\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模
型技术，研究人员和工程师可以构建更加准确和复杂的模型，从而提升机
Chunk 81: 器学习系统的性能和效果。\n\n然
而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员和
工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。&#x27;)]
格式化输出：
# 使用列表生成式打印每一个消息的内容
contents = [msg.content for msg in reponse]
# 打印出内容
for content in contents:
print(content, &quot;\n---\n&quot;)
机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来改善自身的性能，而无需明
确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规律、模式以及知识的技术。通
过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预测、分类、识别等任务。机器
学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测等领域。
---
深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神经网络层来学习和理解复杂
的数据。深度学习模型可以自动发现数据中的模式和特征
Chunk 82: ，从而实现诸如图像识别、语音识别、自然语言处理
等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对新数据进行准确预测和分类。
深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金融、交通等各个领域。
---
大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部署复杂的深度学习模型。这
些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优化。
大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模型技术，
研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。
然而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员
和工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。
---
异步调用
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
from langcha
Chunk 83: in_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
reponse = await chat.ainvoke(messages1)
reponse
AIMessage(content=&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并
改进性能，而无需显式地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断
优化自身的表现。机器学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常
见的机器学习方法包括监督学习、无监督学习、强化学习等。&#x27;)
reponse.content
&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并改进性能，而无需显式
地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断优化自身的表现。机器
学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常见的机器学习方法包括
监督学习、无监督学习、强化学习等。&#x27;
流式调用
a
Chunk 84: sync for chunk in chat.astream(messages1):
print(chunk.content, end=&quot;&quot;, flush=True)
机器学习是一种人工智能的分支，它致力于让计算机系统通过学习数据和模式，从而改进自身的性能。简而言
之，机器学习让计算机具有从经验中学习的能力，而不需要明确地编程指令。
在机器学习中，计算机系统利用大量数据进行训练，通过识别数据中的模式和规律来建立模型。这些模型可以
用来进行预测、分类、识别等任务。机器学习在各个领域中都有广泛的应用，如自然语言处理、图像识别、医
疗诊断、金融预测等。
常见的机器学习算法包括监督学习、无监督学习、强化学习等。监督学习是通过已标记的数据进行训练，无监
督学习则是在没有标记的数据中发现模式，强化学习则通过试错的方式学习最优策略。机器学习技术的发展使
得计算机系统能够更加智能地处理复杂任务，为人们的生活和工作带来了许多便利。
通过上述描述，我们展示了在LangChain中使用LLMs类模型和Chat Model类模型的不同方法，其
核心区别在于输入Prompt的格式。除此之外其他的工作可以直接利用
Chunk 85: 统一的抽象接口，实现与模型交
互的快速过程。而针对不同的模型，LangChain也提供个对应的接入方法，其相关说明文档地址：http
s://python.langchain.com/docs/integrations/chat/
比如我们以Baichuan2的在线API模型为例快速接入一下：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;我日薪8块钱，请问在闰年的二月，我月薪多少&quot;)])
response
AIMessage(content=&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所
以，你在闰年的二月月薪是232元。
Chunk 86: &#x27;)
response.content
&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所以，你在闰年的二月月
薪是232元。&#x27;
如未实践过Baichuan2的在线API，请查看《APICh.16 BaiChuan 2 大模型生态介绍及本地私有化
部署方案》中1. Baichuan在线大模型生态介绍小节
可以看出，LangChain能够迅速接入不同的模型API。至于接入本地私有部署的开源大模型的方
法，我们将在后续课程中进行详细介绍。
4.2 LangChain中如何使用Prompt Template
提示工程（Prompt Engineering）大家应该比较熟悉，这个概念是指在与大语言模型（LLMs），
如GPT-3、Qwen等模型进行交互时，精心设计输入文本（即提示）的过程，以获得更精准、相关或有
创造性的输出。正如我们在大模型技术应用课程中介绍的，通过采用Few-Shot、Chain of Thought
(CoT)等高级提示技巧，可以显著提高大模型在推理任务上的表现。目前，提示工程已经发展成为一个
专业领域，非常多的公司设立了专
Chunk 87: 门的职位，负责为特定任务编写精确且具有创造力的提示。
以使用ChatGPT等网页端对话交互应用中，大部分人常见的做法是将提示（Prompt）做硬编码，
例如将一段提示文本固定在System Messages中。而在应用开发领域，开发者往往无法预知用户的具体
输入内容，同时又希望大模型能够根据不同的应用任务以一种较为统一的逻辑来处理用户输入。所以，
LangChain通过提供Prompt Templates功能，优雅地解决了这个问题。Prompt Templates将用户输入
到完整格式化提示的转换逻辑进行封装，使得模型能够更灵活、高效地处理各种输入。
LangChain 提供了创建和使用提示模板的工具，其使用PromptTemplate 方法创建字符串提示模
板，因为目前为止，语言模型接收的提示基本都是字符串或聊天消息列表。
4.2.1 使用str.format语言构建模版
在LangChain的默认设置下， PromptTemplate 使用 Python 的 str.format() 方法进行模板
化。
Python的str.format()方法是一种字符串格式化的手段，允许我们在
Chunk 88: 字符串中插入变量。使用这
种方法，可以创建包含占位符的字符串模板，占位符由花括号{}标识。调用format()方法时，可以传入
一个或多个参数，这些参数将被顺序替换进占位符中。str.format()提供了灵活的方式来构造字符串，支
持多种格式化选项，包括数字格式化、对齐、填充、宽度设置等。
基本用法
# 简单示例，直接替换
greeting = &quot;Hello, {}!&quot;.format(&quot;Alice&quot;)
print(greeting)
# 输出: Hello, Alice!
Hello, Alice!
带有位置参数的用法
# 使用位置参数
info = &quot;Name: {0}, Age: {1}&quot;.format(&quot;Bob&quot;, 30)
print(info)
# 输出: Name: Bob, Age: 30
Name: Bob, Age: 30
带有关键字参数的用法
# 使用关键字参数
info = &quot;Name: {name}, Age: {age}&quot;.format(name=&quot;Charlie&quot;, age=25)
print(info)
# 输出: Name: Charlie, Age: 
Chunk 89: 25
Name: Charlie, Age: 25
使用字典解包的方式：
# 使用字典解包
person = {&quot;name&quot;: &quot;David&quot;, &quot;age&quot;: 40}
info = &quot;Name: {name}, Age: {age}&quot;.format(**person)
print(info)
# 输出: Name: David, Age: 40
Name: David, Age: 40
在LangChain中，基本采用了Python的原生str.format()方法对输入数据进行格式化，这样在模
型接收输入前，可以根据需要对数据进行预处理和结构化，以此来引导大模型进行更准确的推理。
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(
&quot;请给我一个关于{topic}的{type}解释。&quot;
)
prompt= prompt_template.format(type=&quot;详细&quot;, topic=&quot;量子力学&quot;)
prompt
&#x27;请给我一个关于量子力学的详细解释。&#x27;

Chunk 90: 如上所示，可以使用PromptTemplate的from_template方法创建一个提示模板实例，这个模板
包含了两个占位符：{topic} 和 {type}，这些占位符在实际调用时可以被实际的值替换。
调用LLMs模型
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
我觉得我已经对量子力学的基本理解了，但是我还是很想听听有关量子力学的详细解释，比如：量子力学是怎
么起作用的。
谢谢了.
量子力学是在克里姆林宫试验时的实验结果发现.一开始是用熟悉的物理学法则去解释它.比如说,量子力学是
量子力学的结果,而不是量子力学的原因.玻尔兹曼方程就是克里姆林宫试验结论的表示.后来人们发现,等效
性,柏格效应等现象.然后才发现,原来我们的光学和电磁学等理论是有问题的.这些现象的实现是在离子束中.
所以量子力学是很实际的.
[ 编辑 stuartm 在 06-07-08 12:11
Chunk 91:  ]
prompt= prompt_template.format(type=&quot;简短&quot;, topic=&quot;机器学习&quot;)
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
2)目前的时代，机器学习已经发展到了什么程度，也有什么应用场景。 3)目前的研究方向是什么。 4)什么
时候会用到这些技术。 5)最后一个问题是，对于我自己来说，是不是还有其他的方向比如说算法之类的，还
有在个人的技术知识里面是不是需要这些东西，不是特别需要。 也许这些都是我自己的猜测，如果你有什么
好的建议，我也非常感谢。 最后，我想问一下，你是在哪里学的，是在哪个大学，我就是在这里问问。 谢
谢。 2019.6.16
电影《伪装者》中的重要概念
原理：1. 无线电波的能量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无
线电波的方向不会发生改变，传播途中，波的方向会发生改变（这
Chunk 92: 就是偏转）3. 无线电波的方向会发生改
变，传播途中，波的方向不会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途
中，波的方向会发生改变（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变
（这就是遮挡）3. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）4. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）5. 无线电波的方向会发生改变，
传播途中，波的方向不会发生改变（这就是遮挡）6. 无线电波的方向会发生改变，传播途中，波的方向不会
发生改变（这就是遮挡）7. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）
8. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）原理：1. 无线电波的能
量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无线电波的方向不会发生改
变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改变，传播途中，波的方向不
会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不
Chunk 93: 会发生改变，传播途中，波的方向会发生改变
（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）3. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这
除此之外，这种模板生成方式支持任意数量的变量，包括无变量，可以做如下测试：
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(&quot;给我讲一个笑话&quot;)
prompt_template.format()
&#x27;给我讲一个笑话&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, )
result = llm.invoke(prompt)
print(result)
如果我是一个机器，会被称为什么？
谢谢
调用Chat Model
上述示例展示了如何使用字符串模板生成和调用LLMs类模型。对于聊天模型，也需要使用 Python
的 str.format() 方法进行模板化
Chunk 94: ，但其构造的输入形式则有所区别，应采用聊天消息列表的形式，
每条消息都包含内容和一个名为role的附加参数。例如，在OpenAI的Chat Completions API中，聊天
消息需要包含为assistant、user或system。在这种情况下，应使用ChatPromptTemplate来构建对话
模板。
from langchain_core.prompts import ChatPromptTemplate
chat_template = ChatPromptTemplate.from_messages(
[
(&quot;system&quot;, &quot;你是一个有帮助的AI机器人，你的名字是{name}。&quot;),
(&quot;human&quot;, &quot;你好，最近怎么样？&quot;),
(&quot;ai&quot;, &quot;我很好，谢谢！&quot;),
(&quot;human&quot;, &quot;{user_input}&quot;),
]
)
messages = chat_template.format_messages(name=&quot;小明&quot;, user_input=&quot;你叫什么名字？&quot;)
messages
[SystemMessage(content=&#x27;你是一个有帮助的AI机器人，
Chunk 95: 你的名字是小明。&#x27;),
HumanMessage(content=&#x27;你好，最近怎么样？&#x27;),
AIMessage(content=&#x27;我很好，谢谢！&#x27;),
HumanMessage(content=&#x27;你叫什么名字？&#x27;)]
print(chat_template)
input_variables=[&#x27;name&#x27;, &#x27;user_input&#x27;] messages=
[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;name&#x27;],
template=&#x27;你是一个有帮助的AI机器人，你的名字是{name}。&#x27;)),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[],
template=&#x27;你好，最近怎么样？&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;我很
好，谢谢！&#x27;)), HumanMessagePromptTe
Chunk 96: mplate(prompt=PromptTemplate(input_variables=
[&#x27;user_input&#x27;], template=&#x27;{user_input}&#x27;))]
从输出上看，其构造函数在实例化prompt_template时，主要由两个关键参数进行指定：
input_variables：这是一个列表，包含模板中需要动态填充的变量名。这些变量名在模板字符串
中以花括号（如{name}）标记。通过指定这些变量，可以在后续过程中动态地替换这些占位符。
template：这是定义具体提示文本的模板字符串。它可以包含静态文本和input_variables列表中
指定的变量占位符。当调用format方法时，这些占位符会被实际的变量值替换，生成最终的提示
文本。
调用Chat Model，这里我们选择GPT 3.5。代码如下：
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(me
Chunk 97: ssages)
print(result.content)
我叫小明，很高兴为您提供帮助！有什么问题可以帮到您？
messages = chat_template.format_messages(name=&quot;大熊&quot;, user_input=&quot;你要去哪里玩？&quot;)
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
作为一个AI机器人，我没有身体，所以无法去任何地方玩耍。但我可以帮助你规划旅行或提供有关旅游目的地
的信息。有什么我可以帮助你的吗？
通过上述流程，我们演示了如何在LangChain框架内使用ChatPromptTemplate来构建与聊天模型
交互的对话模板。通过此方法，能够创建一个包含不同参与者角色和定制化消息内容的对话流程，这对
于开发复杂的聊天应用或增强AI助手的交互能力至关重要。
4.2.2 构造Few-S
Chunk 98: hot模版
在LangChain中，很多的功能抽象、链路抽象本质上都是在对大模型的“涌现能力”能够应用落地的
一种具体实现方法，而其推理的不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度
依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。最为简单的
提示工程的方法就是通过输入一些类似问题和问题答案，让模型参考学习，并在同一个prompt的末尾
提出新的问题，依次提升模型的推理能力。
比如我们在《Ch.3 基于思维链的进阶提示工程》中测试的经典推理问题：
import os
import openai
openai.api_key = os.getenv(&quot;OPENAI_API_KEY&quot;)
prompt = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次？&#x27;
prompt
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from openai import OpenAI
client = OpenAI()
resp
Chunk 99: onse1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n在关闭之前，艾米能滑14次。&#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n\n15分钟 = 900秒\n\n每次滑下来需要1分钟，所以在15分钟内，可以滑下900秒/（1分钟+4分钟）=
900/5 = 180次。&#x27;
可以看到多次执行，其推理均是错误的。正确的计算过程应该是先计算艾米一次爬上爬下总共需要
5分钟，然后滑梯还有15分钟关闭，因此关闭之前能够再滑15/5=3次。
而我们当时在OpenAI的API规范下，是通过这种方
Chunk 100: 式来进行Few-Shot的提示：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在
Chunk 101: 总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
在Few-Shot的提示下，再次尝试进行提问：
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
promp
Chunk 102: t=prompt_Few_shot_CoT4,
max_tokens=1000,
)
response1.choices[0].text
&#x27;”艾米花了1分钟滑下来，滑一次上去需要4分钟，因此在15分钟关闭之前，她能滑15分钟-1分钟=14分钟，
14分钟除以4分钟每次，她能滑14÷4=3次，答案是3次。“ &#x27;
从输出结果上看，通过Few-Shot提示可以激发大模型的涌现能力从而回答对复杂推理问题，在应用
开发中同样需要这种能力来提升大模型的推理稳定性。所以LangChain抽象了
FewShotPromptTemplate方法来实现。我们可以先测试下在LangChain的接口下直接提问：
直接提问
prompt1 = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在
关闭之前她能滑多少次？&#x27;
prompt1
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gp
Chunk 103: t-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在关闭之前，艾米能滑(15-4-1)/4=2次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在15分钟内，她能滑15次。&#x27;
其实也很容易想到，LangChain的工作仅仅是集成并接入OpenAI的GPT模型，因此其本质上与直接
调用GPT模型的效果相同。出现错误回答也属于正常情况。接下来，我们根据LangChain对Few-Shot模
板的定义规范，进行提示词的构建。
在LangChain中，需要使用 PromptTemplate 创建字符串提示模板。模板可以包括说明、少量示
例以及适合给定任务的特定上下文和问题。因此，我们要创建一个少量示例的列表。每个示例都是一个
字典，其中键是输入变量，值是这些输入变量的值。
适合LLMs模型的模版构造形式
from langchain.pro
Chunk 104: mpts.few_shot import FewShotPromptTemplate
from langchain.prompts.prompt import PromptTemplate
examples = [
{
&quot;question&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个
网球？&quot;,
&quot;answer&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现
在总共由5+6=11个网球。因此答案是11。&quot;
},
{
&quot;question&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在
食堂总共有多少个苹果？&quot;,
&quot;answer&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答
案是9。&quot;
},
{
&quot;question&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？&quot;,
&quot;answer&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4
个，答案是4个
Chunk 105: 。&quot;
},
]
接下来需要实例化一个 PromptTemplate 对象，将少量示例格式化为字符串。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[0]))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[1]))
Question: 食堂总共有2
Chunk 106: 3个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
最后，创建一个 FewShotPromptTemplate 对象。该对象接受经过上一步格式化后的少数样本示
例。
# 导入 FewShotPromptTemplate 类
from langchain.prompts.few_shot import FewShotPromptTemplate
# 创建一个 FewShotPromptTemplate 对象
few_shot_prompt = FewShotPromptTemplate(
examples=examples, # 最开始定义的 examples 示例
example_prompt=example_prompt, # 上一步定义的 example_prompt 作为提示模板
suffix=&quot;Question: {input}&quot;, # 后缀模板，其中 {input} 会被替换为实际输入
input_variables=[&quot;input&quot;] # 定义输
Chunk 107: 入变量的列表
)
# 使用给定的输入格式化 prompt，并打印结果
print(few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，
水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Question: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有
多少个蓝色高尔夫球？
总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
Question: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将
Chunk 108: 在15分钟后关闭，请问在关
闭之前她能滑多少次？
final_input = few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟
才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)
final_input
&#x27;Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰
一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此
答案是11。\n\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现
在食堂总共有多少个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹
果，答案是9。\n\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是
蓝色的。请问总共有多少个蓝色高尔夫球？\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是
蓝色的，也就是4个，答案是4个。\n\nQuestion: 艾米需要4分
Chunk 109: 钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&#x27;
将带有Few-Shot提示的输入，输入到大模型中执行推理。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米需要4分钟爬到顶部，1分钟滑下来，一共花费5分钟。在15分钟内，她能滑3次，因为15÷5=3。因此
答案是3次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米花了5分钟（4分钟爬上去+1分钟滑下来），因此在15分钟内可以滑3次（15÷5=3）。答案是3次。&#x27;
从输出结果来看，显然受到了提示工程的正面影响，通过问题的拆解过程显著提高了回答的准确
性。
适合Chat Model的提示模版构
Chunk 110: 造形式
我们在前面解释过LLMs和Chat Model 的区别，因其接受的输入形式不同，所以构造的提示模版也
是不一样的，对于Chat Model来说，它接收的是聊天形式的对话格式，所以对于其对应的Few-Shot，
在LangChain中的基本结构如下：
examples ：要包含在最终提示中的字典示例列表。
example_prompt ：通过其 format_messages 方法将每个示例转换为 1 条或多条消息。一个常
见的示例是将每个示例转换为一条人工消息和一条人工智能消息响应，或者一条人工消息后跟一条
函数调用消息。
我们还是使用四个数学推理题来进行测试：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉2
Chunk 111: 0个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有
Chunk 112: 多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
直接调用
对于Chat Model，需要将输入转化成对话聊天格式：
from langchain_core.messages import HumanMessage, SystemMessage
messages = [SystemMessage(content=&quot;你是一个擅长数学推理的专家&quot;),
HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟
后关闭，请问在关闭之前她能滑多少次？&quot;),]
实例化模型进行推理：
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
resonse = chat.invoke(messages)
resons
Chunk 113: e.content
&#x27;首先，艾米花了1分钟滑下来，所以她每次滑下来后需要等待3分钟（4分钟上去 + 1分钟下来 = 5分钟，15
分钟总时间 - 5分钟 = 10分钟，10分钟 / 3分钟 = 3次）。\n\n在水滑梯关闭之前，艾米可以滑3次。&#x27;
&amp;esmp; 实际上，在使用GPT-3.5时，我们可以观察到，即使不采用Few-Shot提示，模型也能以很高
的概率正确回答问题，这归功于模型本身已经非常强大的能力。所以我们改为Baichuan2模型来测试：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关
Chunk 114: 闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟滑下来，所以她在滑梯关闭前可以滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟从滑梯顶部滑下来，所以她可以在滑梯关闭前滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，所以每次滑行需要4+1=5分钟。\n\n水滑梯将在
15分钟后关闭，所以她能在关闭之前滑行15÷5=3次。&#x27;
可以看到，在切换到Baichuan模型进行测试时，三次测试中出现了两次错误回答。因此，我们就
使用Baichuan2模型，尝试通过添加Few-Shot来探索是否能增
Chunk 115: 强其推理能力。
首先，安装LangChain的规范，导入生成示例的模块：
from langchain.prompts import (
ChatPromptTemplate,
FewShotChatMessagePromptTemplate,
)
接下来以input-output对的形式构建Few-Shot提示模板。在这里，“input”可以被视作人类发出的
Promot，而“output”则是大模型给出的回答。
examples = [
{&quot;input&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网
球？&quot;,
&quot;output&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总
共由5+6=11个网球。因此答案是11。&quot;},
{&quot;input&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共
有多少个苹果？&quot;,
&quot;output&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&quot;},
{&quot;input&quot;: &quot;杂耍者可以杂耍16个球。
Chunk 116: 其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问
总共有多少个蓝色高尔夫球？&quot;,
&quot;output&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答
案是4个。&quot;},
]
然后，使用from_messages方法进行模版构造。
# This is a prompt template used to format each individual example.
example_prompt = ChatPromptTemplate.from_messages(
[
(&quot;human&quot;, &quot;{input}&quot;),
(&quot;ai&quot;, &quot;{output}&quot;),
]
)
few_shot_prompt = FewShotChatMessagePromptTemplate(
example_prompt=example_prompt,
examples=examples,
)
print(few_shot_prompt.format())
Human: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
AI: 罗杰一开始有五个网球
Chunk 117: ，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网
球。因此答案是11。
Human: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹
果？
AI: 食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Human: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少
个蓝色高尔夫球？
AI: 总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
生成最终的输入模版，设置 input 作为动态变量：
final_prompt = ChatPromptTemplate.from_messages(
[
few_shot_prompt,
(&quot;human&quot;, &quot;{input}&quot;),
]
)
final_prompt
ChatPromptTemplate(input_variables=[&#x27;input&#x27;], messages=
[FewShotChatMessagePromptTemplate(examp
Chunk 118: les=[{&#x27;input&#x27;: &#x27;罗杰有五个网球，他又买了两盒网
球，每盒有3个网球，请问他现在总共有多少个网球？&#x27;, &#x27;output&#x27;: &#x27;罗杰一开始有五个网球，又购买了两
盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此答案是11。&#x27;}, {&#x27;input&#x27;:
&#x27;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？&#x27;,
&#x27;output&#x27;: &#x27;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&#x27;}, {&#x27;input&#x27;: &#x27;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。
请问总共有多少个蓝色高尔夫球？&#x27;, &#x27;output&#x27;: &#x27;总共有16个球，其中一半是高尔夫球，也就是8个，其中
一半是蓝色的，也就是4个，答案是4个。&#x27;}],
example_prompt=ChatPromptTemplate(input_variables=[&#x27;input&#x27;, &#x27;output&#x27;], messages=
[HumanMessagePromptTemplate(pro
Chunk 119: mpt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;output&#x27;],
template=&#x27;{output}&#x27;))])),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;))])
实例化baichuan2模型：
最后，使用LangChain中的chain的抽象，合并最终的提示和大模型共同执行。
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-e9&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后
Chunk 120: 关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米需要4分钟才能爬上去，1分钟滑下来，总共需要4+1=5分钟。15分钟可以滑15÷5=3次，所以答案是3
次。&#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米爬上去需要4分钟，滑下来只需要1分钟，所以她可以在15分钟内完成4次滑行，因为每次滑行需要
4+1=5分钟。&#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54695e9529&quot;,
streaming=True,
)
response = c
Chunk 121: hain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，然后1分钟滑下来，总共需要4+1=5分钟。因此，在滑梯关闭之前，她可
以滑15÷5=3次。&#x27;
4.3 LangChain中的Output Parsers
Output Parsers，即输出解析器，这个概念非常好理解，就是负责获取大模型的输出并将其转换为
更合适的格式。这在应用开发中及其重要。在大多数复杂应用场景中，处理逻辑往往环环相扣，执行某
项业务逻辑可能需要多次调用大模型，其中上一次的调用结果将被用于指导下一次调用的逻辑。在这种
情况下，结构化的信息会比纯文本又有价值，同时这也是输出解析器的价值所在。
LangChain构造的输出解释器必须实现两个主要方法：
Get format instructions：该方法会返回一个字符串，其中包含有关如何格式化语言模型输出的指
令。
Parse：该方法会接收字符串，并将其解析为某种结构
目前已经支持的解析格式已
Chunk 122: 经包括Json、Xml、Csv以及OpenAI的Tools和Functions等多种格式，
具体可看：https://python.langchain.com/docs/modules/model_io/output_parsers/
简而言之，这个过程本质上是对文本的格式化处理。但与传统的后处理不同，LangChain采取的实
现策略是通过在Prompt上增加输出解析，直接引导大模型输出的格式化方式。
我们以格式化日期来看一下LangChain的构造思路，首先导入相关的模块：
from langchain.output_parsers import DatetimeOutputParser
from langchain.prompts import PromptTemplate
from langchain_openai import OpenAI
实例化日期解析器：
output_parser = DatetimeOutputParser()
通过.get_format_instructions方法，可以查看定义的解析格式：
output_parser.get_format_ins
Chunk 123: tructions()
&quot;Write a datetime string that matches the following pattern: &#x27;%Y-%m-
%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1181-01-05T02:20:33.675873Z, 1938-12-
13T09:46:36.791229Z, 1468-01-17T07:34:51.108688Z\n\nReturn ONLY this string, no
other words!&quot;
构造输入模版，这里的区别是：在输入的Prompt Template中，加入了OutPut Parse的内容：
template = &quot;&quot;&quot;用户发起的提问:
{question}
{format_instructions}&quot;&quot;&quot;
生成最终实际输入大模型的Promot内容：
prompt = PromptTemplate.from_template(
template,
# 预定义的变量，这里我们传入格式化指令
partial_variables={&quot;format_instructions&quot;:
output_p
Chunk 124: arser.get_format_instructions()},
)
prompt
PromptTemplate(input_variables=[&#x27;question&#x27;], partial_variables=
{&#x27;format_instructions&#x27;: &quot;Write a datetime string that matches the following
pattern: &#x27;%Y-%m-%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1472-10-23T17:39:56.199188Z,
1643-01-14T10:55:11.903091Z, 0631-12-11T23:04:04.345333Z\n\nReturn ONLY this
string, no other words!&quot;}, template=&#x27;用户发起的提
问:\n\n{question}\n\n{format_instructions}&#x27;)
最后，使用LangChain中的chain的抽象，合并最终的提示、大模型实例及OutPut Parse共同执
行。
chain = prompt | O
Chunk 125: penAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请问你叫什么？&quot;})
print(output)
2021-10-07 12:02:13.345333
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是机器学习&quot;})
print(output)
2018-01-23 09:42:32.123456
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是深度学习&quot;})
print(output)
2019-08-07 18:30:00
通过测试可以明显观察到，以此方式定义的输出解析器极大地规范了模型的输出。当然，这也直接
受到大模型实际能力和模板定义情况的影响。
LangChain中不同模版的使用方式，除了可以在官方文档上看到，在其源码上看会更加直观，
Chunk 126: 而且
可以自定义修改，其所有的OutPut Parse的模版存储路径为：
langchain/libs/langchain/langchain/output_parsers ， 大家可以自行尝试。比如我们上面使
用的日期解析的源码对应如下：
到此为止，我们已经非常详尽地介绍了LangChain的核心概念。如果有学习过大模型技术实战课程
的同学来说，会发现LangChain的Model I/O模块中讨论的所有概念都是之前接触过的，需要学习的主
要是LangChain对大模型进行抽象的方法以及在LangChain框架下的应用开发技巧。
而从LangChain的整体开源情况看，其内容丰富且迭代非常快，通过模块化的方式介绍，可以帮助
大家快速构建起对LangChain以及应用开发的思维框架。尽管在Model I/O模块中还有许多更高级的内
容，例如如何接入本地部署的开源大模型，关于函数调用、Memory缓存等更深入的主题，这些内容我
们将在后续章节中一一接触并进行详细的探讨。
```

然而，这种方法的一个明显缺点是由于仅依据长度进行切分，切分后的片段可能无法保持完整的语义。但并不意味着它不适用于文本切分任务。例如，这种方法非常适合于处理日志文件或代码块，其中文本通常以固定长度或格式出现，或者在处理来自传感器或其他实时数据源的流数据时，固定长度切分可以确保数据被均匀地处理和分析。这些应用场景中，数据的结构和形式通常是预定和规范的，因此即便是按固定长度进行切分，反而会更有利于对数据的理解和使用。

### 3.2.3 结合重叠窗口的固定字符数切分

重复窗口的意义是：块之间保持一些重叠，以确保语义上下文不会在块之间丢失。在文本处理和其他数据分析领域，"重叠"（overlap）指的是连续数据块之间共享的部分。这种方法特别常见于信号处理、语音分析、自然语言处理等领域，其中数据的连续性和上下文信息非常重要。比如下述代码所示：

```
def split_text_by_fixed_length_with_overlap(text, length, overlap):
    # 使用列表推导式按固定长度及重叠长度切分文本
    return [text[i:i + length] for i in range(0, len(text) - overlap, length - overlap)]
```

调用测试：

```
# 文本
text = &quot;春节的脚步越来越近，大街小巷都布满了节日的气氛。商店门口挂满了红灯笼和春联，家家户户都在忙着打扫卫生，准备迎接新的一年。\
小明回到家乡，感受到了浓浓的过年氛围。他在街上走着，看到小朋友们手持烟花棒，欢笑声此起彼伏。\
夜幕降临，整个城市亮起了五彩缤纷的灯光，映照着人们脸上的喜悦与期待。老人们聚在一起，回忆过去，展望未来。\
而年轻人则在夜市享受美食，放松心情。这是一个充满希望和喜悦的时刻，每个人都在以自己的方式庆祝这个特殊的节日。&quot;

# 定义每个片段的长度和重叠长度
chunk_length = 100
overlap_length = 30

# 调用函数进行分割
result = split_text_by_fixed_length_with_overlap(text, chunk_length, overlap_length)

# 打印结果
for i, segment in enumerate(result):
    print(f&quot;Chunk {i+1}: {segment}&quot;)
```

```
Chunk 1: 春节的脚步越来越近，大街小巷都布满了节日的气氛。商店门口挂满了红灯笼和春联，家家户户都在忙着打扫卫生，准备迎接新的一年。小明回到家乡，感受到了浓浓的过年氛围。他在街上走着，看到小朋友们手持烟花棒，欢笑
Chunk 2: 了浓浓的过年氛围。他在街上走着，看到小朋友们手持烟花棒，欢笑声此起彼伏。夜幕降临，整个城市亮起了五彩缤纷的灯光，映照着人们脸上的喜悦与期待。老人们聚在一起，回忆过去，展望未来。而年轻人则在夜市享受美食
Chunk 3: 老人们聚在一起，回忆过去，展望未来。而年轻人则在夜市享受美食，放松心情。这是一个充满希望和喜悦的时刻，每个人都在以自己的方式庆祝这个特殊的节日。
```

接下来，我们切分刚才的加载的PDF文档的全部内容，其存储在变量extracted_text中。

```
# 定义每个片段的长度和重叠长度
chunk_length = 500
overlap_length = 20

# 调用函数进行分割
result = split_text_by_fixed_length_with_overlap(extracted_text, chunk_length, overlap_length)

# 打印结果
for i, segment in enumerate(result):
    print(f&quot;Chunk {i+1}: {segment}&quot;)
```

```
Chunk 1: 开源大模型应用开发实战
Ch 18 LangChain整体项目介绍与核心模块Model I/O详
解
注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情
况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，
将迅速更新教材内容以确保信息的准确性和实用性。
本期课件更新时间：2024年3月7日
在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地
私有化部署开源大模型。在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家
关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部
署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部
署。
但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也
是最简单的一步。熟练掌握基于大模型的上层应用开发还有非常长的路需要走。目前，大模型技术开发
总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API
Chunk 2: 基于OpenAI、GLM4这种在线API的应用开发，而另一类，就是先私有化
部署开源大模型，再借助开发框架构建大模型应用。当然了，目前主流的大模型应用开发框架也支持在
线模型API的调用，这正是我们之前一直强调学习掌握开源或在线API调用技巧的原因，因为这些方法和
技巧直接对应于应用开发的最佳实践。
而不论采用哪种开发形式，其基座和所依赖的，核心还是大模型本身。对于在线大模型，以
OpenAI为例，开发者需要配置科学上网的环境，在其官网创建 API Key，最后遵循API的使用说明去进
行应用配置。这一过程中涉及到的网络环境适配，数据隐私安全，成本管理及如何去定制化自己需求的
开发流程，往往都需要遵循官方提供的功能来实现。也正是因为这些复杂的因素使得尽管目前公认最强
的大语言模型GPT 4开放给开发者使用，还是会有非常多的公司和个人去拥抱开源，选择性能可能略逊
一筹的开源大模型，因为开源方案在某些方面提供了更多的灵活性和自主控制能力。
大家也能够发现，在我们的课程中每当涉及到大模型的私有化部署，我们都会向大家展示如何通过
命令行、网页端访问和API调用等三种方式与部署的大模型进行交互。
Chunk 3: I调用等三种方式与部署的大模型进行交互。此外，还通过text-generate-
webui这样的开源项目，这一切应用是帮助大家能够以最简单、最直接的方式与开源大模型进行对话，
了解大模型的运行模型。但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶
段肯定无法满足实际的需求。虽然目前的大模型基本都可以按照OpenAI的API规范形式进行调用，但如
果进行实操我们会发现，在《大模型技术实战》的课程中，借助OpenAI的GPT系列模型开发的项目，
如智能邮件处理、全自动AI开发流程，以及MateGen Agents等，尽管在开源模型上能够被复现，但这
背后的成功关键并非仅仅在于模型调用。例如，在《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进
行外部工具API调用开发实践》中，我们通过Qwen模型复现了《Ch.10 借助Function calling调用外部
工具API方法》中的天气信息调用过程。这一复现过程揭示了，对在线模型来说，我们需要学习的是其
API使用方法。而在开源大模型的技术领域下，要想实现某些特定功能，重点在于理解该模型的一些使
用原理，以及
Chunk 4: 重点在于理解该模型的一些使
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
代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度
Chunk 5: 加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高
的。LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架。它的做法是：通过提
供标准化且丰富的模块抽象，构建大语言模型的输入输入规范，利用其核心概念chains，灵活地连接
整个应用开发流程。而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出
来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发
领域内被普遍高度认可的通用框架。
为什么需要这样做？
首先，我们需考虑当前大模型的发展态势。尽管OpenAI的GPT系列模型作为大模型领域的领军人
物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大
模型间的使用方式完全相同。例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不
适用。因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是
十分低下的。
其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据
连接方式以
Chunk 6: 外部API调用能力、私有数据
连接方式以及输出结果的不稳定性等问题。在应用开发中，如何找到这些问题的有效解决策略？
上述提到的每个限制都紧密关联于大模型本身的特性。尽管理论上可以通过重新训练、微调来增强
模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资
源、时间和财力，选择这条路径一定会导致方向越来越偏离目标。我们之前讨论的OpenAI的Function
Calling和通过ReAct流程接入第三方API能够提供一些解决方案，但这每一步都需大量的研发投入，而且
最终实现后的应用效果，也取决于研发人员的个人技术能力。在这种背景下，既然大家都有不同的想法
和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的
快速搭建，这就是LangChain一直想要做到，且正在做的事情。
LangChain的做法
从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对
大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接
口以适配各种大模型。到
Chunk 7: 象，设计出统一接
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
记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的&quot;Chain&quot;高级抽象，定义了
不同形式下标准的链接方法，这就能够允许开发者
Chunk 8: 形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整
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
Chunk 9: 们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过
程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API
调用规范：
response = openai.ChatCompletion.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是机器学习？&quot;}
]
)
而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话
交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象
出一个chain，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核
心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和
Output Parser（对应下图中的Parse部分）。
Format：即指代Prompts Template
Chunk 10: ：即指代Prompts Template，通过模板化来管理大模型的输入；
Predict：即指代Models，使用通用接口调用不同的大语言模型；
Parse：即指代Output部分，用来从模型的推理中提取信息，并按照预先设定好的模版来规范化
输出。
整个Model I/O工作流如下图所示：
Format
对于Prompt Template第一部分，传统上我们创建提示词是通过手工编写来实现的，在这个过程中
会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能。然而，
在应用开发中，一个关键的考量是提示词不能是一成不变的。其原因在于，应用开发需要适应多变的用
户需求和场景。固定的提示词限制了模型的灵活性和适用范围。例如，如果我们正在开发一个天气查询
应用，用户可能会以多种方式提出查询，如“今天的天气怎么样？”或“明天纽约的温度是多少度？”。如
果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求。
而Prompt Template，就像我们在介绍《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进行外
部工具API调
Chunk 11: 理介绍及流程拆解，进行外
部工具API调用开发实践》中不断优化出来的ReAct模版，将API的使用、问题解答过程等复杂逻辑封装
成了一套结构化的格式。我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引
导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：
# 将一个插件的关键信息拼接成一段文本的模版。
TOOL_DESC = &quot;&quot;&quot;{name_for_model}: Call this tool to interact with the
{name_for_human} API. What is the {name_for_human} API useful for?
{description_for_model} Parameters: {parameters}&quot;&quot;&quot;
# ReAct prompting 的 instruction 模版，将包含插件的详细信息。
PROMPT_REACT = &quot;&quot;&quot;Answer the following questions as best you can. You have
access to the followin
Chunk 12: cess to the following APIs:
{tool_descs}
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can be repeated zero or more
times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question
Begin!
Question: 
Chunk 13: on
Begin!
Question: {query}&quot;&quot;&quot;
因此，引入Prompt Template可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输
入动态调整提示词，从而更好地响应用户的具体需求。LangChain通过这种方式来提高应用的通用性和
用户体验。
Predict
在Predict部分，实质上是处理模型从接收输入到执行推理的整个过程。考虑到存在两种主要类型的
大模型——Base类模型和Chat类模型，LangChain在其Model I/O模块中对这两种模型都进行了抽象，
分别归类为LLMs（Large Language Models）和Chat Models。我们还是以OpenAI 的 Completion 和
Chatcompletions方法为例：
# Base类模型
client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=&quot;Say this is a test&quot;,
)
# 聊天模型
client.chat.completions.create(
model=&quot;gpt-3.5-t
Chunk 14: te(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
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
Chunk 15: l I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如
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
伙伴包，安装此依赖包后，LangChain才能够与Ope
Chunk 16: 赖包后，LangChain才能够与OpenAI的API进行交互。后续我们在使用相关功能的时
候，会提供额外的说明。
LangChain安装官方说明文档：https://python.langchain.com/docs/get_started/installation
3.1 使用包版本管理工具安装
LangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建
过程。这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成
LangChain的安装。具体操作如下：
! pip install langchain
Requirement already satisfied: langchain in d:\software\anaconda3\lib\site-
packages (0.1.10)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain) (6.0)
Requir
Chunk 17: gchain) (6.0)
Requirement already satisfied: SQLAlchemy&lt;3,&gt;=1.4 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.4.39)
Requirement already satisfied: aiohttp&lt;4.0.0,&gt;=3.8.3 in
d:\software\anaconda3\lib\site-packages (from langchain) (3.8.5)
Requirement already satisfied: dataclasses-json&lt;0.7,&gt;=0.5.7 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.6.4)
Requirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain) (1.33)
Chunk 18: om langchain) (1.33)
Requirement already satisfied: langchain-community&lt;0.1,&gt;=0.0.25 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.25)
Requirement already satisfied: langchain-core&lt;0.2,&gt;=0.1.28 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.1.28)
Requirement already satisfied: langchain-text-splitters&lt;0.1,&gt;=0.0.1 in
d:\software\anaconda3\lib\site-packages (from langchain) (0.0.1)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\li
Chunk 19: oftware\anaconda3\lib\site-packages (from langchain) (0.1.14)
Requirement already satisfied: numpy&lt;2,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (1.24.3)
Requirement already satisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packag
Chunk 20: nda3\lib\site-packages (from langchain) (8.2.2)
Requirement already satisfied: attrs&gt;=17.3.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (22.1.0)
Requirement already satisfied: charset-normalizer&lt;4.0,&gt;=2.0 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(2.0.4)
Requirement already satisfied: multidict&lt;7.0,&gt;=4.5 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(6.0.2)
Requirement already s
Chunk 21: equirement already satisfied: async-timeout&lt;5.0,&gt;=4.0.0a3 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(4.0.2)
Requirement already satisfied: yarl&lt;2.0,&gt;=1.0 in d:\software\anaconda3\lib\site-
packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (1.8.1)
Requirement already satisfied: frozenlist&gt;=1.1.1 in
d:\software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.3.3)
Requirement already satisfied: aiosignal&gt;=1.1.2 in
d:\software\anaconda3\
Chunk 22: \software\anaconda3\lib\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)
(1.2.0)
Requirement already satisfied: marshmallow&lt;4.0.0,&gt;=3.18.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (3.21.0)
Requirement already satisfied: typing-inspect&lt;1,&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-
&gt;langchain) (0.9.0)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from json
Chunk 23: -packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain)
(2.1)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (3.5.0)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2,&gt;=0.1.28-
&gt;langchain) (23.2)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain) (3.9.15)
Requir
Chunk 24: ain) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain)
(2.16.1)
Requirement already satisfied: typing-extensions&gt;=4.6.1 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (4.7.1)
Requirement already satisfied: idna&lt;4,&gt;=2.5 in d:\software\anaconda3\l
Chunk 25: software\anaconda3\lib\site-
packages (from requests&lt;3,&gt;=2-&gt;langchain) (3.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(1.26.16)
Requirement already satisfied: certifi&gt;=2017.4.17 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)
(2023.11.17)
Requirement already satisfied: greenlet!=0.4.17 in
d:\software\anaconda3\lib\site-packages (from SQLAlchemy&lt;3,&gt;=1.4-&gt;langchain)
(2.0.1)
Requirement 
Chunk 26: (2.0.1)
Requirement already satisfied: sniffio&gt;=1.1 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (1.2.0)
Requirement already satisfied: mypy-extensions&gt;=0.3.0 in
d:\software\anaconda3\lib\site-packages (from typing-inspect&lt;1,&gt;=0.4.0-
&gt;dataclasses-json&lt;0.7,&gt;=0.5.7-&gt;langchain) (1.0.0)
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：
import langchain
print(langchain.__version__)
0.1.10
如果能正常输出LangChain
Chunk 27: .10
如果能正常输出LangChain的版本，说明在当前环境下的安装成功。
3.2 源码安装
除了通过pip安装外，还有一种通过源码安装的方法。这需要使用git拉取远程仓库，然后进入项目
文件夹并执行pip install -e .命令。这种方法不仅会安装必要的依赖，同时也将程序的源代码保存
在本地。通常来说，除非大家打算作为LangChain的协作开发者参与到项目的功能更新中，否则没必要
采用这种安装方式，直接使用pip安装更为简便。
对于课程学习而言，我们推荐采用源码安装方式，这将非常有助于在后续的LangChain功能探索
中，通过源码分析深入理解框架的构建原理和详细机制。
接下来我们以Ubuntu操作系统为例，介绍源码安装LangChain的详细步骤：
Step 1. 安装Anaconda
如果本地服务器是Ubuntu系统，请参考：《Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B
模型》中的2.3 安装Anaconda环境小节；而如果是Windows操作系统，请参考：《Ch.6 在Windows
系统下部署运行ChatGLM3-6B模型》中的3.2.
Chunk 28: ChatGLM3-6B模型》中的3.2.1 方式一：使用Anaconda创建项目依赖环境（推荐）小节
中，执行完Step 6. 验证conda步骤，回到本文继续执行后续的操作：
注：如果部署环境是租赁的云服务器，Anaconda环境已经预设好，并不需要我们再次手动配置。
按照对应的教程内容配置好Anaconda后，可以通过如下命令验证当前环境的Conda：
conda --version
如果可以正常输出Conda的版本，则说明Anaconda安装成功。
Step 2. 使用Conda创建LangChain的Python虚拟环境
安装好Anaconda后，我们需要借助Conda包版本工具，为LangChain项目创建一个新的Python虚
拟运行环境，执行代码如下：
conda create --name langchain python==3.11
如上所示，新创建了一个名为langchain的Python虚拟环境，其Python版本为3.11。创建完成后，
通过如下命令进入该虚拟环境，执行后续的操作：
conda activate langchain
Step 3. 下载LangC
Chunk 29: hain
Step 3. 下载LangChain的项目文件
进入LangChain的官方Github，地址：https://github.com/langchain-ai/langchain ， 在 GitHub
上将项目文件下载到有两种方式：克隆 (Clone) 和 下载 ZIP 压缩包。推荐使用克隆 (Clone)的方式。我
们首先在GitHub上找到其仓库的URL。
在执行命令之前，需要先安装git软件包，执行命令如下：
sudo apt install git
执行克隆命令，将LangChain Github上的项目文件下载至本地的当前路径下，如下：
git clone https://github.com/langchain-ai/langchain.git
Step 4. 升级pip版本
建议在执行项目的依赖安装之前升级 pip 的版本，如果使用的是旧版本的 pip，可能无法安装一些
最新的包，或者可能无法正确解析依赖关系。升级 pip 很简单，只需要运行命令如下命令：
python -m pip install --upgrade pip
Step 5. 源码安装项目依赖
Chunk 30: pip
Step 5. 源码安装项目依赖
不同于我们之前一直使用的pip install -r requirements.txt方式，这种方法用于批量安装多
个依赖包，是在部署项目或确保开发环境与其他开发者/环境一致时的常用方式。而对于LangChain，我
们需要使用pip install -e的方式，以可编辑模式安装包。这种方式主要用于开发过程中。当以可编
辑模式安装一个包时，依赖包会被直接从源代码所在位置安装，而不是复制到Python的site-packages
目录，是开发模式下用于安装并实时反映对本地包更改的方法。需要执行的步骤如下：
cd langchain/libs/langchain/
pip install -e .
如在安装过程未发生任何报错，则说明安装成功。在安装完依赖后，我们就正式进入LangChain的
Model I/O模块的实践。
注：如果采用的是源码安装，需要参考《Ch.11 大模型开发工具Transformers库的使用及配置大
模型开发环境》中远程服务器部署大模型，本地Jupyter NoteBook加载小节，在LangChain的虚拟
环境下启动J
Chunk 31: 在LangChain的虚拟
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
Chunk 32: ngChain框架下先定义相关的类和方法来规定如
何与模型进行交互，包括数据的输入和输出格式以及如何连接到模型本身。然后按照 OpenAI GPT 模型
的接口规范来集成这些功能。通过这种方式，LangChain 充当一个桥梁，使我们能够按照统一的标准来
接入和使用多种不同的大语言模型。
在LangChain的Model I/O模块中，集成了两类模型，分别是LLMs和聊天模型，这两种模型在输
入和输出的数据类型上有所区别。
接下来我们就以OpenAI的API为示例，分别实践一下在LangChain框架中如何使用这两种不同类型
的大语言模型。首先来看LLMs类模型。
4.1.1 LLMs类模型接入方法
LLMs指的是纯文本补全模型，其封装的 API 输入接收的是字符串，输出也是字符串。 OpenAI 的
GPT-3 是作为 LLM 实现的。最简单的理解：LLMs指代的就是Base类模型，这类模型仅仅可以理解并
生成自然语言或代码，并未按照特定指令进行训练。截止到2024年3月07日，OpenAI可支持调用的
Base类模型仅有2个，分别是babbage-002和davinci-002，除
Chunk 33: ge-002和davinci-002，除此之外，还可以使用gpt-3.5-turbo-
instruct调用OpenAI 的 Completion方法。
首先需要安装OpenAI的集成依赖包langchain-openai，执行如下命令：
! pip install langchain-openai
Requirement already satisfied: langchain-openai in
d:\software\anaconda3\lib\site-packages (0.0.8)
Requirement already satisfied: langchain-core&lt;0.2.0,&gt;=0.1.27 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.1.28)
Requirement already satisfied: openai&lt;2.0.0,&gt;=1.10.0 in
d:\software\anaconda3\lib\site-packages (from langchain-o
Chunk 34: es (from langchain-openai) (1.13.3)
Requirement already satisfied: tiktoken&lt;1,&gt;=0.5.2 in
d:\software\anaconda3\lib\site-packages (from langchain-openai) (0.6.0)
Requirement already satisfied: PyYAML&gt;=5.3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (6.0)
Requirement already satisfied: anyio&lt;5,&gt;=3 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.5.0)
Requirement already satisfied: jsonpatch&lt;
Chunk 35: atisfied: jsonpatch&lt;2.0,&gt;=1.33 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (1.33)
Requirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (0.1.14)
Requirement already satisfied: packaging&lt;24.0,&gt;=23.2 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (23.2)
Requirement already satisfied: pydantic&lt;3
Chunk 36: atisfied: pydantic&lt;3,&gt;=1 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.6.0)
Requirement already satisfied: requests&lt;3,&gt;=2 in d:\software\anaconda3\lib\site-
packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.31.0)
Requirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in
d:\software\anaconda3\lib\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-
&gt;langchain-openai) (8.2.2)
Requirement already satisfied: distro&lt;2,&gt;=1.7.0 in
d:
Chunk 37: stro&lt;2,&gt;=1.7.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (1.8.0)
Requirement already satisfied: httpx&lt;1,&gt;=0.23.0 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (0.25.1)
Requirement already satisfied: sniffio in d:\software\anaconda3\lib\site-
packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (1.2.0)
Requirement already satisfied: tqdm&gt;4 in d:\software\anaconda3\lib\site-packages
(from openai&lt;2.0.0,
Chunk 38: 
(from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (4.65.0)
Requirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in
d:\software\anaconda3\lib\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-
openai) (4.7.1)
Requirement already satisfied: regex&gt;=2022.1.18 in
d:\software\anaconda3\lib\site-packages (from tiktoken&lt;1,&gt;=0.5.2-&gt;langchain-
openai) (2022.7.9)
Requirement already satisfied: idna&gt;=2.8 in d:\software\anaconda3\lib\site-
packages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langc
Chunk 39: .2.0,&gt;=0.1.27-&gt;langchain-openai)
(3.4)
Requirement already satisfied: certifi in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(2023.11.17)
Requirement already satisfied: httpcore in d:\software\anaconda3\lib\site-
packages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)
(1.0.1)
Requirement already satisfied: jsonpointer&gt;=1.9 in
d:\software\anaconda3\lib\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain-
core&lt;0.2.0,&gt;=
Chunk 40: chain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.1)
Requirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in
d:\software\anaconda3\lib\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-
&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.9.15)
Requirement already satisfied: annotated-types&gt;=0.4.0 in
d:\software\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (0.6.0)
Requirement already satisfied: pydantic-core==2.16.1 in
d:\software\anaconda3\lib\sit
Chunk 41: re\anaconda3\lib\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.16.1)
Requirement already satisfied: charset-normalizer&lt;4,&gt;=2 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.0.4)
Requirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in
d:\software\anaconda3\lib\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-
core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (1.26.16)
Requirement already satisfied: col
Chunk 42: ready satisfied: colorama in d:\software\anaconda3\lib\site-
packages (from tqdm&gt;4-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.4.6)
Requirement already satisfied: h11&lt;0.15,&gt;=0.13 in
d:\software\anaconda3\lib\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-
&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.14.0)
使用OpenAI的 GPT 系列模型的API有两个前提条件：首先，需要配置科学上网以确保可以访问
OpenAI服务；其次，必须拥有一个有效的API Key。这两项要求的详细配置在《Ch.1 本地调用OpenAI
API流程及OpenAI官网使用指南》中有详细说明，因此我们在此不再赘述，直接进入实操过程。
查看OpenAI的依赖包版本
OpenAI 的 API 在 0.28 版本和 
Chunk 43: AI 的 API 在 0.28 版本和 1.xx 版本之间的调用方式存在差异。我们这里使用的是最新的调用
方法，即采用 1.13.XX 版本。因此，需要确认当前 Python 虚拟环境中安装的 openai 包版本是 1.xx。
可以通过执行以下命令来检查当前安装的版本：
import openai
print(openai.__version__)
0.28.0
如果如上显示是0.28.0版本，执行如下命令将openai包升级到最新版本：
# ! pip install --upgrade openai
注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。
import openai
print(openai.__version__)
1.13.3
设置科学上网环境
根据所使用的科学上网软件，查找相应的代理端口，并按照以下代码进行网络环境配置：
# 这里的端口，请修改为实际使用科学上网软件配置的代理端口
!set HTTP_PROXY=http://127.0.0.1:15732
!set HTTPS_PROXY=http://127.0.0.1:
Chunk 44: XY=http://127.0.0.1:15732
新版本的OpenAI API调用方法不允许使用openai.api_key= &quot;sk-xxx&quot;这种老版本的显式指定API
Key，所以如果没有在系统变量中进行配置的话，使用下面的命令设置环境变量：
# 将your- API -key-here替换为实际的API密钥:
# ! setx OPENAI_API_KEY &quot;sk-xxxx&quot;
成功: 指定的值已得到保存。
注意：完成环境变量的初次配置后，需重启电脑以激活设置。这样，在下次使用时就可以直接调用
配置好的环境。为验证环境变量中的API Key是否已正确设置为自己的，可以执行以下命令进行检查。
# ! echo %OPENAI_API_KEY%
完成了科学上网环境和API Key的设置后，在接入LangChain之前，我们先使用如下测试代码检查
当前环境及API的运行状态是否正常，执行代码如下：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=
Chunk 45: tions.create(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
]
)
print(completion.choices[0].message.content)
你好，我是一位可以回答问题、提供信息和与您交流的AI助手。我可以帮助解决问题，回答疑惑，提供建议，
而且乐意与您分享知识和信息。有什么我可以帮助你的吗？
若测试代码能正常输出结果，表明OpenAI的环境配置无误。需要注意的是，LangChain接入
OpenAI GPT模型只是利用其定义的标准模型接入框架来整合OpenAI API，因此进行上述连通性测试
是必要的。
OpenAI GPT Base模型官方说明：https://platform.openai.com/docs/models/moderation
LangChain为了使开发者可以轻松地创建自定义链，整体采用Runnable协议。Runnabl
Chunk 46: 采用Runnable协议。Runnable 协议是
编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可
以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了
Runnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。
LangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch
ain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable
而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解
的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。
如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI
Chunk 47: 间去看看源码的抽象逻辑。比如OpenAI：
LangChain Interface说明：https://python.langchain.com/docs/modules/model_io/llms/quic
k_start
LangChain已集成LLMs说明：https://python.langchain.com/docs/integrations/llms/
首先，我们来了解常规的调用方法。具体支持的调用方式如下所示：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
接下来我们对上述方法依次进行尝试。在LangChain框架中，为了规范地接入LLMs类型的大模
型，首先需要导入OpenAI的依赖包。导入命令如下：
from langchain_openai import OpenAI
字符串形式的输入
调用invoke方法，并提供字符串格式的Prompt，便可触发指定模型按照所给Prompt进行续写操
作。
from langchain_op
Chunk 48: 作。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
llm.invoke(
&quot;你好呀，请你介绍一下你自己&quot;
)
&#x27;吧。”韩寒摇头，说：“我下次再来，你们自己给我介绍好了。”这一次，韩寒的风趣让陈航漂了一下，他说：
“还是先看看大家的房间吧。”陈航带着韩寒到了楼下，这时连小李都已经来到了楼下，他对韩寒说：“你看，
这是我们的宿舍。”陈航的语气是那么的平淡，既没有过多的炫耀，也没有明显的拒绝，只是那么的平淡，这
让韩寒很有点奇怪，他说：“你们是在这里住吗？”小李说：“是啊，我们是这里的住宿生。”韩寒对小李说：
“那我是来参加考试的，你&#x27;
从上述模型的返回输出，我们可以看到它本质上作为一个补全模型，通过使用大模型原生的
.generate() 方法来对输入的Prompt进行续写。这部分的详细内容已在《Ch.2 Completions模型API
使用指南》中进行了深入讲解，因此此处不再进行赘述。
上述调用方法相当于使用我们熟悉的OpenAI的Completion方法来调用GPT Base类型
Chunk 49: etion方法来调用GPT Base类型模型的过
程：
from openai import OpenAI
client = OpenAI()
completion = client.completions.create(
model=&quot;davinci-002&quot;,
prompt=&quot;你好呀，请你介绍一下你自己&quot;,
max_tokens=1024,
)
print(completion.choices[0].text)
。» 她把咳嗽调整成百米冲刺那种负重颤抖。»不，我们在上面，请你告诉会场。哪一地方过去你都要做朋友
呢？」Larsen一转眼注意到卡萨姆旁站着一位古老的男人。他身着粗布制服，最左边有一种圆盏形的供奶
头。透映着那个老的质朴魅力和吓坏人恶心的样子。»LarsenKeberichTaylor.Ch02 3/5/07 5:27 PM
Page 39LEARNING WHERE WECAME FROM»喂主持人。自从我给你们滴剂的时候，我就耐心等待了。请为
快点谈到刚才提的内容。»正赞成的木质松枝变苍白了。既然他是最耐烦的老爷，我们劝劝他这样三小时也
烦。时光久了会累人的人，他更应
Chunk 50: 三小时也
烦。时光久了会累人的人，他更应该知道这些。»好，每个人都听到。»那些选手的名字有老项目拯救人」，
Brickman说。AnderMartin、Taylor, här nere – efter en minut eller så. Likdant för
LarsenKeberich, rätt så bra ger du tillbaka vätsken, yeah, and the eighth
man.»»Eighth man?」Brickman坏坏垂下眼帘扬扬眉头，头盔上的唇影随着他挺起了脸颊。好，一位是
我。不，我不是。Just a moment.»六记地雷爆炸。嫣红的色泽弥漫在鼻子周围极其耀眼之下，淀粉饼糊塞
了全脸，剔饭一般的巧克力混合在一起。»»Would you all please proceed into the next
building and make sure that you use the turnstiles.»伤口破口流血，簇瓣的血色俗套，但去
还是要用。套房好美啊、清生。套房饲養下只要一个；_ 户功能装置就。»艰难地活着足以获得一天的欢乐
Chunk 51: 能装置就。»艰难地活着足以获得一天的欢乐的
人确实算够奇特的。咳嗽的木质松枝_cancel6 3/5/07 5:27 PM Page 40IT IS NOT ENOUGH
TOMAKE MANKINDHAPPY,LARSEN KEBERICH向报纸走来，她在走廊哪一个厅卡萨姆等待着;在这个保证已
经到达那种别有心碎的正式屋子里。她穿过镶有玫瑰花的拉链的抢洗衣区和单人舔饭的厕所区圆朝圣地的道。
先是葡萄干牛油，透过臭风从供应箱中飘来，刚开始时她便一直寻找那一种苏查尔的酸味。那种味美常是发出
着从薄饼里流发出来的所以一圈难找到对不起，小姐，我们之前选择做了决定，就算更改好一个时刻，也无法
改变决定。拨动键盘-找麻烦你的，怎么做了、哦，听起来好像你一突然卸了山猫带来了 Asktohell你疼
吗。杨虎大吼着，冲向对方。丁
流式输出
要进行流式调用，可以调用stream方法。这种方式允许在接收到模型响应的同时，逐块输出结
果，实现即时展示而无需等待全部内容处理完毕。
from langchain_openai import OpenAI
for chunk in llm.stream(
&quot;你好
Chunk 52: k in llm.stream(
&quot;你好呀，请你介绍一下你自己&quot;
):
print(chunk, end=&quot;&quot;, flush=True)
吧，你是谁，你在哪里，你是什么学问，你喜欢什么，你不喜欢什么，你不会什么，你会什么，你梦想什么，
你的优点是什么，你的缺点是什么，你的性格是什么，你的兴趣是什么，你喜欢的食物是什么，你不喜欢的食
物是什么，你的爱好是什么，你的愿望是什么。介绍一下你自己好的。
[ 编辑 xiaofan1 在 07-03-18 23:49 ]
批量生成
通过调用batch方法，可以同时对多个不同的Prompt进行调用处理。在这个例子中，输入包括“你
好”、“什么是机器学习”和“什么是深度学习”三个独立的Prompt。这种批量处理方式允许一次性提交多个
Prompt，从而提高推理效率。
from langchain_openai import OpenAI
llm.batch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，习惯了你的成熟，可是这次你的决定让我感到非常惊讶，我不知道你的这次决定是不是真的，我不知道你
是否真的那么爱我，虽然我知道
Chunk 53: 我不知道你
是否真的那么爱我，虽然我知道你非常爱我，可是我不知道你是不是真的爱我，假如你为了我不想去上学，我
也可以放弃一切，去读书，去上大学，但是你为什么要这样做呢，我不清楚你的想法，我不知道你的想法，我
只知道我爱你，我只知道我不想失去你，我只知道我不想让你失去你的希望，我只知道我不想让你失去你的未
来，可是我想知道你为什么要这样做呢，我想知道你的想法，你是&#x27;,
&#x27; 机器学习的原理 机器学习的技术 机器学习的应用机器学习在生产过程中的作用机器学习在生产过程中的
作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产
过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学
习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作
用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学&#x27;,
&#x27;？ 2. Why is the industry so interested in deep learning? 3. How will de
Chunk 54: ning? 3. How will deep
learning transform the financial industry? 4. How can you use deep learning to
build better AI? 5. What are the ingredients of a deep learning model?\n\nA
study of the best deep learning algorithms and how they work, the techniques
involved in building them, and the results they can bring to the table: 1.
Unsupervised learning 2. Convolutional Neural Networks (CNN) 3. Recurrent Neural
Networks (RNN) 4. Deep Reinforcement Learning (RL) 5. Generative Adversarial
Networks (GA
Chunk 55: rsarial
Networks (GAN)\n\nA step-by-step guide to building a real-world, deep learning
app: 1. What is a deep learning app? 2. How do you build a deep learning app? 3.
The various components involved in building a deep learning app 4. How do you
deploy a deep learning app? 5. The future of deep learning apps\n\nHow to build
a deep learning app: A guide to the various tools, frameworks, and libraries
that you can use to build deep learning apps: 1. Deep Learning Frameworks 2.
Deep Learning Librar
Chunk 56: Deep Learning Libraries 3. Deep Learning Tools 4&#x27;]
异步调用
llm.invoke(...)本质上是一个同步调用。在这种情况下，程序会在调用返回结果之前停止执行
任何后续代码。这意味着如果invoke操作耗时较长，它会导致程序暂时挂起，直到操作完成。我们可
以通过这样一个测试代码来直观的理解同步调用：
import time
def call_model():
# 模拟同步API调用
print(&quot;开始调用模型...&quot;)
time.sleep(3) # 模拟调用等待
print(&quot;模型调用完成。&quot;)
def perform_other_tasks():
# 模拟执行其他任务
for i in range(3):
print(f&quot;执行其他任务 {i + 1}&quot;)
time.sleep(1)
def main():
start_time = time.time()
call_model()
perform_other_tasks()
end_time = time.time()
total_time = end_time - start_t
Chunk 57: = end_time - start_time
return f&quot;总共耗时：{total_time}秒&quot;
# 运行同步任务并打印完成时间
main_time = main()
main_time
开始调用模型...
模型调用完成。
执行其他任务 1
执行其他任务 2
执行其他任务 3
&#x27;总共耗时：6.001835107803345秒&#x27;
这段同步调用的程序先模拟了一个耗时3秒的模型调用，随后执行了三个其他任务，每个任务耗时1
秒。实际的执行时间为约6.00秒。这体现了同步执行的特点：每个操作依次执行，直到当前操作完成后
才开始下一个操作，从而导致总的执行时间是各个操作时间的总和。
而异步调用，允许程序在等待某些操作完成时继续执行其他任务，而不是阻塞等待。这在处理I/O
操作（如网络请求、文件读写等）时特别有用，可以显著提高程序的效率和响应性。
我们可以设计这样如下情景：使用async和await关键字， 通过async def定义异步函数
async_call和perform_other_tasks。await asyncio.sleep(3)模拟了一个耗时3秒的异步操
作。这里的awa
Chunk 58: 了一个耗时3秒的异步操
作。这里的await表明程序在这里“等待”这个操作完成，但与同步操作不同，这个等待是非阻塞的。这
意味着程序可以在这3秒内执行其他异步代码。这种方式不会阻塞整个程序的运行，而是让出控制权，
允许事件循环继续运行其他任务。
! pip install asyncio
Collecting asyncio
Obtaining dependency information for asyncio from
https://files.pythonhosted.org/packages/22/74/07679c5b9f98a7cb0fc147b1ef1cc1853b
c07a4eb9cb5731e24732c5f773/asyncio-3.4.3-py3-none-any.whl.metadata
Downloading asyncio-3.4.3-py3-none-any.whl.metadata (1.7 kB)
Downloading asyncio-3.4.3-py3-none-any.whl (101 kB)
-------------------------
Chunk 59: ----------------------------------- 0.0/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--
----------- --------------------------- 30.7/101.8 kB 163.8 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
Chunk 60: 1.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01
----------------------- --------------- 61.4/101.8 kB 113.0 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
-----------------
Chunk 61: 01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01
-------------------------------------- 101.8/101.8 kB 136.2 kB/s eta 0:00:00
Installing collected packages: asyncio
Successfully installed asyncio-3.4.3
import asyncio
import time
async def async_call(llm):
await asyncio.sleep(3) # 模拟异步操作
print(&quot;异步调用完成&quot;)
async def perform_other_tasks():
await asyncio.sleep(3) # 模拟异步操作
print(&quot;其他任务完成&quot;)
async d
Chunk 62: nt(&quot;其他任务完成&quot;)
async def run_async_tasks():
start_time = time.time()
await asyncio.gather(
async_call(None), # 示例调用，替换None为模拟的LLM对象
perform_other_tasks()
)
end_time = time.time()
return f&quot;总共耗时：{end_time - start_time}秒&quot;
# 运行异步任务并打印完成时间
await run_async_tasks()
异步调用完成
其他任务完成
&#x27;总共耗时：3.009281873703003秒&#x27;
使用asyncio.gather()并行执行时，理想情况下，因为两个任务几乎同时开始，它们的执行时间
将重叠。如果两个任务的执行时间相同（这里都是3秒），那么总执行时间应该接近单个任务的执行时
间（3秒左右），而不是两者时间之和。
理解了同步调用和异步调用的区别后，我们接下来看一下LangChain中集成的异步调用链使用方
法。
要实现异步调用并优化处理多个请求的场景，在LangChain中，可以使用a
Chunk 63: 场景，在LangChain中，可以使用ainvoke方法来达成。这
种方法允许同时启动多个请求，而不必等待每个请求依次完成。这对于提高应用程序的响应性和效率尤
其有用，特别是在处理需要与外部服务交互的任务时。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;)
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;这次您知道了吗？这是鸡肋大战，您是来玩的吧，我也是。我来纠结你的，看看你敢不敢。我一般不来这种
鸡肋大战，因为我很懒，这次我还是来了，因为我懒惯了，其实我很想快点解脱了，我很想走。你的目的在哪
里？我想敌对你，我还是要意识到了，你想不想敌对我呢？你敢不敢这样问？我不敢，因为我懒。你的目的是
什么？我很想快点解脱，我不想你，我只想快点走，早点走。你敢不敢这样说？我敢，因为我懒。你的目的是
什么？我&#x27;
异步流式响应
from langchain_openai import OpenAI
async for chunk in llm.astream(
&quot;你好，&quot;,
):
p
Chunk 64: astream(
&quot;你好，&quot;,
):
print(chunk, end=&quot;&quot;, flush=True)
我是来投诉的 [Image]
你现在没有抱怨，说的确实很有道理，如果他们为了你不让你上，你能不上吗，我想大多数人应该会上，所以
他们是不会因为你不上而阻止你上的，但是如果你上了，你的优惠和你的一切都是不可能了，你上了，他们是
不能为你划上你的学费的，所以我觉得就算你上了也没有用，他们不会为你划学费，因为你上了从而给了他们
非常大的麻烦，你上了，他们是不能有一点好处的，他们还要为你上的学费等等付出，所以他们会说你上不
了，就是为了不让你上，不让你上，他们不会为你划学费，所以我觉得你如果上了，你就没有任何
异步批量处理
from langchain_openai import OpenAI
await llm.abatch(
[
&quot;你好&quot;,
&quot;什么是机器学习&quot;,
&quot;什么是深度学习&quot;,
]
)
[&#x27;，前来亲自拜访的，因为我们这里很少有人来拜访，你就打算在这里待上几个礼拜了吧？”“我是回来找你，
我是到桐谷这里来的。”李晓婷冷冷的回道。这一次很明显的不是她想让他留下来的，而是让他走了。众人看
Chunk 65: 是她想让他留下来的，而是让他走了。众人看
着李晓婷，这里的她就这么冷了，竟然连郑晓东都不看一眼，郑晓东是她的男朋友，他们是一对，她怎么可能
不看一眼？“那你为什么不和我说，你的事情怎么样了？”郑晓东看向李晓婷的脸，她的表情很冷，她怎么不看
一眼自己&#x27;,
&#x27;？\n\n2:王景仪 2021-01-07 23:12:57\n\nannounced\n\n3:李桂林 2021-01-01
23:12:57\n\nMy Sunshine is a TV drama adapted from Gu Man’s novel of the same
title. It stars Wallace Huo and Tang Yan, premiering in January, 2015. The drama
depicts a man and a woman’s sweet love story.\n\n4:王威 2020-12-27
23:12:57\n\nThe new data released yesterday represent the first snapshot of
global t
Chunk 66: snapshot of
global trade for 2015. And the figures suggest that 2015 was not a good year for
world trade, as it had been hoped.\n\n5:苏晓波 2021-01-01 23:12:57\n\nMark Rutte,
the Dutch prime minister whose People’s Party for Freedom and Democracy (VVD) is
slightly ahead of the PVV, has seen his poll numbers rise in recent days because
of his hardline stance with Ankara. On Tuesday he called Mr Erdogan’s comments
“a disgusting distortion of history”, adding: “We will not lower ourselves to
this leve
Chunk 67: rselves to
this level. It is totally unacceptable.”\n\n&#x27;,
&#x27;？深度学习是一种模仿大脑思维的机器学习方法，深度学习方法被广泛用于图像识别、语音识别、自然语言
处理等领域。深度学习过程中，模型经常需要大量的训练数据，因此大量的数据获取和数据刷新是有必要。.
1) Find a free server to host the application in. I have been trying to create a
simple neural network by using the TensorFlow Object Detection API to run an
object detection model. Cloud TPU is a Google Cloud service that accelerates
machine learning workloads. Major cloud providers offer free tier of services to
help you get started. Google
Chunk 68:  get started. Google Cloud Storage is a RESTful web service for storing
and accessing data on Google Cloud Platform. In this video, you will learn about
Cloud TPU, a high-performance, scalable, and cost-effective machine learning
service. This IP address 157. 0, a neural network framework. Contribute.
TensorFlow Lite for mobile and embedded devices For Production TensorFlow
Extended for end-to-end ML components. The Google Cloud Platform&#x27;]
除此之外，在Jupyter Notebook或Python交互式解释器中进行操作的话，可以使用?来查看关于
ll
Chunk 69: 进行操作的话，可以使用?来查看关于
llm中内置的方法和帮助文档，执行如下代码：
llm?
•[1;31mSignature:•[0m
•[0mllm•[0m•[1;33m(•[0m•[1;33m
•[0m •[0mprompt•[0m•[1;33m:•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mstop•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mcallbacks•[0m•[1;33m:•[0m •[1;34m&#x27;Callbacks&#x27;•[0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m*•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mtags•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m
•[1
Chunk 70: [0m •[1;33m=•[0m
•[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[0mmetadata•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[Dict[str, Any]]&#x27;•[0m
•[1;33m=•[0m •[1;32mNone•[0m•[1;33m,•[0m•[1;33m
•[0m •[1;33m**•[0m•[0mkwargs•[0m•[1;33m:•[0m
•[1;34m&#x27;Any&#x27;•[0m•[1;33m,•[0m•[1;33m
•[0m•[1;33m)•[0m •[1;33m-&gt;•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m•[0m•[1;33m•[0m•[0m
•[1;31mType:•[0m OpenAI
•[1;31mString form:•[0m
•[1mOpenAI•[0m
Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalt
Chunk 71:  0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {},
&#x27;max_tokens&#x27;: 256}
•[1;31mFile:•[0m d:\software\anaconda3\lib\site-
packages\langchain_openai\llms\base.py
•[1;31mDocstring:•[0m
OpenAI large language models.
To use, you should have the ``openai`` python package installed, and the
environment variable ``OPENAI_API_KEY`` set with your API key.
Any parameters that are valid to be passed to the openai.create call can be
passed
in, even if not explicitly saved on this class.
Example:
.. code-bloc
Chunk 72: xample:
.. code-block:: python
from langchain_community.llms import OpenAI
openai = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
•[1;31mInit docstring:•[0m
Create a new model by parsing and validating input data from keyword arguments.
Raises ValidationError if the input data cannot be parsed to form a valid model.
•[1;31mCall docstring:•[0m
[*Deprecated*] Check Cache and run the LLM on the given prompt and input.
Notes
-----
.. deprecated:: 0.1.7
Use invoke instead.
这里能看到非常多的关键信息，比如：
默认参数：Params
Chunk 73: 关键信息，比如：
默认参数：Params: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,
&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {}, &#x27;max_tokens&#x27;: 256}
当不输入model_name时，默认会调用davinci-002模型：
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
)
&#x27;我是小影。很高兴认识你，有什么可以帮到你的吗？&#x27;
同时，也可以调整温度temperature:
from langchain_openai import OpenAI
llm = OpenAI()
await llm.ainvoke(
&quot;哈喽，你好呀，&quot;,
temperature=0
)
&#x27;我是一个人工智能助手，很高兴认识你！有什么可以帮到你的吗？&#x27;
model：必选参数，具体调用的Completions
Chunk 74: 选参数，具体调用的Completions模型名称，可以调用的模型包括text-davinci-003、
text-davinci-002、text-curie-001、text-babbage-001、text-ada-001等，不同模型参数规模不
同；这里需要注意，大模型领域不同于机器学习领域，后者哪怕是简单模型在某些场景下可能也会
拥有比复杂模型更好的表现。在大模型领域，（就OpenAI提供的A、B、C、D四大模型来看）参
数规模越大、越新版本的模型效果更好（当然费用也更高），因此课程中主要以text-davinci-003
使用为例进行讲解；
prompt：必选参数，提示词；
suffix：可选参数，默认为空，具体指模型返回结果的后缀；
max_tokens：可选参数，默认为16，代表返回结果的token数量；
temperature：可选参数，取值范围为0-2，默认值为1。参数代表采样温度，数值越小，则模型会
倾向于选择概率较高的词汇，生成的文本会更加保守；而当temperature值较高时，模型会更多地
选择概率较低的词汇，生成的文本会更加多样；
top_p：可选参数，取值范
Chunk 75: 更加多样；
top_p：可选参数，取值范围为0-1，默认值为1，和temperature作用类似，用于控制输出文本的
随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节
文本随机性，top_p和temperature两个参数选择一个进行调整即可；这里更推荐使用
temperature参数进行文本随机性调整；
n：可选参数，默认值为1，表示一个提示返回几个Completion；
stream：可选参数，默认值为False，表示回复响应的方式，当为False时，模型会等待返回结果
全部生成后一次性返回全部结果，而为True时，则会逐个字进行返回；
logprobs：可选参数，默认为null，该参数用于指定模型返回前N个概率最高的token及其对数概
率。例如，如果logprobs设为10，那么对于生成的每个token，API会返回模型预测的前10个
token及其对数概率；
echo：可选参数，默认为False，该参数用于控制模型是否应该简单地复述用户的输入。如果设为
True，模型的响应会尽可能地复述用户的输入；
stop：可选参数，默认为null，该
Chunk 76: 
stop：可选参数，默认为null，该参数接受一个或多个字符串，用于指定生成文本的停止信号。当
模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成。这可以用来控制模型的输出长
度或格式；
presence_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容
（例如新的概念或主题）的倾向性。较高的值会使模型更倾向于生成新内容，而较低的值则会使模
型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取
值；
frequency_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾
向性。较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果
篇幅较大并且存在前后语言重复时，可以提高该参数的取值；
best_of：该参数用于控制模型的生成过程。它会让模型进行多次尝试（例如，生成5个不同的响
应），然后选择这些响应中得分最高的一个；
logit_bias：该参数接受一个字典，用于调整特定token的概率。字典的键是token的ID，值是应用
于该t
Chunk 77: 的键是token的ID，值是应用
于该token的对数概率的偏置；在GPT中我们可以使用tokenizer tool查看文本Token的标记。一般
不建议修改；
user：可选参数，使用用户的身份标记，可以通过人为设置标记，来注明当前使用者身份。需要
注意的是，Completion.create函数中的user和后续介绍的对话类模型的user参数含义并不相同，
需要注意区分；
更详细的参数说明和使用细节可在《Ch.2 Completions模型API使用指南》中找到。此处不在展开
重复说明。
目前在LangChain中已经集成的LLMs有82个，均已经实现了对异步、流式处理和批处理的基本支
持，查看地址如下：https://python.langchain.com/docs/integrations/llms/
4.1.2 Chat Models接入方法
所谓聊天模型，简单理解就是我们经常使用的xxx-chat类模型，比如Baichuan2-7b-chat，
Qwen-14b-chat，这类模型是基于Base模型经过针对特定数据格式的微调得到，以更贴近人类对话的
方式进行输入和输出。所
Chunk 78: 更贴近人类对话的
方式进行输入和输出。所以这类模型不会使用“文本输入、文本输出”的形式，而是采用聊天消息作为输
入并返回聊天消息作为输出（非纯文本）。LangChain框架与OpenAI、Cohere、Hugging Face等多家
模型提供商已经实现集成，提供了一个标准化的接口，以便与这些不同的模型进行交互。
我们先回顾下OpenAI规范下使用Chat模型进行一次交互需遵循的代码执行方式：
from openai import OpenAI
client = OpenAI()
completion = client.chat.completions.create(
model=&quot;gpt-3.5-turbo-0125&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一个乐于助人的智能AI小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}
]
)
print(completion.choices[0].message.content)
您好，我是一个智能AI助手，可以帮助您回答问题、提供信息、解
Chunk 79: I助手，可以帮助您回答问题、提供信息、解决问题等。如果您有任何疑问或需要帮助的
话，都可以随时向我提问。我会尽力帮助您解决问题。
这里不得不再次提到，LangChain作为一个应用开发框架，需要集成各种不同的大模型，如上述
OpenAI的GPT系列模型调用示例，通过Message数据输入规范，定义不同的role，即system、user和
assistant来区分对话过程，但对于其他大模型，并不意味这一定会遵守这种输入输出及角色的定义，所
以LangChain的做法是，因为Chat Model基于消息而不是原始文本，LangChain目前就抽象出来的消息
类型有 AIMessage 、 HumanMessage 、 SystemMessage 、 FunctionMessage 和 ChatMessage ，
但大多时候我们只需要处理 HumanMessage 、 AIMessage 和 SystemMessage，即：
SystemMessage ：用于启动 AI 行为，作为输入消息序列中的第一个传入。
HumanMessage ：表示来自与聊天模型交互的人的消息。
AIMessag
Chunk 80: 天模型交互的人的消息。
AIMessage ：表示来自聊天模型的消息。这可以是文本，也可以是调用工具的请求。
同样，Chat Models实现了 Runnable 接口，也支持 invoke 、 ainvoke 、 stream 、 astream 、
batch 、 abatch 等调用。即：
方法 说明
invoke 处理单条输入
batch 处理批量输入
stream 流式响应
ainvoke 异步处理单条输入
abatch 异步处理批量输入
astream 异步流式响应
所以对于LangChain中调用OpenAI的GPT模型，首先需要导入对应的模块，代码如下：
from langchain_core.messages import HumanMessage, SystemMessage
消息形式输入调用
定义消息对象：
messages = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;你好，请你介绍一下你自己&quot;),]
messages
[SystemMessage(content=&#x27;你是一位乐
Chunk 81: ssage(content=&#x27;你是一位乐于助人的智能小助手&#x27;), HumanMessage(content=&#x27;你好，请
你介绍一下你自己&#x27;)]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
执行推理：
chat.invoke(messages)
AIMessage(content=&#x27;你好，我是一位可以回答问题、提供帮助的智能小助手。无论是关于知识、技能还是
日常生活中的问题，我都会尽力为您提供准确和有用的答案。有什么可以帮助您的吗？&#x27;)
如果只想获取到纯净的输出，可以通过.content来格式化。
reponse.content
&#x27;你好！我是一位智能助手，可以回答各种问题，提供信息和帮助。无论是关于知识、技能、娱乐、生活等方
面，我都会尽力为你提供帮助。有什么问题需要我帮忙的吗？&#x27;
流式调用
for chunk in chat.stream(messages):
print(chunk.content, end=&quot;&quot;, flush=True)
你好！我
Chunk 82: &quot;&quot;, flush=True)
你好！我是一位智能助手，专门为用户提供信息咨询、问题解答和帮助服务。无论你有什么疑问或需求，我都
会尽力帮助你，让你更方便地获取所需的信息和支持。有什么我可以帮助你的吗？
批量调用
先定义三个不同的消息对象：
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
messages2 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是深度学习&quot;),]
messages3 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是大模型技术&quot;),]
将上述三个消息对象放在一个列表中，使用.batch方法执行批量调用。
reponse = chat.batch([messages1,
messages2,
messages3,])
r
Chunk 83: ges2,
messages3,])
reponse
[AIMessage(content=&#x27;机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来
改善自身的性能，而无需明确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规
律、模式以及知识的技术。通过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预
测、分类、识别等任务。机器学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测
等领域。&#x27;),
AIMessage(content=&#x27;深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神
经网络层来学习和理解复杂的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识
别、语音识别、自然语言处理等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对
新数据进行准确预测和分类。深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金
融、交通等各个领域。&#x27;),
AIMessage(content=&#x27;大模型技术是指利用大规模、高性能的计算资源和先进的机器学
Chunk 84: 用大规模、高性能的计算资源和先进的机器学习算法来训练和部
署复杂的深度学习模型。这些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优
化。\n\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模
型技术，研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。\n\n然
而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员和
工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。&#x27;)]
格式化输出：
# 使用列表生成式打印每一个消息的内容
contents = [msg.content for msg in reponse]
# 打印出内容
for content in contents:
print(content, &quot;\n---\n&quot;)
机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来改善自身的性能，而无需明
确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规律、模式以及知识的技术。通
过利用统计学和
Chunk 85: 、模式以及知识的技术。通
过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预测、分类、识别等任务。机器
学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测等领域。
---
深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神经网络层来学习和理解复杂
的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识别、语音识别、自然语言处理
等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对新数据进行准确预测和分类。
深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金融、交通等各个领域。
---
大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部署复杂的深度学习模型。这
些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优化。
大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模型技术，
研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。
然而，大模型技术也面临着一些挑战，如计算资
Chunk 86: 而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员
和工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。
---
异步调用
messages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),
HumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
reponse = await chat.ainvoke(messages1)
reponse
AIMessage(content=&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并
改进性能，而无需显式地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断
优化自身的表现。机器学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常
见的机器学习方法包括监督学习、无监督
Chunk 87: 常
见的机器学习方法包括监督学习、无监督学习、强化学习等。&#x27;)
reponse.content
&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并改进性能，而无需显式
地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断优化自身的表现。机器
学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常见的机器学习方法包括
监督学习、无监督学习、强化学习等。&#x27;
流式调用
async for chunk in chat.astream(messages1):
print(chunk.content, end=&quot;&quot;, flush=True)
机器学习是一种人工智能的分支，它致力于让计算机系统通过学习数据和模式，从而改进自身的性能。简而言
之，机器学习让计算机具有从经验中学习的能力，而不需要明确地编程指令。
在机器学习中，计算机系统利用大量数据进行训练，通过识别数据中的模式和规律来建立模型。这些模型可以
用来进行预测、分类、识别等任务。机器学习在各个领域中都有广泛的应用，如自然语言处理、图像识别、医
疗诊断、金融预测等。
常见的
Chunk 88: 像识别、医
疗诊断、金融预测等。
常见的机器学习算法包括监督学习、无监督学习、强化学习等。监督学习是通过已标记的数据进行训练，无监
督学习则是在没有标记的数据中发现模式，强化学习则通过试错的方式学习最优策略。机器学习技术的发展使
得计算机系统能够更加智能地处理复杂任务，为人们的生活和工作带来了许多便利。
通过上述描述，我们展示了在LangChain中使用LLMs类模型和Chat Model类模型的不同方法，其
核心区别在于输入Prompt的格式。除此之外其他的工作可以直接利用统一的抽象接口，实现与模型交
互的快速过程。而针对不同的模型，LangChain也提供个对应的接入方法，其相关说明文档地址：http
s://python.langchain.com/docs/integrations/chat/
比如我们以Baichuan2的在线API模型为例快速接入一下：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat =
Chunk 89:  HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;我日薪8块钱，请问在闰年的二月，我月薪多少&quot;)])
response
AIMessage(content=&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所
以，你在闰年的二月月薪是232元。&#x27;)
response.content
&#x27;闰年的二月有29天，所以你的月薪是：\n\n$8\\times29$=$232$（元）\n\n所以，你在闰年的二月月
薪是232元。&#x27;
如未实践过Baichuan2的在线API，请查看《APICh.16 BaiChuan 2 大模型生态介绍及本地私有化
部署方案》中1. Baichuan在线大模型生态介绍小节
可以看出，LangChain能够迅速接入不同的模型API。至于接入本地私有部署的开源大模型的方
法，我们将在后续课程中进行详细
Chunk 90: 模型的方
法，我们将在后续课程中进行详细介绍。
4.2 LangChain中如何使用Prompt Template
提示工程（Prompt Engineering）大家应该比较熟悉，这个概念是指在与大语言模型（LLMs），
如GPT-3、Qwen等模型进行交互时，精心设计输入文本（即提示）的过程，以获得更精准、相关或有
创造性的输出。正如我们在大模型技术应用课程中介绍的，通过采用Few-Shot、Chain of Thought
(CoT)等高级提示技巧，可以显著提高大模型在推理任务上的表现。目前，提示工程已经发展成为一个
专业领域，非常多的公司设立了专门的职位，负责为特定任务编写精确且具有创造力的提示。
以使用ChatGPT等网页端对话交互应用中，大部分人常见的做法是将提示（Prompt）做硬编码，
例如将一段提示文本固定在System Messages中。而在应用开发领域，开发者往往无法预知用户的具体
输入内容，同时又希望大模型能够根据不同的应用任务以一种较为统一的逻辑来处理用户输入。所以，
LangChain通过提供Prompt Templates功能，优雅地解决了这个问题。Pr
Chunk 91: ates功能，优雅地解决了这个问题。Prompt Templates将用户输入
到完整格式化提示的转换逻辑进行封装，使得模型能够更灵活、高效地处理各种输入。
LangChain 提供了创建和使用提示模板的工具，其使用PromptTemplate 方法创建字符串提示模
板，因为目前为止，语言模型接收的提示基本都是字符串或聊天消息列表。
4.2.1 使用str.format语言构建模版
在LangChain的默认设置下， PromptTemplate 使用 Python 的 str.format() 方法进行模板
化。
Python的str.format()方法是一种字符串格式化的手段，允许我们在字符串中插入变量。使用这
种方法，可以创建包含占位符的字符串模板，占位符由花括号{}标识。调用format()方法时，可以传入
一个或多个参数，这些参数将被顺序替换进占位符中。str.format()提供了灵活的方式来构造字符串，支
持多种格式化选项，包括数字格式化、对齐、填充、宽度设置等。
基本用法
# 简单示例，直接替换
greeting = &quot;Hello, {}!&quot;.format(&quot;Alic
Chunk 92: o, {}!&quot;.format(&quot;Alice&quot;)
print(greeting)
# 输出: Hello, Alice!
Hello, Alice!
带有位置参数的用法
# 使用位置参数
info = &quot;Name: {0}, Age: {1}&quot;.format(&quot;Bob&quot;, 30)
print(info)
# 输出: Name: Bob, Age: 30
Name: Bob, Age: 30
带有关键字参数的用法
# 使用关键字参数
info = &quot;Name: {name}, Age: {age}&quot;.format(name=&quot;Charlie&quot;, age=25)
print(info)
# 输出: Name: Charlie, Age: 25
Name: Charlie, Age: 25
使用字典解包的方式：
# 使用字典解包
person = {&quot;name&quot;: &quot;David&quot;, &quot;age&quot;: 40}
info = &quot;Name: {name}, Age: {age}&quot;.format(**person)
print(info)
# 输出: Name: David, Age: 40
Name: 
Chunk 93: avid, Age: 40
Name: David, Age: 40
在LangChain中，基本采用了Python的原生str.format()方法对输入数据进行格式化，这样在模
型接收输入前，可以根据需要对数据进行预处理和结构化，以此来引导大模型进行更准确的推理。
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(
&quot;请给我一个关于{topic}的{type}解释。&quot;
)
prompt= prompt_template.format(type=&quot;详细&quot;, topic=&quot;量子力学&quot;)
prompt
&#x27;请给我一个关于量子力学的详细解释。&#x27;
如上所示，可以使用PromptTemplate的from_template方法创建一个提示模板实例，这个模板
包含了两个占位符：{topic} 和 {type}，这些占位符在实际调用时可以被实际的值替换。
调用LLMs模型
from langchain_openai import OpenAI
llm = OpenAI
Chunk 94:  OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
我觉得我已经对量子力学的基本理解了，但是我还是很想听听有关量子力学的详细解释，比如：量子力学是怎
么起作用的。
谢谢了.
量子力学是在克里姆林宫试验时的实验结果发现.一开始是用熟悉的物理学法则去解释它.比如说,量子力学是
量子力学的结果,而不是量子力学的原因.玻尔兹曼方程就是克里姆林宫试验结论的表示.后来人们发现,等效
性,柏格效应等现象.然后才发现,原来我们的光学和电磁学等理论是有问题的.这些现象的实现是在离子束中.
所以量子力学是很实际的.
[ 编辑 stuartm 在 06-07-08 12:11 ]
prompt= prompt_template.format(type=&quot;简短&quot;, topic=&quot;机器学习&quot;)
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, max_to
Chunk 95: davinci-002&quot;, max_tokens=1000)
result = llm.invoke(prompt)
print(result)
2)目前的时代，机器学习已经发展到了什么程度，也有什么应用场景。 3)目前的研究方向是什么。 4)什么
时候会用到这些技术。 5)最后一个问题是，对于我自己来说，是不是还有其他的方向比如说算法之类的，还
有在个人的技术知识里面是不是需要这些东西，不是特别需要。 也许这些都是我自己的猜测，如果你有什么
好的建议，我也非常感谢。 最后，我想问一下，你是在哪里学的，是在哪个大学，我就是在这里问问。 谢
谢。 2019.6.16
电影《伪装者》中的重要概念
原理：1. 无线电波的能量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无
线电波的方向不会发生改变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改
变，传播途中，波的方向不会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途
中，波的方向会发生改变（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变

Chunk 96: 生改变，传播途中，波的方向不会发生改变
（这就是遮挡）3. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）4. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）5. 无线电波的方向会发生改变，
传播途中，波的方向不会发生改变（这就是遮挡）6. 无线电波的方向会发生改变，传播途中，波的方向不会
发生改变（这就是遮挡）7. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）
8. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）原理：1. 无线电波的能
量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无线电波的方向不会发生改
变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改变，传播途中，波的方向不
会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途中，波的方向会发生改变
（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）3. 无线
电波的方向会发生改变，传播途中，波的方向不会发生改变（这
Chunk 97: 改变，传播途中，波的方向不会发生改变（这
除此之外，这种模板生成方式支持任意数量的变量，包括无变量，可以做如下测试：
from langchain.prompts import PromptTemplate
prompt_template = PromptTemplate.from_template(&quot;给我讲一个笑话&quot;)
prompt_template.format()
&#x27;给我讲一个笑话&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;davinci-002&quot;, )
result = llm.invoke(prompt)
print(result)
如果我是一个机器，会被称为什么？
谢谢
调用Chat Model
上述示例展示了如何使用字符串模板生成和调用LLMs类模型。对于聊天模型，也需要使用 Python
的 str.format() 方法进行模板化，但其构造的输入形式则有所区别，应采用聊天消息列表的形式，
每条消息都包含内容和一个名为role的附加参数。例如，在OpenAI的Chat Completion
Chunk 98: enAI的Chat Completions API中，聊天
消息需要包含为assistant、user或system。在这种情况下，应使用ChatPromptTemplate来构建对话
模板。
from langchain_core.prompts import ChatPromptTemplate
chat_template = ChatPromptTemplate.from_messages(
[
(&quot;system&quot;, &quot;你是一个有帮助的AI机器人，你的名字是{name}。&quot;),
(&quot;human&quot;, &quot;你好，最近怎么样？&quot;),
(&quot;ai&quot;, &quot;我很好，谢谢！&quot;),
(&quot;human&quot;, &quot;{user_input}&quot;),
]
)
messages = chat_template.format_messages(name=&quot;小明&quot;, user_input=&quot;你叫什么名字？&quot;)
messages
[SystemMessage(content=&#x27;你是一个有帮助的AI机器人，你的名字是小明。&#x27;),
HumanMessage(content=&#x27;你好，最近怎么样？&#x27;),
AIMessage(con
Chunk 99: 么样？&#x27;),
AIMessage(content=&#x27;我很好，谢谢！&#x27;),
HumanMessage(content=&#x27;你叫什么名字？&#x27;)]
print(chat_template)
input_variables=[&#x27;name&#x27;, &#x27;user_input&#x27;] messages=
[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;name&#x27;],
template=&#x27;你是一个有帮助的AI机器人，你的名字是{name}。&#x27;)),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[],
template=&#x27;你好，最近怎么样？&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;我很
好，谢谢！&#x27;)), HumanMessagePromptTemplate(prompt=PromptTemplate(input_varia
Chunk 100: Template(input_variables=
[&#x27;user_input&#x27;], template=&#x27;{user_input}&#x27;))]
从输出上看，其构造函数在实例化prompt_template时，主要由两个关键参数进行指定：
input_variables：这是一个列表，包含模板中需要动态填充的变量名。这些变量名在模板字符串
中以花括号（如{name}）标记。通过指定这些变量，可以在后续过程中动态地替换这些占位符。
template：这是定义具体提示文本的模板字符串。它可以包含静态文本和input_variables列表中
指定的变量占位符。当调用format方法时，这些占位符会被实际的变量值替换，生成最终的提示
文本。
调用Chat Model，这里我们选择GPT 3.5。代码如下：
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result
Chunk 101: ssages)
print(result.content)
我叫小明，很高兴为您提供帮助！有什么问题可以帮到您？
messages = chat_template.format_messages(name=&quot;大熊&quot;, user_input=&quot;你要去哪里玩？&quot;)
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)
result = llm.invoke(messages)
print(result.content)
作为一个AI机器人，我没有身体，所以无法去任何地方玩耍。但我可以帮助你规划旅行或提供有关旅游目的地
的信息。有什么我可以帮助你的吗？
通过上述流程，我们演示了如何在LangChain框架内使用ChatPromptTemplate来构建与聊天模型
交互的对话模板。通过此方法，能够创建一个包含不同参与者角色和定制化消息内容的对话流程，这对
于开发复杂的聊天应用或增强AI助手的交互能力至关重要。
4.2.2 构造Few-S
Chunk 102: 力至关重要。
4.2.2 构造Few-Shot模版
在LangChain中，很多的功能抽象、链路抽象本质上都是在对大模型的“涌现能力”能够应用落地的
一种具体实现方法，而其推理的不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度
依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。最为简单的
提示工程的方法就是通过输入一些类似问题和问题答案，让模型参考学习，并在同一个prompt的末尾
提出新的问题，依次提升模型的推理能力。
比如我们在《Ch.3 基于思维链的进阶提示工程》中测试的经典推理问题：
import os
import openai
openai.api_key = os.getenv(&quot;OPENAI_API_KEY&quot;)
prompt = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次？&#x27;
prompt
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from openai import OpenAI
cl
Chunk 103: nai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n在关闭之前，艾米能滑14次。&#x27;
from openai import OpenAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt,
max_tokens=1000
)
response1.choices[0].text
&#x27;\n\n15分钟 = 900秒\n\n每次滑下来需要1分钟，所以在15分钟内，可以滑下900秒/（1分钟+4分钟）=
900/5 = 180次。&#x27;
可以看到多次执行，其推理均是错误的。正确的计算过程应该是先计算艾米一次爬上爬下总共需要
5分钟，然后滑梯还有15分钟关闭，因此关闭之
Chunk 104: 钟，然后滑梯还有15分钟关闭，因此关闭之前能够再滑15/5=3次。
而我们当时在OpenAI的API规范下，是通过这种方式来进行Few-Shot的提示：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
Chunk 105: 关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
在Few-Shot的提示下，再次尝试进行提问：
from openai import OpenAI
client = OpenA
Chunk 106: penAI
client = OpenAI()
response1 = client.completions.create(
model=&quot;gpt-3.5-turbo-instruct&quot;,
prompt=prompt_Few_shot_CoT4,
max_tokens=1000,
)
response1.choices[0].text
&#x27;”艾米花了1分钟滑下来，滑一次上去需要4分钟，因此在15分钟关闭之前，她能滑15分钟-1分钟=14分钟，
14分钟除以4分钟每次，她能滑14÷4=3次，答案是3次。“ &#x27;
从输出结果上看，通过Few-Shot提示可以激发大模型的涌现能力从而回答对复杂推理问题，在应用
开发中同样需要这种能力来提升大模型的推理稳定性。所以LangChain抽象了
FewShotPromptTemplate方法来实现。我们可以先测试下在LangChain的接口下直接提问：
直接提问
prompt1 = &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在
关闭之前她能滑多少次？&#x27;
prompt1
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1
Chunk 107: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在关闭之前，艾米能滑(15-4-1)/4=2次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=prompt1
)
&#x27;\n\n在15分钟内，她能滑15次。&#x27;
其实也很容易想到，LangChain的工作仅仅是集成并接入OpenAI的GPT模型，因此其本质上与直接
调用GPT模型的效果相同。出现错误回答也属于正常情况。接下来，我们根据LangChain对Few-Shot模
板的定义规范，进行提示词的构建。
在LangChain中，需要使用 PromptTemplate 创建字符串提示
Chunk 108: omptTemplate 创建字符串提示模板。模板可以包括说明、少量示
例以及适合给定任务的特定上下文和问题。因此，我们要创建一个少量示例的列表。每个示例都是一个
字典，其中键是输入变量，值是这些输入变量的值。
适合LLMs模型的模版构造形式
from langchain.prompts.few_shot import FewShotPromptTemplate
from langchain.prompts.prompt import PromptTemplate
examples = [
{
&quot;question&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个
网球？&quot;,
&quot;answer&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现
在总共由5+6=11个网球。因此答案是11。&quot;
},
{
&quot;question&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在
食堂总共有多少个苹果？&quot;,
&quot;answer&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果
Chunk 109: 买了6个，总共有23-20+6=9个苹果，答
案是9。&quot;
},
{
&quot;question&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？&quot;,
&quot;answer&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4
个，答案是4个。&quot;
},
]
接下来需要实例化一个 PromptTemplate 对象，将少量示例格式化为字符串。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[0]))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
example_prompt = 
Chunk 110: 1。
example_prompt = PromptTemplate(
input_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:
{question}\n{answer}&quot;
)
print(example_prompt.format(**examples[1]))
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
最后，创建一个 FewShotPromptTemplate 对象。该对象接受经过上一步格式化后的少数样本示
例。
# 导入 FewShotPromptTemplate 类
from langchain.prompts.few_shot import FewShotPromptTemplate
# 创建一个 FewShotPromptTemplate 对象
few_shot_prompt = FewShotPromptTemplate(
examples=
Chunk 111: tTemplate(
examples=examples, # 最开始定义的 examples 示例
example_prompt=example_prompt, # 上一步定义的 example_prompt 作为提示模板
suffix=&quot;Question: {input}&quot;, # 后缀模板，其中 {input} 会被替换为实际输入
input_variables=[&quot;input&quot;] # 定义输入变量的列表
)
# 使用给定的输入格式化 prompt，并打印结果
print(few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，
水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;))
Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。
因此答案是11。
Question: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少
个苹果？
Chunk 112: 个苹果，请问现在食堂总共有多少
个苹果？
食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Question: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有
多少个蓝色高尔夫球？
总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
Question: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关
闭之前她能滑多少次？
final_input = few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟
才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)
final_input
&#x27;Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰
一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此
答案是11。\n\nQuestion: 食堂总共有23个苹果，如果他
Chunk 113: tion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现
在食堂总共有多少个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹
果，答案是9。\n\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是
蓝色的。请问总共有多少个蓝色高尔夫球？\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是
蓝色的，也就是4个，答案是4个。\n\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&#x27;
将带有Few-Shot提示的输入，输入到大模型中执行推理。
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米需要4分钟爬到顶部，1分钟滑下来，一共花费5分钟。在15分钟内，她能滑3次，因为15÷5=3。因此
答案是3次。
Chunk 114: 次，因为15÷5=3。因此
答案是3次。&#x27;
from langchain_openai import OpenAI
llm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)
llm.invoke(
input=final_input
)
&#x27;\n艾米花了5分钟（4分钟爬上去+1分钟滑下来），因此在15分钟内可以滑3次（15÷5=3）。答案是3次。&#x27;
从输出结果来看，显然受到了提示工程的正面影响，通过问题的拆解过程显著提高了回答的准确
性。
适合Chat Model的提示模版构造形式
我们在前面解释过LLMs和Chat Model 的区别，因其接受的输入形式不同，所以构造的提示模版也
是不一样的，对于Chat Model来说，它接收的是聊天形式的对话格式，所以对于其对应的Few-Shot，
在LangChain中的基本结构如下：
examples ：要包含在最终提示中的字典示例列表。
example_prompt ：通过其 format_messages 方法将每个示例转换为 1 条或多条消息。一个常
见的示例是将每个示例转换为一条人工消息和一条人
Chunk 115: 例是将每个示例转换为一条人工消息和一条人工智能消息响应，或者一条人工消息后跟一条
函数调用消息。
我们还是使用四个数学推理题来进行测试：
prompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共
有多少个网球？” \
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网
球，因此现在总共由5+6=11个网球。因此答案是11。” \
Q：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，
请问现在食堂总共有多少个苹果？” \
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9
个苹果，答案是9。” \
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫
球是蓝色的。请问总共有多少个蓝色高尔夫球？” \
A：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色
的，也就是4个，答案是4个。” \
Q：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在
15分钟后关闭，请问在关闭之前她能滑多少次？” \
A：&#x27;
pro
Chunk 116: 之前她能滑多少次？” \
A：&#x27;
prompt_Few_shot_CoT4
&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”
A：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在
总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果
他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”
A：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”
Q：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色
的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高
尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：
“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能
滑多少次？” A：&#x27;
直接调用
对于Chat Model，需要将输入转化成对话聊天格式：
from langchain_core.messages import Hu
Chunk 117: e.messages import HumanMessage, SystemMessage
messages = [SystemMessage(content=&quot;你是一个擅长数学推理的专家&quot;),
HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟
后关闭，请问在关闭之前她能滑多少次？&quot;),]
实例化模型进行推理：
from langchain_openai import ChatOpenAI
chat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)
resonse = chat.invoke(messages)
resonse.content
&#x27;首先，艾米花了1分钟滑下来，所以她每次滑下来后需要等待3分钟（4分钟上去 + 1分钟下来 = 5分钟，15
分钟总时间 - 5分钟 = 10分钟，10分钟 / 3分钟 = 3次）。\n\n在水滑梯关闭之前，艾米可以滑3次。&#x27;
&amp;esmp; 实际上，在使用GPT-3.5时，我们可以观察到，即使不采用Few-Shot提示，模型也能以很高
的
Chunk 118: Few-Shot提示，模型也能以很高
的概率正确回答问题，这归功于模型本身已经非常强大的能力。所以我们改为Baichuan2模型来测试：
from langchain_community.chat_models import ChatBaichuan
from langchain_core.messages import HumanMessage
chat = ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-xxx&quot;,
streaming=True,
)
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟滑下来，所以她在滑梯关闭前可以滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少
Chunk 119: 15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要1分钟从滑梯顶部滑下来，所以她可以在滑梯关闭前滑15次。&#x27;
response = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下
来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，所以每次滑行需要4+1=5分钟。\n\n水滑梯将在
15分钟后关闭，所以她能在关闭之前滑行15÷5=3次。&#x27;
可以看到，在切换到Baichuan模型进行测试时，三次测试中出现了两次错误回答。因此，我们就
使用Baichuan2模型，尝试通过添加Few-Shot来探索是否能增强其推理能力。
首先，安装LangChain的规范，导入生成示例的模块：
from langchain.prompts import (
ChatPromptTemplate,
FewShotChatMessagePromptTemplate,
)
接下来以input-outpu
Chunk 120: e,
)
接下来以input-output对的形式构建Few-Shot提示模板。在这里，“input”可以被视作人类发出的
Promot，而“output”则是大模型给出的回答。
examples = [
{&quot;input&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网
球？&quot;,
&quot;output&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总
共由5+6=11个网球。因此答案是11。&quot;},
{&quot;input&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共
有多少个苹果？&quot;,
&quot;output&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&quot;},
{&quot;input&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问
总共有多少个蓝色高尔夫球？&quot;,
&quot;output&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答
案是4个。&quot;},
]
然后，使用from_me
Chunk 121: 个。&quot;},
]
然后，使用from_messages方法进行模版构造。
# This is a prompt template used to format each individual example.
example_prompt = ChatPromptTemplate.from_messages(
[
(&quot;human&quot;, &quot;{input}&quot;),
(&quot;ai&quot;, &quot;{output}&quot;),
]
)
few_shot_prompt = FewShotChatMessagePromptTemplate(
example_prompt=example_prompt,
examples=examples,
)
print(few_shot_prompt.format())
Human: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？
AI: 罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网
球。因此答案是11。
Human: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有
Chunk 122: ，然后又买了6个苹果，请问现在食堂总共有多少个苹
果？
AI: 食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。
Human: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少
个蓝色高尔夫球？
AI: 总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。
生成最终的输入模版，设置 input 作为动态变量：
final_prompt = ChatPromptTemplate.from_messages(
[
few_shot_prompt,
(&quot;human&quot;, &quot;{input}&quot;),
]
)
final_prompt
ChatPromptTemplate(input_variables=[&#x27;input&#x27;], messages=
[FewShotChatMessagePromptTemplate(examples=[{&#x27;input&#x27;: &#x27;罗杰有五个网球，他又买了两盒网
球，每盒有3个网球，请问他现在总共有多少个网球？&#x27;, &#x27;output&#x27;: &#x27;罗杰一开始有五个网球
Chunk 123: output&#x27;: &#x27;罗杰一开始有五个网球，又购买了两
盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此答案是11。&#x27;}, {&#x27;input&#x27;:
&#x27;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？&#x27;,
&#x27;output&#x27;: &#x27;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是
9。&#x27;}, {&#x27;input&#x27;: &#x27;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。
请问总共有多少个蓝色高尔夫球？&#x27;, &#x27;output&#x27;: &#x27;总共有16个球，其中一半是高尔夫球，也就是8个，其中
一半是蓝色的，也就是4个，答案是4个。&#x27;}],
example_prompt=ChatPromptTemplate(input_variables=[&#x27;input&#x27;, &#x27;output&#x27;], messages=
[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{inp
Chunk 124: ut&#x27;],
template=&#x27;{input}&#x27;)),
AIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;output&#x27;],
template=&#x27;{output}&#x27;))])),
HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],
template=&#x27;{input}&#x27;))])
实例化baichuan2模型：
最后，使用LangChain中的chain的抽象，合并最终的提示和大模型共同执行。
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-e9&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾
Chunk 125: 
response.content
&#x27;艾米需要4分钟才能爬上去，1分钟滑下来，总共需要4+1=5分钟。15分钟可以滑15÷5=3次，所以答案是3
次。&#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米爬上去需要4分钟，滑下来只需要1分钟，所以她可以在15分钟内完成4次滑行，因为每次滑行需要
4+1=5分钟。&#x27;
chain = final_prompt | ChatBaichuan(
# 这里替换成个人的有效 API KEY
baichuan_api_key=&quot;sk-ed54695e9529&quot;,
streaming=True,
)
response = chain.invoke({&quot;input&quot;
Chunk 126: hain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水
滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})
response.content
&#x27;艾米需要4分钟才能爬到滑梯顶部，然后1分钟滑下来，总共需要4+1=5分钟。因此，在滑梯关闭之前，她可
以滑15÷5=3次。&#x27;
4.3 LangChain中的Output Parsers
Output Parsers，即输出解析器，这个概念非常好理解，就是负责获取大模型的输出并将其转换为
更合适的格式。这在应用开发中及其重要。在大多数复杂应用场景中，处理逻辑往往环环相扣，执行某
项业务逻辑可能需要多次调用大模型，其中上一次的调用结果将被用于指导下一次调用的逻辑。在这种
情况下，结构化的信息会比纯文本又有价值，同时这也是输出解析器的价值所在。
LangChain构造的输出解释器必须实现两个主要方法：
Get format instructions：该方法会返回一个字符串，其中包含有关如何格式化语言模型输出的指
令。
Parse：该方法会接收字符串，并将其解析为某种结构
目前已经支持的解析格式已
Chunk 127: 解析为某种结构
目前已经支持的解析格式已经包括Json、Xml、Csv以及OpenAI的Tools和Functions等多种格式，
具体可看：https://python.langchain.com/docs/modules/model_io/output_parsers/
简而言之，这个过程本质上是对文本的格式化处理。但与传统的后处理不同，LangChain采取的实
现策略是通过在Prompt上增加输出解析，直接引导大模型输出的格式化方式。
我们以格式化日期来看一下LangChain的构造思路，首先导入相关的模块：
from langchain.output_parsers import DatetimeOutputParser
from langchain.prompts import PromptTemplate
from langchain_openai import OpenAI
实例化日期解析器：
output_parser = DatetimeOutputParser()
通过.get_format_instructions方法，可以查看定义的解析格式：
output_p
Chunk 128: 以查看定义的解析格式：
output_parser.get_format_instructions()
&quot;Write a datetime string that matches the following pattern: &#x27;%Y-%m-
%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1181-01-05T02:20:33.675873Z, 1938-12-
13T09:46:36.791229Z, 1468-01-17T07:34:51.108688Z\n\nReturn ONLY this string, no
other words!&quot;
构造输入模版，这里的区别是：在输入的Prompt Template中，加入了OutPut Parse的内容：
template = &quot;&quot;&quot;用户发起的提问:
{question}
{format_instructions}&quot;&quot;&quot;
生成最终实际输入大模型的Promot内容：
prompt = PromptTemplate.from_template(
template,
# 预定义的变量，这里我们传入格式化指令
partial_va
Chunk 129: 我们传入格式化指令
partial_variables={&quot;format_instructions&quot;:
output_parser.get_format_instructions()},
)
prompt
PromptTemplate(input_variables=[&#x27;question&#x27;], partial_variables=
{&#x27;format_instructions&#x27;: &quot;Write a datetime string that matches the following
pattern: &#x27;%Y-%m-%dT%H:%M:%S.%fZ&#x27;.\n\nExamples: 1472-10-23T17:39:56.199188Z,
1643-01-14T10:55:11.903091Z, 0631-12-11T23:04:04.345333Z\n\nReturn ONLY this
string, no other words!&quot;}, template=&#x27;用户发起的提
问:\n\n{question}\n\n{format_instructions}&#x27;)
最后，使用LangChain中的
Chunk 130: }&#x27;)
最后，使用LangChain中的chain的抽象，合并最终的提示、大模型实例及OutPut Parse共同执
行。
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请问你叫什么？&quot;})
print(output)
2021-10-07 12:02:13.345333
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是机器学习&quot;})
print(output)
2018-01-23 09:42:32.123456
chain = prompt | OpenAI() | output_parser
output = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是深度学习&quot;})
print(output)
2019-08-07 18:30:00
通过测试可以明显观察到，以此方式定义的输出解析器极大地规范了
Chunk 131: 到，以此方式定义的输出解析器极大地规范了模型的输出。当然，这也直接
受到大模型实际能力和模板定义情况的影响。
LangChain中不同模版的使用方式，除了可以在官方文档上看到，在其源码上看会更加直观，而且
可以自定义修改，其所有的OutPut Parse的模版存储路径为：
langchain/libs/langchain/langchain/output_parsers ， 大家可以自行尝试。比如我们上面使
用的日期解析的源码对应如下：
到此为止，我们已经非常详尽地介绍了LangChain的核心概念。如果有学习过大模型技术实战课程
的同学来说，会发现LangChain的Model I/O模块中讨论的所有概念都是之前接触过的，需要学习的主
要是LangChain对大模型进行抽象的方法以及在LangChain框架下的应用开发技巧。
而从LangChain的整体开源情况看，其内容丰富且迭代非常快，通过模块化的方式介绍，可以帮助
大家快速构建起对LangChain以及应用开发的思维框架。尽管在Model I/O模块中还有许多更高级的内
容，例如如何接入本地部署的开源大模型，关于函数调用、Mem
Chunk 132: 地部署的开源大模型，关于函数调用、Memory缓存等更深入的主题，这些内容我
们将在后续章节中一一接触并进行详细的探讨。
```

```
result
```

```
[&#x27;开源大模型应用开发实战\nCh 18 LangChain整体项目介绍与核心模块Model I/O详\n解\n注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情\n况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，\n将迅速更新教材内容以确保信息的准确性和实用性。\n本期课件更新时间：2024年3月7日\n在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地\n私有化部署开源大模型。在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家\n关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部\n署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部\n署。\n但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也\n是最简单的一步。熟练掌握基于大模型的上层应用开发还有非常长的路需要走。目前，大模型技术开发\n总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API&#x27;,
 &#x27;基于OpenAI、GLM4这种在线API的应用开发，而另一类，就是先私有化\n部署开源大模型，再借助开发框架构建大模型应用。当然了，目前主流的大模型应用开发框架也支持在\n线模型API的调用，这正是我们之前一直强调学习掌握开源或在线API调用技巧的原因，因为这些方法和\n技巧直接对应于应用开发的最佳实践。\n而不论采用哪种开发形式，其基座和所依赖的，核心还是大模型本身。对于在线大模型，以\nOpenAI为例，开发者需要配置科学上网的环境，在其官网创建 API Key，最后遵循API的使用说明去进\n行应用配置。这一过程中涉及到的网络环境适配，数据隐私安全，成本管理及如何去定制化自己需求的\n开发流程，往往都需要遵循官方提供的功能来实现。也正是因为这些复杂的因素使得尽管目前公认最强\n的大语言模型GPT 4开放给开发者使用，还是会有非常多的公司和个人去拥抱开源，选择性能可能略逊\n一筹的开源大模型，因为开源方案在某些方面提供了更多的灵活性和自主控制能力。\n大家也能够发现，在我们的课程中每当涉及到大模型的私有化部署，我们都会向大家展示如何通过\n命令行、网页端访问和API调用等三种方式与部署的大模型进行交互。&#x27;,
 &#x27;I调用等三种方式与部署的大模型进行交互。此外，还通过text-generate-\nwebui这样的开源项目，这一切应用是帮助大家能够以最简单、最直接的方式与开源大模型进行对话，\n了解大模型的运行模型。但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶\n段肯定无法满足实际的需求。虽然目前的大模型基本都可以按照OpenAI的API规范形式进行调用，但如\n果进行实操我们会发现，在《大模型技术实战》的课程中，借助OpenAI的GPT系列模型开发的项目，\n如智能邮件处理、全自动AI开发流程，以及MateGen Agents等，尽管在开源模型上能够被复现，但这\n背后的成功关键并非仅仅在于模型调用。例如，在《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进\n行外部工具API调用开发实践》中，我们通过Qwen模型复现了《Ch.10 借助Function calling调用外部\n工具API方法》中的天气信息调用过程。这一复现过程揭示了，对在线模型来说，我们需要学习的是其\nAPI使用方法。而在开源大模型的技术领域下，要想实现某些特定功能，重点在于理解该模型的一些使\n用原理，以及&#x27;,
 &#x27;重点在于理解该模型的一些使\n用原理，以及如何调整和构建tools与Schema表示。因此，整个开发过程中花费的大部分时间并非直接\n与模型调用相关，而是在于深入理解和应用这些底层原理和技术细节。\n换言之，在构建大模型应用的过程中，虽然大模型是核心，但实际针对大模型编写的代码量相对较\n小。而在这些有限的代码中，提示工程又往往会占据主要的工作量。这进一步引出一个关键问题：除了\n大模型本身，其他各环节如何高效串联，以及这些环节的代码开发和工作量如何分配和执行，将在很大\n程度上影响最终应用的用户体验。\n在这种情况下，解决这一窘境且广受认可和高度实用的解决方案，便是LangChain。\n1. 什么是LangChain\nLangChain现在归属于LangChain AI公司，LangChain作为其中的一个核心项目，开源发布在\nGitub上：https://github.com/langchain-ai/langchain\n从LangChain的GitHub版本迭代历史上看，从2023年1月16日起已经经历了320个大小版本的迭\n代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度&#x27;,
 &#x27;加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高\n的。LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架。它的做法是：通过提\n供标准化且丰富的模块抽象，构建大语言模型的输入输入规范，利用其核心概念chains，灵活地连接\n整个应用开发流程。而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出\n来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发\n领域内被普遍高度认可的通用框架。\n为什么需要这样做？\n首先，我们需考虑当前大模型的发展态势。尽管OpenAI的GPT系列模型作为大模型领域的领军人\n物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大\n模型间的使用方式完全相同。例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不\n适用。因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是\n十分低下的。\n其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据\n连接方式以&#x27;,
 &#x27;外部API调用能力、私有数据\n连接方式以及输出结果的不稳定性等问题。在应用开发中，如何找到这些问题的有效解决策略？\n上述提到的每个限制都紧密关联于大模型本身的特性。尽管理论上可以通过重新训练、微调来增强\n模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资\n源、时间和财力，选择这条路径一定会导致方向越来越偏离目标。我们之前讨论的OpenAI的Function\nCalling和通过ReAct流程接入第三方API能够提供一些解决方案，但这每一步都需大量的研发投入，而且\n最终实现后的应用效果，也取决于研发人员的个人技术能力。在这种背景下，既然大家都有不同的想法\n和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的\n快速搭建，这就是LangChain一直想要做到，且正在做的事情。\nLangChain的做法\n从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对\n大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接\n口以适配各种大模型。到&#x27;,
 &#x27;象，设计出统一接\n口以适配各种大模型。到目前为止，LangChain抽象出最重要的核心模块如下：\n1. Model I/O ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；\n2. Retrieval ：检索外部数据，然后在执行生成步骤时将其传递到 LLM，包括文档加载、切割、\nEmbedding等；\n3. Chains ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多\n功能的高级抽象；\n4. Memory ： 记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；\n5. Agents ： 目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；\n6. Callbacks ：回调系统，允许连接到 LLM 应用程序的各个阶段。用于日志记录、监控、流传输和\n其他任务；\n从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文\n记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的&quot;Chain&quot;高级抽象，定义了\n不同形式下标准的链接方法，这就能够允许开发者&#x27;,
 &#x27;形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整\n的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。\n也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和\n使用方法，那么如何去搭这个“积木”，就需要我们对其每个核心模块都要有一个比较清楚的认知。所以\n我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实\n操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业\n务情况，选择合适的构造模块和构造方法。\n在整体上理解了LangChain之后，我们首先从Model I/O模块进行深入的探讨和实践。\n2. LangChain核心模块：Model I/O\nLangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的\nModel I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就\n是通过该模块，我们可以快速与某个大模型进行对话交互，整个&#x27;,
 &#x27;们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过\n程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API\n调用规范：\nresponse = openai.ChatCompletion.create(\nmodel=&quot;gpt-3.5-turbo&quot;,\nmessages=[\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是机器学习？&quot;}\n]\n)\n而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话\n交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象\n出一个chain，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核\n心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和\nOutput Parser（对应下图中的Parse部分）。\nFormat：即指代Prompts Template&#x27;,
 &#x27;：即指代Prompts Template，通过模板化来管理大模型的输入；\nPredict：即指代Models，使用通用接口调用不同的大语言模型；\nParse：即指代Output部分，用来从模型的推理中提取信息，并按照预先设定好的模版来规范化\n输出。\n整个Model I/O工作流如下图所示：\nFormat\n对于Prompt Template第一部分，传统上我们创建提示词是通过手工编写来实现的，在这个过程中\n会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能。然而，\n在应用开发中，一个关键的考量是提示词不能是一成不变的。其原因在于，应用开发需要适应多变的用\n户需求和场景。固定的提示词限制了模型的灵活性和适用范围。例如，如果我们正在开发一个天气查询\n应用，用户可能会以多种方式提出查询，如“今天的天气怎么样？”或“明天纽约的温度是多少度？”。如\n果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求。\n而Prompt Template，就像我们在介绍《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进行外\n部工具API调&#x27;,
 &#x27;理介绍及流程拆解，进行外\n部工具API调用开发实践》中不断优化出来的ReAct模版，将API的使用、问题解答过程等复杂逻辑封装\n成了一套结构化的格式。我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引\n导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：\n# 将一个插件的关键信息拼接成一段文本的模版。\nTOOL_DESC = &quot;&quot;&quot;{name_for_model}: Call this tool to interact with the\n{name_for_human} API. What is the {name_for_human} API useful for?\n{description_for_model} Parameters: {parameters}&quot;&quot;&quot;\n# ReAct prompting 的 instruction 模版，将包含插件的详细信息。\nPROMPT_REACT = &quot;&quot;&quot;Answer the following questions as best you can. You have\naccess to the followin&#x27;,
 &#x27;cess to the following APIs:\n{tool_descs}\nUse the following format:\nQuestion: the input question you must answer\nThought: you should always think about what to do\nAction: the action to take, should be one of [{tool_names}]\nAction Input: the input to the action\nObservation: the result of the action\n... (this Thought/Action/Action Input/Observation can be repeated zero or more\ntimes)\nThought: I now know the final answer\nFinal Answer: the final answer to the original input question\nBegin!\nQuestion: &#x27;,
 &#x27;on\nBegin!\nQuestion: {query}&quot;&quot;&quot;\n因此，引入Prompt Template可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输\n入动态调整提示词，从而更好地响应用户的具体需求。LangChain通过这种方式来提高应用的通用性和\n用户体验。\nPredict\n在Predict部分，实质上是处理模型从接收输入到执行推理的整个过程。考虑到存在两种主要类型的\n大模型——Base类模型和Chat类模型，LangChain在其Model I/O模块中对这两种模型都进行了抽象，\n分别归类为LLMs（Large Language Models）和Chat Models。我们还是以OpenAI 的 Completion 和\nChatcompletions方法为例：\n# Base类模型\nclient.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=&quot;Say this is a test&quot;,\n)\n# 聊天模型\nclient.chat.completions.create(\nmodel=&quot;gpt-3.5-t&#x27;,
 &#x27;te(\nmodel=&quot;gpt-3.5-turbo&quot;,\nmessages=[\n{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}\n]\n)\nLLMs是简化的大语言模型抽象，即基于给定的Prompt提供内容生成的功能。而Chat Models则专\n注于聊天API的抽象，需要维护上下文的记忆（聊天记录），呈现出更接近对话或聊天形式的交互。\nParse\n我们知道，大模型的输出是不稳定的，同样的输入Prompt往往会得到不同形式的输出。在自然语\n言交互中，不同的语言表达方式通常不会造成理解上的障碍。但在应用开发中，大模型的输出可能是下\n一步逻辑处理的关键输入。因此，在这种情况下，规范化输出是必须要做的任务，以确保应用能够顺利\n进行后续的逻辑处理。\n输出解析器 Output Parser就是一个帮助结构化语言模型响应的抽象，可以获取格式指令或者进行\n更深层次的解析。这我们会在后面的实践中直观的体验到。\n整体而言，在Model I/O模块的抽象中，其一能够让开发者&#x27;,
 &#x27;l I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如\nOpenAI、ChatGLM、Qwen等，按照既定规范执行模型推理。其二通过输入和输出的模板化处理，使\n其更贴合于应用开发的最佳实践。接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行\n集成和操作的。\n3 安装LangChain\nLangChain的安装过程非常简单，可以通过常用的Python包管理工具，如pip或conda，直接进行\n安装。稍复杂一点的还可以通过源码进行安装。但有一点大家一定要明确：LangChain的真正价值在于\n它能够与多种模型提供商、数据存储解决方案等进行集成。默认情况下，使用上述两种安装方式中的任\n意一种来进行LangChain安装后，安装的仅仅是LangChain的默认功能，并不包括这些集成所需的额外\n依赖项。也就是说，如果我们想要使用特定的集成功能，还需要额外安装这些特定的依赖。以调用\nOpenAI的API为例，我们首先需要通过运行命令pip install langchain-openai安装OpenAI的合作\n伙伴包，安装此依赖包后，LangChain才能够与Ope&#x27;,
 &#x27;赖包后，LangChain才能够与OpenAI的API进行交互。后续我们在使用相关功能的时\n候，会提供额外的说明。\nLangChain安装官方说明文档：https://python.langchain.com/docs/get_started/installation\n3.1 使用包版本管理工具安装\nLangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建\n过程。这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成\nLangChain的安装。具体操作如下：\n! pip install langchain\nRequirement already satisfied: langchain in d:\\software\\anaconda3\\lib\\site-\npackages (0.1.10)\nRequirement already satisfied: PyYAML&gt;=5.3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (6.0)\nRequir&#x27;,
 &#x27;gchain) (6.0)\nRequirement already satisfied: SQLAlchemy&lt;3,&gt;=1.4 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (1.4.39)\nRequirement already satisfied: aiohttp&lt;4.0.0,&gt;=3.8.3 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (3.8.5)\nRequirement already satisfied: dataclasses-json&lt;0.7,&gt;=0.5.7 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.6.4)\nRequirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (1.33)&#x27;,
 &#x27;om langchain) (1.33)\nRequirement already satisfied: langchain-community&lt;0.1,&gt;=0.0.25 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.0.25)\nRequirement already satisfied: langchain-core&lt;0.2,&gt;=0.1.28 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.1.28)\nRequirement already satisfied: langchain-text-splitters&lt;0.1,&gt;=0.0.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.0.1)\nRequirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in\nd:\\software\\anaconda3\\li&#x27;,
 &#x27;oftware\\anaconda3\\lib\\site-packages (from langchain) (0.1.14)\nRequirement already satisfied: numpy&lt;2,&gt;=1 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (1.24.3)\nRequirement already satisfied: pydantic&lt;3,&gt;=1 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (2.6.0)\nRequirement already satisfied: requests&lt;3,&gt;=2 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (2.31.0)\nRequirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in\nd:\\software\\anaconda3\\lib\\site-packag&#x27;,
 &#x27;nda3\\lib\\site-packages (from langchain) (8.2.2)\nRequirement already satisfied: attrs&gt;=17.3.0 in d:\\software\\anaconda3\\lib\\site-\npackages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (22.1.0)\nRequirement already satisfied: charset-normalizer&lt;4.0,&gt;=2.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(2.0.4)\nRequirement already satisfied: multidict&lt;7.0,&gt;=4.5 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(6.0.2)\nRequirement already s&#x27;,
 &#x27;equirement already satisfied: async-timeout&lt;5.0,&gt;=4.0.0a3 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(4.0.2)\nRequirement already satisfied: yarl&lt;2.0,&gt;=1.0 in d:\\software\\anaconda3\\lib\\site-\npackages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (1.8.1)\nRequirement already satisfied: frozenlist&gt;=1.1.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(1.3.3)\nRequirement already satisfied: aiosignal&gt;=1.1.2 in\nd:\\software\\anaconda3\\&#x27;,
 &#x27;\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(1.2.0)\nRequirement already satisfied: marshmallow&lt;4.0.0,&gt;=3.18.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-\n&gt;langchain) (3.21.0)\nRequirement already satisfied: typing-inspect&lt;1,&gt;=0.4.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-\n&gt;langchain) (0.9.0)\nRequirement already satisfied: jsonpointer&gt;=1.9 in\nd:\\software\\anaconda3\\lib\\site-packages (from json&#x27;,
 &#x27;-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain)\n(2.1)\nRequirement already satisfied: anyio&lt;5,&gt;=3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (3.5.0)\nRequirement already satisfied: packaging&lt;24.0,&gt;=23.2 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2,&gt;=0.1.28-\n&gt;langchain) (23.2)\nRequirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in\nd:\\software\\anaconda3\\lib\\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-\n&gt;langchain) (3.9.15)\nRequir&#x27;,
 &#x27;ain) (3.9.15)\nRequirement already satisfied: annotated-types&gt;=0.4.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (0.6.0)\nRequirement already satisfied: pydantic-core==2.16.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain)\n(2.16.1)\nRequirement already satisfied: typing-extensions&gt;=4.6.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (4.7.1)\nRequirement already satisfied: idna&lt;4,&gt;=2.5 in d:\\software\\anaconda3\\l&#x27;,
 &#x27;software\\anaconda3\\lib\\site-\npackages (from requests&lt;3,&gt;=2-&gt;langchain) (3.4)\nRequirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)\n(1.26.16)\nRequirement already satisfied: certifi&gt;=2017.4.17 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)\n(2023.11.17)\nRequirement already satisfied: greenlet!=0.4.17 in\nd:\\software\\anaconda3\\lib\\site-packages (from SQLAlchemy&lt;3,&gt;=1.4-&gt;langchain)\n(2.0.1)\nRequirement &#x27;,
 &#x27;(2.0.1)\nRequirement already satisfied: sniffio&gt;=1.1 in d:\\software\\anaconda3\\lib\\site-\npackages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (1.2.0)\nRequirement already satisfied: mypy-extensions&gt;=0.3.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from typing-inspect&lt;1,&gt;=0.4.0-\n&gt;dataclasses-json&lt;0.7,&gt;=0.5.7-&gt;langchain) (1.0.0)\n注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。\n重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：\nimport langchain\nprint(langchain.__version__)\n0.1.10\n如果能正常输出LangChain&#x27;,
 &#x27;.10\n如果能正常输出LangChain的版本，说明在当前环境下的安装成功。\n3.2 源码安装\n除了通过pip安装外，还有一种通过源码安装的方法。这需要使用git拉取远程仓库，然后进入项目\n文件夹并执行pip install -e .命令。这种方法不仅会安装必要的依赖，同时也将程序的源代码保存\n在本地。通常来说，除非大家打算作为LangChain的协作开发者参与到项目的功能更新中，否则没必要\n采用这种安装方式，直接使用pip安装更为简便。\n对于课程学习而言，我们推荐采用源码安装方式，这将非常有助于在后续的LangChain功能探索\n中，通过源码分析深入理解框架的构建原理和详细机制。\n接下来我们以Ubuntu操作系统为例，介绍源码安装LangChain的详细步骤：\nStep 1. 安装Anaconda\n如果本地服务器是Ubuntu系统，请参考：《Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B\n模型》中的2.3 安装Anaconda环境小节；而如果是Windows操作系统，请参考：《Ch.6 在Windows\n系统下部署运行ChatGLM3-6B模型》中的3.2.&#x27;,
 &#x27;ChatGLM3-6B模型》中的3.2.1 方式一：使用Anaconda创建项目依赖环境（推荐）小节\n中，执行完Step 6. 验证conda步骤，回到本文继续执行后续的操作：\n注：如果部署环境是租赁的云服务器，Anaconda环境已经预设好，并不需要我们再次手动配置。\n按照对应的教程内容配置好Anaconda后，可以通过如下命令验证当前环境的Conda：\nconda --version\n如果可以正常输出Conda的版本，则说明Anaconda安装成功。\nStep 2. 使用Conda创建LangChain的Python虚拟环境\n安装好Anaconda后，我们需要借助Conda包版本工具，为LangChain项目创建一个新的Python虚\n拟运行环境，执行代码如下：\nconda create --name langchain python==3.11\n如上所示，新创建了一个名为langchain的Python虚拟环境，其Python版本为3.11。创建完成后，\n通过如下命令进入该虚拟环境，执行后续的操作：\nconda activate langchain\nStep 3. 下载LangC&#x27;,
 &#x27;hain\nStep 3. 下载LangChain的项目文件\n进入LangChain的官方Github，地址：https://github.com/langchain-ai/langchain ， 在 GitHub\n上将项目文件下载到有两种方式：克隆 (Clone) 和 下载 ZIP 压缩包。推荐使用克隆 (Clone)的方式。我\n们首先在GitHub上找到其仓库的URL。\n在执行命令之前，需要先安装git软件包，执行命令如下：\nsudo apt install git\n执行克隆命令，将LangChain Github上的项目文件下载至本地的当前路径下，如下：\ngit clone https://github.com/langchain-ai/langchain.git\nStep 4. 升级pip版本\n建议在执行项目的依赖安装之前升级 pip 的版本，如果使用的是旧版本的 pip，可能无法安装一些\n最新的包，或者可能无法正确解析依赖关系。升级 pip 很简单，只需要运行命令如下命令：\npython -m pip install --upgrade pip\nStep 5. 源码安装项目依赖&#x27;,
 &#x27;pip\nStep 5. 源码安装项目依赖\n不同于我们之前一直使用的pip install -r requirements.txt方式，这种方法用于批量安装多\n个依赖包，是在部署项目或确保开发环境与其他开发者/环境一致时的常用方式。而对于LangChain，我\n们需要使用pip install -e的方式，以可编辑模式安装包。这种方式主要用于开发过程中。当以可编\n辑模式安装一个包时，依赖包会被直接从源代码所在位置安装，而不是复制到Python的site-packages\n目录，是开发模式下用于安装并实时反映对本地包更改的方法。需要执行的步骤如下：\ncd langchain/libs/langchain/\npip install -e .\n如在安装过程未发生任何报错，则说明安装成功。在安装完依赖后，我们就正式进入LangChain的\nModel I/O模块的实践。\n注：如果采用的是源码安装，需要参考《Ch.11 大模型开发工具Transformers库的使用及配置大\n模型开发环境》中远程服务器部署大模型，本地Jupyter NoteBook加载小节，在LangChain的虚拟\n环境下启动J&#x27;,
 &#x27;在LangChain的虚拟\n环境下启动Jupyter Lab，才能正常执行接下来的实践。\n4.Models I/O模块实战\n任何语言模型应用的核心都是大语言模型（LLMs），无论涉及到提示模板（Prompt Template）\n还是输出解析（OutPut Parse），其核心还是在于大语言模型（LLMs）。因此，在讨论和实践Model\nI/O模块时，首先应当关注如何集成这些大模型。只有成功集成后才能深入探讨和实践如何有效地设计\n提示模板和解析模型输出。所以我们先要实践的是：如何借助LangChain框架使用不同的大模型。\n4.1 LangChain接入大模型的方法\nLangChain 提供了一套与任何大语言模型进行交互的标准构建模块。所以需要明确的一点是：虽然\nLLMs 是 LangChain 的核心元素，但 LangChain 本身不提供 LLMs，它仅仅是为多种不同的 LLMs 进\n行交互提供了一个统一的接口。简单理解：以OpenAI的GPT系列模型为例，如果我们想通过\nLangChain 接入 OpenAI 的 GPT 模型，我们需要在LangChain框架下先定义相关的类和方法&#x27;,
 &#x27;ngChain框架下先定义相关的类和方法来规定如\n何与模型进行交互，包括数据的输入和输出格式以及如何连接到模型本身。然后按照 OpenAI GPT 模型\n的接口规范来集成这些功能。通过这种方式，LangChain 充当一个桥梁，使我们能够按照统一的标准来\n接入和使用多种不同的大语言模型。\n在LangChain的Model I/O模块中，集成了两类模型，分别是LLMs和聊天模型，这两种模型在输\n入和输出的数据类型上有所区别。\n接下来我们就以OpenAI的API为示例，分别实践一下在LangChain框架中如何使用这两种不同类型\n的大语言模型。首先来看LLMs类模型。\n4.1.1 LLMs类模型接入方法\nLLMs指的是纯文本补全模型，其封装的 API 输入接收的是字符串，输出也是字符串。 OpenAI 的\nGPT-3 是作为 LLM 实现的。最简单的理解：LLMs指代的就是Base类模型，这类模型仅仅可以理解并\n生成自然语言或代码，并未按照特定指令进行训练。截止到2024年3月07日，OpenAI可支持调用的\nBase类模型仅有2个，分别是babbage-002和davinci-002，除&#x27;,
 &#x27;ge-002和davinci-002，除此之外，还可以使用gpt-3.5-turbo-\ninstruct调用OpenAI 的 Completion方法。\n首先需要安装OpenAI的集成依赖包langchain-openai，执行如下命令：\n! pip install langchain-openai\nRequirement already satisfied: langchain-openai in\nd:\\software\\anaconda3\\lib\\site-packages (0.0.8)\nRequirement already satisfied: langchain-core&lt;0.2.0,&gt;=0.1.27 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-openai) (0.1.28)\nRequirement already satisfied: openai&lt;2.0.0,&gt;=1.10.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-o&#x27;,
 &#x27;es (from langchain-openai) (1.13.3)\nRequirement already satisfied: tiktoken&lt;1,&gt;=0.5.2 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-openai) (0.6.0)\nRequirement already satisfied: PyYAML&gt;=5.3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (6.0)\nRequirement already satisfied: anyio&lt;5,&gt;=3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.5.0)\nRequirement already satisfied: jsonpatch&lt;&#x27;,
 &#x27;atisfied: jsonpatch&lt;2.0,&gt;=1.33 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (1.33)\nRequirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (0.1.14)\nRequirement already satisfied: packaging&lt;24.0,&gt;=23.2 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (23.2)\nRequirement already satisfied: pydantic&lt;3&#x27;,
 &#x27;atisfied: pydantic&lt;3,&gt;=1 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.6.0)\nRequirement already satisfied: requests&lt;3,&gt;=2 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.31.0)\nRequirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (8.2.2)\nRequirement already satisfied: distro&lt;2,&gt;=1.7.0 in\nd:&#x27;,
 &#x27;stro&lt;2,&gt;=1.7.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-\nopenai) (1.8.0)\nRequirement already satisfied: httpx&lt;1,&gt;=0.23.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-\nopenai) (0.25.1)\nRequirement already satisfied: sniffio in d:\\software\\anaconda3\\lib\\site-\npackages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (1.2.0)\nRequirement already satisfied: tqdm&gt;4 in d:\\software\\anaconda3\\lib\\site-packages\n(from openai&lt;2.0.0,&#x27;,
 &#x27;\n(from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (4.65.0)\nRequirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in\nd:\\software\\anaconda3\\lib\\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-\nopenai) (4.7.1)\nRequirement already satisfied: regex&gt;=2022.1.18 in\nd:\\software\\anaconda3\\lib\\site-packages (from tiktoken&lt;1,&gt;=0.5.2-&gt;langchain-\nopenai) (2022.7.9)\nRequirement already satisfied: idna&gt;=2.8 in d:\\software\\anaconda3\\lib\\site-\npackages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langc&#x27;,
 &#x27;.2.0,&gt;=0.1.27-&gt;langchain-openai)\n(3.4)\nRequirement already satisfied: certifi in d:\\software\\anaconda3\\lib\\site-\npackages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)\n(2023.11.17)\nRequirement already satisfied: httpcore in d:\\software\\anaconda3\\lib\\site-\npackages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)\n(1.0.1)\nRequirement already satisfied: jsonpointer&gt;=1.9 in\nd:\\software\\anaconda3\\lib\\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain-\ncore&lt;0.2.0,&gt;=&#x27;,
 &#x27;chain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.1)\nRequirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in\nd:\\software\\anaconda3\\lib\\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-\n&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.9.15)\nRequirement already satisfied: annotated-types&gt;=0.4.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (0.6.0)\nRequirement already satisfied: pydantic-core==2.16.1 in\nd:\\software\\anaconda3\\lib\\sit&#x27;,
 &#x27;re\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.16.1)\nRequirement already satisfied: charset-normalizer&lt;4,&gt;=2 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.0.4)\nRequirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (1.26.16)\nRequirement already satisfied: col&#x27;,
 &#x27;ready satisfied: colorama in d:\\software\\anaconda3\\lib\\site-\npackages (from tqdm&gt;4-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.4.6)\nRequirement already satisfied: h11&lt;0.15,&gt;=0.13 in\nd:\\software\\anaconda3\\lib\\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-\n&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.14.0)\n使用OpenAI的 GPT 系列模型的API有两个前提条件：首先，需要配置科学上网以确保可以访问\nOpenAI服务；其次，必须拥有一个有效的API Key。这两项要求的详细配置在《Ch.1 本地调用OpenAI\nAPI流程及OpenAI官网使用指南》中有详细说明，因此我们在此不再赘述，直接进入实操过程。\n查看OpenAI的依赖包版本\nOpenAI 的 API 在 0.28 版本和 &#x27;,
 &#x27;AI 的 API 在 0.28 版本和 1.xx 版本之间的调用方式存在差异。我们这里使用的是最新的调用\n方法，即采用 1.13.XX 版本。因此，需要确认当前 Python 虚拟环境中安装的 openai 包版本是 1.xx。\n可以通过执行以下命令来检查当前安装的版本：\nimport openai\nprint(openai.__version__)\n0.28.0\n如果如上显示是0.28.0版本，执行如下命令将openai包升级到最新版本：\n# ! pip install --upgrade openai\n注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。\nimport openai\nprint(openai.__version__)\n1.13.3\n设置科学上网环境\n根据所使用的科学上网软件，查找相应的代理端口，并按照以下代码进行网络环境配置：\n# 这里的端口，请修改为实际使用科学上网软件配置的代理端口\n!set HTTP_PROXY=http://127.0.0.1:15732\n!set HTTPS_PROXY=http://127.0.0.1:&#x27;,
 &#x27;XY=http://127.0.0.1:15732\n新版本的OpenAI API调用方法不允许使用openai.api_key= &quot;sk-xxx&quot;这种老版本的显式指定API\nKey，所以如果没有在系统变量中进行配置的话，使用下面的命令设置环境变量：\n# 将your- API -key-here替换为实际的API密钥:\n# ! setx OPENAI_API_KEY &quot;sk-xxxx&quot;\n成功: 指定的值已得到保存。\n注意：完成环境变量的初次配置后，需重启电脑以激活设置。这样，在下次使用时就可以直接调用\n配置好的环境。为验证环境变量中的API Key是否已正确设置为自己的，可以执行以下命令进行检查。\n# ! echo %OPENAI_API_KEY%\n完成了科学上网环境和API Key的设置后，在接入LangChain之前，我们先使用如下测试代码检查\n当前环境及API的运行状态是否正常，执行代码如下：\nfrom openai import OpenAI\nclient = OpenAI()\ncompletion = client.chat.completions.create(\nmodel=&#x27;,
 &#x27;tions.create(\nmodel=&quot;gpt-3.5-turbo&quot;,\nmessages=[\n{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}\n]\n)\nprint(completion.choices[0].message.content)\n你好，我是一位可以回答问题、提供信息和与您交流的AI助手。我可以帮助解决问题，回答疑惑，提供建议，\n而且乐意与您分享知识和信息。有什么我可以帮助你的吗？\n若测试代码能正常输出结果，表明OpenAI的环境配置无误。需要注意的是，LangChain接入\nOpenAI GPT模型只是利用其定义的标准模型接入框架来整合OpenAI API，因此进行上述连通性测试\n是必要的。\nOpenAI GPT Base模型官方说明：https://platform.openai.com/docs/models/moderation\nLangChain为了使开发者可以轻松地创建自定义链，整体采用Runnable协议。Runnabl&#x27;,
 &#x27;采用Runnable协议。Runnable 协议是\n编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可\n以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了\nRunnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。\nLangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch\nain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable\n而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解\n的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。\n如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI&#x27;,
 &#x27;间去看看源码的抽象逻辑。比如OpenAI：\nLangChain Interface说明：https://python.langchain.com/docs/modules/model_io/llms/quic\nk_start\nLangChain已集成LLMs说明：https://python.langchain.com/docs/integrations/llms/\n首先，我们来了解常规的调用方法。具体支持的调用方式如下所示：\n方法 说明\ninvoke 处理单条输入\nbatch 处理批量输入\nstream 流式响应\nainvoke 异步处理单条输入\nabatch 异步处理批量输入\nastream 异步流式响应\n接下来我们对上述方法依次进行尝试。在LangChain框架中，为了规范地接入LLMs类型的大模\n型，首先需要导入OpenAI的依赖包。导入命令如下：\nfrom langchain_openai import OpenAI\n字符串形式的输入\n调用invoke方法，并提供字符串格式的Prompt，便可触发指定模型按照所给Prompt进行续写操\n作。\nfrom langchain_op&#x27;,
 &#x27;作。\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;)\nllm.invoke(\n&quot;你好呀，请你介绍一下你自己&quot;\n)\n\&#x27;吧。”韩寒摇头，说：“我下次再来，你们自己给我介绍好了。”这一次，韩寒的风趣让陈航漂了一下，他说：\n“还是先看看大家的房间吧。”陈航带着韩寒到了楼下，这时连小李都已经来到了楼下，他对韩寒说：“你看，\n这是我们的宿舍。”陈航的语气是那么的平淡，既没有过多的炫耀，也没有明显的拒绝，只是那么的平淡，这\n让韩寒很有点奇怪，他说：“你们是在这里住吗？”小李说：“是啊，我们是这里的住宿生。”韩寒对小李说：\n“那我是来参加考试的，你\&#x27;\n从上述模型的返回输出，我们可以看到它本质上作为一个补全模型，通过使用大模型原生的\n.generate() 方法来对输入的Prompt进行续写。这部分的详细内容已在《Ch.2 Completions模型API\n使用指南》中进行了深入讲解，因此此处不再进行赘述。\n上述调用方法相当于使用我们熟悉的OpenAI的Completion方法来调用GPT Base类型&#x27;,
 &#x27;etion方法来调用GPT Base类型模型的过\n程：\nfrom openai import OpenAI\nclient = OpenAI()\ncompletion = client.completions.create(\nmodel=&quot;davinci-002&quot;,\nprompt=&quot;你好呀，请你介绍一下你自己&quot;,\nmax_tokens=1024,\n)\nprint(completion.choices[0].text)\n。» 她把咳嗽调整成百米冲刺那种负重颤抖。»不，我们在上面，请你告诉会场。哪一地方过去你都要做朋友\n呢？」Larsen一转眼注意到卡萨姆旁站着一位古老的男人。他身着粗布制服，最左边有一种圆盏形的供奶\n头。透映着那个老的质朴魅力和吓坏人恶心的样子。»LarsenKeberichTaylor.Ch02 3/5/07 5:27 PM\nPage 39LEARNING WHERE WECAME FROM»喂主持人。自从我给你们滴剂的时候，我就耐心等待了。请为\n快点谈到刚才提的内容。»正赞成的木质松枝变苍白了。既然他是最耐烦的老爷，我们劝劝他这样三小时也\n烦。时光久了会累人的人，他更应&#x27;,
 &#x27;三小时也\n烦。时光久了会累人的人，他更应该知道这些。»好，每个人都听到。»那些选手的名字有老项目拯救人」，\nBrickman说。AnderMartin、Taylor, här nere – efter en minut eller så. Likdant för\nLarsenKeberich, rätt så bra ger du tillbaka vätsken, yeah, and the eighth\nman.»»Eighth man?」Brickman坏坏垂下眼帘扬扬眉头，头盔上的唇影随着他挺起了脸颊。好，一位是\n我。不，我不是。Just a moment.»六记地雷爆炸。嫣红的色泽弥漫在鼻子周围极其耀眼之下，淀粉饼糊塞\n了全脸，剔饭一般的巧克力混合在一起。»»Would you all please proceed into the next\nbuilding and make sure that you use the turnstiles.»伤口破口流血，簇瓣的血色俗套，但去\n还是要用。套房好美啊、清生。套房饲養下只要一个；_ 户功能装置就。»艰难地活着足以获得一天的欢乐&#x27;,
 &#x27;能装置就。»艰难地活着足以获得一天的欢乐的\n人确实算够奇特的。咳嗽的木质松枝_cancel6 3/5/07 5:27 PM Page 40IT IS NOT ENOUGH\nTOMAKE MANKINDHAPPY,LARSEN KEBERICH向报纸走来，她在走廊哪一个厅卡萨姆等待着;在这个保证已\n经到达那种别有心碎的正式屋子里。她穿过镶有玫瑰花的拉链的抢洗衣区和单人舔饭的厕所区圆朝圣地的道。\n先是葡萄干牛油，透过臭风从供应箱中飘来，刚开始时她便一直寻找那一种苏查尔的酸味。那种味美常是发出\n着从薄饼里流发出来的所以一圈难找到对不起，小姐，我们之前选择做了决定，就算更改好一个时刻，也无法\n改变决定。拨动键盘-找麻烦你的，怎么做了、哦，听起来好像你一突然卸了山猫带来了 Asktohell你疼\n吗。杨虎大吼着，冲向对方。丁\n流式输出\n要进行流式调用，可以调用stream方法。这种方式允许在接收到模型响应的同时，逐块输出结\n果，实现即时展示而无需等待全部内容处理完毕。\nfrom langchain_openai import OpenAI\nfor chunk in llm.stream(\n&quot;你好&#x27;,
 &#x27;k in llm.stream(\n&quot;你好呀，请你介绍一下你自己&quot;\n):\nprint(chunk, end=&quot;&quot;, flush=True)\n吧，你是谁，你在哪里，你是什么学问，你喜欢什么，你不喜欢什么，你不会什么，你会什么，你梦想什么，\n你的优点是什么，你的缺点是什么，你的性格是什么，你的兴趣是什么，你喜欢的食物是什么，你不喜欢的食\n物是什么，你的爱好是什么，你的愿望是什么。介绍一下你自己好的。\n[ 编辑 xiaofan1 在 07-03-18 23:49 ]\n批量生成\n通过调用batch方法，可以同时对多个不同的Prompt进行调用处理。在这个例子中，输入包括“你\n好”、“什么是机器学习”和“什么是深度学习”三个独立的Prompt。这种批量处理方式允许一次性提交多个\nPrompt，从而提高推理效率。\nfrom langchain_openai import OpenAI\nllm.batch(\n[\n&quot;你好&quot;,\n&quot;什么是机器学习&quot;,\n&quot;什么是深度学习&quot;,\n]\n)\n[\&#x27;，习惯了你的成熟，可是这次你的决定让我感到非常惊讶，我不知道你的这次决定是不是真的，我不知道你\n是否真的那么爱我，虽然我知道&#x27;,
 &quot;我不知道你\n是否真的那么爱我，虽然我知道你非常爱我，可是我不知道你是不是真的爱我，假如你为了我不想去上学，我\n也可以放弃一切，去读书，去上大学，但是你为什么要这样做呢，我不清楚你的想法，我不知道你的想法，我\n只知道我爱你，我只知道我不想失去你，我只知道我不想让你失去你的希望，我只知道我不想让你失去你的未\n来，可是我想知道你为什么要这样做呢，我想知道你的想法，你是&#x27;,\n&#x27; 机器学习的原理 机器学习的技术 机器学习的应用机器学习在生产过程中的作用机器学习在生产过程中的\n作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产\n过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学\n习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作\n用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学&#x27;,\n&#x27;？ 2. Why is the industry so interested in deep learning? 3. How will de&quot;,
 &#x27;ning? 3. How will deep\nlearning transform the financial industry? 4. How can you use deep learning to\nbuild better AI? 5. What are the ingredients of a deep learning model?\\n\\nA\nstudy of the best deep learning algorithms and how they work, the techniques\ninvolved in building them, and the results they can bring to the table: 1.\nUnsupervised learning 2. Convolutional Neural Networks (CNN) 3. Recurrent Neural\nNetworks (RNN) 4. Deep Reinforcement Learning (RL) 5. Generative Adversarial\nNetworks (GA&#x27;,
 &#x27;rsarial\nNetworks (GAN)\\n\\nA step-by-step guide to building a real-world, deep learning\napp: 1. What is a deep learning app? 2. How do you build a deep learning app? 3.\nThe various components involved in building a deep learning app 4. How do you\ndeploy a deep learning app? 5. The future of deep learning apps\\n\\nHow to build\na deep learning app: A guide to the various tools, frameworks, and libraries\nthat you can use to build deep learning apps: 1. Deep Learning Frameworks 2.\nDeep Learning Librar&#x27;,
 &#x27;Deep Learning Libraries 3. Deep Learning Tools 4\&#x27;]\n异步调用\nllm.invoke(...)本质上是一个同步调用。在这种情况下，程序会在调用返回结果之前停止执行\n任何后续代码。这意味着如果invoke操作耗时较长，它会导致程序暂时挂起，直到操作完成。我们可\n以通过这样一个测试代码来直观的理解同步调用：\nimport time\ndef call_model():\n# 模拟同步API调用\nprint(&quot;开始调用模型...&quot;)\ntime.sleep(3) # 模拟调用等待\nprint(&quot;模型调用完成。&quot;)\ndef perform_other_tasks():\n# 模拟执行其他任务\nfor i in range(3):\nprint(f&quot;执行其他任务 {i + 1}&quot;)\ntime.sleep(1)\ndef main():\nstart_time = time.time()\ncall_model()\nperform_other_tasks()\nend_time = time.time()\ntotal_time = end_time - start_t&#x27;,
 &#x27;= end_time - start_time\nreturn f&quot;总共耗时：{total_time}秒&quot;\n# 运行同步任务并打印完成时间\nmain_time = main()\nmain_time\n开始调用模型...\n模型调用完成。\n执行其他任务 1\n执行其他任务 2\n执行其他任务 3\n\&#x27;总共耗时：6.001835107803345秒\&#x27;\n这段同步调用的程序先模拟了一个耗时3秒的模型调用，随后执行了三个其他任务，每个任务耗时1\n秒。实际的执行时间为约6.00秒。这体现了同步执行的特点：每个操作依次执行，直到当前操作完成后\n才开始下一个操作，从而导致总的执行时间是各个操作时间的总和。\n而异步调用，允许程序在等待某些操作完成时继续执行其他任务，而不是阻塞等待。这在处理I/O\n操作（如网络请求、文件读写等）时特别有用，可以显著提高程序的效率和响应性。\n我们可以设计这样如下情景：使用async和await关键字， 通过async def定义异步函数\nasync_call和perform_other_tasks。await asyncio.sleep(3)模拟了一个耗时3秒的异步操\n作。这里的awa&#x27;,
 &#x27;了一个耗时3秒的异步操\n作。这里的await表明程序在这里“等待”这个操作完成，但与同步操作不同，这个等待是非阻塞的。这\n意味着程序可以在这3秒内执行其他异步代码。这种方式不会阻塞整个程序的运行，而是让出控制权，\n允许事件循环继续运行其他任务。\n! pip install asyncio\nCollecting asyncio\nObtaining dependency information for asyncio from\nhttps://files.pythonhosted.org/packages/22/74/07679c5b9f98a7cb0fc147b1ef1cc1853b\nc07a4eb9cb5731e24732c5f773/asyncio-3.4.3-py3-none-any.whl.metadata\nDownloading asyncio-3.4.3-py3-none-any.whl.metadata (1.7 kB)\nDownloading asyncio-3.4.3-py3-none-any.whl (101 kB)\n-------------------------&#x27;,
 &#x27;----------------------------------- 0.0/101.8 kB ? eta -:--:--\n---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--\n---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--\n---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--\n----------- --------------------------- 30.7/101.8 kB 163.8 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01&#x27;,
 &#x27;1.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n----------------------- --------------- 61.4/101.8 kB 113.0 kB/s eta 0:00:01\n--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01\n-----------------&#x27;,
 &#x27;01\n--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01\n--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01\n-------------------------------------- 101.8/101.8 kB 136.2 kB/s eta 0:00:00\nInstalling collected packages: asyncio\nSuccessfully installed asyncio-3.4.3\nimport asyncio\nimport time\nasync def async_call(llm):\nawait asyncio.sleep(3) # 模拟异步操作\nprint(&quot;异步调用完成&quot;)\nasync def perform_other_tasks():\nawait asyncio.sleep(3) # 模拟异步操作\nprint(&quot;其他任务完成&quot;)\nasync d&#x27;,
 &#x27;nt(&quot;其他任务完成&quot;)\nasync def run_async_tasks():\nstart_time = time.time()\nawait asyncio.gather(\nasync_call(None), # 示例调用，替换None为模拟的LLM对象\nperform_other_tasks()\n)\nend_time = time.time()\nreturn f&quot;总共耗时：{end_time - start_time}秒&quot;\n# 运行异步任务并打印完成时间\nawait run_async_tasks()\n异步调用完成\n其他任务完成\n\&#x27;总共耗时：3.009281873703003秒\&#x27;\n使用asyncio.gather()并行执行时，理想情况下，因为两个任务几乎同时开始，它们的执行时间\n将重叠。如果两个任务的执行时间相同（这里都是3秒），那么总执行时间应该接近单个任务的执行时\n间（3秒左右），而不是两者时间之和。\n理解了同步调用和异步调用的区别后，我们接下来看一下LangChain中集成的异步调用链使用方\n法。\n要实现异步调用并优化处理多个请求的场景，在LangChain中，可以使用a&#x27;,
 &#x27;场景，在LangChain中，可以使用ainvoke方法来达成。这\n种方法允许同时启动多个请求，而不必等待每个请求依次完成。这对于提高应用程序的响应性和效率尤\n其有用，特别是在处理需要与外部服务交互的任务时。\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;)\nawait llm.ainvoke(\n&quot;哈喽，你好呀，&quot;,\n)\n\&#x27;这次您知道了吗？这是鸡肋大战，您是来玩的吧，我也是。我来纠结你的，看看你敢不敢。我一般不来这种\n鸡肋大战，因为我很懒，这次我还是来了，因为我懒惯了，其实我很想快点解脱了，我很想走。你的目的在哪\n里？我想敌对你，我还是要意识到了，你想不想敌对我呢？你敢不敢这样问？我不敢，因为我懒。你的目的是\n什么？我很想快点解脱，我不想你，我只想快点走，早点走。你敢不敢这样说？我敢，因为我懒。你的目的是\n什么？我\&#x27;\n异步流式响应\nfrom langchain_openai import OpenAI\nasync for chunk in llm.astream(\n&quot;你好，&quot;,\n):\np&#x27;,
 &#x27;astream(\n&quot;你好，&quot;,\n):\nprint(chunk, end=&quot;&quot;, flush=True)\n我是来投诉的 [Image]\n你现在没有抱怨，说的确实很有道理，如果他们为了你不让你上，你能不上吗，我想大多数人应该会上，所以\n他们是不会因为你不上而阻止你上的，但是如果你上了，你的优惠和你的一切都是不可能了，你上了，他们是\n不能为你划上你的学费的，所以我觉得就算你上了也没有用，他们不会为你划学费，因为你上了从而给了他们\n非常大的麻烦，你上了，他们是不能有一点好处的，他们还要为你上的学费等等付出，所以他们会说你上不\n了，就是为了不让你上，不让你上，他们不会为你划学费，所以我觉得你如果上了，你就没有任何\n异步批量处理\nfrom langchain_openai import OpenAI\nawait llm.abatch(\n[\n&quot;你好&quot;,\n&quot;什么是机器学习&quot;,\n&quot;什么是深度学习&quot;,\n]\n)\n[\&#x27;，前来亲自拜访的，因为我们这里很少有人来拜访，你就打算在这里待上几个礼拜了吧？”“我是回来找你，\n我是到桐谷这里来的。”李晓婷冷冷的回道。这一次很明显的不是她想让他留下来的，而是让他走了。众人看&#x27;,
 &quot;是她想让他留下来的，而是让他走了。众人看\n着李晓婷，这里的她就这么冷了，竟然连郑晓东都不看一眼，郑晓东是她的男朋友，他们是一对，她怎么可能\n不看一眼？“那你为什么不和我说，你的事情怎么样了？”郑晓东看向李晓婷的脸，她的表情很冷，她怎么不看\n一眼自己&#x27;,\n&#x27;？\\n\\n2:王景仪 2021-01-07 23:12:57\\n\\nannounced\\n\\n3:李桂林 2021-01-01\n23:12:57\\n\\nMy Sunshine is a TV drama adapted from Gu Man’s novel of the same\ntitle. It stars Wallace Huo and Tang Yan, premiering in January, 2015. The drama\ndepicts a man and a woman’s sweet love story.\\n\\n4:王威 2020-12-27\n23:12:57\\n\\nThe new data released yesterday represent the first snapshot of\nglobal t&quot;,
 &#x27;snapshot of\nglobal trade for 2015. And the figures suggest that 2015 was not a good year for\nworld trade, as it had been hoped.\\n\\n5:苏晓波 2021-01-01 23:12:57\\n\\nMark Rutte,\nthe Dutch prime minister whose People’s Party for Freedom and Democracy (VVD) is\nslightly ahead of the PVV, has seen his poll numbers rise in recent days because\nof his hardline stance with Ankara. On Tuesday he called Mr Erdogan’s comments\n“a disgusting distortion of history”, adding: “We will not lower ourselves to\nthis leve&#x27;,
 &quot;rselves to\nthis level. It is totally unacceptable.”\\n\\n&#x27;,\n&#x27;？深度学习是一种模仿大脑思维的机器学习方法，深度学习方法被广泛用于图像识别、语音识别、自然语言\n处理等领域。深度学习过程中，模型经常需要大量的训练数据，因此大量的数据获取和数据刷新是有必要。.\n1) Find a free server to host the application in. I have been trying to create a\nsimple neural network by using the TensorFlow Object Detection API to run an\nobject detection model. Cloud TPU is a Google Cloud service that accelerates\nmachine learning workloads. Major cloud providers offer free tier of services to\nhelp you get started. Google&quot;,
 &quot; get started. Google Cloud Storage is a RESTful web service for storing\nand accessing data on Google Cloud Platform. In this video, you will learn about\nCloud TPU, a high-performance, scalable, and cost-effective machine learning\nservice. This IP address 157. 0, a neural network framework. Contribute.\nTensorFlow Lite for mobile and embedded devices For Production TensorFlow\nExtended for end-to-end ML components. The Google Cloud Platform&#x27;]\n除此之外，在Jupyter Notebook或Python交互式解释器中进行操作的话，可以使用?来查看关于\nll&quot;,
 &quot;进行操作的话，可以使用?来查看关于\nllm中内置的方法和帮助文档，执行如下代码：\nllm?\n•[1;31mSignature:•[0m\n•[0mllm•[0m•[1;33m(•[0m•[1;33m\n•[0m •[0mprompt•[0m•[1;33m:•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mstop•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m\n•[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mcallbacks•[0m•[1;33m:•[0m •[1;34m&#x27;Callbacks&#x27;•[0m •[1;33m=•[0m\n•[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[1;33m*•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mtags•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m\n•[1&quot;,
 &quot;[0m •[1;33m=•[0m\n•[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mmetadata•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[Dict[str, Any]]&#x27;•[0m\n•[1;33m=•[0m •[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[1;33m**•[0m•[0mkwargs•[0m•[1;33m:•[0m\n•[1;34m&#x27;Any&#x27;•[0m•[1;33m,•[0m•[1;33m\n•[0m•[1;33m)•[0m •[1;33m-&gt;•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m•[0m•[1;33m•[0m•[0m\n•[1;31mType:•[0m OpenAI\n•[1;31mString form:•[0m\n•[1mOpenAI•[0m\nParams: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,\n&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalt&quot;,
 &quot; 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {},\n&#x27;max_tokens&#x27;: 256}\n•[1;31mFile:•[0m d:\\software\\anaconda3\\lib\\site-\npackages\\langchain_openai\\llms\\base.py\n•[1;31mDocstring:•[0m\nOpenAI large language models.\nTo use, you should have the ``openai`` python package installed, and the\nenvironment variable ``OPENAI_API_KEY`` set with your API key.\nAny parameters that are valid to be passed to the openai.create call can be\npassed\nin, even if not explicitly saved on this class.\nExample:\n.. code-bloc&quot;,
 &#x27;xample:\n.. code-block:: python\nfrom langchain_community.llms import OpenAI\nopenai = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\n•[1;31mInit docstring:•[0m\nCreate a new model by parsing and validating input data from keyword arguments.\nRaises ValidationError if the input data cannot be parsed to form a valid model.\n•[1;31mCall docstring:•[0m\n[*Deprecated*] Check Cache and run the LLM on the given prompt and input.\nNotes\n-----\n.. deprecated:: 0.1.7\nUse invoke instead.\n这里能看到非常多的关键信息，比如：\n默认参数：Params&#x27;,
 &#x27;关键信息，比如：\n默认参数：Params: {\&#x27;model_name\&#x27;: \&#x27;davinci-002\&#x27;, \&#x27;temperature\&#x27;: 0.7, \&#x27;top_p\&#x27;: 1,\n\&#x27;frequency_penalty\&#x27;: 0, \&#x27;presence_penalty\&#x27;: 0, \&#x27;n\&#x27;: 1, \&#x27;logit_bias\&#x27;: {}, \&#x27;max_tokens\&#x27;: 256}\n当不输入model_name时，默认会调用davinci-002模型：\nfrom langchain_openai import OpenAI\nllm = OpenAI()\nawait llm.ainvoke(\n&quot;哈喽，你好呀，&quot;,\n)\n\&#x27;我是小影。很高兴认识你，有什么可以帮到你的吗？\&#x27;\n同时，也可以调整温度temperature:\nfrom langchain_openai import OpenAI\nllm = OpenAI()\nawait llm.ainvoke(\n&quot;哈喽，你好呀，&quot;,\ntemperature=0\n)\n\&#x27;我是一个人工智能助手，很高兴认识你！有什么可以帮到你的吗？\&#x27;\nmodel：必选参数，具体调用的Completions&#x27;,
 &#x27;选参数，具体调用的Completions模型名称，可以调用的模型包括text-davinci-003、\ntext-davinci-002、text-curie-001、text-babbage-001、text-ada-001等，不同模型参数规模不\n同；这里需要注意，大模型领域不同于机器学习领域，后者哪怕是简单模型在某些场景下可能也会\n拥有比复杂模型更好的表现。在大模型领域，（就OpenAI提供的A、B、C、D四大模型来看）参\n数规模越大、越新版本的模型效果更好（当然费用也更高），因此课程中主要以text-davinci-003\n使用为例进行讲解；\nprompt：必选参数，提示词；\nsuffix：可选参数，默认为空，具体指模型返回结果的后缀；\nmax_tokens：可选参数，默认为16，代表返回结果的token数量；\ntemperature：可选参数，取值范围为0-2，默认值为1。参数代表采样温度，数值越小，则模型会\n倾向于选择概率较高的词汇，生成的文本会更加保守；而当temperature值较高时，模型会更多地\n选择概率较低的词汇，生成的文本会更加多样；\ntop_p：可选参数，取值范&#x27;,
 &#x27;更加多样；\ntop_p：可选参数，取值范围为0-1，默认值为1，和temperature作用类似，用于控制输出文本的\n随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节\n文本随机性，top_p和temperature两个参数选择一个进行调整即可；这里更推荐使用\ntemperature参数进行文本随机性调整；\nn：可选参数，默认值为1，表示一个提示返回几个Completion；\nstream：可选参数，默认值为False，表示回复响应的方式，当为False时，模型会等待返回结果\n全部生成后一次性返回全部结果，而为True时，则会逐个字进行返回；\nlogprobs：可选参数，默认为null，该参数用于指定模型返回前N个概率最高的token及其对数概\n率。例如，如果logprobs设为10，那么对于生成的每个token，API会返回模型预测的前10个\ntoken及其对数概率；\necho：可选参数，默认为False，该参数用于控制模型是否应该简单地复述用户的输入。如果设为\nTrue，模型的响应会尽可能地复述用户的输入；\nstop：可选参数，默认为null，该&#x27;,
 &#x27;\nstop：可选参数，默认为null，该参数接受一个或多个字符串，用于指定生成文本的停止信号。当\n模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成。这可以用来控制模型的输出长\n度或格式；\npresence_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容\n（例如新的概念或主题）的倾向性。较高的值会使模型更倾向于生成新内容，而较低的值则会使模\n型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取\n值；\nfrequency_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾\n向性。较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果\n篇幅较大并且存在前后语言重复时，可以提高该参数的取值；\nbest_of：该参数用于控制模型的生成过程。它会让模型进行多次尝试（例如，生成5个不同的响\n应），然后选择这些响应中得分最高的一个；\nlogit_bias：该参数接受一个字典，用于调整特定token的概率。字典的键是token的ID，值是应用\n于该t&#x27;,
 &#x27;的键是token的ID，值是应用\n于该token的对数概率的偏置；在GPT中我们可以使用tokenizer tool查看文本Token的标记。一般\n不建议修改；\nuser：可选参数，使用用户的身份标记，可以通过人为设置标记，来注明当前使用者身份。需要\n注意的是，Completion.create函数中的user和后续介绍的对话类模型的user参数含义并不相同，\n需要注意区分；\n更详细的参数说明和使用细节可在《Ch.2 Completions模型API使用指南》中找到。此处不在展开\n重复说明。\n目前在LangChain中已经集成的LLMs有82个，均已经实现了对异步、流式处理和批处理的基本支\n持，查看地址如下：https://python.langchain.com/docs/integrations/llms/\n4.1.2 Chat Models接入方法\n所谓聊天模型，简单理解就是我们经常使用的xxx-chat类模型，比如Baichuan2-7b-chat，\nQwen-14b-chat，这类模型是基于Base模型经过针对特定数据格式的微调得到，以更贴近人类对话的\n方式进行输入和输出。所&#x27;,
 &#x27;更贴近人类对话的\n方式进行输入和输出。所以这类模型不会使用“文本输入、文本输出”的形式，而是采用聊天消息作为输\n入并返回聊天消息作为输出（非纯文本）。LangChain框架与OpenAI、Cohere、Hugging Face等多家\n模型提供商已经实现集成，提供了一个标准化的接口，以便与这些不同的模型进行交互。\n我们先回顾下OpenAI规范下使用Chat模型进行一次交互需遵循的代码执行方式：\nfrom openai import OpenAI\nclient = OpenAI()\ncompletion = client.chat.completions.create(\nmodel=&quot;gpt-3.5-turbo-0125&quot;,\nmessages=[\n{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一个乐于助人的智能AI小助手&quot;},\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}\n]\n)\nprint(completion.choices[0].message.content)\n您好，我是一个智能AI助手，可以帮助您回答问题、提供信息、解&#x27;,
 &#x27;I助手，可以帮助您回答问题、提供信息、解决问题等。如果您有任何疑问或需要帮助的\n话，都可以随时向我提问。我会尽力帮助您解决问题。\n这里不得不再次提到，LangChain作为一个应用开发框架，需要集成各种不同的大模型，如上述\nOpenAI的GPT系列模型调用示例，通过Message数据输入规范，定义不同的role，即system、user和\nassistant来区分对话过程，但对于其他大模型，并不意味这一定会遵守这种输入输出及角色的定义，所\n以LangChain的做法是，因为Chat Model基于消息而不是原始文本，LangChain目前就抽象出来的消息\n类型有 AIMessage 、 HumanMessage 、 SystemMessage 、 FunctionMessage 和 ChatMessage ，\n但大多时候我们只需要处理 HumanMessage 、 AIMessage 和 SystemMessage，即：\nSystemMessage ：用于启动 AI 行为，作为输入消息序列中的第一个传入。\nHumanMessage ：表示来自与聊天模型交互的人的消息。\nAIMessag&#x27;,
 &#x27;天模型交互的人的消息。\nAIMessage ：表示来自聊天模型的消息。这可以是文本，也可以是调用工具的请求。\n同样，Chat Models实现了 Runnable 接口，也支持 invoke 、 ainvoke 、 stream 、 astream 、\nbatch 、 abatch 等调用。即：\n方法 说明\ninvoke 处理单条输入\nbatch 处理批量输入\nstream 流式响应\nainvoke 异步处理单条输入\nabatch 异步处理批量输入\nastream 异步流式响应\n所以对于LangChain中调用OpenAI的GPT模型，首先需要导入对应的模块，代码如下：\nfrom langchain_core.messages import HumanMessage, SystemMessage\n消息形式输入调用\n定义消息对象：\nmessages = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;你好，请你介绍一下你自己&quot;),]\nmessages\n[SystemMessage(content=\&#x27;你是一位乐&#x27;,
 &#x27;ssage(content=\&#x27;你是一位乐于助人的智能小助手\&#x27;), HumanMessage(content=\&#x27;你好，请\n你介绍一下你自己\&#x27;)]\nfrom langchain_openai import ChatOpenAI\nchat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)\n执行推理：\nchat.invoke(messages)\nAIMessage(content=\&#x27;你好，我是一位可以回答问题、提供帮助的智能小助手。无论是关于知识、技能还是\n日常生活中的问题，我都会尽力为您提供准确和有用的答案。有什么可以帮助您的吗？\&#x27;)\n如果只想获取到纯净的输出，可以通过.content来格式化。\nreponse.content\n\&#x27;你好！我是一位智能助手，可以回答各种问题，提供信息和帮助。无论是关于知识、技能、娱乐、生活等方\n面，我都会尽力为你提供帮助。有什么问题需要我帮忙的吗？\&#x27;\n流式调用\nfor chunk in chat.stream(messages):\nprint(chunk.content, end=&quot;&quot;, flush=True)\n你好！我&#x27;,
 &#x27;&quot;&quot;, flush=True)\n你好！我是一位智能助手，专门为用户提供信息咨询、问题解答和帮助服务。无论你有什么疑问或需求，我都\n会尽力帮助你，让你更方便地获取所需的信息和支持。有什么我可以帮助你的吗？\n批量调用\n先定义三个不同的消息对象：\nmessages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]\nmessages2 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是深度学习&quot;),]\nmessages3 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是大模型技术&quot;),]\n将上述三个消息对象放在一个列表中，使用.batch方法执行批量调用。\nreponse = chat.batch([messages1,\nmessages2,\nmessages3,])\nr&#x27;,
 &quot;ges2,\nmessages3,])\nreponse\n[AIMessage(content=&#x27;机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来\n改善自身的性能，而无需明确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规\n律、模式以及知识的技术。通过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预\n测、分类、识别等任务。机器学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测\n等领域。&#x27;),\nAIMessage(content=&#x27;深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神\n经网络层来学习和理解复杂的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识\n别、语音识别、自然语言处理等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对\n新数据进行准确预测和分类。深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金\n融、交通等各个领域。&#x27;),\nAIMessage(content=&#x27;大模型技术是指利用大规模、高性能的计算资源和先进的机器学&quot;,
 &#x27;用大规模、高性能的计算资源和先进的机器学习算法来训练和部\n署复杂的深度学习模型。这些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优\n化。\\n\\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模\n型技术，研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。\\n\\n然\n而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员和\n工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。\&#x27;)]\n格式化输出：\n# 使用列表生成式打印每一个消息的内容\ncontents = [msg.content for msg in reponse]\n# 打印出内容\nfor content in contents:\nprint(content, &quot;\\n---\\n&quot;)\n机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来改善自身的性能，而无需明\n确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规律、模式以及知识的技术。通\n过利用统计学和&#x27;,
 &#x27;、模式以及知识的技术。通\n过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预测、分类、识别等任务。机器\n学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测等领域。\n---\n深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神经网络层来学习和理解复杂\n的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识别、语音识别、自然语言处理\n等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对新数据进行准确预测和分类。\n深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金融、交通等各个领域。\n---\n大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部署复杂的深度学习模型。这\n些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优化。\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模型技术，\n研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。\n然而，大模型技术也面临着一些挑战，如计算资&#x27;,
 &#x27;而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员\n和工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。\n---\n异步调用\nmessages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]\nfrom langchain_openai import ChatOpenAI\nchat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)\nreponse = await chat.ainvoke(messages1)\nreponse\nAIMessage(content=\&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并\n改进性能，而无需显式地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断\n优化自身的表现。机器学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常\n见的机器学习方法包括监督学习、无监督&#x27;,
 &#x27;常\n见的机器学习方法包括监督学习、无监督学习、强化学习等。\&#x27;)\nreponse.content\n\&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并改进性能，而无需显式\n地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断优化自身的表现。机器\n学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常见的机器学习方法包括\n监督学习、无监督学习、强化学习等。\&#x27;\n流式调用\nasync for chunk in chat.astream(messages1):\nprint(chunk.content, end=&quot;&quot;, flush=True)\n机器学习是一种人工智能的分支，它致力于让计算机系统通过学习数据和模式，从而改进自身的性能。简而言\n之，机器学习让计算机具有从经验中学习的能力，而不需要明确地编程指令。\n在机器学习中，计算机系统利用大量数据进行训练，通过识别数据中的模式和规律来建立模型。这些模型可以\n用来进行预测、分类、识别等任务。机器学习在各个领域中都有广泛的应用，如自然语言处理、图像识别、医\n疗诊断、金融预测等。\n常见的&#x27;,
 &#x27;像识别、医\n疗诊断、金融预测等。\n常见的机器学习算法包括监督学习、无监督学习、强化学习等。监督学习是通过已标记的数据进行训练，无监\n督学习则是在没有标记的数据中发现模式，强化学习则通过试错的方式学习最优策略。机器学习技术的发展使\n得计算机系统能够更加智能地处理复杂任务，为人们的生活和工作带来了许多便利。\n通过上述描述，我们展示了在LangChain中使用LLMs类模型和Chat Model类模型的不同方法，其\n核心区别在于输入Prompt的格式。除此之外其他的工作可以直接利用统一的抽象接口，实现与模型交\n互的快速过程。而针对不同的模型，LangChain也提供个对应的接入方法，其相关说明文档地址：http\ns://python.langchain.com/docs/integrations/chat/\n比如我们以Baichuan2的在线API模型为例快速接入一下：\nfrom langchain_community.chat_models import ChatBaichuan\nfrom langchain_core.messages import HumanMessage\nchat =&#x27;,
 &#x27; HumanMessage\nchat = ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-xxx&quot;,\nstreaming=True,\n)\nresponse = chat([HumanMessage(content=&quot;我日薪8块钱，请问在闰年的二月，我月薪多少&quot;)])\nresponse\nAIMessage(content=\&#x27;闰年的二月有29天，所以你的月薪是：\\n\\n$8\\\\times29$=$232$（元）\\n\\n所\n以，你在闰年的二月月薪是232元。\&#x27;)\nresponse.content\n\&#x27;闰年的二月有29天，所以你的月薪是：\\n\\n$8\\\\times29$=$232$（元）\\n\\n所以，你在闰年的二月月\n薪是232元。\&#x27;\n如未实践过Baichuan2的在线API，请查看《APICh.16 BaiChuan 2 大模型生态介绍及本地私有化\n部署方案》中1. Baichuan在线大模型生态介绍小节\n可以看出，LangChain能够迅速接入不同的模型API。至于接入本地私有部署的开源大模型的方\n法，我们将在后续课程中进行详细&#x27;,
 &#x27;模型的方\n法，我们将在后续课程中进行详细介绍。\n4.2 LangChain中如何使用Prompt Template\n提示工程（Prompt Engineering）大家应该比较熟悉，这个概念是指在与大语言模型（LLMs），\n如GPT-3、Qwen等模型进行交互时，精心设计输入文本（即提示）的过程，以获得更精准、相关或有\n创造性的输出。正如我们在大模型技术应用课程中介绍的，通过采用Few-Shot、Chain of Thought\n(CoT)等高级提示技巧，可以显著提高大模型在推理任务上的表现。目前，提示工程已经发展成为一个\n专业领域，非常多的公司设立了专门的职位，负责为特定任务编写精确且具有创造力的提示。\n以使用ChatGPT等网页端对话交互应用中，大部分人常见的做法是将提示（Prompt）做硬编码，\n例如将一段提示文本固定在System Messages中。而在应用开发领域，开发者往往无法预知用户的具体\n输入内容，同时又希望大模型能够根据不同的应用任务以一种较为统一的逻辑来处理用户输入。所以，\nLangChain通过提供Prompt Templates功能，优雅地解决了这个问题。Pr&#x27;,
 &#x27;ates功能，优雅地解决了这个问题。Prompt Templates将用户输入\n到完整格式化提示的转换逻辑进行封装，使得模型能够更灵活、高效地处理各种输入。\nLangChain 提供了创建和使用提示模板的工具，其使用PromptTemplate 方法创建字符串提示模\n板，因为目前为止，语言模型接收的提示基本都是字符串或聊天消息列表。\n4.2.1 使用str.format语言构建模版\n在LangChain的默认设置下， PromptTemplate 使用 Python 的 str.format() 方法进行模板\n化。\nPython的str.format()方法是一种字符串格式化的手段，允许我们在字符串中插入变量。使用这\n种方法，可以创建包含占位符的字符串模板，占位符由花括号{}标识。调用format()方法时，可以传入\n一个或多个参数，这些参数将被顺序替换进占位符中。str.format()提供了灵活的方式来构造字符串，支\n持多种格式化选项，包括数字格式化、对齐、填充、宽度设置等。\n基本用法\n# 简单示例，直接替换\ngreeting = &quot;Hello, {}!&quot;.format(&quot;Alic&#x27;,
 &#x27;o, {}!&quot;.format(&quot;Alice&quot;)\nprint(greeting)\n# 输出: Hello, Alice!\nHello, Alice!\n带有位置参数的用法\n# 使用位置参数\ninfo = &quot;Name: {0}, Age: {1}&quot;.format(&quot;Bob&quot;, 30)\nprint(info)\n# 输出: Name: Bob, Age: 30\nName: Bob, Age: 30\n带有关键字参数的用法\n# 使用关键字参数\ninfo = &quot;Name: {name}, Age: {age}&quot;.format(name=&quot;Charlie&quot;, age=25)\nprint(info)\n# 输出: Name: Charlie, Age: 25\nName: Charlie, Age: 25\n使用字典解包的方式：\n# 使用字典解包\nperson = {&quot;name&quot;: &quot;David&quot;, &quot;age&quot;: 40}\ninfo = &quot;Name: {name}, Age: {age}&quot;.format(**person)\nprint(info)\n# 输出: Name: David, Age: 40\nName: &#x27;,
 &#x27;avid, Age: 40\nName: David, Age: 40\n在LangChain中，基本采用了Python的原生str.format()方法对输入数据进行格式化，这样在模\n型接收输入前，可以根据需要对数据进行预处理和结构化，以此来引导大模型进行更准确的推理。\nfrom langchain.prompts import PromptTemplate\nprompt_template = PromptTemplate.from_template(\n&quot;请给我一个关于{topic}的{type}解释。&quot;\n)\nprompt= prompt_template.format(type=&quot;详细&quot;, topic=&quot;量子力学&quot;)\nprompt\n\&#x27;请给我一个关于量子力学的详细解释。\&#x27;\n如上所示，可以使用PromptTemplate的from_template方法创建一个提示模板实例，这个模板\n包含了两个占位符：{topic} 和 {type}，这些占位符在实际调用时可以被实际的值替换。\n调用LLMs模型\nfrom langchain_openai import OpenAI\nllm = OpenAI&#x27;,
 &#x27; OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)\nresult = llm.invoke(prompt)\nprint(result)\n我觉得我已经对量子力学的基本理解了，但是我还是很想听听有关量子力学的详细解释，比如：量子力学是怎\n么起作用的。\n谢谢了.\n量子力学是在克里姆林宫试验时的实验结果发现.一开始是用熟悉的物理学法则去解释它.比如说,量子力学是\n量子力学的结果,而不是量子力学的原因.玻尔兹曼方程就是克里姆林宫试验结论的表示.后来人们发现,等效\n性,柏格效应等现象.然后才发现,原来我们的光学和电磁学等理论是有问题的.这些现象的实现是在离子束中.\n所以量子力学是很实际的.\n[ 编辑 stuartm 在 06-07-08 12:11 ]\nprompt= prompt_template.format(type=&quot;简短&quot;, topic=&quot;机器学习&quot;)\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;, max_to&#x27;,
 &#x27;davinci-002&quot;, max_tokens=1000)\nresult = llm.invoke(prompt)\nprint(result)\n2)目前的时代，机器学习已经发展到了什么程度，也有什么应用场景。 3)目前的研究方向是什么。 4)什么\n时候会用到这些技术。 5)最后一个问题是，对于我自己来说，是不是还有其他的方向比如说算法之类的，还\n有在个人的技术知识里面是不是需要这些东西，不是特别需要。 也许这些都是我自己的猜测，如果你有什么\n好的建议，我也非常感谢。 最后，我想问一下，你是在哪里学的，是在哪个大学，我就是在这里问问。 谢\n谢。 2019.6.16\n电影《伪装者》中的重要概念\n原理：1. 无线电波的能量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无\n线电波的方向不会发生改变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改\n变，传播途中，波的方向不会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途\n中，波的方向会发生改变（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变\n&#x27;,
 &#x27;生改变，传播途中，波的方向不会发生改变\n（这就是遮挡）3. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）4. 无线\n电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）5. 无线电波的方向会发生改变，\n传播途中，波的方向不会发生改变（这就是遮挡）6. 无线电波的方向会发生改变，传播途中，波的方向不会\n发生改变（这就是遮挡）7. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）\n8. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）原理：1. 无线电波的能\n量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无线电波的方向不会发生改\n变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改变，传播途中，波的方向不\n会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途中，波的方向会发生改变\n（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）3. 无线\n电波的方向会发生改变，传播途中，波的方向不会发生改变（这&#x27;,
 &#x27;改变，传播途中，波的方向不会发生改变（这\n除此之外，这种模板生成方式支持任意数量的变量，包括无变量，可以做如下测试：\nfrom langchain.prompts import PromptTemplate\nprompt_template = PromptTemplate.from_template(&quot;给我讲一个笑话&quot;)\nprompt_template.format()\n\&#x27;给我讲一个笑话\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;, )\nresult = llm.invoke(prompt)\nprint(result)\n如果我是一个机器，会被称为什么？\n谢谢\n调用Chat Model\n上述示例展示了如何使用字符串模板生成和调用LLMs类模型。对于聊天模型，也需要使用 Python\n的 str.format() 方法进行模板化，但其构造的输入形式则有所区别，应采用聊天消息列表的形式，\n每条消息都包含内容和一个名为role的附加参数。例如，在OpenAI的Chat Completion&#x27;,
 &#x27;enAI的Chat Completions API中，聊天\n消息需要包含为assistant、user或system。在这种情况下，应使用ChatPromptTemplate来构建对话\n模板。\nfrom langchain_core.prompts import ChatPromptTemplate\nchat_template = ChatPromptTemplate.from_messages(\n[\n(&quot;system&quot;, &quot;你是一个有帮助的AI机器人，你的名字是{name}。&quot;),\n(&quot;human&quot;, &quot;你好，最近怎么样？&quot;),\n(&quot;ai&quot;, &quot;我很好，谢谢！&quot;),\n(&quot;human&quot;, &quot;{user_input}&quot;),\n]\n)\nmessages = chat_template.format_messages(name=&quot;小明&quot;, user_input=&quot;你叫什么名字？&quot;)\nmessages\n[SystemMessage(content=\&#x27;你是一个有帮助的AI机器人，你的名字是小明。\&#x27;),\nHumanMessage(content=\&#x27;你好，最近怎么样？\&#x27;),\nAIMessage(con&#x27;,
 &quot;么样？&#x27;),\nAIMessage(content=&#x27;我很好，谢谢！&#x27;),\nHumanMessage(content=&#x27;你叫什么名字？&#x27;)]\nprint(chat_template)\ninput_variables=[&#x27;name&#x27;, &#x27;user_input&#x27;] messages=\n[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;name&#x27;],\ntemplate=&#x27;你是一个有帮助的AI机器人，你的名字是{name}。&#x27;)),\nHumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[],\ntemplate=&#x27;你好，最近怎么样？&#x27;)),\nAIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;我很\n好，谢谢！&#x27;)), HumanMessagePromptTemplate(prompt=PromptTemplate(input_varia&quot;,
 &#x27;Template(input_variables=\n[\&#x27;user_input\&#x27;], template=\&#x27;{user_input}\&#x27;))]\n从输出上看，其构造函数在实例化prompt_template时，主要由两个关键参数进行指定：\ninput_variables：这是一个列表，包含模板中需要动态填充的变量名。这些变量名在模板字符串\n中以花括号（如{name}）标记。通过指定这些变量，可以在后续过程中动态地替换这些占位符。\ntemplate：这是定义具体提示文本的模板字符串。它可以包含静态文本和input_variables列表中\n指定的变量占位符。当调用format方法时，这些占位符会被实际的变量值替换，生成最终的提示\n文本。\n调用Chat Model，这里我们选择GPT 3.5。代码如下：\nfrom langchain.chat_models import ChatOpenAI\nllm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)\nresult = llm.invoke(messages)\nprint(result&#x27;,
 &#x27;ssages)\nprint(result.content)\n我叫小明，很高兴为您提供帮助！有什么问题可以帮到您？\nmessages = chat_template.format_messages(name=&quot;大熊&quot;, user_input=&quot;你要去哪里玩？&quot;)\nfrom langchain.chat_models import ChatOpenAI\nllm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)\nresult = llm.invoke(messages)\nprint(result.content)\n作为一个AI机器人，我没有身体，所以无法去任何地方玩耍。但我可以帮助你规划旅行或提供有关旅游目的地\n的信息。有什么我可以帮助你的吗？\n通过上述流程，我们演示了如何在LangChain框架内使用ChatPromptTemplate来构建与聊天模型\n交互的对话模板。通过此方法，能够创建一个包含不同参与者角色和定制化消息内容的对话流程，这对\n于开发复杂的聊天应用或增强AI助手的交互能力至关重要。\n4.2.2 构造Few-S&#x27;,
 &#x27;力至关重要。\n4.2.2 构造Few-Shot模版\n在LangChain中，很多的功能抽象、链路抽象本质上都是在对大模型的“涌现能力”能够应用落地的\n一种具体实现方法，而其推理的不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度\n依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。最为简单的\n提示工程的方法就是通过输入一些类似问题和问题答案，让模型参考学习，并在同一个prompt的末尾\n提出新的问题，依次提升模型的推理能力。\n比如我们在《Ch.3 基于思维链的进阶提示工程》中测试的经典推理问题：\nimport os\nimport openai\nopenai.api_key = os.getenv(&quot;OPENAI_API_KEY&quot;)\nprompt = \&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关\n闭之前她能滑多少次？\&#x27;\nprompt\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？\&#x27;\nfrom openai import OpenAI\ncl&#x27;,
 &#x27;nai import OpenAI\nclient = OpenAI()\nresponse1 = client.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=prompt,\nmax_tokens=1000\n)\nresponse1.choices[0].text\n\&#x27;\\n在关闭之前，艾米能滑14次。\&#x27;\nfrom openai import OpenAI\nclient = OpenAI()\nresponse1 = client.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=prompt,\nmax_tokens=1000\n)\nresponse1.choices[0].text\n\&#x27;\\n\\n15分钟 = 900秒\\n\\n每次滑下来需要1分钟，所以在15分钟内，可以滑下900秒/（1分钟+4分钟）=\n900/5 = 180次。\&#x27;\n可以看到多次执行，其推理均是错误的。正确的计算过程应该是先计算艾米一次爬上爬下总共需要\n5分钟，然后滑梯还有15分钟关闭，因此关闭之&#x27;,
 &quot;钟，然后滑梯还有15分钟关闭，因此关闭之前能够再滑15/5=3次。\n而我们当时在OpenAI的API规范下，是通过这种方式来进行Few-Shot的提示：\nprompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共\n有多少个网球？” \\\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网\n球，因此现在总共由5+6=11个网球。因此答案是11。” \\\nQ：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，\n请问现在食堂总共有多少个苹果？” \\\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9\n个苹果，答案是9。” \\\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫\n球是蓝色的。请问总共有多少个蓝色高尔夫球？” \\\nA：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色\n的，也就是4个，答案是4个。” \\\nQ：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在\n15分钟后关闭，请问在关闭之前她能滑多少次？” \\&quot;,
 &quot;关闭，请问在关闭之前她能滑多少次？” \\\nA：&#x27;\nprompt_Few_shot_CoT4\n&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在\n总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果\n他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色\n的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高\n尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：\n“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？” A：&#x27;\n在Few-Shot的提示下，再次尝试进行提问：\nfrom openai import OpenAI\nclient = OpenA&quot;,
 &#x27;penAI\nclient = OpenAI()\nresponse1 = client.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=prompt_Few_shot_CoT4,\nmax_tokens=1000,\n)\nresponse1.choices[0].text\n\&#x27;”艾米花了1分钟滑下来，滑一次上去需要4分钟，因此在15分钟关闭之前，她能滑15分钟-1分钟=14分钟，\n14分钟除以4分钟每次，她能滑14÷4=3次，答案是3次。“ \&#x27;\n从输出结果上看，通过Few-Shot提示可以激发大模型的涌现能力从而回答对复杂推理问题，在应用\n开发中同样需要这种能力来提升大模型的推理稳定性。所以LangChain抽象了\nFewShotPromptTemplate方法来实现。我们可以先测试下在LangChain的接口下直接提问：\n直接提问\nprompt1 = \&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在\n关闭之前她能滑多少次？\&#x27;\nprompt1\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1&#x27;,
 &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=prompt1\n)\n\&#x27;\\n\\n在关闭之前，艾米能滑(15-4-1)/4=2次。\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=prompt1\n)\n\&#x27;\\n\\n在15分钟内，她能滑15次。\&#x27;\n其实也很容易想到，LangChain的工作仅仅是集成并接入OpenAI的GPT模型，因此其本质上与直接\n调用GPT模型的效果相同。出现错误回答也属于正常情况。接下来，我们根据LangChain对Few-Shot模\n板的定义规范，进行提示词的构建。\n在LangChain中，需要使用 PromptTemplate 创建字符串提示&#x27;,
 &#x27;omptTemplate 创建字符串提示模板。模板可以包括说明、少量示\n例以及适合给定任务的特定上下文和问题。因此，我们要创建一个少量示例的列表。每个示例都是一个\n字典，其中键是输入变量，值是这些输入变量的值。\n适合LLMs模型的模版构造形式\nfrom langchain.prompts.few_shot import FewShotPromptTemplate\nfrom langchain.prompts.prompt import PromptTemplate\nexamples = [\n{\n&quot;question&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个\n网球？&quot;,\n&quot;answer&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现\n在总共由5+6=11个网球。因此答案是11。&quot;\n},\n{\n&quot;question&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在\n食堂总共有多少个苹果？&quot;,\n&quot;answer&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果&#x27;,
 &#x27;买了6个，总共有23-20+6=9个苹果，答\n案是9。&quot;\n},\n{\n&quot;question&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色\n的。请问总共有多少个蓝色高尔夫球？&quot;,\n&quot;answer&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4\n个，答案是4个。&quot;\n},\n]\n接下来需要实例化一个 PromptTemplate 对象，将少量示例格式化为字符串。\nexample_prompt = PromptTemplate(\ninput_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:\n{question}\\n{answer}&quot;\n)\nprint(example_prompt.format(**examples[0]))\nQuestion: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。\n因此答案是11。\nexample_prompt = &#x27;,
 &#x27;1。\nexample_prompt = PromptTemplate(\ninput_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:\n{question}\\n{answer}&quot;\n)\nprint(example_prompt.format(**examples[1]))\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少\n个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。\n最后，创建一个 FewShotPromptTemplate 对象。该对象接受经过上一步格式化后的少数样本示\n例。\n# 导入 FewShotPromptTemplate 类\nfrom langchain.prompts.few_shot import FewShotPromptTemplate\n# 创建一个 FewShotPromptTemplate 对象\nfew_shot_prompt = FewShotPromptTemplate(\nexamples=&#x27;,
 &#x27;tTemplate(\nexamples=examples, # 最开始定义的 examples 示例\nexample_prompt=example_prompt, # 上一步定义的 example_prompt 作为提示模板\nsuffix=&quot;Question: {input}&quot;, # 后缀模板，其中 {input} 会被替换为实际输入\ninput_variables=[&quot;input&quot;] # 定义输入变量的列表\n)\n# 使用给定的输入格式化 prompt，并打印结果\nprint(few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，\n水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;))\nQuestion: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。\n因此答案是11。\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少\n个苹果？&#x27;,
 &#x27;个苹果，请问现在食堂总共有多少\n个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有\n多少个蓝色高尔夫球？\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关\n闭之前她能滑多少次？\nfinal_input = few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟\n才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)\nfinal_input\n\&#x27;Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\\n罗杰\n一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此\n答案是11。\\n\\nQuestion: 食堂总共有23个苹果，如果他&#x27;,
 &#x27;tion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现\n在食堂总共有多少个苹果？\\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹\n果，答案是9。\\n\\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是\n蓝色的。请问总共有多少个蓝色高尔夫球？\\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是\n蓝色的，也就是4个，答案是4个。\\n\\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？\&#x27;\n将带有Few-Shot提示的输入，输入到大模型中执行推理。\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=final_input\n)\n\&#x27;\\n艾米需要4分钟爬到顶部，1分钟滑下来，一共花费5分钟。在15分钟内，她能滑3次，因为15÷5=3。因此\n答案是3次。&#x27;,
 &#x27;次，因为15÷5=3。因此\n答案是3次。\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=final_input\n)\n\&#x27;\\n艾米花了5分钟（4分钟爬上去+1分钟滑下来），因此在15分钟内可以滑3次（15÷5=3）。答案是3次。\&#x27;\n从输出结果来看，显然受到了提示工程的正面影响，通过问题的拆解过程显著提高了回答的准确\n性。\n适合Chat Model的提示模版构造形式\n我们在前面解释过LLMs和Chat Model 的区别，因其接受的输入形式不同，所以构造的提示模版也\n是不一样的，对于Chat Model来说，它接收的是聊天形式的对话格式，所以对于其对应的Few-Shot，\n在LangChain中的基本结构如下：\nexamples ：要包含在最终提示中的字典示例列表。\nexample_prompt ：通过其 format_messages 方法将每个示例转换为 1 条或多条消息。一个常\n见的示例是将每个示例转换为一条人工消息和一条人&#x27;,
 &quot;例是将每个示例转换为一条人工消息和一条人工智能消息响应，或者一条人工消息后跟一条\n函数调用消息。\n我们还是使用四个数学推理题来进行测试：\nprompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共\n有多少个网球？” \\\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网\n球，因此现在总共由5+6=11个网球。因此答案是11。” \\\nQ：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，\n请问现在食堂总共有多少个苹果？” \\\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9\n个苹果，答案是9。” \\\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫\n球是蓝色的。请问总共有多少个蓝色高尔夫球？” \\\nA：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色\n的，也就是4个，答案是4个。” \\\nQ：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在\n15分钟后关闭，请问在关闭之前她能滑多少次？” \\\nA：&#x27;\npro&quot;,
 &quot;之前她能滑多少次？” \\\nA：&#x27;\nprompt_Few_shot_CoT4\n&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在\n总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果\n他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色\n的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高\n尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：\n“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？” A：&#x27;\n直接调用\n对于Chat Model，需要将输入转化成对话聊天格式：\nfrom langchain_core.messages import Hu&quot;,
 &#x27;e.messages import HumanMessage, SystemMessage\nmessages = [SystemMessage(content=&quot;你是一个擅长数学推理的专家&quot;),\nHumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟\n后关闭，请问在关闭之前她能滑多少次？&quot;),]\n实例化模型进行推理：\nfrom langchain_openai import ChatOpenAI\nchat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)\nresonse = chat.invoke(messages)\nresonse.content\n\&#x27;首先，艾米花了1分钟滑下来，所以她每次滑下来后需要等待3分钟（4分钟上去 + 1分钟下来 = 5分钟，15\n分钟总时间 - 5分钟 = 10分钟，10分钟 / 3分钟 = 3次）。\\n\\n在水滑梯关闭之前，艾米可以滑3次。\&#x27;\n&amp;esmp; 实际上，在使用GPT-3.5时，我们可以观察到，即使不采用Few-Shot提示，模型也能以很高\n的&#x27;,
 &#x27;Few-Shot提示，模型也能以很高\n的概率正确回答问题，这归功于模型本身已经非常强大的能力。所以我们改为Baichuan2模型来测试：\nfrom langchain_community.chat_models import ChatBaichuan\nfrom langchain_core.messages import HumanMessage\nchat = ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-xxx&quot;,\nstreaming=True,\n)\nresponse = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])\nresponse.content\n\&#x27;艾米需要1分钟滑下来，所以她在滑梯关闭前可以滑15次。\&#x27;\nresponse = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少&#x27;,
 &#x27;15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])\nresponse.content\n\&#x27;艾米需要1分钟从滑梯顶部滑下来，所以她可以在滑梯关闭前滑15次。\&#x27;\nresponse = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])\nresponse.content\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，所以每次滑行需要4+1=5分钟。\\n\\n水滑梯将在\n15分钟后关闭，所以她能在关闭之前滑行15÷5=3次。\&#x27;\n可以看到，在切换到Baichuan模型进行测试时，三次测试中出现了两次错误回答。因此，我们就\n使用Baichuan2模型，尝试通过添加Few-Shot来探索是否能增强其推理能力。\n首先，安装LangChain的规范，导入生成示例的模块：\nfrom langchain.prompts import (\nChatPromptTemplate,\nFewShotChatMessagePromptTemplate,\n)\n接下来以input-outpu&#x27;,
 &#x27;e,\n)\n接下来以input-output对的形式构建Few-Shot提示模板。在这里，“input”可以被视作人类发出的\nPromot，而“output”则是大模型给出的回答。\nexamples = [\n{&quot;input&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网\n球？&quot;,\n&quot;output&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总\n共由5+6=11个网球。因此答案是11。&quot;},\n{&quot;input&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共\n有多少个苹果？&quot;,\n&quot;output&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是\n9。&quot;},\n{&quot;input&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问\n总共有多少个蓝色高尔夫球？&quot;,\n&quot;output&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答\n案是4个。&quot;},\n]\n然后，使用from_me&#x27;,
 &#x27;个。&quot;},\n]\n然后，使用from_messages方法进行模版构造。\n# This is a prompt template used to format each individual example.\nexample_prompt = ChatPromptTemplate.from_messages(\n[\n(&quot;human&quot;, &quot;{input}&quot;),\n(&quot;ai&quot;, &quot;{output}&quot;),\n]\n)\nfew_shot_prompt = FewShotChatMessagePromptTemplate(\nexample_prompt=example_prompt,\nexamples=examples,\n)\nprint(few_shot_prompt.format())\nHuman: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\nAI: 罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网\n球。因此答案是11。\nHuman: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有&#x27;,
 &#x27;，然后又买了6个苹果，请问现在食堂总共有多少个苹\n果？\nAI: 食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。\nHuman: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少\n个蓝色高尔夫球？\nAI: 总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。\n生成最终的输入模版，设置 input 作为动态变量：\nfinal_prompt = ChatPromptTemplate.from_messages(\n[\nfew_shot_prompt,\n(&quot;human&quot;, &quot;{input}&quot;),\n]\n)\nfinal_prompt\nChatPromptTemplate(input_variables=[\&#x27;input\&#x27;], messages=\n[FewShotChatMessagePromptTemplate(examples=[{\&#x27;input\&#x27;: \&#x27;罗杰有五个网球，他又买了两盒网\n球，每盒有3个网球，请问他现在总共有多少个网球？\&#x27;, \&#x27;output\&#x27;: \&#x27;罗杰一开始有五个网球&#x27;,
 &quot;output&#x27;: &#x27;罗杰一开始有五个网球，又购买了两\n盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此答案是11。&#x27;}, {&#x27;input&#x27;:\n&#x27;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？&#x27;,\n&#x27;output&#x27;: &#x27;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是\n9。&#x27;}, {&#x27;input&#x27;: &#x27;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。\n请问总共有多少个蓝色高尔夫球？&#x27;, &#x27;output&#x27;: &#x27;总共有16个球，其中一半是高尔夫球，也就是8个，其中\n一半是蓝色的，也就是4个，答案是4个。&#x27;}],\nexample_prompt=ChatPromptTemplate(input_variables=[&#x27;input&#x27;, &#x27;output&#x27;], messages=\n[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],\ntemplate=&#x27;{inp&quot;,
 &#x27;ut\&#x27;],\ntemplate=\&#x27;{input}\&#x27;)),\nAIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[\&#x27;output\&#x27;],\ntemplate=\&#x27;{output}\&#x27;))])),\nHumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[\&#x27;input\&#x27;],\ntemplate=\&#x27;{input}\&#x27;))])\n实例化baichuan2模型：\n最后，使用LangChain中的chain的抽象，合并最终的提示和大模型共同执行。\nchain = final_prompt | ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-e9&quot;,\nstreaming=True,\n)\nresponse = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水\n滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})\nresponse.content\n\&#x27;艾&#x27;,
 &#x27;\nresponse.content\n\&#x27;艾米需要4分钟才能爬上去，1分钟滑下来，总共需要4+1=5分钟。15分钟可以滑15÷5=3次，所以答案是3\n次。\&#x27;\nchain = final_prompt | ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-ed54e9529&quot;,\nstreaming=True,\n)\nresponse = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水\n滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})\nresponse.content\n\&#x27;艾米爬上去需要4分钟，滑下来只需要1分钟，所以她可以在15分钟内完成4次滑行，因为每次滑行需要\n4+1=5分钟。\&#x27;\nchain = final_prompt | ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-ed54695e9529&quot;,\nstreaming=True,\n)\nresponse = chain.invoke({&quot;input&quot;&#x27;,
 &#x27;hain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水\n滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})\nresponse.content\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，然后1分钟滑下来，总共需要4+1=5分钟。因此，在滑梯关闭之前，她可\n以滑15÷5=3次。\&#x27;\n4.3 LangChain中的Output Parsers\nOutput Parsers，即输出解析器，这个概念非常好理解，就是负责获取大模型的输出并将其转换为\n更合适的格式。这在应用开发中及其重要。在大多数复杂应用场景中，处理逻辑往往环环相扣，执行某\n项业务逻辑可能需要多次调用大模型，其中上一次的调用结果将被用于指导下一次调用的逻辑。在这种\n情况下，结构化的信息会比纯文本又有价值，同时这也是输出解析器的价值所在。\nLangChain构造的输出解释器必须实现两个主要方法：\nGet format instructions：该方法会返回一个字符串，其中包含有关如何格式化语言模型输出的指\n令。\nParse：该方法会接收字符串，并将其解析为某种结构\n目前已经支持的解析格式已&#x27;,
 &#x27;解析为某种结构\n目前已经支持的解析格式已经包括Json、Xml、Csv以及OpenAI的Tools和Functions等多种格式，\n具体可看：https://python.langchain.com/docs/modules/model_io/output_parsers/\n简而言之，这个过程本质上是对文本的格式化处理。但与传统的后处理不同，LangChain采取的实\n现策略是通过在Prompt上增加输出解析，直接引导大模型输出的格式化方式。\n我们以格式化日期来看一下LangChain的构造思路，首先导入相关的模块：\nfrom langchain.output_parsers import DatetimeOutputParser\nfrom langchain.prompts import PromptTemplate\nfrom langchain_openai import OpenAI\n实例化日期解析器：\noutput_parser = DatetimeOutputParser()\n通过.get_format_instructions方法，可以查看定义的解析格式：\noutput_p&#x27;,
 &#x27;以查看定义的解析格式：\noutput_parser.get_format_instructions()\n&quot;Write a datetime string that matches the following pattern: \&#x27;%Y-%m-\n%dT%H:%M:%S.%fZ\&#x27;.\\n\\nExamples: 1181-01-05T02:20:33.675873Z, 1938-12-\n13T09:46:36.791229Z, 1468-01-17T07:34:51.108688Z\\n\\nReturn ONLY this string, no\nother words!&quot;\n构造输入模版，这里的区别是：在输入的Prompt Template中，加入了OutPut Parse的内容：\ntemplate = &quot;&quot;&quot;用户发起的提问:\n{question}\n{format_instructions}&quot;&quot;&quot;\n生成最终实际输入大模型的Promot内容：\nprompt = PromptTemplate.from_template(\ntemplate,\n# 预定义的变量，这里我们传入格式化指令\npartial_va&#x27;,
 &#x27;我们传入格式化指令\npartial_variables={&quot;format_instructions&quot;:\noutput_parser.get_format_instructions()},\n)\nprompt\nPromptTemplate(input_variables=[\&#x27;question\&#x27;], partial_variables=\n{\&#x27;format_instructions\&#x27;: &quot;Write a datetime string that matches the following\npattern: \&#x27;%Y-%m-%dT%H:%M:%S.%fZ\&#x27;.\\n\\nExamples: 1472-10-23T17:39:56.199188Z,\n1643-01-14T10:55:11.903091Z, 0631-12-11T23:04:04.345333Z\\n\\nReturn ONLY this\nstring, no other words!&quot;}, template=\&#x27;用户发起的提\n问:\\n\\n{question}\\n\\n{format_instructions}\&#x27;)\n最后，使用LangChain中的&#x27;,
 &#x27;}\&#x27;)\n最后，使用LangChain中的chain的抽象，合并最终的提示、大模型实例及OutPut Parse共同执\n行。\nchain = prompt | OpenAI() | output_parser\noutput = chain.invoke({&quot;question&quot;: &quot;你好，请问你叫什么？&quot;})\nprint(output)\n2021-10-07 12:02:13.345333\nchain = prompt | OpenAI() | output_parser\noutput = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是机器学习&quot;})\nprint(output)\n2018-01-23 09:42:32.123456\nchain = prompt | OpenAI() | output_parser\noutput = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是深度学习&quot;})\nprint(output)\n2019-08-07 18:30:00\n通过测试可以明显观察到，以此方式定义的输出解析器极大地规范了&#x27;,
 &#x27;到，以此方式定义的输出解析器极大地规范了模型的输出。当然，这也直接\n受到大模型实际能力和模板定义情况的影响。\nLangChain中不同模版的使用方式，除了可以在官方文档上看到，在其源码上看会更加直观，而且\n可以自定义修改，其所有的OutPut Parse的模版存储路径为：\nlangchain/libs/langchain/langchain/output_parsers ， 大家可以自行尝试。比如我们上面使\n用的日期解析的源码对应如下：\n到此为止，我们已经非常详尽地介绍了LangChain的核心概念。如果有学习过大模型技术实战课程\n的同学来说，会发现LangChain的Model I/O模块中讨论的所有概念都是之前接触过的，需要学习的主\n要是LangChain对大模型进行抽象的方法以及在LangChain框架下的应用开发技巧。\n而从LangChain的整体开源情况看，其内容丰富且迭代非常快，通过模块化的方式介绍，可以帮助\n大家快速构建起对LangChain以及应用开发的思维框架。尽管在Model I/O模块中还有许多更高级的内\n容，例如如何接入本地部署的开源大模型，关于函数调用、Mem&#x27;,
 &#x27;地部署的开源大模型，关于函数调用、Memory缓存等更深入的主题，这些内容我\n们将在后续章节中一一接触并进行详细的探讨。\n&#x27;]
```

在大多数情况下，结合overlap windows的固定大小的分块是一种更加经济且易于使用的方式。因为它在分块过程中不需要使用任何NLP库或者模型等。

如上所示的前三种方法涉及的是对数据静态字符的切分，这些方法基本上只会将文本分割成固定数量字符的片段，而不考虑其内容或结构。这些是最基本且相对简单实现的文本切分方式。

我们可以用LangChain提供的文本切分可视化小工具进行直观的理解：https://langchain-text-splitter.streamlit.app/

![](img/043dd4b6b269c211c512a5f8be0ef3fb.png)

> 关于更多的检索生成前沿技巧，《大模型RAG技术企业项目实战课》（首期）正式课程中会给大家详细梳理目前主流的RAG核心结构、适用场景和具体实现的代码，助力大家能够根据自己的实际工作情况快速的选定有效的RAG技术策略。

## 3.3 文本向量化表示

计算机本质上无法直接理解自然语言。自然语言是非常复杂且充满细微差别的，而计算机则依赖于数学和逻辑运算。因此，我们需要将自然语言转化为计算机可以理解和操作的形式——这就是嵌入向量（Embeddings）的作用。特别是在计算句子之间的相似度时。

```
from openai import OpenAI
client = OpenAI()

def get_embeddings(text):
    
    # 向OpenAI API请求生成嵌入向量
    response = client.embeddings.create(
        input=text,
        model=&#x27;text-embedding-3-small&#x27;,  # 指定使用的模型
    )
    
    # 从响应中提取嵌入向量并返回
    embeddings = [data.embedding for data in response.data]
    return embeddings
```

```
# 获取单个查询的嵌入向量
single_query = get_embeddings(&quot;测试生成Embedding&quot;)

# 打印嵌入向量的维度，用中文显示
print(&quot;嵌入向量的维度长度为:&quot;, len(single_query[0]))
```

```
嵌入向量的维度长度为: 1536
```

```
# 获取多个文本片段的嵌入向量
multi_docs = get_embeddings([&quot;我是chunks 1的文本内容&quot;, &quot;我是chunks 2的文本内容&quot;])

# 打印获取的嵌入向量组的数量和第一个嵌入向量的维度
print(&quot;嵌入向量组的数量:&quot;, len(multi_docs), &quot;第一个嵌入向量的维度长度为:&quot;, len(multi_docs[0]))
```

```
嵌入向量组的数量: 2 第一个嵌入向量的维度长度为: 1536
```

接下来生成PDF文档中 每个 Chunks的向量，我们要对每一个文本片段（chunk）进行嵌入向量（embedding）的生成，并将原始文本、嵌入向量及片段编号一起存储为字典格式，代码如下：

```
# 定义每个片段的长度和重叠长度
chunk_length = 500
overlap_length = 20

# 调用函数进行分割
chunks = split_text_by_fixed_length_with_overlap(extracted_text, chunk_length, overlap_length)

chunks
```

```
[&#x27;开源大模型应用开发实战\nCh 18 LangChain整体项目介绍与核心模块Model I/O详\n解\n注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情\n况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，\n将迅速更新教材内容以确保信息的准确性和实用性。\n本期课件更新时间：2024年3月7日\n在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地\n私有化部署开源大模型。在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家\n关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部\n署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部\n署。\n但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也\n是最简单的一步。熟练掌握基于大模型的上层应用开发还有非常长的路需要走。目前，大模型技术开发\n总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API&#x27;,
 &#x27;基于OpenAI、GLM4这种在线API的应用开发，而另一类，就是先私有化\n部署开源大模型，再借助开发框架构建大模型应用。当然了，目前主流的大模型应用开发框架也支持在\n线模型API的调用，这正是我们之前一直强调学习掌握开源或在线API调用技巧的原因，因为这些方法和\n技巧直接对应于应用开发的最佳实践。\n而不论采用哪种开发形式，其基座和所依赖的，核心还是大模型本身。对于在线大模型，以\nOpenAI为例，开发者需要配置科学上网的环境，在其官网创建 API Key，最后遵循API的使用说明去进\n行应用配置。这一过程中涉及到的网络环境适配，数据隐私安全，成本管理及如何去定制化自己需求的\n开发流程，往往都需要遵循官方提供的功能来实现。也正是因为这些复杂的因素使得尽管目前公认最强\n的大语言模型GPT 4开放给开发者使用，还是会有非常多的公司和个人去拥抱开源，选择性能可能略逊\n一筹的开源大模型，因为开源方案在某些方面提供了更多的灵活性和自主控制能力。\n大家也能够发现，在我们的课程中每当涉及到大模型的私有化部署，我们都会向大家展示如何通过\n命令行、网页端访问和API调用等三种方式与部署的大模型进行交互。&#x27;,
 &#x27;I调用等三种方式与部署的大模型进行交互。此外，还通过text-generate-\nwebui这样的开源项目，这一切应用是帮助大家能够以最简单、最直接的方式与开源大模型进行对话，\n了解大模型的运行模型。但实际上，无论是基于大模型去做任何形式的应用开发过程，仅停留在这个阶\n段肯定无法满足实际的需求。虽然目前的大模型基本都可以按照OpenAI的API规范形式进行调用，但如\n果进行实操我们会发现，在《大模型技术实战》的课程中，借助OpenAI的GPT系列模型开发的项目，\n如智能邮件处理、全自动AI开发流程，以及MateGen Agents等，尽管在开源模型上能够被复现，但这\n背后的成功关键并非仅仅在于模型调用。例如，在《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进\n行外部工具API调用开发实践》中，我们通过Qwen模型复现了《Ch.10 借助Function calling调用外部\n工具API方法》中的天气信息调用过程。这一复现过程揭示了，对在线模型来说，我们需要学习的是其\nAPI使用方法。而在开源大模型的技术领域下，要想实现某些特定功能，重点在于理解该模型的一些使\n用原理，以及&#x27;,
 &#x27;重点在于理解该模型的一些使\n用原理，以及如何调整和构建tools与Schema表示。因此，整个开发过程中花费的大部分时间并非直接\n与模型调用相关，而是在于深入理解和应用这些底层原理和技术细节。\n换言之，在构建大模型应用的过程中，虽然大模型是核心，但实际针对大模型编写的代码量相对较\n小。而在这些有限的代码中，提示工程又往往会占据主要的工作量。这进一步引出一个关键问题：除了\n大模型本身，其他各环节如何高效串联，以及这些环节的代码开发和工作量如何分配和执行，将在很大\n程度上影响最终应用的用户体验。\n在这种情况下，解决这一窘境且广受认可和高度实用的解决方案，便是LangChain。\n1. 什么是LangChain\nLangChain现在归属于LangChain AI公司，LangChain作为其中的一个核心项目，开源发布在\nGitub上：https://github.com/langchain-ai/langchain\n从LangChain的GitHub版本迭代历史上看，从2023年1月16日起已经经历了320个大小版本的迭\n代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度&#x27;,
 &#x27;加速项目的功能上线，从整体上看，其关注度和社区活跃度是非常高\n的。LangChain给自身的定位是：用于开发由大语言模型支持的应用程序的框架。它的做法是：通过提\n供标准化且丰富的模块抽象，构建大语言模型的输入输入规范，利用其核心概念chains，灵活地连接\n整个应用开发流程。而针对每个功能模块，都源于对大模型领域的深入理解和实践经验，开发者提供出\n来的标准化流程和解决方案的抽象，再通过灵活的模块化组合，才有了目前这样一款在大模型应用开发\n领域内被普遍高度认可的通用框架。\n为什么需要这样做？\n首先，我们需考虑当前大模型的发展态势。尽管OpenAI的GPT系列模型作为大模型领域的领军人\n物，在很大程度上了影响了大模型的使用规范和基于大模型进行应用开发的范式，但并不意味着所有大\n模型间的使用方式完全相同。例如，我们熟悉的OpenAI GPT模型API调用方式对于Baichuan2模型就不\n适用。因此，对于每个新模型都要花费大量时间学习其特定规范再进行应用探索，这种工作效率显然是\n十分低下的。\n其次，必须谈论的是大模型目前面临的局限，如知识更新的滞后性、外部API调用能力、私有数据\n连接方式以&#x27;,
 &#x27;外部API调用能力、私有数据\n连接方式以及输出结果的不稳定性等问题。在应用开发中，如何找到这些问题的有效解决策略？\n上述提到的每个限制都紧密关联于大模型本身的特性。尽管理论上可以通过重新训练、微调来增强\n模型的原生能力，这种方法确实有效，但实际上，大多数开发者并不具备进行这样操作所需的技术资\n源、时间和财力，选择这条路径一定会导致方向越来越偏离目标。我们之前讨论的OpenAI的Function\nCalling和通过ReAct流程接入第三方API能够提供一些解决方案，但这每一步都需大量的研发投入，而且\n最终实现后的应用效果，也取决于研发人员的个人技术能力。在这种背景下，既然大家都有不同的想法\n和解决方案，那LangChain就来集中做这件事，提供一个统一的平台和明确的定义，来实现应用框架的\n快速搭建，这就是LangChain一直想要做到，且正在做的事情。\nLangChain的做法\n从本质上分析，LangChain还是依然采用从大模型自身出发的策略，通过开发人员在实践过程中对\n大模型能力的深入理解及其在不同场景下的涌现潜力，使用模块化的方式进行高级抽象，设计出统一接\n口以适配各种大模型。到&#x27;,
 &#x27;象，设计出统一接\n口以适配各种大模型。到目前为止，LangChain抽象出最重要的核心模块如下：\n1. Model I/O ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；\n2. Retrieval ：检索外部数据，然后在执行生成步骤时将其传递到 LLM，包括文档加载、切割、\nEmbedding等；\n3. Chains ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多\n功能的高级抽象；\n4. Memory ： 记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；\n5. Agents ： 目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；\n6. Callbacks ：回调系统，允许连接到 LLM 应用程序的各个阶段。用于日志记录、监控、流传输和\n其他任务；\n从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文\n记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的&quot;Chain&quot;高级抽象，定义了\n不同形式下标准的链接方法，这就能够允许开发者&#x27;,
 &#x27;形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整\n的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。\n也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和\n使用方法，那么如何去搭这个“积木”，就需要我们对其每个核心模块都要有一个比较清楚的认知。所以\n我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实\n操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业\n务情况，选择合适的构造模块和构造方法。\n在整体上理解了LangChain之后，我们首先从Model I/O模块进行深入的探讨和实践。\n2. LangChain核心模块：Model I/O\nLangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的\nModel I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就\n是通过该模块，我们可以快速与某个大模型进行对话交互，整个&#x27;,
 &#x27;们可以快速与某个大模型进行对话交互，整个内部逻辑就相当于我们最熟悉的这个过\n程：输入Prompt，得到大模型针对该Prompt的推理结果。如下示例为OpenAI的 GPT 系列模型的API\n调用规范：\nresponse = openai.ChatCompletion.create(\nmodel=&quot;gpt-3.5-turbo&quot;,\nmessages=[\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;请问，什么是机器学习？&quot;}\n]\n)\n而我们前面提到了，LangChain项目的定位为一个应用开发框架，所以如果仅仅集成到这样的对话\n交互程度，那相较于直接使用OpenAI的API调用又有何异呢？所以在这个模块中，LangChain同样抽象\n出一个chain，用于进一步简化和增强交互流程。在LangChain的Model I/O模块设计中，包含三个核\n心部分： Prompt Template（对应下图中的Format部分）， Model（对应下图中的Predict部分） 和\nOutput Parser（对应下图中的Parse部分）。\nFormat：即指代Prompts Template&#x27;,
 &#x27;：即指代Prompts Template，通过模板化来管理大模型的输入；\nPredict：即指代Models，使用通用接口调用不同的大语言模型；\nParse：即指代Output部分，用来从模型的推理中提取信息，并按照预先设定好的模版来规范化\n输出。\n整个Model I/O工作流如下图所示：\nFormat\n对于Prompt Template第一部分，传统上我们创建提示词是通过手工编写来实现的，在这个过程中\n会利用各种提示工程技巧，如Few-Shot、链式推理（CoT）等方法，以提高大模型的推理性能。然而，\n在应用开发中，一个关键的考量是提示词不能是一成不变的。其原因在于，应用开发需要适应多变的用\n户需求和场景。固定的提示词限制了模型的灵活性和适用范围。例如，如果我们正在开发一个天气查询\n应用，用户可能会以多种方式提出查询，如“今天的天气怎么样？”或“明天纽约的温度是多少度？”。如\n果提示词是固定的，它可能只能处理一种特定类型的查询，而无法适应这种多样性的需求。\n而Prompt Template，就像我们在介绍《Ch.13 Qwen模型的ReAct原理介绍及流程拆解，进行外\n部工具API调&#x27;,
 &#x27;理介绍及流程拆解，进行外\n部工具API调用开发实践》中不断优化出来的ReAct模版，将API的使用、问题解答过程等复杂逻辑封装\n成了一套结构化的格式。我们只需准备具体的外部函数信息和用户查询，即可生成定制化的提示词，引\n导模型按照既定逻辑进行思考和回答，从而实现外部函数的调用过程，即：\n# 将一个插件的关键信息拼接成一段文本的模版。\nTOOL_DESC = &quot;&quot;&quot;{name_for_model}: Call this tool to interact with the\n{name_for_human} API. What is the {name_for_human} API useful for?\n{description_for_model} Parameters: {parameters}&quot;&quot;&quot;\n# ReAct prompting 的 instruction 模版，将包含插件的详细信息。\nPROMPT_REACT = &quot;&quot;&quot;Answer the following questions as best you can. You have\naccess to the followin&#x27;,
 &#x27;cess to the following APIs:\n{tool_descs}\nUse the following format:\nQuestion: the input question you must answer\nThought: you should always think about what to do\nAction: the action to take, should be one of [{tool_names}]\nAction Input: the input to the action\nObservation: the result of the action\n... (this Thought/Action/Action Input/Observation can be repeated zero or more\ntimes)\nThought: I now know the final answer\nFinal Answer: the final answer to the original input question\nBegin!\nQuestion: &#x27;,
 &#x27;on\nBegin!\nQuestion: {query}&quot;&quot;&quot;\n因此，引入Prompt Template可以支持变量和动态内容的插入，使得同一个应用可以根据不同的输\n入动态调整提示词，从而更好地响应用户的具体需求。LangChain通过这种方式来提高应用的通用性和\n用户体验。\nPredict\n在Predict部分，实质上是处理模型从接收输入到执行推理的整个过程。考虑到存在两种主要类型的\n大模型——Base类模型和Chat类模型，LangChain在其Model I/O模块中对这两种模型都进行了抽象，\n分别归类为LLMs（Large Language Models）和Chat Models。我们还是以OpenAI 的 Completion 和\nChatcompletions方法为例：\n# Base类模型\nclient.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=&quot;Say this is a test&quot;,\n)\n# 聊天模型\nclient.chat.completions.create(\nmodel=&quot;gpt-3.5-t&#x27;,
 &#x27;te(\nmodel=&quot;gpt-3.5-turbo&quot;,\nmessages=[\n{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}\n]\n)\nLLMs是简化的大语言模型抽象，即基于给定的Prompt提供内容生成的功能。而Chat Models则专\n注于聊天API的抽象，需要维护上下文的记忆（聊天记录），呈现出更接近对话或聊天形式的交互。\nParse\n我们知道，大模型的输出是不稳定的，同样的输入Prompt往往会得到不同形式的输出。在自然语\n言交互中，不同的语言表达方式通常不会造成理解上的障碍。但在应用开发中，大模型的输出可能是下\n一步逻辑处理的关键输入。因此，在这种情况下，规范化输出是必须要做的任务，以确保应用能够顺利\n进行后续的逻辑处理。\n输出解析器 Output Parser就是一个帮助结构化语言模型响应的抽象，可以获取格式指令或者进行\n更深层次的解析。这我们会在后面的实践中直观的体验到。\n整体而言，在Model I/O模块的抽象中，其一能够让开发者&#x27;,
 &#x27;l I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如\nOpenAI、ChatGLM、Qwen等，按照既定规范执行模型推理。其二通过输入和输出的模板化处理，使\n其更贴合于应用开发的最佳实践。接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行\n集成和操作的。\n3 安装LangChain\nLangChain的安装过程非常简单，可以通过常用的Python包管理工具，如pip或conda，直接进行\n安装。稍复杂一点的还可以通过源码进行安装。但有一点大家一定要明确：LangChain的真正价值在于\n它能够与多种模型提供商、数据存储解决方案等进行集成。默认情况下，使用上述两种安装方式中的任\n意一种来进行LangChain安装后，安装的仅仅是LangChain的默认功能，并不包括这些集成所需的额外\n依赖项。也就是说，如果我们想要使用特定的集成功能，还需要额外安装这些特定的依赖。以调用\nOpenAI的API为例，我们首先需要通过运行命令pip install langchain-openai安装OpenAI的合作\n伙伴包，安装此依赖包后，LangChain才能够与Ope&#x27;,
 &#x27;赖包后，LangChain才能够与OpenAI的API进行交互。后续我们在使用相关功能的时\n候，会提供额外的说明。\nLangChain安装官方说明文档：https://python.langchain.com/docs/get_started/installation\n3.1 使用包版本管理工具安装\nLangChain可以使用pip 或者 conda直接安装，适用于仅使用的场景，即不需要了解其源码构建\n过程。这种安装方法十分简洁明了，只需执行一条命令，就可以在当前的虚拟环境中迅速完成\nLangChain的安装。具体操作如下：\n! pip install langchain\nRequirement already satisfied: langchain in d:\\software\\anaconda3\\lib\\site-\npackages (0.1.10)\nRequirement already satisfied: PyYAML&gt;=5.3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (6.0)\nRequir&#x27;,
 &#x27;gchain) (6.0)\nRequirement already satisfied: SQLAlchemy&lt;3,&gt;=1.4 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (1.4.39)\nRequirement already satisfied: aiohttp&lt;4.0.0,&gt;=3.8.3 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (3.8.5)\nRequirement already satisfied: dataclasses-json&lt;0.7,&gt;=0.5.7 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.6.4)\nRequirement already satisfied: jsonpatch&lt;2.0,&gt;=1.33 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (1.33)&#x27;,
 &#x27;om langchain) (1.33)\nRequirement already satisfied: langchain-community&lt;0.1,&gt;=0.0.25 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.0.25)\nRequirement already satisfied: langchain-core&lt;0.2,&gt;=0.1.28 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.1.28)\nRequirement already satisfied: langchain-text-splitters&lt;0.1,&gt;=0.0.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain) (0.0.1)\nRequirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in\nd:\\software\\anaconda3\\li&#x27;,
 &#x27;oftware\\anaconda3\\lib\\site-packages (from langchain) (0.1.14)\nRequirement already satisfied: numpy&lt;2,&gt;=1 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (1.24.3)\nRequirement already satisfied: pydantic&lt;3,&gt;=1 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (2.6.0)\nRequirement already satisfied: requests&lt;3,&gt;=2 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain) (2.31.0)\nRequirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in\nd:\\software\\anaconda3\\lib\\site-packag&#x27;,
 &#x27;nda3\\lib\\site-packages (from langchain) (8.2.2)\nRequirement already satisfied: attrs&gt;=17.3.0 in d:\\software\\anaconda3\\lib\\site-\npackages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (22.1.0)\nRequirement already satisfied: charset-normalizer&lt;4.0,&gt;=2.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(2.0.4)\nRequirement already satisfied: multidict&lt;7.0,&gt;=4.5 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(6.0.2)\nRequirement already s&#x27;,
 &#x27;equirement already satisfied: async-timeout&lt;5.0,&gt;=4.0.0a3 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(4.0.2)\nRequirement already satisfied: yarl&lt;2.0,&gt;=1.0 in d:\\software\\anaconda3\\lib\\site-\npackages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain) (1.8.1)\nRequirement already satisfied: frozenlist&gt;=1.1.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(1.3.3)\nRequirement already satisfied: aiosignal&gt;=1.1.2 in\nd:\\software\\anaconda3\\&#x27;,
 &#x27;\\software\\anaconda3\\lib\\site-packages (from aiohttp&lt;4.0.0,&gt;=3.8.3-&gt;langchain)\n(1.2.0)\nRequirement already satisfied: marshmallow&lt;4.0.0,&gt;=3.18.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-\n&gt;langchain) (3.21.0)\nRequirement already satisfied: typing-inspect&lt;1,&gt;=0.4.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from dataclasses-json&lt;0.7,&gt;=0.5.7-\n&gt;langchain) (0.9.0)\nRequirement already satisfied: jsonpointer&gt;=1.9 in\nd:\\software\\anaconda3\\lib\\site-packages (from json&#x27;,
 &#x27;-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain)\n(2.1)\nRequirement already satisfied: anyio&lt;5,&gt;=3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (3.5.0)\nRequirement already satisfied: packaging&lt;24.0,&gt;=23.2 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2,&gt;=0.1.28-\n&gt;langchain) (23.2)\nRequirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in\nd:\\software\\anaconda3\\lib\\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-\n&gt;langchain) (3.9.15)\nRequir&#x27;,
 &#x27;ain) (3.9.15)\nRequirement already satisfied: annotated-types&gt;=0.4.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (0.6.0)\nRequirement already satisfied: pydantic-core==2.16.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain)\n(2.16.1)\nRequirement already satisfied: typing-extensions&gt;=4.6.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain) (4.7.1)\nRequirement already satisfied: idna&lt;4,&gt;=2.5 in d:\\software\\anaconda3\\l&#x27;,
 &#x27;software\\anaconda3\\lib\\site-\npackages (from requests&lt;3,&gt;=2-&gt;langchain) (3.4)\nRequirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)\n(1.26.16)\nRequirement already satisfied: certifi&gt;=2017.4.17 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain)\n(2023.11.17)\nRequirement already satisfied: greenlet!=0.4.17 in\nd:\\software\\anaconda3\\lib\\site-packages (from SQLAlchemy&lt;3,&gt;=1.4-&gt;langchain)\n(2.0.1)\nRequirement &#x27;,
 &#x27;(2.0.1)\nRequirement already satisfied: sniffio&gt;=1.1 in d:\\software\\anaconda3\\lib\\site-\npackages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2,&gt;=0.1.28-&gt;langchain) (1.2.0)\nRequirement already satisfied: mypy-extensions&gt;=0.3.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from typing-inspect&lt;1,&gt;=0.4.0-\n&gt;dataclasses-json&lt;0.7,&gt;=0.5.7-&gt;langchain) (1.0.0)\n注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。\n重启完当前的Jupyter Lab后，验证LangChain的安装情况，执行命令如下：\nimport langchain\nprint(langchain.__version__)\n0.1.10\n如果能正常输出LangChain&#x27;,
 &#x27;.10\n如果能正常输出LangChain的版本，说明在当前环境下的安装成功。\n3.2 源码安装\n除了通过pip安装外，还有一种通过源码安装的方法。这需要使用git拉取远程仓库，然后进入项目\n文件夹并执行pip install -e .命令。这种方法不仅会安装必要的依赖，同时也将程序的源代码保存\n在本地。通常来说，除非大家打算作为LangChain的协作开发者参与到项目的功能更新中，否则没必要\n采用这种安装方式，直接使用pip安装更为简便。\n对于课程学习而言，我们推荐采用源码安装方式，这将非常有助于在后续的LangChain功能探索\n中，通过源码分析深入理解框架的构建原理和详细机制。\n接下来我们以Ubuntu操作系统为例，介绍源码安装LangChain的详细步骤：\nStep 1. 安装Anaconda\n如果本地服务器是Ubuntu系统，请参考：《Ch.4 在Ubuntu 22.04系统下部署运行ChatGLM3-6B\n模型》中的2.3 安装Anaconda环境小节；而如果是Windows操作系统，请参考：《Ch.6 在Windows\n系统下部署运行ChatGLM3-6B模型》中的3.2.&#x27;,
 &#x27;ChatGLM3-6B模型》中的3.2.1 方式一：使用Anaconda创建项目依赖环境（推荐）小节\n中，执行完Step 6. 验证conda步骤，回到本文继续执行后续的操作：\n注：如果部署环境是租赁的云服务器，Anaconda环境已经预设好，并不需要我们再次手动配置。\n按照对应的教程内容配置好Anaconda后，可以通过如下命令验证当前环境的Conda：\nconda --version\n如果可以正常输出Conda的版本，则说明Anaconda安装成功。\nStep 2. 使用Conda创建LangChain的Python虚拟环境\n安装好Anaconda后，我们需要借助Conda包版本工具，为LangChain项目创建一个新的Python虚\n拟运行环境，执行代码如下：\nconda create --name langchain python==3.11\n如上所示，新创建了一个名为langchain的Python虚拟环境，其Python版本为3.11。创建完成后，\n通过如下命令进入该虚拟环境，执行后续的操作：\nconda activate langchain\nStep 3. 下载LangC&#x27;,
 &#x27;hain\nStep 3. 下载LangChain的项目文件\n进入LangChain的官方Github，地址：https://github.com/langchain-ai/langchain ， 在 GitHub\n上将项目文件下载到有两种方式：克隆 (Clone) 和 下载 ZIP 压缩包。推荐使用克隆 (Clone)的方式。我\n们首先在GitHub上找到其仓库的URL。\n在执行命令之前，需要先安装git软件包，执行命令如下：\nsudo apt install git\n执行克隆命令，将LangChain Github上的项目文件下载至本地的当前路径下，如下：\ngit clone https://github.com/langchain-ai/langchain.git\nStep 4. 升级pip版本\n建议在执行项目的依赖安装之前升级 pip 的版本，如果使用的是旧版本的 pip，可能无法安装一些\n最新的包，或者可能无法正确解析依赖关系。升级 pip 很简单，只需要运行命令如下命令：\npython -m pip install --upgrade pip\nStep 5. 源码安装项目依赖&#x27;,
 &#x27;pip\nStep 5. 源码安装项目依赖\n不同于我们之前一直使用的pip install -r requirements.txt方式，这种方法用于批量安装多\n个依赖包，是在部署项目或确保开发环境与其他开发者/环境一致时的常用方式。而对于LangChain，我\n们需要使用pip install -e的方式，以可编辑模式安装包。这种方式主要用于开发过程中。当以可编\n辑模式安装一个包时，依赖包会被直接从源代码所在位置安装，而不是复制到Python的site-packages\n目录，是开发模式下用于安装并实时反映对本地包更改的方法。需要执行的步骤如下：\ncd langchain/libs/langchain/\npip install -e .\n如在安装过程未发生任何报错，则说明安装成功。在安装完依赖后，我们就正式进入LangChain的\nModel I/O模块的实践。\n注：如果采用的是源码安装，需要参考《Ch.11 大模型开发工具Transformers库的使用及配置大\n模型开发环境》中远程服务器部署大模型，本地Jupyter NoteBook加载小节，在LangChain的虚拟\n环境下启动J&#x27;,
 &#x27;在LangChain的虚拟\n环境下启动Jupyter Lab，才能正常执行接下来的实践。\n4.Models I/O模块实战\n任何语言模型应用的核心都是大语言模型（LLMs），无论涉及到提示模板（Prompt Template）\n还是输出解析（OutPut Parse），其核心还是在于大语言模型（LLMs）。因此，在讨论和实践Model\nI/O模块时，首先应当关注如何集成这些大模型。只有成功集成后才能深入探讨和实践如何有效地设计\n提示模板和解析模型输出。所以我们先要实践的是：如何借助LangChain框架使用不同的大模型。\n4.1 LangChain接入大模型的方法\nLangChain 提供了一套与任何大语言模型进行交互的标准构建模块。所以需要明确的一点是：虽然\nLLMs 是 LangChain 的核心元素，但 LangChain 本身不提供 LLMs，它仅仅是为多种不同的 LLMs 进\n行交互提供了一个统一的接口。简单理解：以OpenAI的GPT系列模型为例，如果我们想通过\nLangChain 接入 OpenAI 的 GPT 模型，我们需要在LangChain框架下先定义相关的类和方法&#x27;,
 &#x27;ngChain框架下先定义相关的类和方法来规定如\n何与模型进行交互，包括数据的输入和输出格式以及如何连接到模型本身。然后按照 OpenAI GPT 模型\n的接口规范来集成这些功能。通过这种方式，LangChain 充当一个桥梁，使我们能够按照统一的标准来\n接入和使用多种不同的大语言模型。\n在LangChain的Model I/O模块中，集成了两类模型，分别是LLMs和聊天模型，这两种模型在输\n入和输出的数据类型上有所区别。\n接下来我们就以OpenAI的API为示例，分别实践一下在LangChain框架中如何使用这两种不同类型\n的大语言模型。首先来看LLMs类模型。\n4.1.1 LLMs类模型接入方法\nLLMs指的是纯文本补全模型，其封装的 API 输入接收的是字符串，输出也是字符串。 OpenAI 的\nGPT-3 是作为 LLM 实现的。最简单的理解：LLMs指代的就是Base类模型，这类模型仅仅可以理解并\n生成自然语言或代码，并未按照特定指令进行训练。截止到2024年3月07日，OpenAI可支持调用的\nBase类模型仅有2个，分别是babbage-002和davinci-002，除&#x27;,
 &#x27;ge-002和davinci-002，除此之外，还可以使用gpt-3.5-turbo-\ninstruct调用OpenAI 的 Completion方法。\n首先需要安装OpenAI的集成依赖包langchain-openai，执行如下命令：\n! pip install langchain-openai\nRequirement already satisfied: langchain-openai in\nd:\\software\\anaconda3\\lib\\site-packages (0.0.8)\nRequirement already satisfied: langchain-core&lt;0.2.0,&gt;=0.1.27 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-openai) (0.1.28)\nRequirement already satisfied: openai&lt;2.0.0,&gt;=1.10.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-o&#x27;,
 &#x27;es (from langchain-openai) (1.13.3)\nRequirement already satisfied: tiktoken&lt;1,&gt;=0.5.2 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-openai) (0.6.0)\nRequirement already satisfied: PyYAML&gt;=5.3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (6.0)\nRequirement already satisfied: anyio&lt;5,&gt;=3 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.5.0)\nRequirement already satisfied: jsonpatch&lt;&#x27;,
 &#x27;atisfied: jsonpatch&lt;2.0,&gt;=1.33 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (1.33)\nRequirement already satisfied: langsmith&lt;0.2.0,&gt;=0.1.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (0.1.14)\nRequirement already satisfied: packaging&lt;24.0,&gt;=23.2 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (23.2)\nRequirement already satisfied: pydantic&lt;3&#x27;,
 &#x27;atisfied: pydantic&lt;3,&gt;=1 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.6.0)\nRequirement already satisfied: requests&lt;3,&gt;=2 in d:\\software\\anaconda3\\lib\\site-\npackages (from langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.31.0)\nRequirement already satisfied: tenacity&lt;9.0.0,&gt;=8.1.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from langchain-core&lt;0.2.0,&gt;=0.1.27-\n&gt;langchain-openai) (8.2.2)\nRequirement already satisfied: distro&lt;2,&gt;=1.7.0 in\nd:&#x27;,
 &#x27;stro&lt;2,&gt;=1.7.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-\nopenai) (1.8.0)\nRequirement already satisfied: httpx&lt;1,&gt;=0.23.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-\nopenai) (0.25.1)\nRequirement already satisfied: sniffio in d:\\software\\anaconda3\\lib\\site-\npackages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (1.2.0)\nRequirement already satisfied: tqdm&gt;4 in d:\\software\\anaconda3\\lib\\site-packages\n(from openai&lt;2.0.0,&#x27;,
 &#x27;\n(from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (4.65.0)\nRequirement already satisfied: typing-extensions&lt;5,&gt;=4.7 in\nd:\\software\\anaconda3\\lib\\site-packages (from openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-\nopenai) (4.7.1)\nRequirement already satisfied: regex&gt;=2022.1.18 in\nd:\\software\\anaconda3\\lib\\site-packages (from tiktoken&lt;1,&gt;=0.5.2-&gt;langchain-\nopenai) (2022.7.9)\nRequirement already satisfied: idna&gt;=2.8 in d:\\software\\anaconda3\\lib\\site-\npackages (from anyio&lt;5,&gt;=3-&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langc&#x27;,
 &#x27;.2.0,&gt;=0.1.27-&gt;langchain-openai)\n(3.4)\nRequirement already satisfied: certifi in d:\\software\\anaconda3\\lib\\site-\npackages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)\n(2023.11.17)\nRequirement already satisfied: httpcore in d:\\software\\anaconda3\\lib\\site-\npackages (from httpx&lt;1,&gt;=0.23.0-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai)\n(1.0.1)\nRequirement already satisfied: jsonpointer&gt;=1.9 in\nd:\\software\\anaconda3\\lib\\site-packages (from jsonpatch&lt;2.0,&gt;=1.33-&gt;langchain-\ncore&lt;0.2.0,&gt;=&#x27;,
 &#x27;chain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.1)\nRequirement already satisfied: orjson&lt;4.0.0,&gt;=3.9.14 in\nd:\\software\\anaconda3\\lib\\site-packages (from langsmith&lt;0.2.0,&gt;=0.1.0-\n&gt;langchain-core&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (3.9.15)\nRequirement already satisfied: annotated-types&gt;=0.4.0 in\nd:\\software\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (0.6.0)\nRequirement already satisfied: pydantic-core==2.16.1 in\nd:\\software\\anaconda3\\lib\\sit&#x27;,
 &#x27;re\\anaconda3\\lib\\site-packages (from pydantic&lt;3,&gt;=1-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.16.1)\nRequirement already satisfied: charset-normalizer&lt;4,&gt;=2 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (2.0.4)\nRequirement already satisfied: urllib3&lt;3,&gt;=1.21.1 in\nd:\\software\\anaconda3\\lib\\site-packages (from requests&lt;3,&gt;=2-&gt;langchain-\ncore&lt;0.2.0,&gt;=0.1.27-&gt;langchain-openai) (1.26.16)\nRequirement already satisfied: col&#x27;,
 &#x27;ready satisfied: colorama in d:\\software\\anaconda3\\lib\\site-\npackages (from tqdm&gt;4-&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.4.6)\nRequirement already satisfied: h11&lt;0.15,&gt;=0.13 in\nd:\\software\\anaconda3\\lib\\site-packages (from httpcore-&gt;httpx&lt;1,&gt;=0.23.0-\n&gt;openai&lt;2.0.0,&gt;=1.10.0-&gt;langchain-openai) (0.14.0)\n使用OpenAI的 GPT 系列模型的API有两个前提条件：首先，需要配置科学上网以确保可以访问\nOpenAI服务；其次，必须拥有一个有效的API Key。这两项要求的详细配置在《Ch.1 本地调用OpenAI\nAPI流程及OpenAI官网使用指南》中有详细说明，因此我们在此不再赘述，直接进入实操过程。\n查看OpenAI的依赖包版本\nOpenAI 的 API 在 0.28 版本和 &#x27;,
 &#x27;AI 的 API 在 0.28 版本和 1.xx 版本之间的调用方式存在差异。我们这里使用的是最新的调用\n方法，即采用 1.13.XX 版本。因此，需要确认当前 Python 虚拟环境中安装的 openai 包版本是 1.xx。\n可以通过执行以下命令来检查当前安装的版本：\nimport openai\nprint(openai.__version__)\n0.28.0\n如果如上显示是0.28.0版本，执行如下命令将openai包升级到最新版本：\n# ! pip install --upgrade openai\n注：如果是在Jupyter lab操作，需要重启当前的Jupyter Lab使配置生效。\nimport openai\nprint(openai.__version__)\n1.13.3\n设置科学上网环境\n根据所使用的科学上网软件，查找相应的代理端口，并按照以下代码进行网络环境配置：\n# 这里的端口，请修改为实际使用科学上网软件配置的代理端口\n!set HTTP_PROXY=http://127.0.0.1:15732\n!set HTTPS_PROXY=http://127.0.0.1:&#x27;,
 &#x27;XY=http://127.0.0.1:15732\n新版本的OpenAI API调用方法不允许使用openai.api_key= &quot;sk-xxx&quot;这种老版本的显式指定API\nKey，所以如果没有在系统变量中进行配置的话，使用下面的命令设置环境变量：\n# 将your- API -key-here替换为实际的API密钥:\n# ! setx OPENAI_API_KEY &quot;sk-xxxx&quot;\n成功: 指定的值已得到保存。\n注意：完成环境变量的初次配置后，需重启电脑以激活设置。这样，在下次使用时就可以直接调用\n配置好的环境。为验证环境变量中的API Key是否已正确设置为自己的，可以执行以下命令进行检查。\n# ! echo %OPENAI_API_KEY%\n完成了科学上网环境和API Key的设置后，在接入LangChain之前，我们先使用如下测试代码检查\n当前环境及API的运行状态是否正常，执行代码如下：\nfrom openai import OpenAI\nclient = OpenAI()\ncompletion = client.chat.completions.create(\nmodel=&#x27;,
 &#x27;tions.create(\nmodel=&quot;gpt-3.5-turbo&quot;,\nmessages=[\n{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}\n]\n)\nprint(completion.choices[0].message.content)\n你好，我是一位可以回答问题、提供信息和与您交流的AI助手。我可以帮助解决问题，回答疑惑，提供建议，\n而且乐意与您分享知识和信息。有什么我可以帮助你的吗？\n若测试代码能正常输出结果，表明OpenAI的环境配置无误。需要注意的是，LangChain接入\nOpenAI GPT模型只是利用其定义的标准模型接入框架来整合OpenAI API，因此进行上述连通性测试\n是必要的。\nOpenAI GPT Base模型官方说明：https://platform.openai.com/docs/models/moderation\nLangChain为了使开发者可以轻松地创建自定义链，整体采用Runnable协议。Runnabl&#x27;,
 &#x27;采用Runnable协议。Runnable 协议是\n编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可\n以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了\nRunnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。\nLangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch\nain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable\n而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解\n的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。\n如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI&#x27;,
 &#x27;间去看看源码的抽象逻辑。比如OpenAI：\nLangChain Interface说明：https://python.langchain.com/docs/modules/model_io/llms/quic\nk_start\nLangChain已集成LLMs说明：https://python.langchain.com/docs/integrations/llms/\n首先，我们来了解常规的调用方法。具体支持的调用方式如下所示：\n方法 说明\ninvoke 处理单条输入\nbatch 处理批量输入\nstream 流式响应\nainvoke 异步处理单条输入\nabatch 异步处理批量输入\nastream 异步流式响应\n接下来我们对上述方法依次进行尝试。在LangChain框架中，为了规范地接入LLMs类型的大模\n型，首先需要导入OpenAI的依赖包。导入命令如下：\nfrom langchain_openai import OpenAI\n字符串形式的输入\n调用invoke方法，并提供字符串格式的Prompt，便可触发指定模型按照所给Prompt进行续写操\n作。\nfrom langchain_op&#x27;,
 &#x27;作。\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;)\nllm.invoke(\n&quot;你好呀，请你介绍一下你自己&quot;\n)\n\&#x27;吧。”韩寒摇头，说：“我下次再来，你们自己给我介绍好了。”这一次，韩寒的风趣让陈航漂了一下，他说：\n“还是先看看大家的房间吧。”陈航带着韩寒到了楼下，这时连小李都已经来到了楼下，他对韩寒说：“你看，\n这是我们的宿舍。”陈航的语气是那么的平淡，既没有过多的炫耀，也没有明显的拒绝，只是那么的平淡，这\n让韩寒很有点奇怪，他说：“你们是在这里住吗？”小李说：“是啊，我们是这里的住宿生。”韩寒对小李说：\n“那我是来参加考试的，你\&#x27;\n从上述模型的返回输出，我们可以看到它本质上作为一个补全模型，通过使用大模型原生的\n.generate() 方法来对输入的Prompt进行续写。这部分的详细内容已在《Ch.2 Completions模型API\n使用指南》中进行了深入讲解，因此此处不再进行赘述。\n上述调用方法相当于使用我们熟悉的OpenAI的Completion方法来调用GPT Base类型&#x27;,
 &#x27;etion方法来调用GPT Base类型模型的过\n程：\nfrom openai import OpenAI\nclient = OpenAI()\ncompletion = client.completions.create(\nmodel=&quot;davinci-002&quot;,\nprompt=&quot;你好呀，请你介绍一下你自己&quot;,\nmax_tokens=1024,\n)\nprint(completion.choices[0].text)\n。» 她把咳嗽调整成百米冲刺那种负重颤抖。»不，我们在上面，请你告诉会场。哪一地方过去你都要做朋友\n呢？」Larsen一转眼注意到卡萨姆旁站着一位古老的男人。他身着粗布制服，最左边有一种圆盏形的供奶\n头。透映着那个老的质朴魅力和吓坏人恶心的样子。»LarsenKeberichTaylor.Ch02 3/5/07 5:27 PM\nPage 39LEARNING WHERE WECAME FROM»喂主持人。自从我给你们滴剂的时候，我就耐心等待了。请为\n快点谈到刚才提的内容。»正赞成的木质松枝变苍白了。既然他是最耐烦的老爷，我们劝劝他这样三小时也\n烦。时光久了会累人的人，他更应&#x27;,
 &#x27;三小时也\n烦。时光久了会累人的人，他更应该知道这些。»好，每个人都听到。»那些选手的名字有老项目拯救人」，\nBrickman说。AnderMartin、Taylor, här nere – efter en minut eller så. Likdant för\nLarsenKeberich, rätt så bra ger du tillbaka vätsken, yeah, and the eighth\nman.»»Eighth man?」Brickman坏坏垂下眼帘扬扬眉头，头盔上的唇影随着他挺起了脸颊。好，一位是\n我。不，我不是。Just a moment.»六记地雷爆炸。嫣红的色泽弥漫在鼻子周围极其耀眼之下，淀粉饼糊塞\n了全脸，剔饭一般的巧克力混合在一起。»»Would you all please proceed into the next\nbuilding and make sure that you use the turnstiles.»伤口破口流血，簇瓣的血色俗套，但去\n还是要用。套房好美啊、清生。套房饲養下只要一个；_ 户功能装置就。»艰难地活着足以获得一天的欢乐&#x27;,
 &#x27;能装置就。»艰难地活着足以获得一天的欢乐的\n人确实算够奇特的。咳嗽的木质松枝_cancel6 3/5/07 5:27 PM Page 40IT IS NOT ENOUGH\nTOMAKE MANKINDHAPPY,LARSEN KEBERICH向报纸走来，她在走廊哪一个厅卡萨姆等待着;在这个保证已\n经到达那种别有心碎的正式屋子里。她穿过镶有玫瑰花的拉链的抢洗衣区和单人舔饭的厕所区圆朝圣地的道。\n先是葡萄干牛油，透过臭风从供应箱中飘来，刚开始时她便一直寻找那一种苏查尔的酸味。那种味美常是发出\n着从薄饼里流发出来的所以一圈难找到对不起，小姐，我们之前选择做了决定，就算更改好一个时刻，也无法\n改变决定。拨动键盘-找麻烦你的，怎么做了、哦，听起来好像你一突然卸了山猫带来了 Asktohell你疼\n吗。杨虎大吼着，冲向对方。丁\n流式输出\n要进行流式调用，可以调用stream方法。这种方式允许在接收到模型响应的同时，逐块输出结\n果，实现即时展示而无需等待全部内容处理完毕。\nfrom langchain_openai import OpenAI\nfor chunk in llm.stream(\n&quot;你好&#x27;,
 &#x27;k in llm.stream(\n&quot;你好呀，请你介绍一下你自己&quot;\n):\nprint(chunk, end=&quot;&quot;, flush=True)\n吧，你是谁，你在哪里，你是什么学问，你喜欢什么，你不喜欢什么，你不会什么，你会什么，你梦想什么，\n你的优点是什么，你的缺点是什么，你的性格是什么，你的兴趣是什么，你喜欢的食物是什么，你不喜欢的食\n物是什么，你的爱好是什么，你的愿望是什么。介绍一下你自己好的。\n[ 编辑 xiaofan1 在 07-03-18 23:49 ]\n批量生成\n通过调用batch方法，可以同时对多个不同的Prompt进行调用处理。在这个例子中，输入包括“你\n好”、“什么是机器学习”和“什么是深度学习”三个独立的Prompt。这种批量处理方式允许一次性提交多个\nPrompt，从而提高推理效率。\nfrom langchain_openai import OpenAI\nllm.batch(\n[\n&quot;你好&quot;,\n&quot;什么是机器学习&quot;,\n&quot;什么是深度学习&quot;,\n]\n)\n[\&#x27;，习惯了你的成熟，可是这次你的决定让我感到非常惊讶，我不知道你的这次决定是不是真的，我不知道你\n是否真的那么爱我，虽然我知道&#x27;,
 &quot;我不知道你\n是否真的那么爱我，虽然我知道你非常爱我，可是我不知道你是不是真的爱我，假如你为了我不想去上学，我\n也可以放弃一切，去读书，去上大学，但是你为什么要这样做呢，我不清楚你的想法，我不知道你的想法，我\n只知道我爱你，我只知道我不想失去你，我只知道我不想让你失去你的希望，我只知道我不想让你失去你的未\n来，可是我想知道你为什么要这样做呢，我想知道你的想法，你是&#x27;,\n&#x27; 机器学习的原理 机器学习的技术 机器学习的应用机器学习在生产过程中的作用机器学习在生产过程中的\n作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产\n过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学\n习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作\n用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学习在生产过程中的作用机器学&#x27;,\n&#x27;？ 2. Why is the industry so interested in deep learning? 3. How will de&quot;,
 &#x27;ning? 3. How will deep\nlearning transform the financial industry? 4. How can you use deep learning to\nbuild better AI? 5. What are the ingredients of a deep learning model?\\n\\nA\nstudy of the best deep learning algorithms and how they work, the techniques\ninvolved in building them, and the results they can bring to the table: 1.\nUnsupervised learning 2. Convolutional Neural Networks (CNN) 3. Recurrent Neural\nNetworks (RNN) 4. Deep Reinforcement Learning (RL) 5. Generative Adversarial\nNetworks (GA&#x27;,
 &#x27;rsarial\nNetworks (GAN)\\n\\nA step-by-step guide to building a real-world, deep learning\napp: 1. What is a deep learning app? 2. How do you build a deep learning app? 3.\nThe various components involved in building a deep learning app 4. How do you\ndeploy a deep learning app? 5. The future of deep learning apps\\n\\nHow to build\na deep learning app: A guide to the various tools, frameworks, and libraries\nthat you can use to build deep learning apps: 1. Deep Learning Frameworks 2.\nDeep Learning Librar&#x27;,
 &#x27;Deep Learning Libraries 3. Deep Learning Tools 4\&#x27;]\n异步调用\nllm.invoke(...)本质上是一个同步调用。在这种情况下，程序会在调用返回结果之前停止执行\n任何后续代码。这意味着如果invoke操作耗时较长，它会导致程序暂时挂起，直到操作完成。我们可\n以通过这样一个测试代码来直观的理解同步调用：\nimport time\ndef call_model():\n# 模拟同步API调用\nprint(&quot;开始调用模型...&quot;)\ntime.sleep(3) # 模拟调用等待\nprint(&quot;模型调用完成。&quot;)\ndef perform_other_tasks():\n# 模拟执行其他任务\nfor i in range(3):\nprint(f&quot;执行其他任务 {i + 1}&quot;)\ntime.sleep(1)\ndef main():\nstart_time = time.time()\ncall_model()\nperform_other_tasks()\nend_time = time.time()\ntotal_time = end_time - start_t&#x27;,
 &#x27;= end_time - start_time\nreturn f&quot;总共耗时：{total_time}秒&quot;\n# 运行同步任务并打印完成时间\nmain_time = main()\nmain_time\n开始调用模型...\n模型调用完成。\n执行其他任务 1\n执行其他任务 2\n执行其他任务 3\n\&#x27;总共耗时：6.001835107803345秒\&#x27;\n这段同步调用的程序先模拟了一个耗时3秒的模型调用，随后执行了三个其他任务，每个任务耗时1\n秒。实际的执行时间为约6.00秒。这体现了同步执行的特点：每个操作依次执行，直到当前操作完成后\n才开始下一个操作，从而导致总的执行时间是各个操作时间的总和。\n而异步调用，允许程序在等待某些操作完成时继续执行其他任务，而不是阻塞等待。这在处理I/O\n操作（如网络请求、文件读写等）时特别有用，可以显著提高程序的效率和响应性。\n我们可以设计这样如下情景：使用async和await关键字， 通过async def定义异步函数\nasync_call和perform_other_tasks。await asyncio.sleep(3)模拟了一个耗时3秒的异步操\n作。这里的awa&#x27;,
 &#x27;了一个耗时3秒的异步操\n作。这里的await表明程序在这里“等待”这个操作完成，但与同步操作不同，这个等待是非阻塞的。这\n意味着程序可以在这3秒内执行其他异步代码。这种方式不会阻塞整个程序的运行，而是让出控制权，\n允许事件循环继续运行其他任务。\n! pip install asyncio\nCollecting asyncio\nObtaining dependency information for asyncio from\nhttps://files.pythonhosted.org/packages/22/74/07679c5b9f98a7cb0fc147b1ef1cc1853b\nc07a4eb9cb5731e24732c5f773/asyncio-3.4.3-py3-none-any.whl.metadata\nDownloading asyncio-3.4.3-py3-none-any.whl.metadata (1.7 kB)\nDownloading asyncio-3.4.3-py3-none-any.whl (101 kB)\n-------------------------&#x27;,
 &#x27;----------------------------------- 0.0/101.8 kB ? eta -:--:--\n---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--\n---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--\n---- ----------------------------------- 10.2/101.8 kB ? eta -:--:--\n----------- --------------------------- 30.7/101.8 kB 163.8 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01&#x27;,
 &#x27;1.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n--------------- ----------------------- 41.0/101.8 kB 151.3 kB/s eta 0:00:01\n----------------------- --------------- 61.4/101.8 kB 113.0 kB/s eta 0:00:01\n--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01\n-----------------&#x27;,
 &#x27;01\n--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01\n--------------------------- ----------- 71.7/101.8 kB 115.5 kB/s eta 0:00:01\n-------------------------------------- 101.8/101.8 kB 136.2 kB/s eta 0:00:00\nInstalling collected packages: asyncio\nSuccessfully installed asyncio-3.4.3\nimport asyncio\nimport time\nasync def async_call(llm):\nawait asyncio.sleep(3) # 模拟异步操作\nprint(&quot;异步调用完成&quot;)\nasync def perform_other_tasks():\nawait asyncio.sleep(3) # 模拟异步操作\nprint(&quot;其他任务完成&quot;)\nasync d&#x27;,
 &#x27;nt(&quot;其他任务完成&quot;)\nasync def run_async_tasks():\nstart_time = time.time()\nawait asyncio.gather(\nasync_call(None), # 示例调用，替换None为模拟的LLM对象\nperform_other_tasks()\n)\nend_time = time.time()\nreturn f&quot;总共耗时：{end_time - start_time}秒&quot;\n# 运行异步任务并打印完成时间\nawait run_async_tasks()\n异步调用完成\n其他任务完成\n\&#x27;总共耗时：3.009281873703003秒\&#x27;\n使用asyncio.gather()并行执行时，理想情况下，因为两个任务几乎同时开始，它们的执行时间\n将重叠。如果两个任务的执行时间相同（这里都是3秒），那么总执行时间应该接近单个任务的执行时\n间（3秒左右），而不是两者时间之和。\n理解了同步调用和异步调用的区别后，我们接下来看一下LangChain中集成的异步调用链使用方\n法。\n要实现异步调用并优化处理多个请求的场景，在LangChain中，可以使用a&#x27;,
 &#x27;场景，在LangChain中，可以使用ainvoke方法来达成。这\n种方法允许同时启动多个请求，而不必等待每个请求依次完成。这对于提高应用程序的响应性和效率尤\n其有用，特别是在处理需要与外部服务交互的任务时。\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;)\nawait llm.ainvoke(\n&quot;哈喽，你好呀，&quot;,\n)\n\&#x27;这次您知道了吗？这是鸡肋大战，您是来玩的吧，我也是。我来纠结你的，看看你敢不敢。我一般不来这种\n鸡肋大战，因为我很懒，这次我还是来了，因为我懒惯了，其实我很想快点解脱了，我很想走。你的目的在哪\n里？我想敌对你，我还是要意识到了，你想不想敌对我呢？你敢不敢这样问？我不敢，因为我懒。你的目的是\n什么？我很想快点解脱，我不想你，我只想快点走，早点走。你敢不敢这样说？我敢，因为我懒。你的目的是\n什么？我\&#x27;\n异步流式响应\nfrom langchain_openai import OpenAI\nasync for chunk in llm.astream(\n&quot;你好，&quot;,\n):\np&#x27;,
 &#x27;astream(\n&quot;你好，&quot;,\n):\nprint(chunk, end=&quot;&quot;, flush=True)\n我是来投诉的 [Image]\n你现在没有抱怨，说的确实很有道理，如果他们为了你不让你上，你能不上吗，我想大多数人应该会上，所以\n他们是不会因为你不上而阻止你上的，但是如果你上了，你的优惠和你的一切都是不可能了，你上了，他们是\n不能为你划上你的学费的，所以我觉得就算你上了也没有用，他们不会为你划学费，因为你上了从而给了他们\n非常大的麻烦，你上了，他们是不能有一点好处的，他们还要为你上的学费等等付出，所以他们会说你上不\n了，就是为了不让你上，不让你上，他们不会为你划学费，所以我觉得你如果上了，你就没有任何\n异步批量处理\nfrom langchain_openai import OpenAI\nawait llm.abatch(\n[\n&quot;你好&quot;,\n&quot;什么是机器学习&quot;,\n&quot;什么是深度学习&quot;,\n]\n)\n[\&#x27;，前来亲自拜访的，因为我们这里很少有人来拜访，你就打算在这里待上几个礼拜了吧？”“我是回来找你，\n我是到桐谷这里来的。”李晓婷冷冷的回道。这一次很明显的不是她想让他留下来的，而是让他走了。众人看&#x27;,
 &quot;是她想让他留下来的，而是让他走了。众人看\n着李晓婷，这里的她就这么冷了，竟然连郑晓东都不看一眼，郑晓东是她的男朋友，他们是一对，她怎么可能\n不看一眼？“那你为什么不和我说，你的事情怎么样了？”郑晓东看向李晓婷的脸，她的表情很冷，她怎么不看\n一眼自己&#x27;,\n&#x27;？\\n\\n2:王景仪 2021-01-07 23:12:57\\n\\nannounced\\n\\n3:李桂林 2021-01-01\n23:12:57\\n\\nMy Sunshine is a TV drama adapted from Gu Man’s novel of the same\ntitle. It stars Wallace Huo and Tang Yan, premiering in January, 2015. The drama\ndepicts a man and a woman’s sweet love story.\\n\\n4:王威 2020-12-27\n23:12:57\\n\\nThe new data released yesterday represent the first snapshot of\nglobal t&quot;,
 &#x27;snapshot of\nglobal trade for 2015. And the figures suggest that 2015 was not a good year for\nworld trade, as it had been hoped.\\n\\n5:苏晓波 2021-01-01 23:12:57\\n\\nMark Rutte,\nthe Dutch prime minister whose People’s Party for Freedom and Democracy (VVD) is\nslightly ahead of the PVV, has seen his poll numbers rise in recent days because\nof his hardline stance with Ankara. On Tuesday he called Mr Erdogan’s comments\n“a disgusting distortion of history”, adding: “We will not lower ourselves to\nthis leve&#x27;,
 &quot;rselves to\nthis level. It is totally unacceptable.”\\n\\n&#x27;,\n&#x27;？深度学习是一种模仿大脑思维的机器学习方法，深度学习方法被广泛用于图像识别、语音识别、自然语言\n处理等领域。深度学习过程中，模型经常需要大量的训练数据，因此大量的数据获取和数据刷新是有必要。.\n1) Find a free server to host the application in. I have been trying to create a\nsimple neural network by using the TensorFlow Object Detection API to run an\nobject detection model. Cloud TPU is a Google Cloud service that accelerates\nmachine learning workloads. Major cloud providers offer free tier of services to\nhelp you get started. Google&quot;,
 &quot; get started. Google Cloud Storage is a RESTful web service for storing\nand accessing data on Google Cloud Platform. In this video, you will learn about\nCloud TPU, a high-performance, scalable, and cost-effective machine learning\nservice. This IP address 157. 0, a neural network framework. Contribute.\nTensorFlow Lite for mobile and embedded devices For Production TensorFlow\nExtended for end-to-end ML components. The Google Cloud Platform&#x27;]\n除此之外，在Jupyter Notebook或Python交互式解释器中进行操作的话，可以使用?来查看关于\nll&quot;,
 &quot;进行操作的话，可以使用?来查看关于\nllm中内置的方法和帮助文档，执行如下代码：\nllm?\n•[1;31mSignature:•[0m\n•[0mllm•[0m•[1;33m(•[0m•[1;33m\n•[0m •[0mprompt•[0m•[1;33m:•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mstop•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m\n•[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mcallbacks•[0m•[1;33m:•[0m •[1;34m&#x27;Callbacks&#x27;•[0m •[1;33m=•[0m\n•[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[1;33m*•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mtags•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[List[str]]&#x27;•[0m •[1;33m=•[0m\n•[1&quot;,
 &quot;[0m •[1;33m=•[0m\n•[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[0mmetadata•[0m•[1;33m:•[0m •[1;34m&#x27;Optional[Dict[str, Any]]&#x27;•[0m\n•[1;33m=•[0m •[1;32mNone•[0m•[1;33m,•[0m•[1;33m\n•[0m •[1;33m**•[0m•[0mkwargs•[0m•[1;33m:•[0m\n•[1;34m&#x27;Any&#x27;•[0m•[1;33m,•[0m•[1;33m\n•[0m•[1;33m)•[0m •[1;33m-&gt;•[0m •[1;34m&#x27;str&#x27;•[0m•[1;33m•[0m•[1;33m•[0m•[0m\n•[1;31mType:•[0m OpenAI\n•[1;31mString form:•[0m\n•[1mOpenAI•[0m\nParams: {&#x27;model_name&#x27;: &#x27;davinci-002&#x27;, &#x27;temperature&#x27;: 0.7, &#x27;top_p&#x27;: 1,\n&#x27;frequency_penalty&#x27;: 0, &#x27;presence_penalt&quot;,
 &quot; 0, &#x27;presence_penalty&#x27;: 0, &#x27;n&#x27;: 1, &#x27;logit_bias&#x27;: {},\n&#x27;max_tokens&#x27;: 256}\n•[1;31mFile:•[0m d:\\software\\anaconda3\\lib\\site-\npackages\\langchain_openai\\llms\\base.py\n•[1;31mDocstring:•[0m\nOpenAI large language models.\nTo use, you should have the ``openai`` python package installed, and the\nenvironment variable ``OPENAI_API_KEY`` set with your API key.\nAny parameters that are valid to be passed to the openai.create call can be\npassed\nin, even if not explicitly saved on this class.\nExample:\n.. code-bloc&quot;,
 &#x27;xample:\n.. code-block:: python\nfrom langchain_community.llms import OpenAI\nopenai = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\n•[1;31mInit docstring:•[0m\nCreate a new model by parsing and validating input data from keyword arguments.\nRaises ValidationError if the input data cannot be parsed to form a valid model.\n•[1;31mCall docstring:•[0m\n[*Deprecated*] Check Cache and run the LLM on the given prompt and input.\nNotes\n-----\n.. deprecated:: 0.1.7\nUse invoke instead.\n这里能看到非常多的关键信息，比如：\n默认参数：Params&#x27;,
 &#x27;关键信息，比如：\n默认参数：Params: {\&#x27;model_name\&#x27;: \&#x27;davinci-002\&#x27;, \&#x27;temperature\&#x27;: 0.7, \&#x27;top_p\&#x27;: 1,\n\&#x27;frequency_penalty\&#x27;: 0, \&#x27;presence_penalty\&#x27;: 0, \&#x27;n\&#x27;: 1, \&#x27;logit_bias\&#x27;: {}, \&#x27;max_tokens\&#x27;: 256}\n当不输入model_name时，默认会调用davinci-002模型：\nfrom langchain_openai import OpenAI\nllm = OpenAI()\nawait llm.ainvoke(\n&quot;哈喽，你好呀，&quot;,\n)\n\&#x27;我是小影。很高兴认识你，有什么可以帮到你的吗？\&#x27;\n同时，也可以调整温度temperature:\nfrom langchain_openai import OpenAI\nllm = OpenAI()\nawait llm.ainvoke(\n&quot;哈喽，你好呀，&quot;,\ntemperature=0\n)\n\&#x27;我是一个人工智能助手，很高兴认识你！有什么可以帮到你的吗？\&#x27;\nmodel：必选参数，具体调用的Completions&#x27;,
 &#x27;选参数，具体调用的Completions模型名称，可以调用的模型包括text-davinci-003、\ntext-davinci-002、text-curie-001、text-babbage-001、text-ada-001等，不同模型参数规模不\n同；这里需要注意，大模型领域不同于机器学习领域，后者哪怕是简单模型在某些场景下可能也会\n拥有比复杂模型更好的表现。在大模型领域，（就OpenAI提供的A、B、C、D四大模型来看）参\n数规模越大、越新版本的模型效果更好（当然费用也更高），因此课程中主要以text-davinci-003\n使用为例进行讲解；\nprompt：必选参数，提示词；\nsuffix：可选参数，默认为空，具体指模型返回结果的后缀；\nmax_tokens：可选参数，默认为16，代表返回结果的token数量；\ntemperature：可选参数，取值范围为0-2，默认值为1。参数代表采样温度，数值越小，则模型会\n倾向于选择概率较高的词汇，生成的文本会更加保守；而当temperature值较高时，模型会更多地\n选择概率较低的词汇，生成的文本会更加多样；\ntop_p：可选参数，取值范&#x27;,
 &#x27;更加多样；\ntop_p：可选参数，取值范围为0-1，默认值为1，和temperature作用类似，用于控制输出文本的\n随机性，数值越趋近与1，输出文本随机性越强，越趋近于0文本随机性越弱；通常来说若要调节\n文本随机性，top_p和temperature两个参数选择一个进行调整即可；这里更推荐使用\ntemperature参数进行文本随机性调整；\nn：可选参数，默认值为1，表示一个提示返回几个Completion；\nstream：可选参数，默认值为False，表示回复响应的方式，当为False时，模型会等待返回结果\n全部生成后一次性返回全部结果，而为True时，则会逐个字进行返回；\nlogprobs：可选参数，默认为null，该参数用于指定模型返回前N个概率最高的token及其对数概\n率。例如，如果logprobs设为10，那么对于生成的每个token，API会返回模型预测的前10个\ntoken及其对数概率；\necho：可选参数，默认为False，该参数用于控制模型是否应该简单地复述用户的输入。如果设为\nTrue，模型的响应会尽可能地复述用户的输入；\nstop：可选参数，默认为null，该&#x27;,
 &#x27;\nstop：可选参数，默认为null，该参数接受一个或多个字符串，用于指定生成文本的停止信号。当\n模型生成的文本遇到这些字符串中的任何一个时，会立即停止生成。这可以用来控制模型的输出长\n度或格式；\npresence_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型生成新内容\n（例如新的概念或主题）的倾向性。较高的值会使模型更倾向于生成新内容，而较低的值则会使模\n型更倾向于坚持已有的内容，当返回结果篇幅较大并且存在前后主题重复时，可以提高该参数的取\n值；\nfrequency_penalty：可选参数，默认为0，取值范围为[-2, 2]，该参数用于调整模型重复自身的倾\n向性。较高的值会使模型更倾向于避免重复，而较低的值则会使模型更可能重复自身；当返回结果\n篇幅较大并且存在前后语言重复时，可以提高该参数的取值；\nbest_of：该参数用于控制模型的生成过程。它会让模型进行多次尝试（例如，生成5个不同的响\n应），然后选择这些响应中得分最高的一个；\nlogit_bias：该参数接受一个字典，用于调整特定token的概率。字典的键是token的ID，值是应用\n于该t&#x27;,
 &#x27;的键是token的ID，值是应用\n于该token的对数概率的偏置；在GPT中我们可以使用tokenizer tool查看文本Token的标记。一般\n不建议修改；\nuser：可选参数，使用用户的身份标记，可以通过人为设置标记，来注明当前使用者身份。需要\n注意的是，Completion.create函数中的user和后续介绍的对话类模型的user参数含义并不相同，\n需要注意区分；\n更详细的参数说明和使用细节可在《Ch.2 Completions模型API使用指南》中找到。此处不在展开\n重复说明。\n目前在LangChain中已经集成的LLMs有82个，均已经实现了对异步、流式处理和批处理的基本支\n持，查看地址如下：https://python.langchain.com/docs/integrations/llms/\n4.1.2 Chat Models接入方法\n所谓聊天模型，简单理解就是我们经常使用的xxx-chat类模型，比如Baichuan2-7b-chat，\nQwen-14b-chat，这类模型是基于Base模型经过针对特定数据格式的微调得到，以更贴近人类对话的\n方式进行输入和输出。所&#x27;,
 &#x27;更贴近人类对话的\n方式进行输入和输出。所以这类模型不会使用“文本输入、文本输出”的形式，而是采用聊天消息作为输\n入并返回聊天消息作为输出（非纯文本）。LangChain框架与OpenAI、Cohere、Hugging Face等多家\n模型提供商已经实现集成，提供了一个标准化的接口，以便与这些不同的模型进行交互。\n我们先回顾下OpenAI规范下使用Chat模型进行一次交互需遵循的代码执行方式：\nfrom openai import OpenAI\nclient = OpenAI()\ncompletion = client.chat.completions.create(\nmodel=&quot;gpt-3.5-turbo-0125&quot;,\nmessages=[\n{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一个乐于助人的智能AI小助手&quot;},\n{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己&quot;}\n]\n)\nprint(completion.choices[0].message.content)\n您好，我是一个智能AI助手，可以帮助您回答问题、提供信息、解&#x27;,
 &#x27;I助手，可以帮助您回答问题、提供信息、解决问题等。如果您有任何疑问或需要帮助的\n话，都可以随时向我提问。我会尽力帮助您解决问题。\n这里不得不再次提到，LangChain作为一个应用开发框架，需要集成各种不同的大模型，如上述\nOpenAI的GPT系列模型调用示例，通过Message数据输入规范，定义不同的role，即system、user和\nassistant来区分对话过程，但对于其他大模型，并不意味这一定会遵守这种输入输出及角色的定义，所\n以LangChain的做法是，因为Chat Model基于消息而不是原始文本，LangChain目前就抽象出来的消息\n类型有 AIMessage 、 HumanMessage 、 SystemMessage 、 FunctionMessage 和 ChatMessage ，\n但大多时候我们只需要处理 HumanMessage 、 AIMessage 和 SystemMessage，即：\nSystemMessage ：用于启动 AI 行为，作为输入消息序列中的第一个传入。\nHumanMessage ：表示来自与聊天模型交互的人的消息。\nAIMessag&#x27;,
 &#x27;天模型交互的人的消息。\nAIMessage ：表示来自聊天模型的消息。这可以是文本，也可以是调用工具的请求。\n同样，Chat Models实现了 Runnable 接口，也支持 invoke 、 ainvoke 、 stream 、 astream 、\nbatch 、 abatch 等调用。即：\n方法 说明\ninvoke 处理单条输入\nbatch 处理批量输入\nstream 流式响应\nainvoke 异步处理单条输入\nabatch 异步处理批量输入\nastream 异步流式响应\n所以对于LangChain中调用OpenAI的GPT模型，首先需要导入对应的模块，代码如下：\nfrom langchain_core.messages import HumanMessage, SystemMessage\n消息形式输入调用\n定义消息对象：\nmessages = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;你好，请你介绍一下你自己&quot;),]\nmessages\n[SystemMessage(content=\&#x27;你是一位乐&#x27;,
 &#x27;ssage(content=\&#x27;你是一位乐于助人的智能小助手\&#x27;), HumanMessage(content=\&#x27;你好，请\n你介绍一下你自己\&#x27;)]\nfrom langchain_openai import ChatOpenAI\nchat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)\n执行推理：\nchat.invoke(messages)\nAIMessage(content=\&#x27;你好，我是一位可以回答问题、提供帮助的智能小助手。无论是关于知识、技能还是\n日常生活中的问题，我都会尽力为您提供准确和有用的答案。有什么可以帮助您的吗？\&#x27;)\n如果只想获取到纯净的输出，可以通过.content来格式化。\nreponse.content\n\&#x27;你好！我是一位智能助手，可以回答各种问题，提供信息和帮助。无论是关于知识、技能、娱乐、生活等方\n面，我都会尽力为你提供帮助。有什么问题需要我帮忙的吗？\&#x27;\n流式调用\nfor chunk in chat.stream(messages):\nprint(chunk.content, end=&quot;&quot;, flush=True)\n你好！我&#x27;,
 &#x27;&quot;&quot;, flush=True)\n你好！我是一位智能助手，专门为用户提供信息咨询、问题解答和帮助服务。无论你有什么疑问或需求，我都\n会尽力帮助你，让你更方便地获取所需的信息和支持。有什么我可以帮助你的吗？\n批量调用\n先定义三个不同的消息对象：\nmessages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]\nmessages2 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是深度学习&quot;),]\nmessages3 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是大模型技术&quot;),]\n将上述三个消息对象放在一个列表中，使用.batch方法执行批量调用。\nreponse = chat.batch([messages1,\nmessages2,\nmessages3,])\nr&#x27;,
 &quot;ges2,\nmessages3,])\nreponse\n[AIMessage(content=&#x27;机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来\n改善自身的性能，而无需明确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规\n律、模式以及知识的技术。通过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预\n测、分类、识别等任务。机器学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测\n等领域。&#x27;),\nAIMessage(content=&#x27;深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神\n经网络层来学习和理解复杂的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识\n别、语音识别、自然语言处理等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对\n新数据进行准确预测和分类。深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金\n融、交通等各个领域。&#x27;),\nAIMessage(content=&#x27;大模型技术是指利用大规模、高性能的计算资源和先进的机器学&quot;,
 &#x27;用大规模、高性能的计算资源和先进的机器学习算法来训练和部\n署复杂的深度学习模型。这些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优\n化。\\n\\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模\n型技术，研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。\\n\\n然\n而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员和\n工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。\&#x27;)]\n格式化输出：\n# 使用列表生成式打印每一个消息的内容\ncontents = [msg.content for msg in reponse]\n# 打印出内容\nfor content in contents:\nprint(content, &quot;\\n---\\n&quot;)\n机器学习是人工智能的一个子领域，其目的是使计算机系统通过学习数据和经验来改善自身的性能，而无需明\n确地进行编程。简而言之，机器学习是一种让计算机系统从数据中学习和提取规律、模式以及知识的技术。通\n过利用统计学和&#x27;,
 &#x27;、模式以及知识的技术。通\n过利用统计学和算法来使计算机自动地学习和改进，从而实现更准确和高效的预测、分类、识别等任务。机器\n学习应用广泛，包括推荐系统、自然语言处理、图像识别、医疗诊断、金融预测等领域。\n---\n深度学习是一种机器学习的子领域，它模仿人类大脑的结构和功能，通过一系列神经网络层来学习和理解复杂\n的数据。深度学习模型可以自动发现数据中的模式和特征，从而实现诸如图像识别、语音识别、自然语言处理\n等任务。深度学习的核心思想是通过训练神经网络来优化模型参数，使其能够对新数据进行准确预测和分类。\n深度学习在各个领域都取得了显著的成就，被广泛应用于人工智能、医疗、金融、交通等各个领域。\n---\n大模型技术是指利用大规模、高性能的计算资源和先进的机器学习算法来训练和部署复杂的深度学习模型。这\n些模型通常具有数十亿到数万亿个参数，需要大量的数据和计算资源来训练和优化。\n大模型技术在各个领域都有广泛的应用，如自然语言处理、计算机视觉、语音识别等。通过使用大模型技术，\n研究人员和工程师可以构建更加准确和复杂的模型，从而提升机器学习系统的性能和效果。\n然而，大模型技术也面临着一些挑战，如计算资&#x27;,
 &#x27;而，大模型技术也面临着一些挑战，如计算资源的需求高、训练时间长、模型部署复杂等。因此，研究人员\n和工程师在使用大模型技术时需要权衡各种因素，以确保系统的性能和稳定性。\n---\n异步调用\nmessages1 = [SystemMessage(content=&quot;你是一位乐于助人的智能小助手&quot;),\nHumanMessage(content=&quot;请帮我介绍一下什么是机器学习&quot;),]\nfrom langchain_openai import ChatOpenAI\nchat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)\nreponse = await chat.ainvoke(messages1)\nreponse\nAIMessage(content=\&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并\n改进性能，而无需显式地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断\n优化自身的表现。机器学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常\n见的机器学习方法包括监督学习、无监督&#x27;,
 &#x27;常\n见的机器学习方法包括监督学习、无监督学习、强化学习等。\&#x27;)\nreponse.content\n\&#x27;机器学习是一种人工智能的分支领域，旨在让计算机系统通过数据和经验自动学习并改进性能，而无需显式\n地进行编程。通过机器学习，计算机系统可以识别模式、预测结果、做出决策，并不断优化自身的表现。机器\n学习的应用非常广泛，包括语音识别、图像识别、自然语言处理、推荐系统等领域。常见的机器学习方法包括\n监督学习、无监督学习、强化学习等。\&#x27;\n流式调用\nasync for chunk in chat.astream(messages1):\nprint(chunk.content, end=&quot;&quot;, flush=True)\n机器学习是一种人工智能的分支，它致力于让计算机系统通过学习数据和模式，从而改进自身的性能。简而言\n之，机器学习让计算机具有从经验中学习的能力，而不需要明确地编程指令。\n在机器学习中，计算机系统利用大量数据进行训练，通过识别数据中的模式和规律来建立模型。这些模型可以\n用来进行预测、分类、识别等任务。机器学习在各个领域中都有广泛的应用，如自然语言处理、图像识别、医\n疗诊断、金融预测等。\n常见的&#x27;,
 &#x27;像识别、医\n疗诊断、金融预测等。\n常见的机器学习算法包括监督学习、无监督学习、强化学习等。监督学习是通过已标记的数据进行训练，无监\n督学习则是在没有标记的数据中发现模式，强化学习则通过试错的方式学习最优策略。机器学习技术的发展使\n得计算机系统能够更加智能地处理复杂任务，为人们的生活和工作带来了许多便利。\n通过上述描述，我们展示了在LangChain中使用LLMs类模型和Chat Model类模型的不同方法，其\n核心区别在于输入Prompt的格式。除此之外其他的工作可以直接利用统一的抽象接口，实现与模型交\n互的快速过程。而针对不同的模型，LangChain也提供个对应的接入方法，其相关说明文档地址：http\ns://python.langchain.com/docs/integrations/chat/\n比如我们以Baichuan2的在线API模型为例快速接入一下：\nfrom langchain_community.chat_models import ChatBaichuan\nfrom langchain_core.messages import HumanMessage\nchat =&#x27;,
 &#x27; HumanMessage\nchat = ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-xxx&quot;,\nstreaming=True,\n)\nresponse = chat([HumanMessage(content=&quot;我日薪8块钱，请问在闰年的二月，我月薪多少&quot;)])\nresponse\nAIMessage(content=\&#x27;闰年的二月有29天，所以你的月薪是：\\n\\n$8\\\\times29$=$232$（元）\\n\\n所\n以，你在闰年的二月月薪是232元。\&#x27;)\nresponse.content\n\&#x27;闰年的二月有29天，所以你的月薪是：\\n\\n$8\\\\times29$=$232$（元）\\n\\n所以，你在闰年的二月月\n薪是232元。\&#x27;\n如未实践过Baichuan2的在线API，请查看《APICh.16 BaiChuan 2 大模型生态介绍及本地私有化\n部署方案》中1. Baichuan在线大模型生态介绍小节\n可以看出，LangChain能够迅速接入不同的模型API。至于接入本地私有部署的开源大模型的方\n法，我们将在后续课程中进行详细&#x27;,
 &#x27;模型的方\n法，我们将在后续课程中进行详细介绍。\n4.2 LangChain中如何使用Prompt Template\n提示工程（Prompt Engineering）大家应该比较熟悉，这个概念是指在与大语言模型（LLMs），\n如GPT-3、Qwen等模型进行交互时，精心设计输入文本（即提示）的过程，以获得更精准、相关或有\n创造性的输出。正如我们在大模型技术应用课程中介绍的，通过采用Few-Shot、Chain of Thought\n(CoT)等高级提示技巧，可以显著提高大模型在推理任务上的表现。目前，提示工程已经发展成为一个\n专业领域，非常多的公司设立了专门的职位，负责为特定任务编写精确且具有创造力的提示。\n以使用ChatGPT等网页端对话交互应用中，大部分人常见的做法是将提示（Prompt）做硬编码，\n例如将一段提示文本固定在System Messages中。而在应用开发领域，开发者往往无法预知用户的具体\n输入内容，同时又希望大模型能够根据不同的应用任务以一种较为统一的逻辑来处理用户输入。所以，\nLangChain通过提供Prompt Templates功能，优雅地解决了这个问题。Pr&#x27;,
 &#x27;ates功能，优雅地解决了这个问题。Prompt Templates将用户输入\n到完整格式化提示的转换逻辑进行封装，使得模型能够更灵活、高效地处理各种输入。\nLangChain 提供了创建和使用提示模板的工具，其使用PromptTemplate 方法创建字符串提示模\n板，因为目前为止，语言模型接收的提示基本都是字符串或聊天消息列表。\n4.2.1 使用str.format语言构建模版\n在LangChain的默认设置下， PromptTemplate 使用 Python 的 str.format() 方法进行模板\n化。\nPython的str.format()方法是一种字符串格式化的手段，允许我们在字符串中插入变量。使用这\n种方法，可以创建包含占位符的字符串模板，占位符由花括号{}标识。调用format()方法时，可以传入\n一个或多个参数，这些参数将被顺序替换进占位符中。str.format()提供了灵活的方式来构造字符串，支\n持多种格式化选项，包括数字格式化、对齐、填充、宽度设置等。\n基本用法\n# 简单示例，直接替换\ngreeting = &quot;Hello, {}!&quot;.format(&quot;Alic&#x27;,
 &#x27;o, {}!&quot;.format(&quot;Alice&quot;)\nprint(greeting)\n# 输出: Hello, Alice!\nHello, Alice!\n带有位置参数的用法\n# 使用位置参数\ninfo = &quot;Name: {0}, Age: {1}&quot;.format(&quot;Bob&quot;, 30)\nprint(info)\n# 输出: Name: Bob, Age: 30\nName: Bob, Age: 30\n带有关键字参数的用法\n# 使用关键字参数\ninfo = &quot;Name: {name}, Age: {age}&quot;.format(name=&quot;Charlie&quot;, age=25)\nprint(info)\n# 输出: Name: Charlie, Age: 25\nName: Charlie, Age: 25\n使用字典解包的方式：\n# 使用字典解包\nperson = {&quot;name&quot;: &quot;David&quot;, &quot;age&quot;: 40}\ninfo = &quot;Name: {name}, Age: {age}&quot;.format(**person)\nprint(info)\n# 输出: Name: David, Age: 40\nName: &#x27;,
 &#x27;avid, Age: 40\nName: David, Age: 40\n在LangChain中，基本采用了Python的原生str.format()方法对输入数据进行格式化，这样在模\n型接收输入前，可以根据需要对数据进行预处理和结构化，以此来引导大模型进行更准确的推理。\nfrom langchain.prompts import PromptTemplate\nprompt_template = PromptTemplate.from_template(\n&quot;请给我一个关于{topic}的{type}解释。&quot;\n)\nprompt= prompt_template.format(type=&quot;详细&quot;, topic=&quot;量子力学&quot;)\nprompt\n\&#x27;请给我一个关于量子力学的详细解释。\&#x27;\n如上所示，可以使用PromptTemplate的from_template方法创建一个提示模板实例，这个模板\n包含了两个占位符：{topic} 和 {type}，这些占位符在实际调用时可以被实际的值替换。\n调用LLMs模型\nfrom langchain_openai import OpenAI\nllm = OpenAI&#x27;,
 &#x27; OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;, max_tokens=1000)\nresult = llm.invoke(prompt)\nprint(result)\n我觉得我已经对量子力学的基本理解了，但是我还是很想听听有关量子力学的详细解释，比如：量子力学是怎\n么起作用的。\n谢谢了.\n量子力学是在克里姆林宫试验时的实验结果发现.一开始是用熟悉的物理学法则去解释它.比如说,量子力学是\n量子力学的结果,而不是量子力学的原因.玻尔兹曼方程就是克里姆林宫试验结论的表示.后来人们发现,等效\n性,柏格效应等现象.然后才发现,原来我们的光学和电磁学等理论是有问题的.这些现象的实现是在离子束中.\n所以量子力学是很实际的.\n[ 编辑 stuartm 在 06-07-08 12:11 ]\nprompt= prompt_template.format(type=&quot;简短&quot;, topic=&quot;机器学习&quot;)\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;, max_to&#x27;,
 &#x27;davinci-002&quot;, max_tokens=1000)\nresult = llm.invoke(prompt)\nprint(result)\n2)目前的时代，机器学习已经发展到了什么程度，也有什么应用场景。 3)目前的研究方向是什么。 4)什么\n时候会用到这些技术。 5)最后一个问题是，对于我自己来说，是不是还有其他的方向比如说算法之类的，还\n有在个人的技术知识里面是不是需要这些东西，不是特别需要。 也许这些都是我自己的猜测，如果你有什么\n好的建议，我也非常感谢。 最后，我想问一下，你是在哪里学的，是在哪个大学，我就是在这里问问。 谢\n谢。 2019.6.16\n电影《伪装者》中的重要概念\n原理：1. 无线电波的能量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无\n线电波的方向不会发生改变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改\n变，传播途中，波的方向不会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途\n中，波的方向会发生改变（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变\n&#x27;,
 &#x27;生改变，传播途中，波的方向不会发生改变\n（这就是遮挡）3. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）4. 无线\n电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）5. 无线电波的方向会发生改变，\n传播途中，波的方向不会发生改变（这就是遮挡）6. 无线电波的方向会发生改变，传播途中，波的方向不会\n发生改变（这就是遮挡）7. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）\n8. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）原理：1. 无线电波的能\n量不会被吸收，当无线电波被反射后，波的方向就会发生改变（这就是反射）2. 无线电波的方向不会发生改\n变，传播途中，波的方向会发生改变（这就是偏转）3. 无线电波的方向会发生改变，传播途中，波的方向不\n会发生改变（这就是遮挡）重要概念：1. 无线电波的方向不会发生改变，传播途中，波的方向会发生改变\n（这就是遮挡）2. 无线电波的方向会发生改变，传播途中，波的方向不会发生改变（这就是遮挡）3. 无线\n电波的方向会发生改变，传播途中，波的方向不会发生改变（这&#x27;,
 &#x27;改变，传播途中，波的方向不会发生改变（这\n除此之外，这种模板生成方式支持任意数量的变量，包括无变量，可以做如下测试：\nfrom langchain.prompts import PromptTemplate\nprompt_template = PromptTemplate.from_template(&quot;给我讲一个笑话&quot;)\nprompt_template.format()\n\&#x27;给我讲一个笑话\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;davinci-002&quot;, )\nresult = llm.invoke(prompt)\nprint(result)\n如果我是一个机器，会被称为什么？\n谢谢\n调用Chat Model\n上述示例展示了如何使用字符串模板生成和调用LLMs类模型。对于聊天模型，也需要使用 Python\n的 str.format() 方法进行模板化，但其构造的输入形式则有所区别，应采用聊天消息列表的形式，\n每条消息都包含内容和一个名为role的附加参数。例如，在OpenAI的Chat Completion&#x27;,
 &#x27;enAI的Chat Completions API中，聊天\n消息需要包含为assistant、user或system。在这种情况下，应使用ChatPromptTemplate来构建对话\n模板。\nfrom langchain_core.prompts import ChatPromptTemplate\nchat_template = ChatPromptTemplate.from_messages(\n[\n(&quot;system&quot;, &quot;你是一个有帮助的AI机器人，你的名字是{name}。&quot;),\n(&quot;human&quot;, &quot;你好，最近怎么样？&quot;),\n(&quot;ai&quot;, &quot;我很好，谢谢！&quot;),\n(&quot;human&quot;, &quot;{user_input}&quot;),\n]\n)\nmessages = chat_template.format_messages(name=&quot;小明&quot;, user_input=&quot;你叫什么名字？&quot;)\nmessages\n[SystemMessage(content=\&#x27;你是一个有帮助的AI机器人，你的名字是小明。\&#x27;),\nHumanMessage(content=\&#x27;你好，最近怎么样？\&#x27;),\nAIMessage(con&#x27;,
 &quot;么样？&#x27;),\nAIMessage(content=&#x27;我很好，谢谢！&#x27;),\nHumanMessage(content=&#x27;你叫什么名字？&#x27;)]\nprint(chat_template)\ninput_variables=[&#x27;name&#x27;, &#x27;user_input&#x27;] messages=\n[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;name&#x27;],\ntemplate=&#x27;你是一个有帮助的AI机器人，你的名字是{name}。&#x27;)),\nHumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[],\ntemplate=&#x27;你好，最近怎么样？&#x27;)),\nAIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;我很\n好，谢谢！&#x27;)), HumanMessagePromptTemplate(prompt=PromptTemplate(input_varia&quot;,
 &#x27;Template(input_variables=\n[\&#x27;user_input\&#x27;], template=\&#x27;{user_input}\&#x27;))]\n从输出上看，其构造函数在实例化prompt_template时，主要由两个关键参数进行指定：\ninput_variables：这是一个列表，包含模板中需要动态填充的变量名。这些变量名在模板字符串\n中以花括号（如{name}）标记。通过指定这些变量，可以在后续过程中动态地替换这些占位符。\ntemplate：这是定义具体提示文本的模板字符串。它可以包含静态文本和input_variables列表中\n指定的变量占位符。当调用format方法时，这些占位符会被实际的变量值替换，生成最终的提示\n文本。\n调用Chat Model，这里我们选择GPT 3.5。代码如下：\nfrom langchain.chat_models import ChatOpenAI\nllm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)\nresult = llm.invoke(messages)\nprint(result&#x27;,
 &#x27;ssages)\nprint(result.content)\n我叫小明，很高兴为您提供帮助！有什么问题可以帮到您？\nmessages = chat_template.format_messages(name=&quot;大熊&quot;, user_input=&quot;你要去哪里玩？&quot;)\nfrom langchain.chat_models import ChatOpenAI\nllm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, max_tokens=1000)\nresult = llm.invoke(messages)\nprint(result.content)\n作为一个AI机器人，我没有身体，所以无法去任何地方玩耍。但我可以帮助你规划旅行或提供有关旅游目的地\n的信息。有什么我可以帮助你的吗？\n通过上述流程，我们演示了如何在LangChain框架内使用ChatPromptTemplate来构建与聊天模型\n交互的对话模板。通过此方法，能够创建一个包含不同参与者角色和定制化消息内容的对话流程，这对\n于开发复杂的聊天应用或增强AI助手的交互能力至关重要。\n4.2.2 构造Few-S&#x27;,
 &#x27;力至关重要。\n4.2.2 构造Few-Shot模版\n在LangChain中，很多的功能抽象、链路抽象本质上都是在对大模型的“涌现能力”能够应用落地的\n一种具体实现方法，而其推理的不稳定，在不修改模型本身参数（微调）的情况下，模型涌现能力极度\n依赖对模型的提示过程，即对同样一个模型，不同的提示方法将获得质量完全不同的结果。最为简单的\n提示工程的方法就是通过输入一些类似问题和问题答案，让模型参考学习，并在同一个prompt的末尾\n提出新的问题，依次提升模型的推理能力。\n比如我们在《Ch.3 基于思维链的进阶提示工程》中测试的经典推理问题：\nimport os\nimport openai\nopenai.api_key = os.getenv(&quot;OPENAI_API_KEY&quot;)\nprompt = \&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关\n闭之前她能滑多少次？\&#x27;\nprompt\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？\&#x27;\nfrom openai import OpenAI\ncl&#x27;,
 &#x27;nai import OpenAI\nclient = OpenAI()\nresponse1 = client.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=prompt,\nmax_tokens=1000\n)\nresponse1.choices[0].text\n\&#x27;\\n在关闭之前，艾米能滑14次。\&#x27;\nfrom openai import OpenAI\nclient = OpenAI()\nresponse1 = client.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=prompt,\nmax_tokens=1000\n)\nresponse1.choices[0].text\n\&#x27;\\n\\n15分钟 = 900秒\\n\\n每次滑下来需要1分钟，所以在15分钟内，可以滑下900秒/（1分钟+4分钟）=\n900/5 = 180次。\&#x27;\n可以看到多次执行，其推理均是错误的。正确的计算过程应该是先计算艾米一次爬上爬下总共需要\n5分钟，然后滑梯还有15分钟关闭，因此关闭之&#x27;,
 &quot;钟，然后滑梯还有15分钟关闭，因此关闭之前能够再滑15/5=3次。\n而我们当时在OpenAI的API规范下，是通过这种方式来进行Few-Shot的提示：\nprompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共\n有多少个网球？” \\\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网\n球，因此现在总共由5+6=11个网球。因此答案是11。” \\\nQ：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，\n请问现在食堂总共有多少个苹果？” \\\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9\n个苹果，答案是9。” \\\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫\n球是蓝色的。请问总共有多少个蓝色高尔夫球？” \\\nA：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色\n的，也就是4个，答案是4个。” \\\nQ：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在\n15分钟后关闭，请问在关闭之前她能滑多少次？” \\&quot;,
 &quot;关闭，请问在关闭之前她能滑多少次？” \\\nA：&#x27;\nprompt_Few_shot_CoT4\n&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在\n总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果\n他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色\n的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高\n尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：\n“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？” A：&#x27;\n在Few-Shot的提示下，再次尝试进行提问：\nfrom openai import OpenAI\nclient = OpenA&quot;,
 &#x27;penAI\nclient = OpenAI()\nresponse1 = client.completions.create(\nmodel=&quot;gpt-3.5-turbo-instruct&quot;,\nprompt=prompt_Few_shot_CoT4,\nmax_tokens=1000,\n)\nresponse1.choices[0].text\n\&#x27;”艾米花了1分钟滑下来，滑一次上去需要4分钟，因此在15分钟关闭之前，她能滑15分钟-1分钟=14分钟，\n14分钟除以4分钟每次，她能滑14÷4=3次，答案是3次。“ \&#x27;\n从输出结果上看，通过Few-Shot提示可以激发大模型的涌现能力从而回答对复杂推理问题，在应用\n开发中同样需要这种能力来提升大模型的推理稳定性。所以LangChain抽象了\nFewShotPromptTemplate方法来实现。我们可以先测试下在LangChain的接口下直接提问：\n直接提问\nprompt1 = \&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在\n关闭之前她能滑多少次？\&#x27;\nprompt1\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1&#x27;,
 &#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=prompt1\n)\n\&#x27;\\n\\n在关闭之前，艾米能滑(15-4-1)/4=2次。\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=prompt1\n)\n\&#x27;\\n\\n在15分钟内，她能滑15次。\&#x27;\n其实也很容易想到，LangChain的工作仅仅是集成并接入OpenAI的GPT模型，因此其本质上与直接\n调用GPT模型的效果相同。出现错误回答也属于正常情况。接下来，我们根据LangChain对Few-Shot模\n板的定义规范，进行提示词的构建。\n在LangChain中，需要使用 PromptTemplate 创建字符串提示&#x27;,
 &#x27;omptTemplate 创建字符串提示模板。模板可以包括说明、少量示\n例以及适合给定任务的特定上下文和问题。因此，我们要创建一个少量示例的列表。每个示例都是一个\n字典，其中键是输入变量，值是这些输入变量的值。\n适合LLMs模型的模版构造形式\nfrom langchain.prompts.few_shot import FewShotPromptTemplate\nfrom langchain.prompts.prompt import PromptTemplate\nexamples = [\n{\n&quot;question&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个\n网球？&quot;,\n&quot;answer&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现\n在总共由5+6=11个网球。因此答案是11。&quot;\n},\n{\n&quot;question&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在\n食堂总共有多少个苹果？&quot;,\n&quot;answer&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果&#x27;,
 &#x27;买了6个，总共有23-20+6=9个苹果，答\n案是9。&quot;\n},\n{\n&quot;question&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色\n的。请问总共有多少个蓝色高尔夫球？&quot;,\n&quot;answer&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4\n个，答案是4个。&quot;\n},\n]\n接下来需要实例化一个 PromptTemplate 对象，将少量示例格式化为字符串。\nexample_prompt = PromptTemplate(\ninput_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:\n{question}\\n{answer}&quot;\n)\nprint(example_prompt.format(**examples[0]))\nQuestion: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。\n因此答案是11。\nexample_prompt = &#x27;,
 &#x27;1。\nexample_prompt = PromptTemplate(\ninput_variables=[&quot;question&quot;, &quot;answer&quot;], template=&quot;Question:\n{question}\\n{answer}&quot;\n)\nprint(example_prompt.format(**examples[1]))\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少\n个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。\n最后，创建一个 FewShotPromptTemplate 对象。该对象接受经过上一步格式化后的少数样本示\n例。\n# 导入 FewShotPromptTemplate 类\nfrom langchain.prompts.few_shot import FewShotPromptTemplate\n# 创建一个 FewShotPromptTemplate 对象\nfew_shot_prompt = FewShotPromptTemplate(\nexamples=&#x27;,
 &#x27;tTemplate(\nexamples=examples, # 最开始定义的 examples 示例\nexample_prompt=example_prompt, # 上一步定义的 example_prompt 作为提示模板\nsuffix=&quot;Question: {input}&quot;, # 后缀模板，其中 {input} 会被替换为实际输入\ninput_variables=[&quot;input&quot;] # 定义输入变量的列表\n)\n# 使用给定的输入格式化 prompt，并打印结果\nprint(few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，\n水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;))\nQuestion: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\n罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。\n因此答案是11。\nQuestion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少\n个苹果？&#x27;,
 &#x27;个苹果，请问现在食堂总共有多少\n个苹果？\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有\n多少个蓝色高尔夫球？\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关\n闭之前她能滑多少次？\nfinal_input = few_shot_prompt.format(input=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟\n才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)\nfinal_input\n\&#x27;Question: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\\n罗杰\n一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此\n答案是11。\\n\\nQuestion: 食堂总共有23个苹果，如果他&#x27;,
 &#x27;tion: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现\n在食堂总共有多少个苹果？\\n食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹\n果，答案是9。\\n\\nQuestion: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是\n蓝色的。请问总共有多少个蓝色高尔夫球？\\n总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是\n蓝色的，也就是4个，答案是4个。\\n\\nQuestion: 艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？\&#x27;\n将带有Few-Shot提示的输入，输入到大模型中执行推理。\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=final_input\n)\n\&#x27;\\n艾米需要4分钟爬到顶部，1分钟滑下来，一共花费5分钟。在15分钟内，她能滑3次，因为15÷5=3。因此\n答案是3次。&#x27;,
 &#x27;次，因为15÷5=3。因此\n答案是3次。\&#x27;\nfrom langchain_openai import OpenAI\nllm = OpenAI(model_name=&quot;gpt-3.5-turbo-instruct&quot;)\nllm.invoke(\ninput=final_input\n)\n\&#x27;\\n艾米花了5分钟（4分钟爬上去+1分钟滑下来），因此在15分钟内可以滑3次（15÷5=3）。答案是3次。\&#x27;\n从输出结果来看，显然受到了提示工程的正面影响，通过问题的拆解过程显著提高了回答的准确\n性。\n适合Chat Model的提示模版构造形式\n我们在前面解释过LLMs和Chat Model 的区别，因其接受的输入形式不同，所以构造的提示模版也\n是不一样的，对于Chat Model来说，它接收的是聊天形式的对话格式，所以对于其对应的Few-Shot，\n在LangChain中的基本结构如下：\nexamples ：要包含在最终提示中的字典示例列表。\nexample_prompt ：通过其 format_messages 方法将每个示例转换为 1 条或多条消息。一个常\n见的示例是将每个示例转换为一条人工消息和一条人&#x27;,
 &quot;例是将每个示例转换为一条人工消息和一条人工智能消息响应，或者一条人工消息后跟一条\n函数调用消息。\n我们还是使用四个数学推理题来进行测试：\nprompt_Few_shot_CoT4 = &#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共\n有多少个网球？” \\\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网\n球，因此现在总共由5+6=11个网球。因此答案是11。” \\\nQ：“食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，\n请问现在食堂总共有多少个苹果？” \\\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9\n个苹果，答案是9。” \\\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫\n球是蓝色的。请问总共有多少个蓝色高尔夫球？” \\\nA：“总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色\n的，也就是4个，答案是4个。” \\\nQ：“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在\n15分钟后关闭，请问在关闭之前她能滑多少次？” \\\nA：&#x27;\npro&quot;,
 &quot;之前她能滑多少次？” \\\nA：&#x27;\nprompt_Few_shot_CoT4\n&#x27;Q：“罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？”\nA：“罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在\n总共由5+6=11个网球。因此答案是11。” Q：“食堂总共有23个苹果，如果\n他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？”\nA：“食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。”\nQ：“杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色\n的。请问总共有多少个蓝色高尔夫球？” A：“总共有16个球，其中一半是高\n尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。” Q：\n“艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟后关闭，请问在关闭之前她能\n滑多少次？” A：&#x27;\n直接调用\n对于Chat Model，需要将输入转化成对话聊天格式：\nfrom langchain_core.messages import Hu&quot;,
 &#x27;e.messages import HumanMessage, SystemMessage\nmessages = [SystemMessage(content=&quot;你是一个擅长数学推理的专家&quot;),\nHumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水滑梯将在15分钟\n后关闭，请问在关闭之前她能滑多少次？&quot;),]\n实例化模型进行推理：\nfrom langchain_openai import ChatOpenAI\nchat = ChatOpenAI(model_name=&quot;gpt-3.5-turbo-0125&quot;)\nresonse = chat.invoke(messages)\nresonse.content\n\&#x27;首先，艾米花了1分钟滑下来，所以她每次滑下来后需要等待3分钟（4分钟上去 + 1分钟下来 = 5分钟，15\n分钟总时间 - 5分钟 = 10分钟，10分钟 / 3分钟 = 3次）。\\n\\n在水滑梯关闭之前，艾米可以滑3次。\&#x27;\n&amp;esmp; 实际上，在使用GPT-3.5时，我们可以观察到，即使不采用Few-Shot提示，模型也能以很高\n的&#x27;,
 &#x27;Few-Shot提示，模型也能以很高\n的概率正确回答问题，这归功于模型本身已经非常强大的能力。所以我们改为Baichuan2模型来测试：\nfrom langchain_community.chat_models import ChatBaichuan\nfrom langchain_core.messages import HumanMessage\nchat = ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-xxx&quot;,\nstreaming=True,\n)\nresponse = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])\nresponse.content\n\&#x27;艾米需要1分钟滑下来，所以她在滑梯关闭前可以滑15次。\&#x27;\nresponse = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少&#x27;,
 &#x27;15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])\nresponse.content\n\&#x27;艾米需要1分钟从滑梯顶部滑下来，所以她可以在滑梯关闭前滑15次。\&#x27;\nresponse = chat([HumanMessage(content=&quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下\n来，水滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;)])\nresponse.content\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，所以每次滑行需要4+1=5分钟。\\n\\n水滑梯将在\n15分钟后关闭，所以她能在关闭之前滑行15÷5=3次。\&#x27;\n可以看到，在切换到Baichuan模型进行测试时，三次测试中出现了两次错误回答。因此，我们就\n使用Baichuan2模型，尝试通过添加Few-Shot来探索是否能增强其推理能力。\n首先，安装LangChain的规范，导入生成示例的模块：\nfrom langchain.prompts import (\nChatPromptTemplate,\nFewShotChatMessagePromptTemplate,\n)\n接下来以input-outpu&#x27;,
 &#x27;e,\n)\n接下来以input-output对的形式构建Few-Shot提示模板。在这里，“input”可以被视作人类发出的\nPromot，而“output”则是大模型给出的回答。\nexamples = [\n{&quot;input&quot;: &quot;罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网\n球？&quot;,\n&quot;output&quot;: &quot;罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总\n共由5+6=11个网球。因此答案是11。&quot;},\n{&quot;input&quot;: &quot;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共\n有多少个苹果？&quot;,\n&quot;output&quot;: &quot;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是\n9。&quot;},\n{&quot;input&quot;: &quot;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问\n总共有多少个蓝色高尔夫球？&quot;,\n&quot;output&quot;: &quot;总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答\n案是4个。&quot;},\n]\n然后，使用from_me&#x27;,
 &#x27;个。&quot;},\n]\n然后，使用from_messages方法进行模版构造。\n# This is a prompt template used to format each individual example.\nexample_prompt = ChatPromptTemplate.from_messages(\n[\n(&quot;human&quot;, &quot;{input}&quot;),\n(&quot;ai&quot;, &quot;{output}&quot;),\n]\n)\nfew_shot_prompt = FewShotChatMessagePromptTemplate(\nexample_prompt=example_prompt,\nexamples=examples,\n)\nprint(few_shot_prompt.format())\nHuman: 罗杰有五个网球，他又买了两盒网球，每盒有3个网球，请问他现在总共有多少个网球？\nAI: 罗杰一开始有五个网球，又购买了两盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网\n球。因此答案是11。\nHuman: 食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有&#x27;,
 &#x27;，然后又买了6个苹果，请问现在食堂总共有多少个苹\n果？\nAI: 食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是9。\nHuman: 杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。请问总共有多少\n个蓝色高尔夫球？\nAI: 总共有16个球，其中一半是高尔夫球，也就是8个，其中一半是蓝色的，也就是4个，答案是4个。\n生成最终的输入模版，设置 input 作为动态变量：\nfinal_prompt = ChatPromptTemplate.from_messages(\n[\nfew_shot_prompt,\n(&quot;human&quot;, &quot;{input}&quot;),\n]\n)\nfinal_prompt\nChatPromptTemplate(input_variables=[\&#x27;input\&#x27;], messages=\n[FewShotChatMessagePromptTemplate(examples=[{\&#x27;input\&#x27;: \&#x27;罗杰有五个网球，他又买了两盒网\n球，每盒有3个网球，请问他现在总共有多少个网球？\&#x27;, \&#x27;output\&#x27;: \&#x27;罗杰一开始有五个网球&#x27;,
 &quot;output&#x27;: &#x27;罗杰一开始有五个网球，又购买了两\n盒网球，每盒3个，共购买了6个网球，因此现在总共由5+6=11个网球。因此答案是11。&#x27;}, {&#x27;input&#x27;:\n&#x27;食堂总共有23个苹果，如果他们用掉20个苹果，然后又买了6个苹果，请问现在食堂总共有多少个苹果？&#x27;,\n&#x27;output&#x27;: &#x27;食堂最初有23个苹果，用掉20个，然后又买了6个，总共有23-20+6=9个苹果，答案是\n9。&#x27;}, {&#x27;input&#x27;: &#x27;杂耍者可以杂耍16个球。其中一半的球是高尔夫球，其中一半的高尔夫球是蓝色的。\n请问总共有多少个蓝色高尔夫球？&#x27;, &#x27;output&#x27;: &#x27;总共有16个球，其中一半是高尔夫球，也就是8个，其中\n一半是蓝色的，也就是4个，答案是4个。&#x27;}],\nexample_prompt=ChatPromptTemplate(input_variables=[&#x27;input&#x27;, &#x27;output&#x27;], messages=\n[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;],\ntemplate=&#x27;{inp&quot;,
 &#x27;ut\&#x27;],\ntemplate=\&#x27;{input}\&#x27;)),\nAIMessagePromptTemplate(prompt=PromptTemplate(input_variables=[\&#x27;output\&#x27;],\ntemplate=\&#x27;{output}\&#x27;))])),\nHumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[\&#x27;input\&#x27;],\ntemplate=\&#x27;{input}\&#x27;))])\n实例化baichuan2模型：\n最后，使用LangChain中的chain的抽象，合并最终的提示和大模型共同执行。\nchain = final_prompt | ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-e9&quot;,\nstreaming=True,\n)\nresponse = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水\n滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})\nresponse.content\n\&#x27;艾&#x27;,
 &#x27;\nresponse.content\n\&#x27;艾米需要4分钟才能爬上去，1分钟滑下来，总共需要4+1=5分钟。15分钟可以滑15÷5=3次，所以答案是3\n次。\&#x27;\nchain = final_prompt | ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-ed54e9529&quot;,\nstreaming=True,\n)\nresponse = chain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水\n滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})\nresponse.content\n\&#x27;艾米爬上去需要4分钟，滑下来只需要1分钟，所以她可以在15分钟内完成4次滑行，因为每次滑行需要\n4+1=5分钟。\&#x27;\nchain = final_prompt | ChatBaichuan(\n# 这里替换成个人的有效 API KEY\nbaichuan_api_key=&quot;sk-ed54695e9529&quot;,\nstreaming=True,\n)\nresponse = chain.invoke({&quot;input&quot;&#x27;,
 &#x27;hain.invoke({&quot;input&quot;: &quot;艾米需要4分钟才能爬到滑梯顶部，她花了1分钟才滑下来，水\n滑梯将在15分钟后关闭，请问在关闭之前她能滑多少次？&quot;})\nresponse.content\n\&#x27;艾米需要4分钟才能爬到滑梯顶部，然后1分钟滑下来，总共需要4+1=5分钟。因此，在滑梯关闭之前，她可\n以滑15÷5=3次。\&#x27;\n4.3 LangChain中的Output Parsers\nOutput Parsers，即输出解析器，这个概念非常好理解，就是负责获取大模型的输出并将其转换为\n更合适的格式。这在应用开发中及其重要。在大多数复杂应用场景中，处理逻辑往往环环相扣，执行某\n项业务逻辑可能需要多次调用大模型，其中上一次的调用结果将被用于指导下一次调用的逻辑。在这种\n情况下，结构化的信息会比纯文本又有价值，同时这也是输出解析器的价值所在。\nLangChain构造的输出解释器必须实现两个主要方法：\nGet format instructions：该方法会返回一个字符串，其中包含有关如何格式化语言模型输出的指\n令。\nParse：该方法会接收字符串，并将其解析为某种结构\n目前已经支持的解析格式已&#x27;,
 &#x27;解析为某种结构\n目前已经支持的解析格式已经包括Json、Xml、Csv以及OpenAI的Tools和Functions等多种格式，\n具体可看：https://python.langchain.com/docs/modules/model_io/output_parsers/\n简而言之，这个过程本质上是对文本的格式化处理。但与传统的后处理不同，LangChain采取的实\n现策略是通过在Prompt上增加输出解析，直接引导大模型输出的格式化方式。\n我们以格式化日期来看一下LangChain的构造思路，首先导入相关的模块：\nfrom langchain.output_parsers import DatetimeOutputParser\nfrom langchain.prompts import PromptTemplate\nfrom langchain_openai import OpenAI\n实例化日期解析器：\noutput_parser = DatetimeOutputParser()\n通过.get_format_instructions方法，可以查看定义的解析格式：\noutput_p&#x27;,
 &#x27;以查看定义的解析格式：\noutput_parser.get_format_instructions()\n&quot;Write a datetime string that matches the following pattern: \&#x27;%Y-%m-\n%dT%H:%M:%S.%fZ\&#x27;.\\n\\nExamples: 1181-01-05T02:20:33.675873Z, 1938-12-\n13T09:46:36.791229Z, 1468-01-17T07:34:51.108688Z\\n\\nReturn ONLY this string, no\nother words!&quot;\n构造输入模版，这里的区别是：在输入的Prompt Template中，加入了OutPut Parse的内容：\ntemplate = &quot;&quot;&quot;用户发起的提问:\n{question}\n{format_instructions}&quot;&quot;&quot;\n生成最终实际输入大模型的Promot内容：\nprompt = PromptTemplate.from_template(\ntemplate,\n# 预定义的变量，这里我们传入格式化指令\npartial_va&#x27;,
 &#x27;我们传入格式化指令\npartial_variables={&quot;format_instructions&quot;:\noutput_parser.get_format_instructions()},\n)\nprompt\nPromptTemplate(input_variables=[\&#x27;question\&#x27;], partial_variables=\n{\&#x27;format_instructions\&#x27;: &quot;Write a datetime string that matches the following\npattern: \&#x27;%Y-%m-%dT%H:%M:%S.%fZ\&#x27;.\\n\\nExamples: 1472-10-23T17:39:56.199188Z,\n1643-01-14T10:55:11.903091Z, 0631-12-11T23:04:04.345333Z\\n\\nReturn ONLY this\nstring, no other words!&quot;}, template=\&#x27;用户发起的提\n问:\\n\\n{question}\\n\\n{format_instructions}\&#x27;)\n最后，使用LangChain中的&#x27;,
 &#x27;}\&#x27;)\n最后，使用LangChain中的chain的抽象，合并最终的提示、大模型实例及OutPut Parse共同执\n行。\nchain = prompt | OpenAI() | output_parser\noutput = chain.invoke({&quot;question&quot;: &quot;你好，请问你叫什么？&quot;})\nprint(output)\n2021-10-07 12:02:13.345333\nchain = prompt | OpenAI() | output_parser\noutput = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是机器学习&quot;})\nprint(output)\n2018-01-23 09:42:32.123456\nchain = prompt | OpenAI() | output_parser\noutput = chain.invoke({&quot;question&quot;: &quot;你好，请你帮我解释一下什么是深度学习&quot;})\nprint(output)\n2019-08-07 18:30:00\n通过测试可以明显观察到，以此方式定义的输出解析器极大地规范了&#x27;,
 &#x27;到，以此方式定义的输出解析器极大地规范了模型的输出。当然，这也直接\n受到大模型实际能力和模板定义情况的影响。\nLangChain中不同模版的使用方式，除了可以在官方文档上看到，在其源码上看会更加直观，而且\n可以自定义修改，其所有的OutPut Parse的模版存储路径为：\nlangchain/libs/langchain/langchain/output_parsers ， 大家可以自行尝试。比如我们上面使\n用的日期解析的源码对应如下：\n到此为止，我们已经非常详尽地介绍了LangChain的核心概念。如果有学习过大模型技术实战课程\n的同学来说，会发现LangChain的Model I/O模块中讨论的所有概念都是之前接触过的，需要学习的主\n要是LangChain对大模型进行抽象的方法以及在LangChain框架下的应用开发技巧。\n而从LangChain的整体开源情况看，其内容丰富且迭代非常快，通过模块化的方式介绍，可以帮助\n大家快速构建起对LangChain以及应用开发的思维框架。尽管在Model I/O模块中还有许多更高级的内\n容，例如如何接入本地部署的开源大模型，关于函数调用、Mem&#x27;,
 &#x27;地部署的开源大模型，关于函数调用、Memory缓存等更深入的主题，这些内容我\n们将在后续章节中一一接触并进行详细的探讨。\n&#x27;]
```

```
# 对每个chunk进行embedding，并存储结果
results = []
for i, chunk in enumerate(chunks):
    embedding = get_embeddings(chunk)  # 获取嵌入向量
    result = {
        &#x27;chunks&#x27;: i + 1,         # Chunk编号
        &#x27;page_content&#x27;: chunk,   # 原始文本内容
        &#x27;embedding&#x27;: embedding,  # 嵌入向量
    }
    results.append(result)
```

```
results[0]
```

```
{&#x27;chunks&#x27;: 1,
 &#x27;page_content&#x27;: &#x27;开源大模型应用开发实战\nCh 18 LangChain整体项目介绍与核心模块Model I/O详\n解\n注意：大模型技术正处于快速发展阶段，因此在实际使用中可能会遇到与教材内容不一致的情\n况。如遇到此类情况，请根据实际情况灵活调整，或及时与我们联系。我们在接到相关反馈后，\n将迅速更新教材内容以确保信息的准确性和实用性。\n本期课件更新时间：2024年3月7日\n在开源大模型系列课程中，截止到本课时我们已经投入了超过20小时的时间去深入讲解如何在本地\n私有化部署开源大模型。在这一部分，我们分别选择了ChaGLM3、Qwen和baichuan2这三个最受大家\n关注的中文开源大模型作为部署示例，无论是Ubuntu还是Windows用户，我们都提供了非常详尽的部\n署教程，通过逐步指导，确保了大家能够根据自己的实际需求，选择最适合的模型并成功完成本地部\n署。\n但大家需要明确的是，私有化部署大模型这个过程，只是开发大模型应用最基础，最核心，同时也\n是最简单的一步。熟练掌握基于大模型的上层应用开发还有非常长的路需要走。目前，大模型技术开发\n总体上分为两个方向，一类是基于OpenAI、GLM4这种在线API&#x27;,
 &#x27;embedding&#x27;: [[-0.012603002600371838,
   0.04118858277797699,
   0.06209971010684967,
   -0.01939735934138298,
   0.01729685813188553,
   -0.010144595056772232,
   0.002895522164180875,
   -0.009153018705546856,
   -0.011599690653383732,
   0.023340197280049324,
   0.03813757747411728,
   -0.03579064831137657,
   -0.042807962745428085,
   -0.02677844651043415,
   0.019772866740822792,
   0.002112234942615032,
   0.012767286971211433,
   -0.02696620114147663,
   -0.031214140355587006,
   0.043042656034231186,
   0.018728483468294144,
   0.017062164843082428,
   -0.020031029358506203,
   0.015489723533391953,
   -0.024619271978735924,
   -0.012673410587012768,
   -0.017238184809684753,
   0.036917176097631454,
   -0.008214247412979603,
   -0.051116086542606354,
   -0.0029263256583362818,
   -0.02216673269867897,
   -0.028491703793406487,
   0.01864634081721306,
   -0.01916266605257988,
   0.03339678421616554,
   0.0037462834734469652,
   0.03576717898249626,
   0.009874698705971241,
   0.011364998295903206,
   0.013882078230381012,
   -0.011247651651501656,
   -0.051209963858127594,
   0.039522264152765274,
   -0.0032768980599939823,
   0.03158964961767197,
   -0.03325596824288368,
   0.006905835121870041,
   -0.021756021305918694,
   0.009610669687390327,
   -0.0016369821969419718,
   0.01641676016151905,
   0.01807134412229061,
   -0.05083445832133293,
   -0.03611921891570091,
   0.01665145345032215,
   -0.007005579769611359,
   0.03766819089651108,
   -0.008355063386261463,
   -0.003822558792307973,
   0.027224361896514893,
   -0.012919837608933449,
   -0.0195616427809,
   0.06468132883310318,
   -0.024267233908176422,
   -0.024807026609778404,
   -0.040202874690294266,
   -0.03496922552585602,
   -0.036752890795469284,
   -0.0032475613988935947,
   -0.05510586500167847,
   -0.020969800651073456,
   0.011670098640024662,
   0.012368309311568737,
   -0.01022087037563324,
   -0.004171664360910654,
   -0.04226817190647125,
   0.0693751871585846,
   -0.03459371626377106,
   -0.012180555611848831,
   -0.04919160529971123,
   0.011077499017119408,
   0.02167387865483761,
   -0.03243454173207283,
   0.005723570007830858,
   -0.06702826172113419,
   -0.030275369063019753,
   0.006407112814486027,
   -0.010126993991434574,
   -0.01771930418908596,
   -0.03334984555840492,
   0.015583600848913193,
   0.01340095791965723,
   0.003884165547788143,
   0.024454988539218903,
   0.01760195940732956,
   -0.007363486103713512,
   0.03588452562689781,
   0.03356106951832771,
   0.06773233413696289,
   0.054730355739593506,
   -0.012556063942611217,
   0.04118858277797699,
   -0.021357042714953423,
   0.009962708689272404,
   -0.03919369354844093,
   0.024595802649855614,
   0.008255318738520145,
   -0.054589539766311646,
   -0.006477520801126957,
   -0.07439760863780975,
   -0.024736618623137474,
   -0.036612074822187424,
   0.002464274177327752,
   0.01491472590714693,
   0.015255031175911427,
   0.03266923502087593,
   -0.030463123694062233,
   0.02410294860601425,
   -0.05590382218360901,
   -0.016475433483719826,
   0.03546207770705223,
   -0.007246139459311962,
   -0.0379498228430748,
   0.027247831225395203,
   -0.03290392830967903,
   0.03356106951832771,
   -0.012215759605169296,
   -0.002169441431760788,
   -0.004280209541320801,
   0.0264264065772295,
   0.013635650277137756,
   0.02945394441485405,
   -0.018399914726614952,
   -0.020183579996228218,
   -0.046023253351449966,
   -0.051256902515888214,
   0.025675389915704727,
   -0.039991650730371475,
   -0.010983622632920742,
   0.051350779831409454,
   -0.06463439017534256,
   -0.02237795665860176,
   0.01667492277920246,
   -0.06740376353263855,
   -0.023035096004605293,
   -0.021603470668196678,
   0.0027341709937900305,
   -0.02926618978381157,
   0.0017689968226477504,
   -0.01326014194637537,
   -0.049003854393959045,
   -0.0016575177432969213,
   -0.012849429622292519,
   -0.000708845560438931,
   -0.028233541175723076,
   -0.0019978221971541643,
   0.024431519210338593,
   0.03503963351249695,
   -0.03168352693319321,
   0.025041719898581505,
   0.030486593022942543,
   -0.03827839344739914,
   0.003411846235394478,
   -0.02242489531636238,
   -0.007551240269094706,
   -0.04196307063102722,
   0.0028573847375810146,
   -0.057265039533376694,
   -0.03569677099585533,
   0.006899967789649963,
   0.0016311148647218943,
   -0.0024422716815024614,
   -0.07571189105510712,
   -0.02405600994825363,
   -0.010737194679677486,
   0.009387711063027382,
   -0.07115884870290756,
   -0.009598935022950172,
   -0.04611713066697121,
   0.0024510726798325777,
   0.014797380194067955,
   -0.060879308730363846,
   0.002414402086287737,
   -0.0584385022521019,
   0.003837226890027523,
   -0.019960621371865273,
   0.03487534821033478,
   -0.011241784319281578,
   0.027670279145240784,
   -0.030463123694062233,
   0.0034998562186956406,
   0.0707833394408226,
   0.05397934094071388,
   0.06266297399997711,
   -0.036612074822187424,
   0.03518044948577881,
   2.990957727888599e-05,
   0.03264576569199562,
   0.009798423387110233,
   0.011018825694918633,
   0.015431050211191177,
   -0.0035291926469653845,
   -0.0831281840801239,
   0.004415158182382584,
   0.004855206701904535,
   0.010584644041955471,
   0.03480494022369385,
   -0.023809581995010376,
   -0.009446384385228157,
   0.012485655955970287,
   -0.0039164358749985695,
   0.06397724896669388,
   0.015501458197832108,
   -0.03182434290647507,
   -0.020570822060108185,
   -0.006853029131889343,
   -0.02107541263103485,
   -0.018869299441576004,
   0.03447636961936951,
   0.007498434279114008,
   -0.025769267231225967,
   -0.002295588841661811,
   0.027294769883155823,
   0.012450451962649822,
   0.03208250179886818,
   0.024760087952017784,
   -0.037104927003383636,
   0.0877751037478447,
   -0.04743141308426857,
   0.02249530330300331,
   -0.001130192424170673,
   0.012145351618528366,
   -0.06289766728878021,
   -0.00364947272464633,
   -0.04440387338399887,
   0.008349196054041386,
   -0.027998849749565125,
   0.012896368280053139,
   -0.04484979063272476,
   -0.010068320669233799,
   0.04088348150253296,
   -0.02307029999792576,
   -0.039780426770448685,
   0.028397826477885246,
   0.007832871749997139,
   0.006313235964626074,
   -0.051397718489170074,
   0.00951092503964901,
   0.01729685813188553,
   0.07796493917703629,
   0.02673150785267353,
   0.013048918917775154,
   -0.01615859754383564,
   -0.0010267809266224504,
   -0.022999892011284828,
   -0.0010319148423150182,
   0.04707937315106392,
   -0.027576401829719543,
   -0.039616141468286514,
   -0.03712839633226395,
   -0.0026681635063141584,
   -0.02917231246829033,
   0.008619092404842377,
   -0.007768331095576286,
   0.012192290276288986,
   -0.04501407593488693,
   0.04998956248164177,
   -0.017355531454086304,
   -0.03344372287392616,
   0.03825492411851883,
   0.0018306036945432425,
   0.05008343979716301,
   -0.017942262813448906,
   -0.03820798546075821,
   -0.021427450701594353,
   -0.0022017117589712143,
   0.03874778002500534,
   0.015313703566789627,
   -0.018869299441576004,
   -0.0017499281093478203,
   -0.003922303207218647,
   0.036659013479948044,
   0.04698549583554268,
   0.003593733301386237,
   0.03717533499002457,
   -0.007199201267212629,
   0.028257010504603386,
   -0.027599871158599854,
   -0.03898246958851814,
   0.030416185036301613,
   0.02931312844157219,
   -0.051491595804691315,
   -0.04381714388728142,
   0.051491595804691315,
   -0.006588999647647142,
   -0.006412980146706104,
   0.00877751037478447,
   -0.006014002487063408,
   0.010297145694494247,
   0.019725928083062172,
   0.024689679965376854,
   -0.03921716287732124,
   -0.01448054425418377,
   0.018411649391055107,
   -0.03485187888145447,
   0.028045786544680595,
   -0.018822360783815384,
   0.02240142598748207,
   -0.029993737116456032,
   -0.008812714368104935,
   0.0009057674324139953,
   -0.005623825825750828,
   -0.0008008891018107533,
   -0.03262229636311531,
   -0.04219776391983032,
   -0.01890450343489647,
   0.05294669046998024,
   0.004024981055408716,
   -0.003922303207218647,
   -0.036752890795469284,
   -0.033795759081840515,
   -0.00282071391120553,
   -0.003153684316202998,
   -0.0372457429766655,
   0.020899392664432526,
   0.02321111597120762,
   -0.000968107720836997,
   0.024877434596419334,
   -0.00725200679153204,
   0.014656564220786095,
   0.01310759223997593,
   -0.006489255465567112,
   0.015736151486635208,
   0.022272344678640366,
   -0.014492278918623924,
   -0.02346927858889103,
   0.04677427187561989,
   0.05642014369368553,
   0.0025889547541737556,
   -0.0447089746594429,
   0.019432561472058296,
   -0.015149419195950031,
   0.040367159992456436,
   -0.005207246169447899,
   0.0017425939440727234,
   0.025393759831786156,
   -0.010191533714532852,
   0.0328335203230381,
   -0.000751750310882926,
   -0.016006046906113625,
   -0.03219984844326973,
   -0.0011741972994059324,
   0.009065008722245693,
   0.02368050254881382,
   -0.020289190113544464,
   0.016428494825959206,
   -0.0316600576043129,
   0.03733962029218674,
   -0.007885677739977837,
   -0.048722222447395325,
   0.046328354626894,
   0.039616141468286514,
   -0.031073324382305145,
   0.04351204261183739,
   0.006500990130007267,
   -0.024947842583060265,
   0.002521480666473508,
   -0.008924192748963833,
   -0.031401894986629486,
   0.012567798607051373,
   0.03806716948747635,
   -0.051585473120212555,
   -0.04668039456009865,
   -0.05430791154503822,
   0.01615859754383564,
   0.04170490801334381,
   0.02940700575709343,
   -0.01909225806593895,
   0.040367159992456436,
   0.0386539027094841,
   -0.02179122529923916,
   0.011417804285883904,
   -0.01797746680676937,
   0.0053891330026090145,
   0.005130970850586891,
   -0.00022460833133663982,
   -0.020077968016266823,
   0.012309635989367962,
   0.008378532715141773,
   0.023481013253331184,
   0.020031029358506203,
   0.0024276033509522676,
   0.0035673303063958883,
   0.03452330827713013,
   -0.019972356036305428,
   0.05782829970121384,
   0.01045556366443634,
   0.03804370015859604,
   0.0069586411118507385,
   0.0023014561738818884,
   -0.013917282223701477,
   -0.05538749694824219,
   -0.024924373254179955,
   0.021345308050513268,
   0.05322832241654396,
   0.03187128156423569,
   0.025135597214102745,
   -0.027717217803001404,
   0.025018250569701195,
   0.02663763053715229,
   -0.05271199718117714,
   0.04109470546245575,
   -0.013952486217021942,
   0.01767236739397049,
   -0.028210071846842766,
   -0.018223894760012627,
   5.844400220667012e-05,
   0.051960982382297516,
   0.015231561847031116,
   0.027787625789642334,
   -0.01326014194637537,
   -0.007340016774833202,
   -0.03527432680130005,
   -0.060456860810518265,
   0.03738655894994736,
   0.07233231514692307,
   0.03360800817608833,
   -0.01315453089773655,
   -0.0784812644124031,
   -0.021521328017115593,
   0.004382887855172157,
   0.010326482355594635,
   -0.007363486103713512,
   -0.03452330827713013,
   -0.01679226942360401,
   -0.0028368490748107433,
   -0.003960440866649151,
   0.045577339828014374,
   -9.25936401472427e-05,
   0.003584932303056121,
   -0.018376445397734642,
   -0.051444657146930695,
   -0.019925417378544807,
   -0.01184025127440691,
   -0.001967018935829401,
   -0.07453842461109161,
   -0.02307029999792576,
   -0.00168392073828727,
   -0.018446853384375572,
   -0.0086132250726223,
   -0.0454130545258522,
   0.05097527056932449,
   0.004192200023680925,
   0.03229372575879097,
   -0.02701313979923725,
   0.05538749694824219,
   -0.020066233351826668,
   0.009833627380430698,
   0.0346875935792923,
   0.01823562942445278,
   0.059987474232912064,
   -0.07200374454259872,
   0.024783557280898094,
   0.0027107016649097204,
   0.0356263630092144,
   0.030064145103096962,
   -0.005764641333371401,
   0.042385514825582504,
   0.0029248588252812624,
   0.024290703237056732,
   -0.00822011474519968,
   -0.05613851174712181,
   -0.04332428798079491,
   -0.03351413086056709,
   -0.04064878821372986,
   0.00450023403391242,
   -0.01753155142068863,
   -0.04775997996330261,
   -0.020852454006671906,
   -0.028327418491244316,
   0.004506101366132498,
   0.014292790554463863,
   0.051491595804691315,
   -0.005192577838897705,
   0.14841970801353455,
   -0.06242828071117401,
   -0.006072675809264183,
   -0.0012717414647340775,
   0.015442784875631332,
   -0.028538642451167107,
   0.003931104205548763,
   0.027787625789642334,
   -0.010842806659638882,
   0.02954782173037529,
   -0.008806847035884857,
   -0.0344529002904892,
   0.005172042176127434,
   -0.03250494971871376,
   0.048487529158592224,
   0.016522372141480446,
   -0.011717037297785282,
   -0.0067826216109097,
   0.003370775142684579,
   -0.012732082977890968,
   -0.018634606152772903,
   -0.006653540302067995,
   0.015102480538189411,
   0.0209580659866333,
   0.006266297306865454,
   -0.03271617367863655,
   -0.017801446840167046,
   0.008671898394823074,
   0.020160110667347908,
   0.006999712437391281,
   0.0363304428756237,
   -0.024361111223697662,
   0.051303841173648834,
   0.016217270866036415,
   0.03438249230384827,
   0.021345308050513268,
   0.019479500129818916,
   -0.020547352731227875,
   0.011265253648161888,
   -0.02621518447995186,
   -0.0011169909266754985,
   -0.012861164286732674,
   -0.0002312090655323118,
   -0.010572909377515316,
   -0.001679520239122212,
   0.02039480209350586,
   0.005309924017637968,
   -0.005949461832642555,
   -0.02346927858889103,
   -0.01932695135474205,
   0.006770886946469545,
   -0.004731993190944195,
   -0.03593146428465843,
   -0.002423203084617853,
   -0.016874410212039948,
   0.02358662523329258,
   0.03642432019114494,
   0.02644987590610981,
   0.02982945181429386,
   -0.008695367723703384,
   -0.0018071343656629324,
   -0.04170490801334381,
   -0.019937152042984962,
   0.014773910865187645,
   0.0019083457300439477,
   0.036541666835546494,
   -0.045882437378168106,
   0.0026109572499990463,
   -0.013999424874782562,
   0.01646369881927967,
   0.05346301570534706,
   -0.02207285538315773,
   -0.022753465920686722,
   -0.025346821174025536,
   -0.007005579769611359,
   0.0022163798566907644,
   0.006154817994683981,
   0.02715395577251911,
   0.01597084291279316,
   -0.009211692027747631,
   0.0034529175609350204,
   0.020922861993312836,
   -0.06571397930383682,
   -0.01615859754383564,
   -0.008202512748539448,
   -0.006319103296846151,
   0.027177423238754272,
   -0.019456030800938606,
   0.07463230192661285,
   0.006377776153385639,
   -0.008261186070740223,
   0.025276413187384605,
   0.009540261700749397,
   0.03205903246998787,
   -0.017355531454086304,
   -0.021662143990397453,
   -0.0074925669468939304,
   -0.027623340487480164,
   -0.015466254204511642,
   -0.03895900025963783,
   -0.007128793280571699,
   0.004268474876880646,
   0.020946331322193146,
   -0.01665145345032215,
   0.017156042158603668,
   0.06698132306337357,
   -0.004051384050399065,
   -0.01641676016151905,
   -0.02235448732972145,
   -0.015442784875631332,
   -0.009563731029629707,
   0.021509593352675438,
   -0.002559618093073368,
   -0.013788200914859772,
   -0.02093459665775299,
   0.015184623189270496,
   -0.01646369881927967,
   -0.019338686019182205,
   0.003053939901292324,
   -0.004558907356113195,
   0.004201001022011042,
   -0.007768331095576286,
   -0.004397556185722351,
   -0.00671221362426877,
   0.0037022787146270275,
   0.01892797276377678,
   0.02233101800084114,
   0.011670098640024662,
   -0.03201209381222725,
   0.007639250252395868,
   0.03198862448334694,
   0.012157086282968521,
   -0.011763975955545902,
   0.05632626637816429,
   0.0014976332895457745,
   -0.01859940215945244,
   -0.0160647202283144,
   0.039311040192842484,
   -0.039498794823884964,
   0.040414098650217056,
   -0.013095857575535774,
   -0.006606601644307375,
   -0.009587200358510017,
   -0.001469763577915728,
   0.02323458530008793,
   -0.006964508444070816,
   -0.0022941220086067915,
   0.04522529989480972,
   -0.036846768110990524,
   0.03457024693489075,
   -0.009540261700749397,
   0.0002319424820598215,
   -0.007474964950233698,
   0.002085832180455327,
   -0.04529570788145065,
   0.028562111780047417,
   0.009088478051126003,
   0.012157086282968521,
   -0.05416709557175636,
   0.046328354626894,
   -0.00411005737259984,
   -0.006964508444070816,
   0.0323641337454319,
   -0.028303949162364006,
   -0.023152442649006844,
   -0.016686657443642616,
   -0.021943775936961174,
   0.005348061677068472,
   -0.007034915965050459,
   0.014750441536307335,
   -0.016874410212039948,
   0.03597840294241905,
   -0.008654296398162842,
   0.009622404351830482,
   0.05266505852341652,
   -0.025065189227461815,
   -0.009669343009591103,
   -0.015067276544868946,
   0.01869327947497368,
   -0.03358453884720802,
   0.011230049654841423,
   -0.004966686014086008,
   0.03752737492322922,
   0.002311724005267024,
   -0.02936006709933281,
   -0.03158964961767197,
   0.027693748474121094,
   0.019268278032541275,
   0.04198653995990753,
   0.024173356592655182,
   0.008947662077844143,
   0.0037638854701071978,
   -0.051303841173648834,
   0.012098412960767746,
   0.036964114755392075,
   0.006178287323564291,
   0.014386667869985104,
   -0.04161103069782257,
   0.003711079712957144,
   0.046328354626894,
   0.025792736560106277,
   0.010643317364156246,
   0.0025288148317486048,
   0.0010099123464897275,
   -0.03914675489068031,
   0.018435118719935417,
   -0.02978251315653324,
   -0.01644022949039936,
   -0.024947842583060265,
   0.0057206363417208195,
   0.0218851026147604,
   0.025276413187384605,
   0.023833051323890686,
   -0.01720298081636429,
   0.008724704384803772,
   -0.014633094891905785,
   0.019937152042984962,
   -0.020253987982869148,
   -0.04534264653921127,
   -0.023833051323890686,
   -0.013048918917775154,
   -0.0006021336885169148,
   -0.004916813690215349,
   0.04381714388728142,
   -0.021298369392752647,
   -0.01658104546368122,
   -0.011717037297785282,
   -0.009827760048210621,
   0.007680321112275124,
   -0.009018070064485073,
   -0.020523883402347565,
   0.02898455783724785,
   0.0032739643938839436,
   0.000849294476211071,
   -0.003244627732783556,
   -0.04527223855257034,
   0.004101256374269724,
   0.011593823321163654,
   -0.02349274791777134,
   -0.02169734798371792,
   0.001855539856478572,
   -0.006911702454090118,
   0.03255188837647438,
   -0.0268957931548357,
   0.01098948996514082,
   0.021591736003756523,
   0.0005570286302827299,
   0.01904531940817833,
   0.010091789998114109,
   0.008484143763780594,
   -0.013048918917775154,
   -0.020946331322193146,
   0.034241676330566406,
   -0.00583798298612237,
   0.012579533271491528,
   0.0005053228815086186,
   0.001774864154867828,
   -0.03454677760601044,
   0.007697923108935356,
   0.008448939770460129,
   -0.039968181401491165,
   0.016451964154839516,
   0.07167517393827438,
   0.011587955988943577,
   -0.009469853714108467,
   -0.028843743726611137,
   0.00792674906551838,
   -0.007234404794871807,
   0.011934127658605576,
   0.010531838983297348,
   -0.024220295250415802,
   0.01625247485935688,
   0.00741629209369421,
   -0.0004103456740267575,
   -0.0030715418979525566,
   0.005210179835557938,
   -0.0017998003168031573,
   0.0447089746594429,
   0.014386667869985104,
   0.012110147625207901,
   -0.012168820947408676,
   -0.028609050437808037,
   0.010508369654417038,
   0.001886343234218657,
   -0.024454988539218903,
   -0.028632519766688347,
   -0.004174598027020693,
   0.021439185366034508,
   0.039616141468286514,
   0.02940700575709343,
   -0.0082670534029603,
   -0.002046227687969804,
   -0.001741127111017704,
   0.01162902731448412,
   -0.0031918217428028584,
   0.02077031135559082,
   0.0006179020856507123,
   -0.00675915228202939,
   -0.025769267231225967,
   0.02668456919491291,
   -0.004541305359452963,
   0.027224361896514893,
   0.00833746138960123,
   -0.002747372491285205,
   -0.004159929696470499,
   -0.030275369063019753,
   -0.022952953353524208,
   -0.002740038326010108,
   0.0023865322582423687,
   0.003954573534429073,
   0.005280587822198868,
   0.0007891544955782592,
   -0.06702826172113419,
   -0.016980022192001343,
   0.006747417617589235,
   -0.0007066453108564019,
   -0.015384111553430557,
   0.016287678852677345,
   0.005673698149621487,
   0.019127462059259415,
   0.016381556168198586,
   -0.00953439436852932,
   -0.003807890461757779,
   -0.04975486919283867,
   -0.0008514947257936001,
   0.049050793051719666,
   0.03173046559095383,
   -0.05327526107430458,
   0.04365285858511925,
   0.0019024783978238702,
   0.011200712993741035,
   -0.003018735907971859,
   0.017613694071769714,
   0.00036285704118199646,
   0.03499269485473633,
   0.014433606527745724,
   -0.018986646085977554,
   -0.039451856166124344,
   -0.011265253648161888,
   -0.01823562942445278,
   0.007134660612791777,
   -0.04757222533226013,
   -0.027951911091804504,
   0.010584644041955471,
   0.07322414964437485,
   -0.0010905880481004715,
   -0.0017660631565377116,
   0.0295712910592556,
   -0.007821137085556984,
   0.01913919672369957,
   0.01104816235601902,
   -0.04081307351589203,
   -0.01928001269698143,
   -0.0180478747934103,
   0.012919837608933449,
   -0.05055282637476921,
   0.030251899734139442,
   0.01618206687271595,
   -0.016874410212039948,
   -0.024900903925299644,
   -0.00953439436852932,
   -0.00364947272464633,
   0.025910083204507828,
   0.008548684418201447,
   0.0014814982423558831,
   0.03783247619867325,
   0.0050224256701767445,
   0.011364998295903206,
   0.0026109572499990463,
   -0.00040594517486169934,
   -0.0008742306381464005,
   0.006090277805924416,
   -0.006606601644307375,
   0.0299233291298151,
   -0.010666786693036556,
   -0.014609625563025475,
   -0.02931312844157219,
   -0.0033854434732347727,
   0.02893761917948723,
   0.04628141596913338,
   -0.08509960025548935,
   -0.006278031971305609,
   -0.010537706315517426,
   0.0008500278927385807,
   0.02936006709933281,
   0.004564774688333273,
   0.00969867967069149,
   0.0060316044837236404,
   0.0010715192183852196,
   0.03919369354844093,
   -0.004811202175915241,
   -0.02698967047035694,
   0.009657608345150948,
   -0.031378425657749176,
   -0.004702656529843807,
   0.008971131406724453,
   -0.02954782173037529,
   -0.002145972102880478,
   -0.001774864154867828,
   -0.00369641138240695,
   -0.011488211341202259,
   0.012074943631887436,
   0.05510586500167847,
   0.015360642224550247,
   -0.022718261927366257,
   -0.0056883664801716805,
   0.01790705882012844,
   4.7167748562060297e-05,
   -0.015689212828874588,
   -0.014151974581182003,
   0.039897773414850235,
   0.0027151021640747786,
   0.02167387865483761,
   0.010261941701173782,
   -0.010725460015237331,
   -0.013729527592658997,
   -0.00944051705300808,
   0.01038515567779541,
   0.02193204127252102,
   -0.028163133189082146,
   -0.010713725350797176,
   -0.042925309389829636,
   0.011511680670082569,
   0.028679458424448967,
   -0.022131528705358505,
   -0.007122925948351622,
   -0.006248695310205221,
   0.02389172464609146,
   -0.024267233908176422,
   0.0060550738126039505,
   -0.014985133893787861,
   -0.01736726611852646,
   0.00369641138240695,
   0.01655757613480091,
   -0.012908102944493294,
   -0.0029292593244463205,
   -0.01128285564482212,
   -0.002754706423729658,
   0.020641230046749115,
   0.01809481345117092,
   -0.005339260678738356,
   -0.028257010504603386,
   -0.01466829888522625,
   -0.028702927753329277,
   -0.0012079344596713781,
   0.02100500464439392,
   -0.0591425821185112,
   0.007991288788616657,
   0.02386825531721115,
   0.036705952137708664,
   -0.04395795986056328,
   0.019878478720784187,
   0.016006046906113625,
   0.011247651651501656,
   0.011276988312602043,
   -0.005521147511899471,
   0.045905906707048416,
   -0.005632626824080944,
   -0.027365177869796753,
   0.018705014139413834,
   -0.024619271978735924,
   0.015912171453237534,
   0.04660998657345772,
   0.016991756856441498,
   0.0037286814767867327,
   0.03337331488728523,
   -0.004444494377821684,
   -0.023551421239972115,
   -0.03590799495577812,
   -0.05707728490233421,
   0.00880097970366478,
   0.01459789089858532,
   -0.017543286085128784,
   0.039639610797166824,
   -0.010179799050092697,
   0.008918325416743755,
   -0.005981732159852982,
   -0.008091033436357975,
   0.014445341192185879,
   -0.016921348869800568,
   -0.0475018173456192,
   0.03245801106095314,
   0.013248407281935215,
   0.021016739308834076,
   -0.012896368280053139,
   0.0021987780928611755,
   -0.008349196054041386,
   0.04372326657176018,
   0.00973388273268938,
   -0.021192757412791252,
   -0.004177531693130732,
   0.01667492277920246,
   0.0037932221312075853,
   0.0044738310389220715,
   -0.0017513949424028397,
   0.030533531680703163,
   0.030979447066783905,
   -0.0316600576043129,
   -0.0032739643938839436,
   -0.02237795665860176,
   0.0003637738118413836,
   -0.02069990336894989,
   0.019984090700745583,
   0.028538642451167107,
   -0.030979447066783905,
   0.03334984555840492,
   0.0026725640054792166,
   -0.04970793053507805,
   -0.034147799015045166,
   0.01883409544825554,
   -0.011124437674880028,
   -0.027670279145240784,
   -0.04226817190647125,
   0.021568266674876213,
   0.01473870687186718,
   ...]]}
```

# 4. Retrieval过程

Retrieval的过程是如何根据用户提出的不同问题，在偌大的文本中找到与该问题最相关的片段。这就涉及到两个Embedding向量之间是如何衡量它们是不是相似的。

计算两个向量之间的相似度通常可以通过几种不同的方法来实现，比如点积，欧氏距离和余弦相似度（Cosine Similarity），每种方法侧重于不同的相似度特征。比较常用的一种方法是余弦相似度，它是衡量两个向量之间角度的一种度量，不受向量长度的影响。余弦相似度广泛用于文本分析、推荐系统等领域，计算公式如下：

> 分子部分是两个向量 𝐴 和 B 的点积，也称为内积，计算是对应元素相乘后的结果相加。

> 分母是两个向量的欧几里得范数（或称为欧氏长度）的乘积。向量的范数是向量从原点到其终点的距离.

转化成代码如下：

```
import numpy as np  

def cosine_similarity(A, B):
    # 计使用np.dot函数计算向量A和B的点积
    dot_product = np.dot(A, B)  
    
    # 使用np.linalg.norm函数计算向量A的欧几里得范数
    norm_A = np.linalg.norm(A)  
    norm_B = np.linalg.norm(B) 

    # 计算余弦相似度
    return dot_product / (norm_A * norm_B)  
```

```
query = get_embeddings(&quot;我喜欢打篮球。&quot;)
answer_1 = get_embeddings(&quot;NBA是篮球的最高赛事。&quot;)
answer_2 = get_embeddings(&quot;中国的乒乓球放眼世界也是最强的。&quot;)
answer_3 = get_embeddings(&quot;今天的天气真好&quot;)
```

```
answer_1
```

```
[[-0.013447309844195843,
  0.022594347596168518,
  -0.013384598307311535,
  0.019655829295516014,
  0.033703386783599854,
  -0.0011209824588149786,
  -0.036211878061294556,
  0.0064638471230864525,
  -0.01828511618077755,
  -0.013366679660975933,
  -0.027575496584177017,
  0.02560453861951828,
  -0.03855910897254944,
  0.008967859670519829,
  0.021483443677425385,
  -0.018473252654075623,
  -0.037197355180978775,
  0.004020306747406721,
  0.00881555862724781,
  -0.03341669961810112,
  -0.007865915074944496,
  0.03723319247364998,
  0.0010196348885074258,
  -0.003973272629082203,
  0.025210347026586533,
  -0.012067639268934727,
  0.02304229326546192,
  -0.010911941528320312,
  0.030191496014595032,
  0.051746610552072525,
  0.0026585538871586323,
  -0.02302437461912632,
  0.025246182456612587,
  0.024583224207162857,
  0.01854492537677288,
  -0.025837469846010208,
  0.02816678397357464,
  -0.0071984766982495785,
  0.037376534193754196,
  -0.023472320288419724,
  0.026966292411088943,
  0.03923998400568962,
  -0.0415692999958992,
  -0.010481913574039936,
  0.03218037262558937,
  0.015642240643501282,
  -0.06532830744981766,
  -0.07288961857557297,
  0.011960132978856564,
  -0.017192130908370018,
  -0.053395047783851624,
  0.03359587863087654,
  0.038200754672288895,
  -0.006248833145946264,
  0.05977378785610199,
  0.04017171263694763,
  0.05608271807432175,
  0.03780656307935715,
  0.023060210049152374,
  -0.04246519133448601,
  -0.057086117565631866,
  0.02101758122444153,
  0.011933255940675735,
  0.01970958150923252,
  -0.022881032899022102,
  0.010562543757259846,
  0.050599873065948486,
  0.0020683861803263426,
  0.05887789651751518,
  0.007328380830585957,
  -0.039526671171188354,
  -0.058161184191703796,
  -0.020981745794415474,
  -0.005536600481718779,
  0.07554145157337189,
  0.00500802556052804,
  -0.054900143295526505,
  0.016108103096485138,
  0.049166448414325714,
  0.0004921795916743577,
  -0.007758408319205046,
  0.004645190201699734,
  0.03418716415762901,
  -0.03035275638103485,
  -0.04457949101924896,
  -0.027629250660538673,
  -0.006647504400461912,
  0.007570271380245686,
  -0.017093582078814507,
  0.0357460156083107,
  -0.0365523137152195,
  -0.03773488849401474,
  0.015113665722310543,
  -0.008309380151331425,
  -0.028220538049936295,
  0.020067937672138214,
  0.005133450031280518,
  -0.04529620334506035,
  0.008618462830781937,
  0.0040986970998346806,
  0.050922393798828125,
  -0.009980215691030025,
  -0.027324646711349487,
  0.03257456421852112,
  -0.014119227416813374,
  -0.018724102526903152,
  -0.034885961562395096,
  0.04282354563474655,
  -0.033380866050720215,
  0.011368845589458942,
  -0.033631712198257446,
  -0.02384859323501587,
  -0.022898949682712555,
  0.003901601303368807,
  -0.03888162970542908,
  -0.015651199966669083,
  -0.004707902204245329,
  0.003668669844046235,
  0.002521930495277047,
  -0.0033259920310229063,
  0.02642875723540783,
  0.03995669633150101,
  -0.019010787829756737,
  0.010132516734302044,
  -0.055079322308301926,
  0.038272425532341,
  0.05468513071537018,
  0.04766135290265083,
  -0.03633730113506317,
  -0.0011142633156850934,
  0.002976594725623727,
  -0.0059352717362344265,
  0.013518981635570526,
  -0.014325282536447048,
  -0.019387060776352882,
  0.027880098670721054,
  -0.07299712300300598,
  0.00481988862156868,
  0.04816305264830589,
  -0.020748814567923546,
  0.03316584974527359,
  0.002128858817741275,
  0.004622792825102806,
  -0.008421367034316063,
  -0.008322819136083126,
  -0.00259584141895175,
  0.02868640050292015,
  0.004976669326424599,
  -0.03938332945108414,
  -0.027055880054831505,
  -0.0032677589915692806,
  -0.04454365372657776,
  0.013232296332716942,
  0.024654895067214966,
  -0.0053036692552268505,
  -0.041103437542915344,
  -0.022486841306090355,
  0.027378400787711143,
  -0.0008802119991742074,
  0.026661688461899757,
  -0.0037067451048642397,
  -0.01288289949297905,
  -0.016403747722506523,
  0.012936652638018131,
  -0.003556683659553528,
  -0.012175146490335464,
  -0.01421777531504631,
  0.031875770539045334,
  -0.03963417559862137,
  0.001954160165041685,
  -0.0052006421610713005,
  -0.030191496014595032,
  0.0349397137761116,
  -0.012838104739785194,
  0.04225017502903938,
  -0.030406508594751358,
  0.027898017317056656,
  0.0012105714995414019,
  -0.029618125408887863,
  0.0037224232219159603,
  0.014343200251460075,
  -0.04669379070401192,
  -0.019655829295516014,
  -0.042357683181762695,
  0.025461196899414062,
  0.050599873065948486,
  -0.00797790102660656,
  0.06561499089002609,
  -0.029385194182395935,
  -0.0013595131458714604,
  -0.0013471946585923433,
  0.05869871750473976,
  -0.04088842496275902,
  0.02843555063009262,
  0.01421777531504631,
  -0.012067639268934727,
  -0.010526708327233791,
  0.02005002088844776,
  -0.023382730782032013,
  0.017675912007689476,
  0.019566239789128304,
  -0.01009668130427599,
  -0.02112508751451969,
  0.050098173320293427,
  0.004418978001922369,
  -0.02976146899163723,
  0.026715442538261414,
  -0.04930979013442993,
  0.0027683002408593893,
  -0.012443913146853447,
  -0.004622792825102806,
  -0.010257941670715809,
  0.039275821298360825,
  0.023633580654859543,
  -0.0349397137761116,
  -0.011404681019484997,
  0.012354324571788311,
  0.05303669348359108,
  0.0031490535475313663,
  0.029277687892317772,
  -0.004273395519703627,
  -0.009245585650205612,
  -0.013751912862062454,
  0.020587554201483727,
  -0.01695024035871029,
  -0.006947627756744623,
  0.0018768897280097008,
  0.021089252084493637,
  -0.03748404234647751,
  -0.021483443677425385,
  -0.03035275638103485,
  0.015902049839496613,
  0.014629884622991085,
  -0.0066385455429553986,
  0.010607338510453701,
  -0.0203546229749918,
  -0.0822785422205925,
  -0.00042050841148011386,
  0.01787300780415535,
  -0.028991002589464188,
  0.005021464079618454,
  -0.02309604547917843,
  0.038774121552705765,
  0.025586619973182678,
  0.04006420448422432,
  -0.025335771963000298,
  -0.01009668130427599,
  0.01076859887689352,
  -0.013904213905334473,
  0.01970958150923252,
  0.0324312224984169,
  0.018141774460673332,
  -0.07539810985326767,
  -0.019387060776352882,
  -0.004918436519801617,
  -0.015848295763134956,
  -0.014701556414365768,
  -0.00741796987131238,
  -0.0026070401072502136,
  0.009603941813111305,
  0.0011277016019448638,
  -0.003798573976382613,
  -0.047338832169771194,
  -0.057086117565631866,
  -0.020641308277845383,
  0.024887826293706894,
  -0.01604539155960083,
  -0.007740490138530731,
  0.030227331444621086,
  0.029958564788103104,
  -0.028883496299386024,
  -0.0027750195004045963,
  -0.03289708495140076,
  0.010714845731854439,
  -0.008770763874053955,
  -0.01513158343732357,
  -0.02934935875236988,
  -0.021340101957321167,
  0.05038485676050186,
  -0.04013587534427643,
  -0.027306729927659035,
  0.0003622755466494709,
  0.0673350989818573,
  -0.034079659730196,
  0.03424092009663582,
  -0.034617193043231964,
  -0.026787113398313522,
  -0.013832543045282364,
  -0.01820448599755764,
  -0.04622792825102806,
  -0.07116951048374176,
  -0.02528201788663864,
  -0.027844263240695,
  0.01979917101562023,
  0.007050654850900173,
  -0.006656463257968426,
  0.02884766086935997,
  -0.0017626637127250433,
  -0.015472021885216236,
  0.002089663641527295,
  0.0036373138427734375,
  -0.020014183595776558,
  -0.03347045183181763,
  0.029958564788103104,
  -0.02012169174849987,
  -0.02060547098517418,
  -0.008304901421070099,
  0.017183171585202217,
  -0.03169659152626991,
  -0.004452573601156473,
  -0.00407406035810709,
  0.06357236206531525,
  -0.03941916301846504,
  -0.02151927910745144,
  -0.02553286775946617,
  -0.004698943346738815,
  -0.0063384221866726875,
  -0.011297173798084259,
  0.024816155433654785,
  0.01845533587038517,
  0.028399715200066566,
  -0.011897420510649681,
  -0.04672962799668312,
  -0.0012497666757553816,
  -0.014181939885020256,
  0.04189182072877884,
  -0.044256970286369324,
  0.006127888336777687,
  -0.013850460760295391,
  -0.009881667792797089,
  0.002199410228058696,
  0.03149949386715889,
  0.014155063778162003,
  0.05916457995772362,
  -0.03834409639239311,
  0.008699093014001846,
  0.003639553440734744,
  0.003552204230800271,
  -0.0034088618122041225,
  0.061135537922382355,
  0.009424763731658459,
  0.012470790185034275,
  -0.007113367319107056,
  0.01936914399266243,
  -0.017747582867741585,
  0.018061144277453423,
  0.02277352660894394,
  -0.011252379044890404,
  -0.043934449553489685,
  -0.012497666291892529,
  -0.02635708637535572,
  0.017908843234181404,
  0.008309380151331425,
  0.05887789651751518,
  -0.043361078947782516,
  0.012300570495426655,
  -0.013384598307311535,
  -0.02693045511841774,
  0.020551718771457672,
  -0.02775467559695244,
  -0.03418716415762901,
  -0.024457799270749092,
  0.018491171300411224,
  -0.013053118251264095,
  0.002434581285342574,
  -0.006472805980592966,
  -0.037519875913858414,
  0.049094777554273605,
  0.05171077698469162,
  -0.005173765122890472,
  -0.06296315789222717,
  0.03042442724108696,
  -0.03888162970542908,
  -0.001445742673240602,
  -0.009917503222823143,
  -0.002813094761222601,
  -0.03373922035098076,
  0.01594684273004532,
  0.013733995147049427,
  -0.04024338349699974,
  -0.04762551560997963,
  -0.018231363967061043,
  0.0052319979295134544,
  -0.048915598541498184,
  -0.024636976420879364,
  0.05912874639034271,
  0.025550784543156624,
  -0.029152262955904007,
  -0.013393556699156761,
  -0.005639628041535616,
  0.008591585792601109,
  -0.038702450692653656,
  0.020766731351614,
  -0.01829407550394535,
  0.018759937956929207,
  -0.04221434146165848,
  0.0465504489839077,
  -0.03995669633150101,
  -0.030818618834018707,
  0.03708985075354576,
  -0.025228265672922134,
  -0.044256970286369324,
  -0.007780805230140686,
  -0.024995332583785057,
  -0.009227667935192585,
  0.0029519577510654926,
  0.012587255798280239,
  0.013035200536251068,
  -0.047338832169771194,
  -0.022468922659754753,
  0.011825748719274998,
  0.04540370777249336,
  0.008390010334551334,
  -0.01755048707127571,
  -0.021340101957321167,
  0.047589682042598724,
  -0.07955504208803177,
  -0.004703423008322716,
  -0.024708649143576622,
  0.03938332945108414,
  0.06780096143484116,
  -0.019404979422688484,
  0.014862816780805588,
  0.029815221205353737,
  0.014316323213279247,
  0.04364776611328125,
  0.050671543926000595,
  -0.043361078947782516,
  -0.0002750662388280034,
  0.058734554797410965,
  0.028650565072894096,
  -0.032968755811452866,
  0.06002463400363922,
  0.002667512744665146,
  -0.02060547098517418,
  0.012909776531159878,
  0.006127888336777687,
  0.03755571320652962,
  -0.03219829127192497,
  0.037197355180978775,
  0.01852700673043728,
  -0.03085445426404476,
  -0.034205082803964615,
  -0.02327522449195385,
  -0.0623539499938488,
  0.022056814283132553,
  -0.029725633561611176,
  -0.04275187477469444,
  -0.04672962799668312,
  0.0072701480239629745,
  0.048091378062963486,
  -0.03533390536904335,
  0.017971554771065712,
  -0.04952480271458626,
  -0.03373922035098076,
  -0.04497368261218071,
  0.09338758140802383,
  -0.0191182941198349,
  -0.0011405800469219685,
  -0.017057746648788452,
  0.038021575659513474,
  0.042859382927417755,
  0.04794803634285927,
  0.001445742673240602,
  0.05801784247159958,
  0.0016070028068497777,
  0.004640710540115833,
  0.013725035823881626,
  0.040028367191553116,
  -0.007843517698347569,
  -0.032216206192970276,
  -0.01409235130995512,
  -0.000736869580578059,
  0.017855089157819748,
  0.010508790612220764,
  0.012085556983947754,
  0.020318787544965744,
  -0.06210310012102127,
  0.021823883056640625,
  -0.04447198286652565,
  -0.010947776958346367,
  -0.019584156572818756,
  -0.024206949397921562,
  -0.03891746699810028,
  -0.029743550345301628,
  0.03780656307935715,
  0.005845682695508003,
  -0.020515883341431618,
  -0.026984209194779396,
  -0.02725297585129738,
  0.0009457239648327231,
  -0.022845197468996048,
  0.012381200678646564,
  0.00048658030573278666,
  -0.018939116969704628,
  0.014558213762938976,
  -0.033452536910772324,
  -0.013008324429392815,
  -0.017362350597977638,
  -0.0170308705419302,
  0.0035947589203715324,
  -0.003175930352881551,
  -0.016582924872636795,
  0.02345440164208412,
  0.033864643424749374,
  -0.018634513020515442,
  -0.026697523891925812,
  -0.009550188668072224,
  0.029116427525877953,
  0.036032699048519135,
  0.016986075788736343,
  -0.021268431097269058,
  0.011539064347743988,
  0.025712044909596443,
  0.028471386060118675,
  -0.008457202464342117,
  -0.03499346598982811,
  0.007520997431129217,
  0.03941916301846504,
  -0.005469408817589283,
  -0.013250214047729969,
  0.009971256367862225,
  -0.010517749935388565,
  0.009299338795244694,
  0.021214677020907402,
  0.002154615707695484,
  -0.016430623829364777,
  -0.006732613779604435,
  0.00819291453808546,
  0.015239090658724308,
  -0.03967001289129257,
  0.011574899777770042,
  -0.029152262955904007,
  0.014782186597585678,
  0.010643173940479755,
  0.07675985991954803,
  -0.011180708184838295,
  0.02359774522483349,
  0.007471723482012749,
  0.006768449675291777,
  0.005254395306110382,
  0.014629884622991085,
  -0.03583560511469841,
  0.008757325820624828,
  0.015033035539090633,
  -0.04056590422987938,
  -0.023812757804989815,
  -0.013572734780609608,
  0.0077718463726341724,
  0.006513121072202921,
  -0.01895703375339508,
  0.005926312878727913,
  -0.008215311914682388,
  -0.026321250945329666,
  0.007176079787313938,
  0.04633543640375137,
  0.008121243678033352,
  -0.03959834203124046,
  0.010033968836069107,
  0.009173914790153503,
  -0.010670050978660583,
  0.02153719775378704,
  -0.024493634700775146,
  0.0024323416873812675,
  0.00017371868307236582,
  -0.017245884984731674,
  0.0077897640876472,
  0.020497964695096016,
  0.02268393710255623,
  0.04432864114642143,
  -0.020067937672138214,
  0.06823098659515381,
  0.02775467559695244,
  0.026805032044649124,
  -0.03563850745558739,
  -0.006732613779604435,
  0.037878233939409256,
  0.024457799270749092,
  -0.007435887586325407,
  0.0027683002408593893,
  0.015257008373737335,
  0.0071536824107170105,
  -0.01629624143242836,
  -0.014208816923201084,
  -0.027414236217737198,
  -0.005496285855770111,
  0.0032856769394129515,
  -0.008206353522837162,
  0.00742244953289628,
  0.022666018456220627,
  0.001140020089223981,
  -0.03648064285516739,
  0.019906677305698395,
  -0.014988240785896778,
  0.018473252654075623,
  -0.035530999302864075,
  0.027647167444229126,
  0.01738922670483589,
  -0.045439545065164566,
  0.0018298554932698607,
  0.030209412798285484,
  -0.052427489310503006,
  0.012515584006905556,
  0.011619694530963898,
  0.001456941245123744,
  -0.019243719056248665,
  0.034025903791189194,
  0.05296502262353897,
  0.0374123714864254,
  -0.0033259920310229063,
  0.0006019261782057583,
  0.020892156288027763,
  0.009379968978464603,
  -0.01138676330447197,
  -0.025962894782423973,
  0.003959834109991789,
  -0.01637687161564827,
  -0.026661688461899757,
  -0.0048422859981656075,
  -0.03859494626522064,
  -0.023472320288419724,
  0.002983313985168934,
  0.030227331444621086,
  -0.01159281749278307,
  0.005809847265481949,
  -0.019745416939258575,
  -0.06772928684949875,
  0.010777557268738747,
  -0.01654708944261074,
  0.013492104597389698,
  -0.0023629101924598217,
  0.0009160476038232446,
  0.030370673164725304,
  0.013805666007101536,
  0.009317256510257721,
  0.025819553062319756,
  -0.02519242838025093,
  -0.02005002088844776,
  -0.011646570637822151,
  -0.02370525151491165,
  0.015480980277061462,
  0.04483034089207649,
  -0.02460114099085331,
  -0.019745416939258575,
  0.0034491769038140774,
  -0.03425883501768112,
  -0.02725297585129738,
  -0.026697523891925812,
  -0.02633916772902012,
  -0.0021198999602347612,
  0.011825748719274998,
  -0.0019216842483729124,
  -0.03533390536904335,
  -0.011297173798084259,
  -0.03035275638103485,
  -0.008031654171645641,
  0.016421664506196976,
  -0.012470790185034275,
  0.0025779237039387226,
  -0.0030348277650773525,
  0.029492702335119247,
  0.004922916181385517,
  -0.01820448599755764,
  0.002441300544887781,
  0.04214267060160637,
  -0.004707902204245329,
  0.030890289694070816,
  0.020981745794415474,
  -0.005267833825200796,
  0.028041359037160873,
  0.025049086660146713,
  -0.03226996213197708,
  -0.0017481055110692978,
  -0.005823285318911076,
  0.0640382245182991,
  0.005299190059304237,
  -0.0013214378850534558,
  -0.003218485042452812,
  -0.007467243820428848,
  -0.008927544578909874,
  -0.007135764695703983,
  0.007113367319107056,
  -0.014423830434679985,
  -0.02734256535768509,
  0.0045712790451943874,
  -0.015086788684129715,
  -0.004712381865829229,
  -0.011691365391016006,
  0.03008398972451687,
  0.019763335585594177,
  -0.024081526324152946,
  0.05178244784474373,
  -0.0032677589915692806,
  -0.022003060206770897,
  0.0011769755510613322,
  -0.007637463044375181,
  -0.025407442823052406,
  0.01695024035871029,
  -0.00947851687669754,
  0.020265033468604088,
  0.0005285751540213823,
  0.021053416654467583,
  -0.0261779073625803,
  -0.054004255682229996,
  0.010033968836069107,
  -0.01317854318767786,
  0.013142707757651806,
  0.015919966623187065,
  0.004860203713178635,
  -0.012282652780413628,
  -0.018939116969704628,
  0.012067639268934727,
  0.021160922944545746,
  -0.024135278537869453,
  0.04963231086730957,
  0.0031400946900248528,
  0.02318563498556614,
  0.02327522449195385,
  0.013062077574431896,
  -0.019225800409913063,
  0.008134681731462479,
  -0.0290985107421875,
  -0.018509089946746826,
  -0.04221434146165848,
  -0.04228601232171059,
  0.008923064917325974,
  0.04565455764532089,
  -0.021985143423080444,
  -0.05583186820149422,
  -0.00927246268838644,
  -0.004412258509546518,
  0.010643173940479755,
  -0.0064056143164634705,
  -0.014737391844391823,
  0.020318787544965744,
  -0.038953300565481186,
  0.009541229344904423,
  -0.0034267795272171497,
  -0.04092425853013992,
  -0.01446862518787384,
  0.01197805069386959,
  -0.0141371451318264,
  0.01978125236928463,
  0.007798722945153713,
  -0.011915338225662708,
  -0.018562842160463333,
  0.0037515396252274513,
  0.018580760806798935,
  0.05583186820149422,
  -0.022397251799702644,
  0.03485012426972389,
  -0.0012923214817419648,
  0.017487773671746254,
  0.013115830719470978,
  9.595822484698147e-05,
  -0.048915598541498184,
  -0.0087259691208601,
  -0.03601478040218353,
  -0.015857255086302757,
  -0.012112434022128582,
  0.022916868329048157,
  -0.009433722123503685,
  -0.01828511618077755,
  -0.010571503080427647,
  0.029797304421663284,
  -0.025156592950224876,
  0.0023069169837981462,
  -0.034868042916059494,
  -0.014316323213279247,
  -0.00500802556052804,
  0.003610437037423253,
  -0.03434842452406883,
  -0.01009668130427599,
  -0.00026792712742462754,
  0.019745416939258575,
  0.040350887924432755,
  -0.017604239284992218,
  -0.028722235932946205,
  0.05504348501563072,
  -0.01605435088276863,
  -0.02777259238064289,
  0.01763111725449562,
  -0.003928477875888348,
  0.009675612673163414,
  -0.015857255086302757,
  -0.08270857483148575,
  -0.047518011182546616,
  -0.024708649143576622,
  -0.006696778349578381,
  -0.01180783100426197,
  -0.01904662325978279,
  -0.022970622405409813,
  0.0009905184851959348,
  -0.03923998400568962,
  0.006347381044179201,
  0.02010377310216427,
  -0.01644854247570038,
  0.014056514948606491,
  -0.01521221362054348,
  0.019422896206378937,
  0.030066071078181267,
  0.0011221023742109537,
  0.019835006445646286,
  -0.002499533351510763,
  -0.02252267673611641,
  0.02112508751451969,
  0.043504420667886734,
  -0.0019698382820934057,
  0.006069655530154705,
  -0.04647877812385559,
  -0.00985479075461626,
  0.008672215975821018,
  -0.05196162313222885,
  0.023293141275644302,
  0.018598677590489388,
  -0.029134346172213554,
  0.0011299413163214922,
  0.010240023955702782,
  0.010598380118608475,
  0.02236141636967659,
  -0.014988240785896778,
  -0.0065668742172420025,
  0.02087423950433731,
  -0.01213931106030941,
  -0.009836873039603233,
  0.008022695779800415,
  -0.04465116187930107,
  0.0005226958892308176,
  0.04440031200647354,
  0.004707902204245329,
  0.02934935875236988,
  0.010329612530767918,
  0.007597147952765226,
  -0.00696554547175765,
  -0.00045102465082891285,
  0.01196909137070179,
  -0.027163388207554817,
  -0.0005282951751723886,
  0.026966292411088943,
  -0.02768300287425518,
  -0.06790846586227417,
  -0.006929709576070309,
  -0.051173239946365356,
  -0.05020568147301674,
  -0.001038672518916428,
  -0.004864683374762535,
  0.02985105849802494,
  -0.003108738688752055,
  -0.01234536524862051,
  -0.003704505506902933,
  -0.029653960838913918,
  0.025156592950224876,
  -0.0023001977242529392,
  -0.006875956431031227,
  0.0207846499979496,
  0.014504460617899895,
  0.00977416057139635,
  -0.019010787829756737,
  0.031553249806165695,
  -0.007641942240297794,
  -0.03182201460003853,
  0.043934449553489685,
  -0.01342043373733759,
  -0.03250289335846901,
  0.03526223450899124,
  -0.01827615685760975,
  0.011548022739589214,
  0.008179476484656334,
  0.007332860492169857,
  0.015552652068436146,
  -0.002398745622485876,
  -0.011440516449511051,
  -0.009702489711344242,
  -0.008658777922391891,
  -0.028113029897212982,
  0.012649968266487122,
  0.02094591036438942,
  0.017335472628474236,
  -0.007386613637208939,
  0.024547388777136803,
  0.011834708042442799,
  -0.03766321763396263,
  -0.007507558912038803,
  0.03026316687464714,
  -0.02768300287425518,
  0.015015117824077606,
  -0.004143491853028536,
  0.0033730261493474245,
  -0.011503228917717934,
  0.006759490817785263,
  0.0005663704941980541,
  0.01738922670483589,
  -0.016573967412114143,
  0.033882562071084976,
  -0.004810929764062166,
  -0.047768861055374146,
  -0.02494158037006855,
  0.019817087799310684,
  0.01570495404303074,
  -0.004488409496843815,
  0.013312926515936852,
  0.011243420653045177,
  -0.02071297913789749,
  0.026088319718837738,
  0.004519765265285969,
  -0.02510284073650837,
  0.018813692033290863,
  0.02486990951001644,
  -0.014961364679038525,
  -0.001315838540904224,
  -0.042680203914642334,
  0.025299936532974243,
  -0.05361006408929825,
  -0.03934749215841293,
  0.05210496857762337,
  0.007619545329362154,
  0.007700175046920776,
  -0.03330919146537781,
  0.007238791789859533,
  -0.010437119752168655,
  0.00015244129463098943,
  0.0034200605005025864,
  0.049668144434690475,
  -0.02203889563679695,
  0.0016876329900696874,
  -0.004640710540115833,
  -0.0034782933071255684,
  -0.02992272935807705,
  -0.01779237762093544,
  -0.009326215833425522,
  -0.023741086944937706,
  -0.04164097085595131,
  -0.01097465306520462,
  -0.02103549987077713,
  0.029313523322343826,
  0.0278621818870306,
  -0.002528649754822254,
  -0.0323953852057457,
  -0.007144723553210497,
  -0.011288215406239033,
  0.0010246742749586701,
  0.0007564671686850488,
  0.008855873718857765,
  -0.06590167433023453,
  -0.005039381794631481,
  -0.02752174250781536,
  0.030066071078181267,
  -0.025819553062319756,
  0.015355556271970272,
  0.005868080072104931,
  0.0067953262478113174,
  -0.012748516164720058,
  0.011315091513097286,
  -0.021590951830148697,
  -0.03182201460003853,
  -0.06783679872751236,
  0.0423935204744339,
  0.00575609365478158,
  0.013133748434484005,
  0.01009668130427599,
  0.029062673449516296,
  -0.018598677590489388,
  0.027127550914883614,
  0.012408077716827393,
  -0.009523311629891396,
  -0.03528015315532684,
  0.009899585507810116,
  0.0027235059533268213,
  0.00024413004575762898,
  -0.035602670162916183,
  0.012417036108672619,
  0.001455821329727769,
  -0.011619694530963898,
  0.0324312224984169,
  -0.0036641904152929783,
  -0.013940049335360527,
  0.04956064000725746,
  0.0047302995808422565,
  -0.02112508751451969,
  -0.004295792896300554,
  -0.022648101672530174,
  0.007722572423517704,
  -0.009308298118412495,
  0.021304266527295113,
  0.036193959414958954,
  0.05597521364688873,
  -0.00501250522211194,
  -0.0044973683543503284,
  -0.004681025631725788,
  -0.01243495475500822,
  0.0047302995808422565,
  0.0004053902521263808,
  0.021573033183813095,
  0.016358952969312668,
  0.006947627756744623,
  0.03425883501768112,
  0.014307364821434021,
  0.03364963084459305,
  -0.040852587670087814,
  -0.01794467866420746,
  ...]]
```

```
# 计算相似度
similarity_1 = cosine_similarity(query[0], answer_1[0])
similarity_2 = cosine_similarity(query[0], answer_2[0])
similarity_3 = cosine_similarity(query[0], answer_3[0])

# 打印相似度结果
print(f&quot;Query和Answer 1的相似度: {similarity_1:.4f}&quot;)
print(f&quot;Query和Answer 2的相似度: {similarity_2:.4f}&quot;)
print(f&quot;Query和Answer 3的相似度: {similarity_3:.4f}&quot;)
```

```
Query和Answer 1的相似度: 0.5054
Query和Answer 2的相似度: 0.3453
Query和Answer 3的相似度: 0.2463
```

那我们来计算一个 Query 和 输入的PDF 的Chunks的相似度。

```
# 生成Query的Embedding
query_text = &quot;什么是LangChain？&quot;
query_embedding = get_embeddings(query_text)

# 假设result已经是包含多个字典的列表，每个字典中包含p
```

```
# 计算相似度并存储结果
similarity_results = []
for item in results:
    similarity = cosine_similarity(query_embedding, item[&quot;embedding&quot;][0])
    similarity_results.append((similarity, item[&quot;page_content&quot;], item[&quot;chunks&quot;]))

# 根据相似度排序
similarity_results.sort(reverse=True, key=lambda x: x[0])

# # 输出结果
# for score, text, chunk_id in similarity_results:
#     print(f&quot;相似度分数: {score:.4f}, Chunk {chunk_id}: {text}&quot;)
```

```
similarity_results[0:5]
```

```
[(array([0.65413659]),
  &#x27;重点在于理解该模型的一些使\n用原理，以及如何调整和构建tools与Schema表示。因此，整个开发过程中花费的大部分时间并非直接\n与模型调用相关，而是在于深入理解和应用这些底层原理和技术细节。\n换言之，在构建大模型应用的过程中，虽然大模型是核心，但实际针对大模型编写的代码量相对较\n小。而在这些有限的代码中，提示工程又往往会占据主要的工作量。这进一步引出一个关键问题：除了\n大模型本身，其他各环节如何高效串联，以及这些环节的代码开发和工作量如何分配和执行，将在很大\n程度上影响最终应用的用户体验。\n在这种情况下，解决这一窘境且广受认可和高度实用的解决方案，便是LangChain。\n1. 什么是LangChain\nLangChain现在归属于LangChain AI公司，LangChain作为其中的一个核心项目，开源发布在\nGitub上：https://github.com/langchain-ai/langchain\n从LangChain的GitHub版本迭代历史上看，从2023年1月16日起已经经历了320个大小版本的迭\n代，并且仍然以高频率的更新在加速项目的功能上线，从整体上看，其关注度&#x27;,
  4),
 (array([0.62798402]),
  &#x27;象，设计出统一接\n口以适配各种大模型。到目前为止，LangChain抽象出最重要的核心模块如下：\n1. Model I/O ：标准化各个大模型的输入和输出，包含输入模版，模型本身和格式化输出；\n2. Retrieval ：检索外部数据，然后在执行生成步骤时将其传递到 LLM，包括文档加载、切割、\nEmbedding等；\n3. Chains ：链条，LangChain框架中最重要的模块，链接多个模块协同构建应用，是实际运作很多\n功能的高级抽象；\n4. Memory ： 记忆模块，以各种方式构建历史信息，维护有关实体及其关系的信息；\n5. Agents ： 目前最热门的Agents开发实践，未来能够真正实现通用人工智能的落地方案；\n6. Callbacks ：回调系统，允许连接到 LLM 应用程序的各个阶段。用于日志记录、监控、流传输和\n其他任务；\n从上图中可以看到，LangChain框架涵盖了模型输入输出的标准化、外部工具接入的规范、上下文\n记忆功能，以及对数据库、SQL、CSV等多种数据源的连接标准。通过核心的&quot;Chain&quot;高级抽象，定义了\n不同形式下标准的链接方法，这就能够允许开发者&#x27;,
  7),
 (array([0.59819352]),
  &#x27;采用Runnable协议。Runnable 协议是\n编程中一种常见的设计模式，用于定义可以执行的任务或行为。在LangChain中通过构建标准接口，可\n以用户轻松定义自定义链并以标准方式调用它们，目前在LangChain已经集成的LLMs中，均实现了\nRunnable接口，目前支持包括invoke、 stream 、 batch 、 astream 等方法的调用。\nLangChain的 Runnable协议：https://api.python.langchain.com/en/stable/runnables/langch\nain_core.runnables.base.Runnable.html#langchain_core.runnables.base.Runnable\n而关于如何在LangChain中某个大模型的定义，我们可以从LangChain的开发源码上更深入的了解\n的LLMs模型的构建方式，其相关程序存储路径在langchain/libs/langchain/langchain/llms下。\n如果代码能力较强的话，还是建议大家花点时间去看看源码的抽象逻辑。比如OpenAI&#x27;,
  46),
 (array([0.58787774]),
  &#x27;l I/O模块的抽象中，其一能够让开发者快速的接入不同的大模型，比如\nOpenAI、ChatGLM、Qwen等，按照既定规范执行模型推理。其二通过输入和输出的模板化处理，使\n其更贴合于应用开发的最佳实践。接下来，我们就逐步的介绍上述三个流程在LangChain下是如何进行\n集成和操作的。\n3 安装LangChain\nLangChain的安装过程非常简单，可以通过常用的Python包管理工具，如pip或conda，直接进行\n安装。稍复杂一点的还可以通过源码进行安装。但有一点大家一定要明确：LangChain的真正价值在于\n它能够与多种模型提供商、数据存储解决方案等进行集成。默认情况下，使用上述两种安装方式中的任\n意一种来进行LangChain安装后，安装的仅仅是LangChain的默认功能，并不包括这些集成所需的额外\n依赖项。也就是说，如果我们想要使用特定的集成功能，还需要额外安装这些特定的依赖。以调用\nOpenAI的API为例，我们首先需要通过运行命令pip install langchain-openai安装OpenAI的合作\n伙伴包，安装此依赖包后，LangChain才能够与Ope&#x27;,
  15),
 (array([0.58292596]),
  &#x27;形式下标准的链接方法，这就能够允许开发者根据实际的应用需求和数据流向快速构建出一套完整\n的应用程序。这个过程类似于搭建积木，可以灵活适应不同的任务需求。\n也正因为如此，LangChain中涉及的概念和模块化是非常多的，每个模块都有其独特的使用场景和\n使用方法，那么如何去搭这个“积木”，就需要我们对其每个核心模块都要有一个比较清楚的认知。所以\n我们课程的安排还是逐个拆解LangChain的功能模块，每一部分都尽可能的给大家做详细的介绍和实\n操，并在接下来的项目部分，进行整体的一个串联，届时大家将能够清晰的明确如何根据自己的实际业\n务情况，选择合适的构造模块和构造方法。\n在整体上理解了LangChain之后，我们首先从Model I/O模块进行深入的探讨和实践。\n2. LangChain核心模块：Model I/O\nLangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的\nModel I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就\n是通过该模块，我们可以快速与某个大模型进行对话交互，整个&#x27;,
  8)]
```

定义成一个函数：

```
def find_similar_chunks(query_text, results, top_n=5, print_content=True):
    &quot;&quot;&quot;
    计算查询文本与文档片段的相似度，并返回/打印最相似的几个片段。

    参数:
    query_text (str): 查询文本。
    results (list of dict): 包含文档片段和相应嵌入向量的列表。
    top_n (int): 要返回的最相似片段的数量。
    print_content (bool): 是否打印每个片段的文本内容。

    返回:
    list: 包含最相似片段的相似度和文本内容的列表。
    &quot;&quot;&quot;
    query_embedding = get_embeddings(query_text)[0]
    similarity_results = []
    
    # 计算相似度
    for item in results:
        similarity = cosine_similarity(query_embedding, item[&#x27;embedding&#x27;][0])
        similarity_results.append((similarity, item[&#x27;page_content&#x27;], item[&#x27;chunks&#x27;]))

    # 排序并选取最相似的top_n个结果
    similarity_results.sort(reverse=True, key=lambda x: x[0])
    top_results = similarity_results[:top_n]

    # 根据选择打印结果
    if print_content:
        for score, text, chunk_id in top_results:
            print(f&quot;相似度分数: {score:.4f}, Chunk {chunk_id}: {text}&quot;)

    return top_results
```

```
# 调用函数，输入查询文本，获取相似度最高的3个结果，选择打印文本内容
similar_chunks = find_similar_chunks(&quot;请帮我解释一下Model I/O模块&quot;, results, top_n=3, print_content=True)
```

```
相似度分数: 0.5076, Chunk 14: te(
model=&quot;gpt-3.5-turbo&quot;,
messages=[
{&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位乐于助人的AI智能小助手&quot;},
{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;你好，请你介绍一下你自己。&quot;}
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

# 8. RAG的Generation过程

```
user_input = &quot;请帮我解释一下Model I/O模块&quot;
```

```
from openai import OpenAI

#client = OpenAI(api_key=&#x27;sk-xxxxxxx&#x27;)

client = OpenAI()

completion = client.chat.completions.create(
  model=&quot;gpt-4&quot;,
  messages=[
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: user_input}
  ]
)

print(completion.choices[0].message.content)
```

```
Model I/O是Apple开发的一个框架，主要用于管理3D模型和相关的纹理、灯光和动画资源。它可以在一系列不同的3D资产类型之间进行转换，包括但不限于.obj, .abc, 和 .usd(通用场景描述)格式。这个框架也涉及物理原理上的一些内容，比如材质和光照模型。

总的来说，Model I/O让开发者不必关注这些底层操作，而能够专注于他们3D内容的创建和编辑，从而提升整体的工作效率。它常被用在iOS, macOS, tvOS, 和 watchOS上的游戏开发或者AR应用开发之中。
```

```
user_input = &quot;请帮我解释一下Model I/O模块&quot;
context = similar_chunks
```

```
from openai import OpenAI

#client = OpenAI(api_key=&#x27;sk-xxxxxxx&#x27;)

client = OpenAI()

completion = client.chat.completions.create(
  model=&quot;gpt-4&quot;,
  messages=[
    {&quot;role&quot;: &quot;system&quot;, &quot;content&quot;: &quot;你是一位文档问答助手，你基于`文档内容`回答用户提出的问题。文档内容如下: %s&quot; % context},
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: user_input}
  ]
)

print(completion.choices[0].message.content)
```

```
LangChain的Model I/O模块提供了标准的、可扩展的接口实现与大语言模型的外部集成。所谓的Model I/O，包括模型输入（Prompts）、模型输出（OutPuts）和模型本身（Models），简单理解就是通过该模块，我们可以快速与某个大模型进行对话交互。在模型输入部分，需要为语言模型提供提示（Prompt）。输出解析器 Output Parser是该模块的一部分，它可以帮助结构化语言模型响应的抽象，可以获取格式指令或者进行更深层次的解析。
```

```

```

```

```

体验课时间有限，若想深度学习大模型RAG技术，欢迎大家报名九天&菜菜老师团队精心研发的《大模型RAG企业项目实战课》：

**更多大模型技术内容学习，请扫码添加英**英，回复“大模型”，即咨询相关信息哦👇

![](img/f5dfd2743a18c30d4008cea11171b377.png)

此外，**扫码回复“LLM”**，即可领取**公开课课件、代码、数据**等~