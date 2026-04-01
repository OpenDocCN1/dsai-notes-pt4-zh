# Agentic RAG

> 来源：[https://v11enp9ok1h.feishu.cn/docx/BuUxdwRk3odXEPxD8jscKYfRnRb](https://v11enp9ok1h.feishu.cn/docx/BuUxdwRk3odXEPxD8jscKYfRnRb)

## 在本教程中，我们将构建一个[检索代理](https://python.langchain.com/docs/tutorials/qa_chat_history)。

当你希望 LLM 决定是从向量存储中检索上下文还是直接响应用户时，检索代理非常有用。

在本教程结束时，你将学习以下任务：

1.  获取并预处理将用于检索的文档。

1.  为语义搜索索引这些文档并为代理创建检索工具。

1.  构建一个代理 RAG 系统，可以决定何时使用检索工具。

![](img/68fd87b2a53aaf4cc0030f099c331b7c.png)

## 设置

+   让我们下载所需的软件包并设置我们的 API 密钥：

```
%%capture --no-stderr
%pip install -U --quiet langgraph &quot;langchain[openai]&quot; langchain-community langchain-text-splitters
```

```
import getpass
import os


def _set_env(key: str):
    if key not in os.environ:
        os.environ[key] = getpass.getpass(f&quot;{key}:&quot;)


_set_env(&quot;OPENAI_API_KEY&quot;)
```

###

### 预处理文档

1.  [获取 RAG 系统所需的文档。我们](https://lilianweng.github.io/)将使用Lilian Weng 的优秀博客中最新的三个页面。首先，使用WebBaseLoader以下实用程序获取页面内容：

```
from langchain_community.document_loaders import WebBaseLoader

urls = [
    &quot;https://lilianweng.github.io/posts/2024-11-28-reward-hacking/&quot;,
    &quot;https://lilianweng.github.io/posts/2024-07-07-hallucination/&quot;,
    &quot;https://lilianweng.github.io/posts/2024-04-12-diffusion-video/&quot;,
]

docs = [WebBaseLoader(url).load() for url in urls]
```

```
docs[0][0].page_content.strip()[:1000]
```

1.  将获取的文档拆分成更小的块，以便索引到我们的矢量存储中：

```
from langchain_text_splitters import RecursiveCharacterTextSplitter

docs_list = [item for sublist in docs for item in sublist]

text_splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder(
    chunk_size=100, chunk_overlap=50
)
doc_splits = text_splitter.split_documents(docs_list)
```

```
doc_splits[0].page_content.strip()
```

###

### 创建检索工具

现在我们有了分割的文档，我们可以将它们索引到用于语义搜索的向量存储中。

1.  使用内存向量存储和 OpenAI 嵌入：

```
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_openai import OpenAIEmbeddings

vectorstore = InMemoryVectorStore.from_documents(
    documents=doc_splits, embedding=OpenAIEmbeddings()
)
retriever = vectorstore.as_retriever()
```

1.  使用 LangChain 的预建创建检索工具create_retriever_tool：

```
from langchain.tools.retriever import create_retriever_tool

retriever_tool = create_retriever_tool(
    retriever,
    &quot;retrieve_blog_posts&quot;,
    &quot;Search and return information about Lilian Weng blog posts.&quot;,
)
```

1.  试工具：

```
retriever_tool.invoke({&quot;query&quot;: &quot;types of reward hacking&quot;})
```

###

### 生成查询

> [**现在**](https://langchain-ai.github.io/langgraph/concepts/low_level/#nodes)我[**们**](https://langchain-ai.github.io/langgraph/concepts/low_level/#edges)[将开](https://langchain-ai.github.io/langgraph/concepts/low_level/#edges)始为我们的代理 RAG 图构建组件（节点和边）。

> 请注意，组件将在包含带有[**聊天消息**](https://python.langchain.com/docs/concepts/messages/)列表的键的[MessagesState](https://langchain-ai.github.io/langgraph/concepts/low_level/#messagesstate)图形状态上运行。**messages**

1.  构建一个g**enerate_query_****or_respond节**点。它将调用 LLM，根据当前图状态（消息列表）生成响应。根据输入消息，它将决定使用检索工具进行检索，或直接回复用户。请注意，我们retriever_tool通过以下方式授予聊天模型访问我们之前创建的 的权限.bind_tools：

```
from langgraph.graph import MessagesState
from langchain.chat_models import init_chat_model

response_model = init_chat_model(&quot;openai:gpt-4.1&quot;, temperature=0)


def generate_query_or_respond(state: MessagesState):
    &quot;&quot;&quot;Call the model to generate a response based on the current state. Given
    the question, it will decide to retrieve using the retriever tool, or simply respond to the user.
    &quot;&quot;&quot;
    response = (
        response_model
        .bind_tools([retriever_tool]).invoke(state[&quot;messages&quot;])
    )
    return {&quot;messages&quot;: [response]}
```

1.  尝试随机输入：

```
input = {&quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;hello!&quot;}]}
generate_query_or_respond(input)[&quot;messages&quot;][-1].pretty_print()
```

**输出：**

```
================================== Ai Message ==================================

Hello! How can I help you today?
```

1.  提出一个需要语义搜索的问题：

```
input = {
    &quot;messages&quot;: [
        {
            &quot;role&quot;: &quot;user&quot;,
            &quot;content&quot;: &quot;What does Lilian Weng say about types of reward hacking?&quot;,
        }
    ]
}
generate_query_or_respond(input)[&quot;messages&quot;][-1].pretty_print()
```

**输出：**

```
================================== Ai Message ==================================
Tool Calls:
retrieve_blog_posts (call_tYQxgfIlnQUDMdtAhdbXNwIM)
Call ID: call_tYQxgfIlnQUDMdtAhdbXNwIM
Args:
    query: types of reward hacking
```

###

###

### 对文件进行评分

1.  添加[条件边](https://langchain-ai.github.io/langgraph/concepts/low_level/#conditional-edges)— grade_documents— ，用于判断检索到的文档是否与问题相关。

> **我们将使用一个具有结构化输出****模式的模型GradeDocum**ents进行文档评分。该函数将根据评分决策（或）grade_documents返回要前往的节点名称：generate_answerrewrite_question

```
from pydantic import BaseModel, Field
from typing import Literal

GRADE_PROMPT = (
    &quot;You are a grader assessing relevance of a retrieved document to a user question. \n &quot;
    &quot;Here is the retrieved document: \n\n {context} \n\n&quot;
    &quot;Here is the user question: {question} \n&quot;
    &quot;If the document contains keyword(s) or semantic meaning related to the user question, grade it as relevant. \n&quot;
    &quot;Give a binary score &#x27;yes&#x27; or &#x27;no&#x27; score to indicate whether the document is relevant to the question.&quot;
)


class GradeDocuments(BaseModel):
    &quot;&quot;&quot;Grade documents using a binary score for relevance check.&quot;&quot;&quot;

    binary_score: str = Field(
        description=&quot;Relevance score: &#x27;yes&#x27; if relevant, or &#x27;no&#x27; if not relevant&quot;
    )


grader_model = init_chat_model(&quot;openai:gpt-4.1&quot;, temperature=0)


def grade_documents(
    state: MessagesState,
) -&gt; Literal[&quot;generate_answer&quot;, &quot;rewrite_question&quot;]:
    &quot;&quot;&quot;Determine whether the retrieved documents are relevant to the question.&quot;&quot;&quot;
    question = state[&quot;messages&quot;][0].content
    context = state[&quot;messages&quot;][-1].content

    prompt = GRADE_PROMPT.format(question=question, context=context)
    response = (
        grader_model
        .with_structured_output(GradeDocuments).invoke(
            [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}]
        )
    )
    score = response.binary_score

    if score == &quot;yes&quot;:
        return &quot;generate_answer&quot;
    else:
        return &quot;rewrite_question&quot;
```

1.  使用工具响应中的不相关文档运行此操作：

```
from langchain_core.messages import convert_to_messages

input = {
    &quot;messages&quot;: convert_to_messages(
        [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;What does Lilian Weng say about types of reward hacking?&quot;,
            },
            {
                &quot;role&quot;: &quot;assistant&quot;,
                &quot;content&quot;: &quot;&quot;,
                &quot;tool_calls&quot;: [
                    {
                        &quot;id&quot;: &quot;1&quot;,
                        &quot;name&quot;: &quot;retrieve_blog_posts&quot;,
                        &quot;args&quot;: {&quot;query&quot;: &quot;types of reward hacking&quot;},
                    }
                ],
            },
            {&quot;role&quot;: &quot;tool&quot;, &quot;content&quot;: &quot;meow&quot;, &quot;tool_call_id&quot;: &quot;1&quot;},
        ]
    )
}
grade_documents(input)
```

1.  确认相关文件分类如下：

```
input = {
    &quot;messages&quot;: convert_to_messages(
        [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;What does Lilian Weng say about types of reward hacking?&quot;,
            },
            {
                &quot;role&quot;: &quot;assistant&quot;,
                &quot;content&quot;: &quot;&quot;,
                &quot;tool_calls&quot;: [
                    {
                        &quot;id&quot;: &quot;1&quot;,
                        &quot;name&quot;: &quot;retrieve_blog_posts&quot;,
                        &quot;args&quot;: {&quot;query&quot;: &quot;types of reward hacking&quot;},
                    }
                ],
            },
            {
                &quot;role&quot;: &quot;tool&quot;,
                &quot;content&quot;: &quot;reward hacking can be categorized into two types: environment or goal misspecification, and reward tampering&quot;,
                &quot;tool_call_id&quot;: &quot;1&quot;,
            },
        ]
    )
}
grade_documents(input)
```

###

### 重写问题

1.  构建rewrite_question节点。检索工具可能会返回一些可能不相关的文档，这表明需要改进原始用户问题。为此，我们将该rewrite_question节点称为：

```
REWRITE_PROMPT = (
    &quot;Look at the input and try to reason about the underlying semantic intent / meaning.\n&quot;
    &quot;Here is the initial question:&quot;
    &quot;\n ------- \n&quot;
    &quot;{question}&quot;
    &quot;\n ------- \n&quot;
    &quot;Formulate an improved question:&quot;
)


def rewrite_question(state: MessagesState):
    &quot;&quot;&quot;Rewrite the original user question.&quot;&quot;&quot;
    messages = state[&quot;messages&quot;]
    question = messages[0].content
    prompt = REWRITE_PROMPT.format(question=question)
    response = response_model.invoke([{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}])
    return {&quot;messages&quot;: [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: response.content}]}
```

1.  尝试一下：

```
input = {
    &quot;messages&quot;: convert_to_messages(
        [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;What does Lilian Weng say about types of reward hacking?&quot;,
            },
            {
                &quot;role&quot;: &quot;assistant&quot;,
                &quot;content&quot;: &quot;&quot;,
                &quot;tool_calls&quot;: [
                    {
                        &quot;id&quot;: &quot;1&quot;,
                        &quot;name&quot;: &quot;retrieve_blog_posts&quot;,
                        &quot;args&quot;: {&quot;query&quot;: &quot;types of reward hacking&quot;},
                    }
                ],
            },
            {&quot;role&quot;: &quot;tool&quot;, &quot;content&quot;: &quot;meow&quot;, &quot;tool_call_id&quot;: &quot;1&quot;},
        ]
    )
}

response = rewrite_question(input)
print(response[&quot;messages&quot;][-1][&quot;content&quot;])
```

**输出：**

```
What are the different types of reward hacking described by Lilian Weng, and how does she explain them?
```

###

### 生成答案

1.  构建**generate_answer**节点：如果我们通过评分员检查，我们可以根据原始问题和检索到的上下文生成最终答案：

```
GENERATE_PROMPT = (
    &quot;You are an assistant for question-answering tasks. &quot;
    &quot;Use the following pieces of retrieved context to answer the question. &quot;
    &quot;If you don&#x27;t know the answer, just say that you don&#x27;t know. &quot;
    &quot;Use three sentences maximum and keep the answer concise.\n&quot;
    &quot;Question: {question} \n&quot;
    &quot;Context: {context}&quot;
)


def generate_answer(state: MessagesState):
    &quot;&quot;&quot;Generate an answer.&quot;&quot;&quot;
    question = state[&quot;messages&quot;][0].content
    context = state[&quot;messages&quot;][-1].content
    prompt = GENERATE_PROMPT.format(question=question, context=context)
    response = response_model.invoke([{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}])
    return {&quot;messages&quot;: [response]}
```

1.  尝试一下：

```
input = {
    &quot;messages&quot;: convert_to_messages(
        [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;What does Lilian Weng say about types of reward hacking?&quot;,
            },
            {
                &quot;role&quot;: &quot;assistant&quot;,
                &quot;content&quot;: &quot;&quot;,
                &quot;tool_calls&quot;: [
                    {
                        &quot;id&quot;: &quot;1&quot;,
                        &quot;name&quot;: &quot;retrieve_blog_posts&quot;,
                        &quot;args&quot;: {&quot;query&quot;: &quot;types of reward hacking&quot;},
                    }
                ],
            },
            {
                &quot;role&quot;: &quot;tool&quot;,
                &quot;content&quot;: &quot;reward hacking can be categorized into two types: environment or goal misspecification, and reward tampering&quot;,
                &quot;tool_call_id&quot;: &quot;1&quot;,
            },
        ]
    )
}

response = generate_answer(input)
response[&quot;messages&quot;][-1].pretty_print()
```

**输出：**

```
================================== Ai Message ==================================

Lilian Weng categorizes reward hacking into two types: environment or goal misspecification, and reward tampering. She considers reward hacking as a broad concept that includes both of these categories. Reward hacking occurs when an agent exploits flaws or ambiguities in the reward function to achieve high rewards without performing the intended behaviors.
```

###

### 组装图表

+   从 a 开始generate_query_or_respond并确定我们是否需要调用retriever_tool

+   使用以下方式进入下一步tools_condition：

+   如果generate_query_or_respond返回tool_calls，则调用retriever_tool以检索上下文

+   否则，直接回复用户

+   根据问题相关性对检索到的文档内容进行评分（grade_documents）并进入下一步：

+   如果不相关，请使用重写问题rewrite_question，然后generate_query_or_respond再次调用

+   如果相关，则继续使用检索到的文档上下文generate_answer生成最终响应

API 参考：[StateGraph](https://langchain-ai.github.io/langgraph/reference/graphs/#langgraph.graph.state.StateGraph) |[开始](https://langchain-ai.github.io/langgraph/reference/constants/#langgraph.constants.START)|[结束](https://langchain-ai.github.io/langgraph/reference/constants/#langgraph.constants.END)| [ToolNode](https://langchain-ai.github.io/langgraph/reference/prebuilt/#langgraph.prebuilt.tool_node.ToolNode) | [tools_condition](https://langchain-ai.github.io/langgraph/reference/prebuilt/#langgraph.prebuilt.tool_node.tools_condition)

```
from langgraph.graph import StateGraph, START, END
from langgraph.prebuilt import ToolNode
from langgraph.prebuilt import tools_condition

workflow = StateGraph(MessagesState)

# Define the nodes we will cycle between
workflow.add_node(generate_query_or_respond)
workflow.add_node(&quot;retrieve&quot;, ToolNode([retriever_tool]))
workflow.add_node(rewrite_question)
workflow.add_node(generate_answer)

workflow.add_edge(START, &quot;generate_query_or_respond&quot;)

# Decide whether to retrieve
workflow.add_conditional_edges(
    &quot;generate_query_or_respond&quot;,
    # Assess LLM decision (call `retriever_tool` tool or respond to the user)
    tools_condition,
    {
        # Translate the condition outputs to nodes in our graph
        &quot;tools&quot;: &quot;retrieve&quot;,
        END: END,
    },
)

# Edges taken after the `action` node is called.
workflow.add_conditional_edges(
    &quot;retrieve&quot;,
    # Assess agent decision
    grade_documents,
)
workflow.add_edge(&quot;generate_answer&quot;, END)
workflow.add_edge(&quot;rewrite_question&quot;, &quot;generate_query_or_respond&quot;)

# Compile
graph = workflow.compile()
```

**可视化图表：**

```
from IPython.display import Image, display

display(Image(graph.get_graph().draw_mermaid_png()))
```

![](img/affa69ca1e7a18336deb64ead3df0855.png)

###

### 运行 agentic RAG

```
for chunk in graph.stream(
    {
        &quot;messages&quot;: [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;What does Lilian Weng say about types of reward hacking?&quot;,
            }
        ]
    }
):
    for node, update in chunk.items():
        print(&quot;Update from node&quot;, node)
        update[&quot;messages&quot;][-1].pretty_print()
        print(&quot;\n\n&quot;)
```

**输出：**

```
Update from node generate_query_or_respond
================================== Ai Message ==================================
Tool Calls:
  retrieve_blog_posts (call_NYu2vq4km9nNNEFqJwefWKu1)
 Call ID: call_NYu2vq4km9nNNEFqJwefWKu1
  Args:
    query: types of reward hacking



Update from node retrieve
================================= Tool Message ==================================
Name: retrieve_blog_posts

(Note: Some work defines reward tampering as a distinct category of misalignment behavior from reward hacking. But I consider reward hacking as a broader concept here.)
At a high level, reward hacking can be categorized into two types: environment or goal misspecification, and reward tampering.

Why does Reward Hacking Exist?#

Pan et al. (2022) investigated reward hacking as a function of agent capabilities, including (1) model size, (2) action space resolution, (3) observation space noise, and (4) training time. They also proposed a taxonomy of three types of misspecified proxy rewards:

Let&#x27;s Define Reward Hacking#
Reward shaping in RL is challenging. Reward hacking occurs when an RL agent exploits flaws or ambiguities in the reward function to obtain high rewards without genuinely learning the intended behaviors or completing the task as designed. In recent years, several related concepts have been proposed, all referring to some form of reward hacking:



Update from node generate_answer
================================== Ai Message ==================================

Lilian Weng categorizes reward hacking into two types: environment or goal misspecification, and reward tampering. She considers reward hacking as a broad concept that includes both of these categories. Reward hacking occurs when an agent exploits flaws or ambiguities in the reward function to achieve high rewards without performing the intended behaviors.
```