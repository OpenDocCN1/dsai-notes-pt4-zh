
### A. 使用Gradio 部署

```python
return self.conversation_history

if __name__ == "__main__":

    folder = "OneFlower"

    bot = ChatbotWithRetrieval(folder)

    # 定义 Gradio 界面

    interface = gr.Interface(

        fn=bot.get_response,  # 使用我们刚刚创建的函数

        inputs="text",  # 输入是文本

        outputs="text",  # 输出也是文本

        live=False,  # 实时更新，这样用户可以连续与模型交互

        title="易速鲜花智能客服",  # 界面标题

        description="请输入问题，然后点击提交。"  # 描述

    )

    interface.launch()  # 启动 Gradio 界面

    # interface.launch(share=True)  # 启动 Gradio Pubelic访问
```

> 这段代码实现了一个基于检索的聊天机器人，并使用`Gradio`创建了一个Web界面。

+ **导入所需的库**：这段代码首先导入了一些`Python`库，这些库将在后续的代码中被使用。这些库包括`os`（用于处理操作系统相关的功能，如环境变量）、`gradio`（用于创建Web应用）、`langchain`（一个用于处理文本和创建聊天机器人的库）等。

+ **设置`OpenAI API密钥`**：这段代码注释掉了一个设置`OpenAI API密钥`的语句。`OpenAI API密钥`是使用`OpenAI`服务（如`GPT-3`等）所必需的。如果你需要使用这些服务，你需要取消这行代码的注释，并将`'Your Key'`替换为你的实际密钥。

+ **`ChatBot`类的实现**：这个类实现了一个基于检索的聊天机器人。它首先从指定的目录中加载文档，然后使用`RecursiveCharacterTextSplitter`将文档分割成小块，接着使用`OpenAIEmbeddings`将这些小块转换成向量，并将这些向量存储在`Qdrant`向量数据库中。然后，它使用`ChatOpenAI`初始化一个语言模型，并使用`ConversationSummaryMemory`初始化一个内存模块。最后，它使用`ConversationalRetrievalChain`设置一个检索链，这个检索链将用于生成机器人的回答。

+ **`Gradio`界面的创建**：这段代码使用`Gradio`创建了一个Web界面。这个界面包含一个标题和一个文本输入框。用户可以在这个输入框中输入他们的问题，然后点击"提交"按钮，机器人就会生成一个回答并显示在界面上。

+ **主函数**：这个函数首先创建一个`ChatbotWithRetrieval`实例，然后定义一个`Gradio`界面，并启动这个界面。这个界面的输入和输出都是文本，用户可以在这个界面上与机器人进行交互。

+ **运行主函数**：如果这个脚本是直接运行的（而不是被其他脚本导入的），那么就会运行主函数。这是一个常见的`Python`编程模式，用于区分脚本是被直接运行还是被导入。

![](img/59e83c2c78d4853384a0a44a464327d7.png)

### B. 使用Streamlit 部署

需要在IDE环境中运行，以`vscode`为例，首先激活`venv`

```powershell
PS C:\temp\lctest> cd .\Scripts\

PS C:\temp\lctest\Scripts> .\activate

(lctest) PS C:\temp\lctest\Scripts> 
```

安装包

```powershell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple streamlit==1.30.0
```

创建`.env`文件

```
OPENAI_API_KEY=sk-xxx

OPENAI_API_BASE=https://api.huaqloud.com/v1
```

代码

