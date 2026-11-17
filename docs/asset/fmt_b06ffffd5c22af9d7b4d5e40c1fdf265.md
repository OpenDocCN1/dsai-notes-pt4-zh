
```
generations=[[Generation(text='\n\nThe hex code of black is #000000.', generation_info={'finish_reason': 'stop', 'logprobs': None})]] llm_output={'token_usage': {'prompt_tokens': 10, 'total_tokens': 21, 'completion_tokens': 11}, 'model_name': 'text-davinci-003'} run=None
```

The hex code of black is #000000.

[1223]

### 2. 通过运行时的函数调用

`Model`，`Agent`，`Tool`，以及 `Chain` 的请求执行函数都接受 `callbacks` 参数，比如 `LLMChain` 的 `run` 函数，`OpenAI` 的 `predict` 函数，等都能接受 `callbacks` 参数，在运行时指定回调处理器。

以 `OpenAI` 模型类为例：

```
timerHandler = TimerHandler()

llm = OpenAI()

response = llm.predict("What is the HEX code of color BLACK?", callbacks=[timerHandler])

print(response)

timerHandler.durations
```

> 这段代码的目的是创建一个`OpenAI`对象，使用它来预测对一个问题的答案，并使用`TimerHandler`对象来测量和记录这个操作的执行时间。

> 首先，它创建了一个`TimerHandler`对象并将其存储在`timerHandler`变量中。这个对象将用于测量和记录操作的执行时间。

> 接着，它创建了一个`OpenAI`对象并将其存储在`llm`变量中。

> 然后，它调用了`llm`对象的`predict`方法，参数是一个字符串"What is the HEX code of color BLACK?"和一个包含`timerHandler`的列表。这个列表被作为`callbacks`参数传入，意味着`timerHandler`对象将被用于处理`predict`方法的回调。`predict`方法的返回结果被存储在`response`变量中，这个结果是对"What is the HEX code of color BLACK?"这个问题的预测结果。接着，打印了`response`的值。

> 最后，打印了`timerHandler`对象的`durations`属性的值，这个值是一个包含了所有操作的执行时间的列表。

> 这段代码的主要作用是预测对一个问题的答案，并测量这个操作的执行时间。

你应该同样期望如下输出：

```
['What is the HEX code of color BLACK?']

generations=[[Generation(text='\n\nThe hex code of black is #000000.', generation_info={'finish_reason': 'stop', 'logprobs': None})]] llm_output={'token_usage': {'prompt_tokens': 10, 'total_tokens': 21, 'completion_tokens': 11}, 'model_name': 'text-davinci-003'} run=None

The hex code of black is #000000.

[1138]
```

关于 `Agent`，`Tool` 等的使用，请参考官方文档API。

### 章节小结

本节课程中，我们学习了什么是 `Callback` 回调，如何使用回调处理器，以及在哪些场景下可以接入回调处理器。下一节，我们将一起完成一个完整的应用案例，来巩固本系列课程的知识点。

本节课程的完整示例代码，请参考 `09_Callbacks.ipynb`。

**相关文档资料链接：**

1.  Python Langchain官方文档

# 演练

## 简介

我们将利用过去9讲学习的知识，来完成一个具备完整功能集的LLM应用。该应用基于 LangChain 框架，以某 PDF 文件的内容为知识库，提供给用户基于该文件内容的问答能力。

我们利用 LangChain 的QA chain，结合 `Chroma` 来实现PDF文档的语义化搜索。示例代码所引用的是AWS Serverless Developer Guide，该PDF文档共84页。

本讲的完整代码请参考 `10_Example.jpynb`

1.  安装必要的 Python 包

```
!pip install -q langchain==0.0.235 openai chromadb pymupdf tiktoken
```

+   这条命令在执行时会静默地（`-q`参数表示quiet，即安装过程中不会打印出过多的信息）安装以下几个库：

+   `langchain`：这是一个用于处理和分析语言数据的库。版本号是0.0.235。

+   `openai`：这是OpenAI的官方Python库，可以用于调用OpenAI的API，例如GPT-3等。

