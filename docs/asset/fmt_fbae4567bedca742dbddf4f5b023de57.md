
# LangChain 可运行程序与内置链条

LangChain 会在遇到`|`操作符时自动将某些对象转换为 [Runnable](https://api.python.langchain.com/en/latest/runnables/langchain_core.runnables.base.RunnableLambda.html)。这里，`format_docs`被转换为`RunnableLambda`，带有键`context`的字典和带有键`question`的字典被转换为`RunnableParallel`。细节并不重要，重要的是，每个对象都是一个`Runnable`。

让我们追踪一下输入问题如何流经上述可运行程序。

正如我们上面所看到的，输入`prompt`预计是一个带有键`context`和键`question`的字典。因此，该链的第一个元素构建了可运行程序，它将根据输入问题计算这两个值：

+   `retriever | format_docs`将问题传递给检索器，生成`Document`对象，然后`format_docs`生成字符串；

+   `RunnablePassthrough()`将输入的问题保持不变。

也就是说，如果你构造

```
chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
)
```

然后`chain.invoke(question)`将构建一个格式化的提示，准备进行推理。（注意：使用 LCEL 进行开发时，使用这样的子链进行测试是可行的。）

该链的最后步骤是`llm`，运行推理，以及`StrOutputParser()`，仅从 LLM 的输出消息中提取字符串内容。

您可以通过其LangSmith 跟踪来分析此链的各个步骤。

## 内置链条

如果需要，LangChain 包含实现上述 LCEL 的便捷函数。我们编写了两个函数：

+   `create_stuff_documents_chain`指定如何将检索到的上下文输入到提示和 LLM 中。在这种情况下，我们将“填充”内容到提示中 - 即，我们将包含所有检索到的上下文，而无需任何总结或其他处理。它主要实现我们上面的`rag_chain`，使用输入键`context`和`input` - 它使用检索到的上下文和查询生成答案。

+   `create_retrieval_chain`添加检索步骤并通过链传播检索到的上下文，将其与最终答案一起提供。它的输入键为`input`，输出包括`input`、`context`和`answer`。

```
from langchain.chains import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain_core.prompts import ChatPromptTemplate

system_prompt = (
    "You are an assistant for question-answering tasks. "
    "Use the following pieces of retrieved context to answer "
    "the question. If you don't know the answer, say that you "
    "don't know. Use three sentences maximum and keep the "
    "answer concise."
    "\n\n"
    "{context}"
)

prompt = ChatPromptTemplate.from_messages(
    [
        ("system", system_prompt),
        ("human", "{input}"),
    ]
)

question_answer_chain = create_stuff_documents_chain(llm, prompt)
rag_chain = create_retrieval_chain(retriever, question_answer_chain)

response = rag_chain.invoke({"input": "What is Task Decomposition?"})
print(response["answer"])
```

> **API 参考：**`create_retrieval_chain` | `create_stuff_documents_chain` | `ChatPromptTemplate`

```
Task Decomposition is a process in which complex tasks are broken down into smaller and simpler steps. Techniques like Chain of Thought (CoT) and Tree of Thoughts are used to enhance model performance on these tasks. The CoT method instructs the model to think step by step, decomposing hard tasks into manageable ones, while Tree of Thoughts extends CoT by exploring multiple reasoning possibilities at each step, creating a tree structure of thoughts.
```

### 返回[来源](https://python.langchain.com/v0.2/docs/tutorials/rag/#returning-sources)

在问答应用中，向用户展示用于生成答案的来源通常很重要。LangChain 的内置功能`create_retrieval_chain`会将检索到的源文档传播到密钥中的输出`context`：

```
for document in response["context"]:
    print(document)
    print()
```

```
page_content='Fig. 1. Overview of a LLM-powered autonomous agent system.\nComponent One: Planning#\nA complicated task usually involves many steps. An agent needs to know what they are and plan ahead.\nTask Decomposition#\nChain of thought (CoT; Wei et al. 2022) has become a standard prompting technique for enhancing model performance on complex tasks. The model is instructed to “think step by step” to utilize more test-time computation to decompose hard tasks into smaller and simpler steps. CoT transforms big tasks into multiple manageable tasks and shed lights into an interpretation of the model’s thinking process.' metadata={'source': 'https://lilianweng.github.io/posts/2023-06-23-agent/'}

page_content='Fig. 1. Overview of a LLM-powered autonomous agent system.\nComponent One: Planning#\nA complicated task usually involves many steps. An agent needs to know what they are and plan ahead.\nTask Decomposition#\nChain of thought (CoT; Wei et al. 2022) has become a standard prompting technique for enhancing model performance on complex tasks. The model is instructed to “think step by step” to utilize more test-time computation to decompose hard tasks into smaller and simpler steps. CoT transforms big tasks into multiple manageable tasks and shed lights into an interpretation of the model’s thinking process.' metadata={'source': 'https://lilianweng.github.io/posts/2023-06-23-agent/', 'start_index': 1585}

page_content='Tree of Thoughts (Yao et al. 2023) extends CoT by exploring multiple reasoning possibilities at each step. It first decomposes the problem into multiple thought steps and generates multiple thoughts per step, creating a tree structure. The search process can be BFS (breadth-first search) or DFS (depth-first search) with each state evaluated by a classifier (via a prompt) or majority vote.\nTask decomposition can be done (1) by LLM with simple prompting like "Steps for XYZ.\n1.", "What are the subgoals for achieving XYZ?", (2) by using task-specific instructions; e.g. "Write a story outline." for writing a novel, or (3) with human inputs.' metadata={'source': 'https://lilianweng.github.io/posts/2023-06-23-agent/', 'start_index': 2192}
```

### 选择[模型](https://python.langchain.com/v0.2/docs/tutorials/rag/#choosing-a-model)

`ChatModel`：LLM 支持的聊天模型。接收一系列消息并返回一条消息。

+   [文档](https://python.langchain.com/v0.2/docs/how_to/#chat-models)

+   [集成](https://python.langchain.com/v0.2/docs/integrations/chat/)：25 多种集成可供选择。

+   [接口](https://api.python.langchain.com/en/latest/language_models/langchain_core.language_models.chat_models.BaseChatModel.html)：基本接口的 API 参考。

`LLM`：文本输入文本输出语言模型。输入一个字符串并返回一个字符串。

+   [文档](https://python.langchain.com/v0.2/docs/how_to/#llms)

+   [集成](https://python.langchain.com/v0.2/docs/integrations/llms/)：75 多种集成可供选择。

+   [接口](https://api.python.langchain.com/en/latest/language_models/langchain_core.language_models.llms.BaseLLM.html)：基本接口的 API 参考。

[在此处](https://python.langchain.com/v0.2/docs/tutorials/local_rag/)查看具有本地运行模型的 RAG 指南。

### 自定义[提示](https://python.langchain.com/v0.2/docs/tutorials/rag/#customizing-the-prompt)

[如上所示，我们可](https://smith.langchain.com/hub/rlm/rag-prompt)以从提示中心加载提示（例如，此 RAG 提示）。提示也可以轻松自定义：

```
from langchain_core.prompts import PromptTemplate

template = """Use the following pieces of context to answer the question at the end.
If you don't know the answer, just say that you don't know, don't try to make up an answer.
Use three sentences maximum and keep the answer as concise as possible.
Always say "thanks for asking!" at the end of the answer.

{context}

Question: {question}

Helpful Answer:"""
custom_rag_prompt = PromptTemplate.from_template(template)

rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | custom_rag_prompt
    | llm
    | StrOutputParser()
)

rag_chain.invoke("What is Task Decomposition?")
```

> **API 参考：**[`PromptTemplate`](https://api.python.langchain.com/en/latest/prompts/langchain_core.prompts.prompt.PromptTemplate.html)

```
'Task decomposition is the process of breaking down a complex task into smaller, more manageable parts. Techniques like Chain of Thought (CoT) and Tree of Thoughts allow an agent to "think step by step" and explore multiple reasoning possibilities, respectively. This process can be executed by a Language Model with simple prompts, task-specific instructions, or human inputs. Thanks for asking!'
```

查看[LangSmith 跟踪](https://smith.langchain.com/public/da23c4d8-3b33-47fd-84df-a3a582eedf84/r)

## 下一步

基于数据构建基本问答应用程序的步骤：

+   [使用文档加载器](https://python.langchain.com/v0.2/docs/concepts/#document-loaders)加载数据