```python
# 导入所需的库

import os

import streamlit as st

from langchain.text_splitter import RecursiveCharacterTextSplitter

from langchain.embeddings import OpenAIEmbeddings

from langchain.vectorstores import Qdrant

from langchain.memory import ConversationSummaryMemory

from langchain.chat_models import ChatOpenAI

from langchain.chains import ConversationalRetrievalChain

from langchain.document_loaders import PyPDFLoader

from langchain.document_loaders import Docx2txtLoader

from langchain.document_loaders import TextLoader

# 设置OpenAI API密钥

from dotenv import load_dotenv

load_dotenv()

OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

OPENAI_API_BASE = os.getenv("OPENAI_API_BASE")

# ChatBot类的实现

class ChatbotWithRetrieval:

    def __init__(self, dir):

        # 加载Documents

        base_dir = '.\\OneFlower' # 文档的存放目录

        documents = []

        for file in os.listdir(base_dir): 

            file_path = os.path.join(base_dir, file)

            if file.endswith('.pdf'):

                loader = PyPDFLoader(file_path)

                documents.extend(loader.load())

            elif file.endswith('.docx') or file.endswith('.doc'):

                loader = Docx2txtLoader(file_path)

                documents.extend(loader.load())

            elif file.endswith('.txt'):

                loader = TextLoader(file_path)

                documents.extend(loader.load())

        # 文本的分割

        text_splitter = RecursiveCharacterTextSplitter(chunk_size=200, chunk_overlap=0)

        all_splits = text_splitter.split_documents(documents)

        # 向量数据库

        self.vectorstore = Qdrant.from_documents(

            documents=all_splits, # 以分块的文档

            embedding=OpenAIEmbeddings(), # 用OpenAI的Embedding Model做嵌入

            location=":memory:",  # in-memory 存储

            collection_name="my_documents",) # 指定collection_name

        # 初始化LLM

        self.llm = ChatOpenAI()

        # 初始化Memory

        self.memory = ConversationSummaryMemory(

            llm=self.llm, 

            memory_key="chat_history", 

            return_messages=True

            )

        # 设置Retrieval Chain

        retriever = self.vectorstore.as_retriever()

        self.qa = ConversationalRetrievalChain.from_llm(

            self.llm, 

            retriever=retriever, 

            memory=self.memory

            )

    def chat_loop(self):

        print("Chatbot 已启动! 输入'exit'来退出程序。")

        while True:

            user_input = input("你: ")

            if user_input.lower() == 'exit':

                print("再见!")

                break

            # 调用 Retrieval Chain  

            response = self.qa(user_input)

            print(f"Chatbot: {response['answer']}")

# Streamlit界面的创建

def main():

    st.title("易速鲜花聊天客服")

    # Check if the 'bot' attribute exists in the session state

    if "bot" not in st.session_state:

        st.session_state.bot = ChatbotWithRetrieval("OneFlower")

    user_input = st.text_input("请输入你的问题：")

    if user_input:

        response = st.session_state.bot.qa(user_input)

        st.write(f"Chatbot: {response['answer']}")

if __name__ == "__main__":

    main()
```

运行

```powershell
(lctest) PS C:\Users\cheng\langchan-workshop\notebook> streamlit run chatbot.py
```

```powershell
streamlit run chatbot.py
```

> 这段代码主要实现了一个基于检索的聊天机器人，并使用`Streamlit`创建了一个Web界面。

+ **导入所需的库**：这段代码首先导入了一些`Python`库，这些库将在后续的代码中被使用。这些库包括`os`（用于处理操作系统相关的功能，如环境变量）、`streamlit`（用于创建Web应用）、`langchain`（一个用于处理文本和创建聊天机器人的库）等。

+ **设置`OpenAI API密钥`**：这段代码从环境变量中获取`OpenAI API密钥`和`API`基础URL。这些信息是使用`OpenAI`服务（如`GPT-3`等）所必需的。

+ **`ChatBot`类的实现**：这个类实现了一个基于检索的聊天机器人。它首先从指定的目录中加载文档，然后使用`RecursiveCharacterTextSplitter`将文档分割成小块，接着使用`OpenAIEmbeddings`将这些小块转换成向量，并将这些向量存储在`Qdrant`向量数据库中。然后，它使用`ChatOpenAI`初始化一个语言模型，并使用`ConversationSummaryMemory`初始化一个内存模块。最后，它使用`ConversationalRetrievalChain`设置一个检索链，这个检索链将用于生成机器人的回答。