+   `chromadb`：这是一个用于处理和查询ChromaDB数据库的库。

+   `pymupdf`：这是一个用于处理PDF文件的库，可以用于读取、修改和生成PDF文件。

+   `tiktoken`：这是OpenAI开发的一个库，可以用于计算一个文本在一个transformer模型中会使用多少tokens。

1.  设置OpenAI环境

```
import os

os.environ['OPENAI_API_KEY'] = '您的有效openai api key'
```

+   这段代码是在Python环境中设置环境变量`OPENAI_API_KEY`的值。这个环境变量通常用于存储OpenAI API的秘钥，这个秘钥是使用OpenAI API时需要的。

+   在这段代码中，`os.environ`是一个包含了所有环境变量的字典。通过为这个字典的`'OPENAI_API_KEY'`键赋值，可以设置环境变量`OPENAI_API_KEY`的值。

+   请注意，你需要将`'您的有效openai api key'`替换为你的实际的OpenAI API秘钥。这个秘钥可以在OpenAI的网站上获取。如果你没有这个秘钥，你将无法使用OpenAI API。

1.  下载PDF文件AWS Serverless Developer Guide

```
!wget https://docs.aws.amazon.com/pdfs/serverless/latest/devguide/serverless-core.pdf

PDF_NAME = 'serverless-core.pdf'
```

+   这段代码首先使用`wget`命令从AWS的网站上下载了一个名为`serverless-core.pdf`的PDF文件。`wget`是一个常用的命令行工具，用于从网络上下载文件。

+   然后，这段代码定义了一个Python变量`PDF_NAME`，并将其值设置为下载的PDF文件的名字，即`'serverless-core.pdf'`。

1.  加载PDF文件

```
from langchain.document_loaders import PyMuPDFLoader

docs = PyMuPDFLoader(PDF_NAME).load()

print (f'There are {len(docs)} document(s) in {PDF_NAME}.')

print (f'There are {len(docs[0].page_content)} characters in the first page of your document.')
```

+   这段代码首先从`langchain.document_loaders`模块导入了`PyMuPDFLoader`类。`PyMuPDFLoader`是一个用于加载和处理PDF文档的类，它使用PyMuPDF库来读取PDF文档。

+   然后，这段代码创建了一个`PyMuPDFLoader`对象，并将之前定义的`PDF_NAME`变量（即PDF文件的名字）作为参数传递给了`PyMuPDFLoader`的构造函数。这个对象的`load`方法被调用，将PDF文件加载到内存中，并返回一个文档对象列表。

+   接下来，这段代码打印出了加载的文档数量，以及第一个文档的第一页中的字符数量。这是通过访问文档对象的`page_content`属性实现的，这个属性包含了文档的每一页的内容。

1.  拆分文档并存储文本嵌入的向量数据

```
from langchain.embeddings.openai import OpenAIEmbeddings

from langchain.text_splitter import RecursiveCharacterTextSplitter

from langchain.vectorstores import Chroma

text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)

split_docs = text_splitter.split_documents(docs)

embeddings = OpenAIEmbeddings()

vectorstore = Chroma.from_documents(split_docs, embeddings, collection_name="serverless_guide")
```

+   这段代码首先从`langchain.embeddings.openai`模块导入了`OpenAIEmbeddings`类，从`langchain.text_splitter`模块导入了`RecursiveCharacterTextSplitter`类，从`langchain.vectorstores`模块导入了`Chroma`类。这些类都是`langchain`库的一部分，用于处理和分析文本数据。

+   然后，这段代码创建了一个`RecursiveCharacterTextSplitter`对象。这是一个用于将文本分割为较小块的对象，它的构造函数接受两个参数：`chunk_size`和`chunk_overlap`。`chunk_size`参数指定每个块的大小，`chunk_overlap`参数指定每个块之间的重叠字符数。这个对象的`split_documents`方法被调用，将之前加载的文档分割为较小的块。

