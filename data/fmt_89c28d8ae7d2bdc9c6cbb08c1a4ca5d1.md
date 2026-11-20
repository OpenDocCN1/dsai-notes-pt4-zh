
# 手把手教你认识学会LangChain

> 来源：[https://agijuejin.feishu.cn/docx/A9DAdQFwioWxaNxLDEccNZZKn3g](https://agijuejin.feishu.cn/docx/A9DAdQFwioWxaNxLDEccNZZKn3g)
> 作者：爱吃牛油果的璐璐
> 原文：`https://zhuanlan.zhihu.com/p/640936557`

## 什么是LangChain

LangChain: 一个让你的LLM变得更强大的开源框架。LangChain 就是一个 LLM 编程框架，你想开发一个基于 LLM 应用，需要什么组件它都有，直接使用就行；甚至针对常规的应用流程，它利用链(LangChain中`Chain`的由来)这个概念已经内置标准化方案了。下面我们从新兴的大语言模型（LLM）技术栈的角度来看看为何它的理念这么受欢迎。

## LangChain起源

LangChain 的作者是 Harrison Chase，最初是于 2022 年 10 月开源的一个项目，在 GitHub 上获得大量关注之后迅速转变为一家初创公司。2017 年 Harrison Chase 还在哈佛上大学，如今已是硅谷的一家热门初创公司的 CEO，这对他来说是一次重大而迅速的跃迁。Insider 独家报道，人工智能初创公司 LangChain 在种子轮一周后，再次获得红杉领投的 2000 万至 2500 万美元融资，估值达到 2 亿美元。

## LangChain六大主要领域

+ 管理和优化`prompt`。不同的任务使用不同`prompt`，如何去管理和优化这些`prompt`是`langchain`的主要功能之一。

+ 链，初步理解为一个具体任务中不同子任务之间的一个调用。

+ 数据增强的生成，数据增强生成涉及特定类型的链，它首先与外部数据源交互以获取数据用于生成步骤。这方面的例子包括对长篇文字的总结和对特定数据源的提问/回答。

+ 代理，根据不同的指令采取不同的行动，直到整个流程完成为止。

+ 评估，生成式模型是出了名的难以用传统的指标来评估。评估它们的一个新方法是使用语言模型本身来进行评估。LangChain提供了一些提示/链来协助这个工作。

+ 内存：在整个流程中帮我们管理一些中间状态。

总的来说LangChain可以理解为：在一个流程的整个生命周期中，管理和优化`prompt`，根据`prompt`使用不同的代理进行不同的动作，在这期间使用内存管理中间的一些状态，然后使用链将不同代理之间进行连接起来，最终形成一个闭环。

## LangChain的主要价值组件

+ 组件：用于处理语言模型的抽象概念，以及每个抽象概念的实现集合。无论你是否使用LangChain框架的其他部分，组件都是模块化的，易于使用。

+ 现成的链：用于完成特定高级任务的组件的结构化组合。现成的链使人容易上手。对于更复杂的应用和细微的用例，组件使得定制现有链或建立新链变得容易。

## LangChain组件

+ `model I/O`：语言模型接口

+ `data connection`：与特定任务的数据接口

+ `chains`：构建调用序列

+ `agents`：给定高级指令，让链选择使用哪些工具

+ `memory`：在一个链的运行之间保持应用状态

+ `callbacks`：记录并流式传输任何链的中间步骤

+ `indexes`：索引指的是结构化文件的方法，以便LLM能够与它们进行最好的交互

## 数据连接组件`data connection`

LLM应用需要用户特定的数据，这些数据不属于模型的训练集。LangChain通过以下方式提供了加载、转换、存储和查询数据的构建模块：

+ 文档加载器：从许多不同的来源加载文档

+ 文档转换器：分割文档，删除多余的文档等

+ 文本嵌入模型：采取非结构化文本，并把它变成一个浮点数的列表 矢量存储：存储和搜索嵌入式数据

+ 检索器：查询你的数据

## `data connection`整体流程

![](img/c4a1ffee2e83c3d97e16f980979b96f6.png)

## data connection——文档加载器

### `python`安装包命令

```
pip install langchain
pip install unstructured
pip install jq
```

### CSV基本用法

```
import os
from pathlib import Path

from langchain.document_loaders import UnstructuredCSVLoader
from langchain.document_loaders.csv_loader import CSVLoader
EXAMPLE_DIRECTORY = file_path = Path(__file__).parent.parent / &quot;examples&quot;


def test_unstructured_csv_loader() -&gt; None:
    &quot;&quot;&quot;Test unstructured loader.&quot;&quot;&quot;
    file_path = os.path.join(EXAMPLE_DIRECTORY, &quot;stanley-cups.csv&quot;)
    loader = UnstructuredCSVLoader(str(file_path))
    docs = loader.load()
    print(docs)
    assert len(docs) == 1

def test_csv_loader():
  file_path = os.path.join(EXAMPLE_DIRECTORY, &quot;stanley-cups.csv&quot;)
  loader = CSVLoader(file_path)
  docs = loader.load()
  print(docs)

test_unstructured_csv_loader()
test_csv_loader()
```

### 文件目录用法

```
from langchain.document_loaders import DirectoryLoader, TextLoader

text_loader_kwargs={&#x27;autodetect_encoding&#x27;: True}
loader = DirectoryLoader(&#x27;../examples/&#x27;, 
              glob=&quot;**/*.txt&quot;,  # 遍历txt文件
              show_progress=True,  # 显示进度
              use_multithreading=True,  # 使用多线程
              loader_cls=TextLoader,  # 使用加载数据的方式
              silent_errors=True,  # 遇到错误继续
              loader_kwargs=text_loader_kwargs)  # 可以使用字典传入参数

docs = loader.load()
print(&quot;\n&quot;)
print(docs[0])
```

### HTML用法

```
from langchain.document_loaders import UnstructuredHTMLLoader, BSHTMLLoader
loader = UnstructuredHTMLLoader(&quot;../examples/example.html&quot;)
docs = loader.load()
print(docs[0])

loader = BSHTMLLoader(&quot;../examples/example.html&quot;)
docs = loader.load()
print(docs[0])
```

### JSON用法

```
import json
from pathlib import Path
from pprint import pprint


file_path=&#x27;../examples/facebook_chat.json&#x27;
data = json.loads(Path(file_path).read_text())
pprint(data)

&quot;&quot;&quot;
{&#x27;image&#x27;: {&#x27;creation_timestamp&#x27;: 1675549016, &#x27;uri&#x27;: &#x27;image_of_the_chat.jpg&#x27;},
 &#x27;is_still_participant&#x27;: True,
 &#x27;joinable_mode&#x27;: {&#x27;link&#x27;: &#x27;&#x27;, &#x27;mode&#x27;: 1},
 &#x27;magic_words&#x27;: [],
 &#x27;messages&#x27;: [{&#x27;content&#x27;: &#x27;Bye!&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 2&#x27;,
               &#x27;timestamp_ms&#x27;: 1675597571851},
              {&#x27;content&#x27;: &#x27;Oh no worries! Bye&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 1&#x27;,
               &#x27;timestamp_ms&#x27;: 1675597435669},
              {&#x27;content&#x27;: &#x27;No Im sorry it was my mistake, the blue one is not &#x27;
                          &#x27;for sale&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 2&#x27;,
               &#x27;timestamp_ms&#x27;: 1675596277579},
              {&#x27;content&#x27;: &#x27;I thought you were selling the blue one!&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 1&#x27;,
               &#x27;timestamp_ms&#x27;: 1675595140251},
              {&#x27;content&#x27;: &#x27;Im not interested in this bag. Im interested in the &#x27;
                          &#x27;blue one!&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 1&#x27;,
               &#x27;timestamp_ms&#x27;: 1675595109305},
              {&#x27;content&#x27;: &#x27;Here is $129&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 2&#x27;,
               &#x27;timestamp_ms&#x27;: 1675595068468},
              {&#x27;photos&#x27;: [{&#x27;creation_timestamp&#x27;: 1675595059,
                           &#x27;uri&#x27;: &#x27;url_of_some_picture.jpg&#x27;}],
               &#x27;sender_name&#x27;: &#x27;User 2&#x27;,
               &#x27;timestamp_ms&#x27;: 1675595060730},
              {&#x27;content&#x27;: &#x27;Online is at least $100&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 2&#x27;,
               &#x27;timestamp_ms&#x27;: 1675595045152},
              {&#x27;content&#x27;: &#x27;How much do you want?&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 1&#x27;,
               &#x27;timestamp_ms&#x27;: 1675594799696},
              {&#x27;content&#x27;: &#x27;Goodmorning! $50 is too low.&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 2&#x27;,
               &#x27;timestamp_ms&#x27;: 1675577876645},
              {&#x27;content&#x27;: &#x27;Hi! Im interested in your bag. Im offering $50. Let &#x27;
                          &#x27;me know if you are interested. Thanks!&#x27;,
               &#x27;sender_name&#x27;: &#x27;User 1&#x27;,
               &#x27;timestamp_ms&#x27;: 1675549022673}],
 &#x27;participants&#x27;: [{&#x27;name&#x27;: &#x27;User 1&#x27;}, {&#x27;name&#x27;: &#x27;User 2&#x27;}],
 &#x27;thread_path&#x27;: &#x27;inbox/User 1 and User 2 chat&#x27;,
 &#x27;title&#x27;: &#x27;User 1 and User 2 chat&#x27;}
&quot;&quot;&quot;
```

使用`langchain`加载数据：

```
from langchain.document_loaders import JSONLoader

loader = JSONLoader(

    file_path=&#x27;../examples/facebook_chat.json&#x27;,

    jq_schema=&#x27;.messages[].content&#x27; # 会报错Expected page_content is string, got &lt;class &#x27;NoneType&#x27;&gt; instead.

    page_content=False, # 报错后添加这一行)

data = loader.load()

print(data[0])
```

### PDF用法

第一种用法

```
from langchain.document_loaders import PyPDFLoader

loader = PyPDFLoader(&quot;../examples/layout-parser-paper.pdf&quot;)

pages = loader.load_and_split()

print(pages[0])
```

第二种用法

```
from langchain.document_loaders import MathpixPDFLoader

loader = MathpixPDFLoader(&quot;example_data/layout-parser-paper.pdf&quot;)

data = loader.load()

print(data[0])
```

第三种用法

```
from langchain.document_loaders import UnstructuredPDFLoader

loader = UnstructuredPDFLoader(&quot;../examples/layout-parser-paper.pdf&quot;)

data = loader.load()

print(data[0])
```

## data connection——文档转换

加载了文件后，经常会需要转换它们以更好地适应应用。最简单的例子是，你可能想把一个长的文档分割成较小的块状，以适应你的模型的上下文窗口。LangChain有许多内置的文档转换工具，可以很容易地对文档进行分割、组合、过滤和其他操作。

### 通过字符进行文本分割