+ **`Streamlit`界面的创建**：这段代码使用`Streamlit`创建了一个Web界面。这个界面包含一个标题和一个文本输入框。用户可以在这个输入框中输入他们的问题，然后点击`Enter`键，机器人就会生成一个回答并显示在界面上。

+ **主函数**：这个函数首先检查`Streamlit`的会话状态中是否存在一个`'bot'`属性。如果不存在，就创建一个新的`ChatbotWithRetrieval`实例并存储在`st.session_state.bot`中。然后，它创建一个文本输入框，让用户可以输入他们的问题。如果用户输入了一个问题，就调用`st.session_state.bot.qa(user_input)`获取机器人的回答，并显示在界面上。

+ **运行主函数**：如果这个脚本是直接运行的（而不是被其他脚本导入的），那么就会运行主函数。这是一个常见的`Python`编程模式，用于区分脚本是被直接运行还是被导入。

![](img/8a6c458722bfa2fe90e0d48a4475a6d7.png)

# 项目演练：构建基于本地知识库的智能问答系统

## 项目需求

**项目名称**: "易速鲜花"内部员工知识库问答系统

**项目介绍**: "易速鲜花"是一个大型在线鲜花销售平台，拥有独立的业务流程和规范，以及专门为员工编写的SOP手册。新员工入职培训时，会分享相关信息。然而，这些信息分散在内部网和HR部门目录中，查询不便；有时文档过长，员工无法快速找到所需内容；有时公司政策已更新，但员工手头的文档仍为旧版。

为满足上述需求，计划开发一套基于各种内部知识手册的`Doc-QA`系统。该系统将充分利用`LangChain`框架，处理从员工手册中产生的各种问题。问答系统将能够理解员工的问题，并基于最新的员工手册提供准确的答案。

## 技术框架

![](img/2adf980c01c7c707ff1936a5184bd447.png)

整个框架分为这样三个部分。

+ **数据源（Data Sources）**：数据可以有很多种，包括`PDF`在内的非结构化的数据（Unstructured Data）、`SQL`在内的结构化的数据（Structured Data），以及`Python`、`Java`之类的代码（Code）。在这个示例中，我们聚焦于对非结构化数据的处理。

+ **大模型应用（Application，即 `LLM App`）**：以大模型为逻辑引擎，生成我们所需要的回答。用例（Use-Cases）：

+ 大模型生成的回答可以构建出`QA`/ 聊天机器人等系统。

## 核心实现机制

![](img/4e024623cf8067764b6bdd11951f8b7e.png)

具体流程分为下面 5 步。

1. `Loading`：文档加载器把 `Documents` 加载为以 `LangChain` 能够读取的形式。

1. `Splitting`：文本分割器把 `Documents` 切分为指定大小的分割，我把它们称为“文档块”或者“文档片”。

1. `Storage`：将上一步中分割好的“文档块”以“嵌入”（`Embedding`）的形式存储到向量数据库（`Vector DB`）中，形成一个个的“嵌入片”。

1. `Retrieval`：应用程序从存储中**检索**分割后的文档（例如通过比较余弦相似度，找到与输入问题类似的嵌入片）。

1. `Output`：把问题和相似的嵌入片传递给语言模型（`LLM`），使用包含问题和检索到的分割的提示生成答案。

## 核心代码分析

首先激活`venv`，然后进入到项目目录

```powershell
(lctest) PS C:\Users\cheng\langchan-workshop\ProjectDrill> dir

    目录: C:\Users\cheng\langchan-workshop\ProjectDrill

Mode                 LastWriteTime         Length Name

----                 -------------         ------ ----

da----        2024/4/3   上午 9:35                OneFlower

da----        2024/4/3   上午 9:35                static

da----        2024/4/3   上午 9:35                templates
```