+   接下来，这段代码创建了一个`OpenAIEmbeddings`对象。`OpenAIEmbeddings`是一个用于获取文本的嵌入向量的对象，它使用OpenAI的GPT-3模型来生成嵌入向量。嵌入向量是一种将文本转换为数值向量的方法，这种向量可以捕获文本的语义信息。

+   最后，这段代码调用了`Chroma`类的`from_documents`方法，生成了一个`Chroma`对象。`Chroma`类是一个用于存储和查询嵌入向量的对象。`from_documents`方法接受三个参数：分割后的文档、嵌入对象和集合名。这个方法会为每个文档块生成一个嵌入向量，并将这些向量存储在`Chroma`对象中。

1.  基于OpenAI创建QA链

```
from langchain.llms import OpenAI

from langchain.chains.question_answering import load_qa_chain

llm = OpenAI(temperature=0)

chain = load_qa_chain(llm, chain_type="stuff")
```

+   这段代码首先从`langchain.llms`模块导入了`OpenAI`类，从`langchain.chains.question_answering`模块导入了`load_qa_chain`函数。这些都是`langchain`库的一部分，用于处理和分析文本数据。

+   然后，这段代码创建了一个`OpenAI`对象，并设置了`temperature`参数为0。在语言模型中，`temperature`参数用于控制生成文本的随机性。`temperature`值越高，生成的文本越随机；`temperature`值越低，生成的文本越确定。在这个例子中，`temperature`被设置为0，表示生成的文本将尽可能确定。

+   接下来，这段代码调用了`load_qa_chain`函数，加载了一个问答链。问答链是一种用于处理问答任务的模型，它可以接受一个问题，并生成一个答案。`load_qa_chain`函数接受两个参数：一个语言模型和一个链类型。在这个例子中，链类型被设置为`"stuff"`。

1.  基于提问，进行相似性查询

```
query = "What is the use case of AWS Serverless?"

similar_docs = vectorstore.similarity_search(query, 3, include_metadata=True)
```

+   这段代码首先定义了一个查询字符串`query`，然后使用`vectorstore`对象的`similarity_search`方法来查找与该查询最相关的文档。

+   `similarity_search`方法接受三个参数：一个查询字符串，一个要返回的最相似文档的数量，以及一个布尔值，指示是否应包含元数据。在这个例子中，查询字符串是"What is the use case of AWS Serverless?"，要返回的最相似文档的数量是3，而且应该包含元数据。

+   `similarity_search`方法的工作原理是，首先，它使用`OpenAIEmbeddings`对象为查询字符串生成一个嵌入向量。然后，它在`vectorstore`对象中查找与该向量最相似的文档向量。相似性是通过计算两个向量之间的余弦相似性来衡量的。最后，它返回与查询最相关的文档及其元数据。

+   这段代码的目的是找出与查询最相关的文档，这可能对于信息检索、问答系统、推荐系统等任务非常有用。然而，实际的效果会取决于`vectorstore`对象中存储的文档的质量和相关性。

1.  基于相关文档，利用QA链完成回答

```
chain.run(input_documents=similar_docs, question=query)
```

+   这段代码调用了之前加载的问答链（`chain`）的`run`方法，以处理你的问题。`run`方法接受两个参数：`input_documents`和`question`。`input_documents`是一个文档列表，这些文档将被用作问题的上下文；`question`是你想问的问题。

+   在这个例子中，`input_documents`是由`vectorstore.similarity_search`方法返回的与查询最相关的文档，`question`是你之前定义的查询字符串："What is the use case of AWS Serverless?"。

+   这段代码的目的是使用问答链来回答你的问题。具体的回答会取决于问答链的实现和`input_documents`的内容。一般来说，问答链会尝试找出`input_documents`中与问题最相关的信息，并生成一个答案。然而，实际的效果会取决于`input_documents`的质量和相关性，以及问答链的能力。

## 演练小结

本节课程中，我们利用所学的知识，完成了第一个完整的LLM应用。希望通过本系列的学习，大家能对 LangChain 框架的使用，有了基本的认识，并且掌握了框架核心组建的使用方法。

**相关文档资料链接：**

1.  Python Langchain官方文档
