
# 项目-RAG智能体工作流

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/J2M5dTdHJoNSZuxRjP7c8Ri4nYt](https://qqs7y1hozd1.feishu.cn/docx/J2M5dTdHJoNSZuxRjP7c8Ri4nYt)

当我们想要决定是否从索引中检索时，检索代理很有用。

要实现检索代理，我们只需要让LLM访问检索工具。

我们可以将其合并到LangGraph中。

```
%%capture --no-stderr
%pip install -U --quiet langchain-community tiktoken langchain-openai langchainhub chromadb langchain langgraph langchain-text-splitters
```

```
import getpass
import os


def _set_env(key: str):
    if key not in os.environ:
        os.environ[key] = getpass.getpass(f&quot;{key}:&quot;)


_set_env(&quot;OPENAI_API_KEY&quot;)

# (Optional) For tracing
os.environ[&quot;LANGCHAIN_TRACING_V2&quot;] = &quot;true&quot;
_set_env(&quot;LANGCHAIN_API_KEY&quot;)
```

## 检索器

首先，我们索引3篇博客文章。

```
from langchain_community.document_loaders import WebBaseLoader
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

urls = [
    &quot;https://lilianweng.github.io/posts/2023-06-23-agent/&quot;,
    &quot;https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/&quot;,
    &quot;https://lilianweng.github.io/posts/2023-10-25-adv-attack-llm/&quot;,
]

docs = [WebBaseLoader(url).load() for url in urls]
docs_list = [item for sublist in docs for item in sublist]

text_splitter = RecursiveCharacterTextSplitter.from_tiktoken_encoder(
    chunk_size=100, chunk_overlap=50
)
doc_splits = text_splitter.split_documents(docs_list)

# Add to vectorDB
vectorstore = Chroma.from_documents(
    documents=doc_splits,
    collection_name=&quot;rag-chroma&quot;,
    embedding=OpenAIEmbeddings(),
)
retriever = vectorstore.as_retriever()
```

然后我们创建一个检索工具。

```
from langchain.tools.retriever import create_retriever_tool

retriever_tool = create_retriever_tool(
    retriever,
    &quot;retrieve_blog_posts&quot;,
    &quot;Search and return information about Lilian Weng blog posts on LLM agents, prompt engineering, and adversarial attacks on LLMs.&quot;,
)

tools = [retriever_tool]
```

## 代理状态

我们将定义一个图。

一个传递给每个节点的状态对象。

我们的状态将是一个消息列表。

图中的每个节点都会向其中添加内容。

```
from typing import Annotated, Sequence, TypedDict

from langchain_core.messages import BaseMessage

from langgraph.graph.message import add_messages


class AgentState(TypedDict):
    # The add_messages function defines how an update should be processed
    # Default is to replace. add_messages says &quot;append&quot;
    messages: Annotated[Sequence[BaseMessage], add_messages]
```

### 节点和边

我们可以像这样布置一个代理型 RAG 图：

+ 状态是一组消息

+ 每个节点都会更新（附加到）状态

+ 条件边决定下一个访问的节点

![](img/6c38353832ffbaf3808a2d707fdf3e29.png)

```
from typing import Annotated, Literal, Sequence, TypedDict

from langchain import hub
from langchain_core.messages import BaseMessage, HumanMessage
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_core.pydantic_v1 import BaseModel, Field
from langchain_openai import ChatOpenAI

from langgraph.prebuilt import tools_condition

### Edges


def grade_documents(state) -&gt; Literal[&quot;generate&quot;, &quot;rewrite&quot;]:
    &quot;&quot;&quot;
    Determines whether the retrieved documents are relevant to the question.

    Args:
        state (messages): The current state

    Returns:
        str: A decision for whether the documents are relevant or not
    &quot;&quot;&quot;

    print(&quot;---CHECK RELEVANCE---&quot;)

    # Data model
    class grade(BaseModel):
        &quot;&quot;&quot;Binary score for relevance check.&quot;&quot;&quot;

        binary_score: str = Field(description=&quot;Relevance score &#x27;yes&#x27; or &#x27;no&#x27;&quot;)

    # LLM
    model = ChatOpenAI(temperature=0, model=&quot;gpt-4-0125-preview&quot;, streaming=True)

    # LLM with tool and validation
    llm_with_tool = model.with_structured_output(grade)

    # Prompt
    prompt = PromptTemplate(
        template=&quot;&quot;&quot;You are a grader assessing relevance of a retrieved document to a user question. \n 
        Here is the retrieved document: \n\n {context} \n\n
        Here is the user question: {question} \n
        If the document contains keyword(s) or semantic meaning related to the user question, grade it as relevant. \n
        Give a binary score &#x27;yes&#x27; or &#x27;no&#x27; score to indicate whether the document is relevant to the question.&quot;&quot;&quot;,
        input_variables=[&quot;context&quot;, &quot;question&quot;],
    )

    # Chain
    chain = prompt | llm_with_tool

    messages = state[&quot;messages&quot;]
    last_message = messages[-1]

    question = messages[0].content
    docs = last_message.content

    scored_result = chain.invoke({&quot;question&quot;: question, &quot;context&quot;: docs})

    score = scored_result.binary_score

    if score == &quot;yes&quot;:
        print(&quot;---DECISION: DOCS RELEVANT---&quot;)
        return &quot;generate&quot;

    else:
        print(&quot;---DECISION: DOCS NOT RELEVANT---&quot;)
        print(score)
        return &quot;rewrite&quot;


### Nodes


def agent(state):
    &quot;&quot;&quot;
    Invokes the agent model to generate a response based on the current state. Given
    the question, it will decide to retrieve using the retriever tool, or simply end.

    Args:
        state (messages): The current state

    Returns:
        dict: The updated state with the agent response appended to messages
    &quot;&quot;&quot;
    print(&quot;---CALL AGENT---&quot;)
    messages = state[&quot;messages&quot;]
    model = ChatOpenAI(temperature=0, streaming=True, model=&quot;gpt-4-turbo&quot;)
    model = model.bind_tools(tools)
    response = model.invoke(messages)
    # We return a list, because this will get added to the existing list
    return {&quot;messages&quot;: [response]}


def rewrite(state):
    &quot;&quot;&quot;
    Transform the query to produce a better question.

    Args:
        state (messages): The current state

    Returns:
        dict: The updated state with re-phrased question
    &quot;&quot;&quot;

    print(&quot;---TRANSFORM QUERY---&quot;)
    messages = state[&quot;messages&quot;]
    question = messages[0].content

    msg = [
        HumanMessage(
            content=f&quot;&quot;&quot; \n 
    Look at the input and try to reason about the underlying semantic intent / meaning. \n 
    Here is the initial question:
    \n ------- \n
    {question} 
    \n ------- \n
    Formulate an improved question: &quot;&quot;&quot;,
        )
    ]

    # Grader
    model = ChatOpenAI(temperature=0, model=&quot;gpt-4-0125-preview&quot;, streaming=True)
    response = model.invoke(msg)
    return {&quot;messages&quot;: [response]}


def generate(state):
    &quot;&quot;&quot;
    Generate answer

    Args:
        state (messages): The current state

    Returns:
         dict: The updated state with re-phrased question
    &quot;&quot;&quot;
    print(&quot;---GENERATE---&quot;)
    messages = state[&quot;messages&quot;]
    question = messages[0].content
    last_message = messages[-1]

    question = messages[0].content
    docs = last_message.content

    # Prompt
    prompt = hub.pull(&quot;rlm/rag-prompt&quot;)

    # LLM
    llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, temperature=0, streaming=True)

    # Post-processing
    def format_docs(docs):
        return &quot;\n\n&quot;.join(doc.page_content for doc in docs)

    # Chain
    rag_chain = prompt | llm | StrOutputParser()

    # Run
    response = rag_chain.invoke({&quot;context&quot;: docs, &quot;question&quot;: question})
    return {&quot;messages&quot;: [response]}


print(&quot;*&quot; * 20 + &quot;Prompt[rlm/rag-prompt]&quot; + &quot;*&quot; * 20)
prompt = hub.pull(&quot;rlm/rag-prompt&quot;).pretty_print()  # Show what the prompt looks like
```
