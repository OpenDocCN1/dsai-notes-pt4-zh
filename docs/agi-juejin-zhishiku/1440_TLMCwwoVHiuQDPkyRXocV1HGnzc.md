# Langchain Agent - 快速入门

> 来源：[https://agijuejin.feishu.cn/docx/Ajz7d6dVKoyFpvxXk6Jcp0VAngd](https://agijuejin.feishu.cn/docx/Ajz7d6dVKoyFpvxXk6Jcp0VAngd)

> 作者：[lucas大叔](https://www.zhihu.com/people/lucasda-shu)

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/694420407)hihu.com/p/694420...

> Agent作为一种智能系统，能够利用其深厚的语言理解和生成能力，帮助你解决各种问题，从技术支持到创意激发，再到信息搜集。它的灵活性使得它适用于多种场景，无论是研究、写作、教育还是娱乐。通过与agent互动，你将体验到智能和效率。本系列文章参考langchain官方文档，带领大家一起学习agent的基础知识，探索基于agent的高级应用，为你打开人工智能之门。

为了更好地理解agent框架，我们创建带2个工具的agent：一个工具用于在线搜索，另一个工具查询已经加载到索引里的特定数据。

##  设置：LangSmith

根据定义，agent在返回面向用户的最终输出之前，会采取一系列自决策、输入依赖的步骤。这使得调试agent变得特别棘手，而可观察性变得尤为重要。只需要设置有关LangSmith的以下环境变量，就可以自动跟踪所有步骤：

```
export LANGCHAIN_TRACING_V2=&quot;true&quot; 
export LANGCHAIN_API_KEY=&quot;&lt;your-api-key&gt;&quot;
```

## 定义工具

首先创建要使用的两个工具：Tavily（在线搜索）和基于本地索引的检索器。

### Tavily

在LangChain中有内置工具可以轻松使用Tavily搜索引擎作为工具。要注意的是，使用Tavily需要API密钥-它们有免费额度，但如果你没有或不想创建，则可以忽略此步骤。

当创建API key后，需要按照下面的方式导入：

```
export TAVILY_API_KEY=&quot;...&quot;

from langchain_community.tools.tavily_search import TavilySearchResults

search = TavilySearchResults()
search.invoke(&quot;what is the weather in SF&quot;)
```

搜索结果

```
[{&#x27;url&#x27;: &#x27;https://www.metoffice.gov.uk/weather/forecast/9q8yym8kr&#x27;,
  &#x27;content&#x27;: &#x27;Thu 11 Jan Thu 11 Jan Seven day forecast for San Francisco  San Francisco (United States of America) weather Find a forecast  Sat 6 Jan Sat 6 Jan Sun 7 Jan Sun 7 Jan Mon 8 Jan Mon 8 Jan Tue 9 Jan Tue 9 Jan Wed 10 Jan Wed 10 Jan Thu 11 Jan  Find a forecast Please choose your location from the nearest places to : Forecast days Today Today Sat 6 Jan Sat 6 JanSan Francisco 7 day weather forecast including weather warnings, temperature, rain, wind, visibility, humidity and UV ... (11 January 2024) Time 00:00 01:00 02:00 03:00 04:00 05:00 06:00 07:00 08:00 09:00 10:00 11:00 12:00 ... Oakland Int. 11.5 miles; San Francisco International 11.5 miles; Corte Madera 12.3 miles; Redwood City 23.4 miles;&#x27;},
 {&#x27;url&#x27;: &#x27;https://www.latimes.com/travel/story/2024-01-11/east-brother-light-station-lighthouse-california&#x27;,
  &#x27;content&#x27;: &quot;May 18, 2023  Jan. 4, 2024 Subscribe for unlimited accessSite Map Follow Us MORE FROM THE L.A. TIMES  Jan. 8, 2024 Travel &amp; Experiences This must be Elysian Valley (a.k.a. Frogtown) Jan. 5, 2024 Food  June 30, 2023The East Brother Light Station in the San Francisco Bay is not a destination for everyone. ... Jan. 11, 2024 3 AM PT ... Champagne and hors d&#x27;oeuvres are served in late afternoon — outdoors if ...&quot;}]
```

### Retriever

在我们自己的一些数据上创建retriever。

```
from langchain_community.document_loaders import WebBaseLoader
from langchain_community.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

loader = WebBaseLoader(&quot;https://docs.smith.langchain.com/overview&quot;)
docs = loader.load()
documents = RecursiveCharacterTextSplitter(
    chunk_size=1000, chunk_overlap=200
).split_documents(docs)
vector = FAISS.from_documents(documents, OpenAIEmbeddings())
retriever = vector.as_retriever()

retriever.get_relevant_documents(&quot;how to upload a dataset&quot;)[0]
```

检索结果

```
Document(page_content=&quot;dataset uploading.Once we have a dataset, how can we use it to test changes to a prompt or chain? The most basic approach is to run the chain over the data points and visualize the outputs. Despite technological advancements, there still is no substitute for looking at outputs by eye. Currently, running the chain over the data points needs to be done client-side. The LangSmith client makes it easy to pull down a dataset and then run a chain over them, logging the results to a new project associated with the dataset. From there, you can review them. We&#x27;ve made it easy to assign feedback to runs and mark them as correct or incorrect directly in the web app, displaying aggregate statistics for each test project.We also make it easier to evaluate these runs. To that end, we&#x27;ve added a set of evaluators to the open-source LangChain library. These evaluators can be specified when initiating a test run and will evaluate the results once the test run completes. If we’re being honest, most of&quot;, metadata={&#x27;source&#x27;: &#x27;https://docs.smith.langchain.com/overview&#x27;, &#x27;title&#x27;: &#x27;LangSmith Overview and User Guide |  ️ ️ LangSmith&#x27;, &#x27;description&#x27;: &#x27;Building reliable LLM applications can be challenging. LangChain simplifies the initial setup, but there is still work needed to bring the performance of prompts, chains and agents up the level where they are reliable enough to be used in production.&#x27;, &#x27;language&#x27;: &#x27;en&#x27;})
```

我们可以很容易地将检索器转换为工具（正确使用agent所需的格式）

```
from langchain.tools.retriever import create_retriever_tool

retriever_tool = create_retriever_tool(
    retriever,
    &quot;langsmith_search&quot;,
    &quot;Search for information about LangSmith. For any questions about LangSmith, you must use this tool!&quot;,
)
```

### 工具

现在我们已经创建了两个工具，然后创建工具列表用于下游任务。

```
tools = [search, retriever_tool]
```

## 创建Agent

既然已经定义了工具，接下来就可以创建agent了。我们将使用OpenAI Function[s a](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/agent_types)gent，有关该类型agent的更多信息，以及其他可选agent，请参阅 指南 。

首先，选择想要引导agent的LLM。

```
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model=&quot;gpt-3.5-turbo&quot;, temperature=0)
```

接[下](https://link.zhihu.com/?target=https%3A//smith.langchain.com/hub/hwchase17/openai-functions-agent)来，选择要用于引导agent的prompt。如果想查看此prompt的内容并访问LangSmith，你可以访问 https:// smith.langchain.com/hub /hwchase17/openai-functions-agent

```
from langchain import hub

# Get the prompt to use - you can modify this!
prompt = hub.pull(&quot;hwchase17/openai-functions-agent&quot;)
prompt.messages
```

prompt内容

```
[SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], template=&#x27;You are a helpful assistant&#x27;)),
 MessagesPlaceholder(variable_name=&#x27;chat_history&#x27;, optional=True),
 HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;input&#x27;], template=&#x27;{input}&#x27;)),
 MessagesPlaceholder(variable_name=&#x27;agent_scratchpad&#x27;)]
```

现在，我们可以使用LLM、prompt**和工具初始**化a**gent。agen**t[负责 接收](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/concepts)输入 并 决定要采取的动作 。至关重要的是，Agent不执行这些操作——这是由AgentExecutitor完成的（下一步）。有关如这些组件的更多信息，请参阅 概念指南 。

```
from langchain.agents import create_openai_functions_agent

agent = create_openai_functions_agent(llm, tools, prompt)
```

最后，我们将agent（大脑）与AgentExecutor内部的工具（它将重复调用agent并执行工具）相结合。

```
from langchain.agents import AgentExecutor

agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
```

## 运行Agent

现在可以对一些query运行agent！请注意，目前这些都是无状态查询（不会记住以前的交互）。

```
agent_executor.invoke({&quot;input&quot;: &quot;hi!&quot;})
```

中间过程

```
&gt; Entering new AgentExecutor chain...
Hello! How can I assist you today?

&gt; Finished chain.
```

输出结果

```
{&#x27;input&#x27;: &#x27;hi!&#x27;, &#x27;output&#x27;: &#x27;Hello! How can I assist you today?&#x27;}
```

再测试一个问题

```
agent_executor.invoke({&quot;input&quot;: &quot;how can langsmith help with testing?&quot;})
```

中间过程

```
&gt; Entering new AgentExecutor chain...

Invoking: `langsmith_search` with `{&#x27;query&#x27;: &#x27;LangSmith testing&#x27;}`


[Document(page_content=&#x27;LangSmith Overview and User Guide |  ️ ️ LangSmith&#x27;, metadata={&#x27;source&#x27;: &#x27;https://docs.smith.langchain.com/overview&#x27;, &#x27;title&#x27;: &#x27;LangSmith Overview and User Guide |  ️ ️ LangSmith&#x27;, &#x27;description&#x27;: &#x27;Building reliable LLM applications can be challenging. LangChain simplifies the initial setup, but there is still work needed to bring the performance of prompts, chains and agents up the level where they are reliable enough to be used in production.&#x27;, &#x27;language&#x27;: &#x27;en&#x27;}), Document(page_content=&#x27;Skip to main content ️ ️ LangSmith DocsPython DocsJS/TS DocsSearchGo to AppLangSmithOverviewTracingTesting &amp; EvaluationOrganizationsHubLangSmith CookbookOverviewOn this pageLangSmith Overview and User GuideBuilding reliable LLM applications can be challenging. LangChain simplifies the initial setup, but there is still work needed to bring the performance of prompts, chains and agents up the level where they are reliable enough to be used in production.Over the past two months, we at LangChain have been building and using LangSmith with the goal of bridging this gap. This is our tactical user guide to outline effective ways to use LangSmith and maximize its benefits.On by default\u200bAt LangChain, all of us have LangSmith’s tracing running in the background by default. On the Python side, this is achieved by setting environment variables, which we establish whenever we launch a virtual environment or open our bash shell and leave them set. The same principle applies to most JavaScript&#x27;, metadata={&#x27;source&#x27;: &#x27;https://docs.smith.langchain.com/overview&#x27;, &#x27;title&#x27;: &#x27;LangSmith Overview and User Guide |  ️ ️ LangSmith&#x27;, &#x27;description&#x27;: &#x27;Building reliable LLM applications can be challenging. LangChain simplifies the initial setup, but there is still work needed to bring the performance of prompts, chains and agents up the level where they are reliable enough to be used in production.&#x27;, &#x27;language&#x27;: &#x27;en&#x27;}), Document(page_content=&#x27;You can also quickly edit examples and add them to datasets to expand the surface area of your evaluation sets or to fine-tune a model for improved quality or reduced costs.Monitoring\u200bAfter all this, your app might finally ready to go in production. LangSmith can also be used to monitor your application in much the same way that you used for debugging. You can log all traces, visualize latency and token usage statistics, and troubleshoot specific issues as they arise. Each run can also be assigned string tags or key-value metadata, allowing you to attach correlation ids or AB test variants, and filter runs accordingly.We’ve also made it possible to associate feedback programmatically with runs. This means that if your application has a thumbs up/down button on it, you can use that to log feedback back to LangSmith. This can be used to track performance over time and pinpoint under performing data points, which you can subsequently add to a dataset for future testing — mirroring the&#x27;, metadata={&#x27;source&#x27;: &#x27;https://docs.smith.langchain.com/overview&#x27;, &#x27;title&#x27;: &#x27;LangSmith Overview and User Guide |  ️ ️ LangSmith&#x27;, &#x27;description&#x27;: &#x27;Building reliable LLM applications can be challenging. LangChain simplifies the initial setup, but there is still work needed to bring the performance of prompts, chains and agents up the level where they are reliable enough to be used in production.&#x27;, &#x27;language&#x27;: &#x27;en&#x27;}), Document(page_content=&#x27;inputs, and see what happens. At some point though, our application is performing\nwell and we want to be more rigorous about testing changes. We can use a dataset\nthat we’ve constructed along the way (see above). Alternatively, we could spend some\ntime constructing a small dataset by hand. For these situations, LangSmith simplifies&#x27;, metadata={&#x27;source&#x27;: &#x27;https://docs.smith.langchain.com/overview&#x27;, &#x27;title&#x27;: &#x27;LangSmith Overview and User Guide |  ️ ️ LangSmith&#x27;, &#x27;description&#x27;: &#x27;Building reliable LLM applications can be challenging. LangChain simplifies the initial setup, but there is still work needed to bring the performance of prompts, chains and agents up the level where they are reliable enough to be used in production.&#x27;, &#x27;language&#x27;: &#x27;en&#x27;})]LangSmith can help with testing in several ways. Here are some ways LangSmith can assist with testing:

1. Tracing: LangSmith provides tracing capabilities that can be used to monitor and debug your application during testing. You can log all traces, visualize latency and token usage statistics, and troubleshoot specific issues as they arise.

2. Evaluation: LangSmith allows you to quickly edit examples and add them to datasets to expand the surface area of your evaluation sets. This can help you test and fine-tune your models for improved quality or reduced costs.

3. Monitoring: Once your application is ready for production, LangSmith can be used to monitor your application. You can log feedback programmatically with runs, track performance over time, and pinpoint underperforming data points. This information can be used to improve your application and add to datasets for future testing.

4. Rigorous Testing: When your application is performing well and you want to be more rigorous about testing changes, LangSmith can simplify the process. You can use existing datasets or construct small datasets by hand to test different scenarios and evaluate the performance of your application.

For more detailed information on how to use LangSmith for testing, you can refer to the [LangSmith Overview and User Guide](https://docs.smith.langchain.com/overview).

&gt; Finished chain.
```

输出结果

```
{&#x27;input&#x27;: &#x27;how can langsmith help with testing?&#x27;,
 &#x27;output&#x27;: &#x27;LangSmith can help with testing in several ways. Here are some ways LangSmith can assist with testing:\n\n1. Tracing: LangSmith provides tracing capabilities that can be used to monitor and debug your application during testing. You can log all traces, visualize latency and token usage statistics, and troubleshoot specific issues as they arise.\n\n2. Evaluation: LangSmith allows you to quickly edit examples and add them to datasets to expand the surface area of your evaluation sets. This can help you test and fine-tune your models for improved quality or reduced costs.\n\n3. Monitoring: Once your application is ready for production, LangSmith can be used to monitor your application. You can log feedback programmatically with runs, track performance over time, and pinpoint underperforming data points. This information can be used to improve your application and add to datasets for future testing.\n\n4. Rigorous Testing: When your application is performing well and you want to be more rigorous about testing changes, LangSmith can simplify the process. You can use existing datasets or construct small datasets by hand to test different scenarios and evaluate the performance of your application.\n\nFor more detailed information on how to use LangSmith for testing, you can refer to the [LangSmith Overview and User Guide](https://docs.smith.langchain.com/overview).&#x27;}
```

## 添加记忆

如前所述，该agent是无状态的。这意味着它不记得以前的交互。为了给它记忆，我们需要传入以前的 chat_history 。注意：由于我们使用的prompt，它需要被称为 chat_history 。如果使用不同的prompt，可以更改变量名。

```
# Here we pass in an empty list of messages for chat_history because it is the first message in the chat
agent_executor.invoke({&quot;input&quot;: &quot;hi! my name is bob&quot;, &quot;chat_history&quot;: []})
```

中间过程

```
&gt; Entering new AgentExecutor chain...
Hello Bob! How can I assist you today?

&gt; Finished chain.
```

输出结果

```
{&#x27;input&#x27;: &#x27;hi! my name is bob&#x27;,
 &#x27;chat_history&#x27;: [],
 &#x27;output&#x27;: &#x27;Hello Bob! How can I assist you today?&#x27;}
```

增加聊天历史

```
from langchain_core.messages import AIMessage, HumanMessage

agent_executor.invoke(
    {
        &quot;chat_history&quot;: [
            HumanMessage(content=&quot;hi! my name is bob&quot;),
            AIMessage(content=&quot;Hello Bob! How can I assist you today?&quot;),
        ],
        &quot;input&quot;: &quot;what&#x27;s my name?&quot;,
    }
)
```

中间过程

```
&gt; Entering new AgentExecutor chain...
Your name is Bob.

&gt; Finished chain.
```

输出结果

```
{&#x27;chat_history&#x27;: [HumanMessage(content=&#x27;hi! my name is bob&#x27;),
  AIMessage(content=&#x27;Hello Bob! How can I assist you today?&#x27;)],
 &#x27;input&#x27;: &quot;what&#x27;s my name?&quot;,
 &#x27;output&#x27;: &#x27;Your name is Bob.&#x27;}
```

如[果想自](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/expression_language/how_to/message_history)动跟踪这些消息，可以将其封装在RunnableWithMessageHistory中。有关如何使用它的更多信息，请参阅 指南 。

```
from langchain_community.chat_message_histories import ChatMessageHistory
from langchain_core.runnables.history import RunnableWithMessageHistory

message_history = ChatMessageHistory()

agent_with_chat_history = RunnableWithMessageHistory(
    agent_executor,
    # This is needed because in most real world scenarios, a session id is needed
    # It isn&#x27;t really used here because we are using a simple in memory ChatMessageHistory
    lambda session_id: message_history,
    input_messages_key=&quot;input&quot;,
    history_messages_key=&quot;chat_history&quot;,
)

agent_with_chat_history.invoke(
    {&quot;input&quot;: &quot;hi! I&#x27;m bob&quot;},
    # This is needed because in most real world scenarios, a session id is needed
    # It isn&#x27;t really used here because we are using a simple in memory ChatMessageHistory
    config={&quot;configurable&quot;: {&quot;session_id&quot;: &quot;&lt;foo&gt;&quot;}},
)
```

中间过程

```
&gt; Entering new AgentExecutor chain...
Hello Bob! How can I assist you today?

&gt; Finished chain.
```

输出结果

```
{&#x27;input&#x27;: &quot;hi! I&#x27;m bob&quot;,
 &#x27;chat_history&#x27;: [],
 &#x27;output&#x27;: &#x27;Hello Bob! How can I assist you today?&#x27;}
```

新一轮对话

```
agent_with_chat_history.invoke(
    {&quot;input&quot;: &quot;what&#x27;s my name?&quot;},
    # This is needed because in most real world scenarios, a session id is needed
    # It isn&#x27;t really used here because we are using a simple in memory ChatMessageHistory
    config={&quot;configurable&quot;: {&quot;session_id&quot;: &quot;&lt;foo&gt;&quot;}},
)
```

中间过程

```
&gt; Entering new AgentExecutor chain...
Your name is Bob.

&gt; Finished chain.
```

输出结果

```
{&#x27;input&#x27;: &quot;what&#x27;s my name?&quot;,
 &#x27;chat_history&#x27;: [HumanMessage(content=&quot;hi! I&#x27;m bob&quot;),
  AIMessage(content=&#x27;Hello Bob! How can I assist you today?&#x27;)],
 &#x27;output&#x27;: &#x27;Your name is Bob.&#x27;}
```

## 参考文档

[h](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/quick_start/)ttps:// python.langchain.com/do cs/modules/agents/quick_start/