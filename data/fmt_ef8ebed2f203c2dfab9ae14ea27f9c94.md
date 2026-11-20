
# 文本分割

斗之力，三段！”

望着测验魔石碑上面闪亮得甚至有些刺眼的五个大字，少年面无表情，唇角有着一抹自嘲，紧握的手掌，因为大力，而导致略微尖锐的指甲深深的刺进了掌心之中，带来一阵阵钻心的疼痛…

“萧炎，斗之力，三段！级别：低级！”测验魔石碑之旁，一位中年男子，看了一眼碑上所显示出来的信息，语气漠然的将之公布了出来…

中年男子话刚刚脱口，便是不出意外的在人头汹涌的广场上带起了一阵嘲讽的骚动。

“三段？嘿嘿，果然不出我所料，这个“天才”这一年又是在原地踏步！”

“哎，这废物真是把家族的脸都给丢光了。”

“要不是族长是他的父亲，这种废物，早就被驱赶出家族，任其自生自灭了，哪还有机会待在家族中白吃白喝。”

“唉，昔年那名闻乌坦城的天才少年，如今怎么落魄成这般模样了啊？”

## CharacterTextSplitter

```python
from langchain.text_splitter import CharacterTextSplitter

text_splitter = CharacterTextSplitter(

    separator="\n\n",

    chunk_size=128,  # 分块长度

    chunk_overlap=10,  # 重合的文本长度

    length_function=len,

)

texts = text_splitter.create_documents([state_of_the_union])

print(texts[0])
```

`metadatas` 用于区分不同的文档。

```python
metadatas = [{"document": 1}, {"document": 2}]

documents = text_splitter.create_documents([state_of_the_union, state_of_the_union], metadatas=metadatas)

pprint(documents)

# 获取切割后的文本

print(text_splitter.split_text(state_of_the_union)[0])
```

## 对代码进行分割

```python
from langchain.text_splitter import (

    RecursiveCharacterTextSplitter,

    Language,

)

print([e.value for e in Language])  # 支持语言

print(RecursiveCharacterTextSplitter.get_separators_for_language(Language.PYTHON))  # 分割符号

PYTHON_CODE = """

def hello_world():

    print("Hello, World!")

# Call the function

hello_world()

"""

python_splitter = RecursiveCharacterTextSplitter.from_language(

    language=Language.PYTHON, chunk_size=50, chunk_overlap=0

)

python_docs = python_splitter.create_documents([PYTHON_CODE])

python_docs
```

```text
[Document(page_content='def hello_world():\n    print("Hello, World!")', metadata={}),

 Document(page_content='# Call the function\nhello_world()', metadata={})]
```

## 通过markdownheader进行分割

举个例子：`md = # Foo\n\n ## Bar\n\nHi this is Jim \nHi this is Joe\n\n ## Baz\n\n Hi this is Molly'`。我们定义分割头：`[("#", "Header 1"),("##", "Header 2")]` 文本应该被公共头进行分割，最终得到：`{'content': 'Hi this is Jim \nHi this is Joe', 'metadata': {'Header 1': 'Foo', 'Header 2': 'Bar'}} {'content': 'Hi this is Molly', 'metadata': {'Header 1': 'Foo', 'Header 2': 'Baz'}}`

```python
from langchain.text_splitter import MarkdownHeaderTextSplitter

markdown_document = "# Foo\n\n    ## Bar\n\nHi this is Jim\n\nHi this is Joe\n\n ### Boo \n\n Hi this is Lance \n\n ## Baz\n\n Hi this is Molly"

headers_to_split_on = [

    ("#", "Header 1"),

    ("##", "Header 2"),

    ("###", "Header 3"),

]

markdown_splitter = MarkdownHeaderTextSplitter(headers_to_split_on=headers_to_split_on)

md_header_splits = markdown_splitter.split_text(markdown_document)

md_header_splits
```

```text
[Document(page_content='Hi this is Jim  \nHi this is Joe', metadata={'Header 1': 'Foo', 'Header 2': 'Bar'}),

 Document(page_content='Hi this is Lance', metadata={'Header 1': 'Foo', 'Header 2': 'Bar', 'Header 3': 'Boo'}),

 Document(page_content='Hi this is Molly', metadata={'Header 1': 'Foo', 'Header 2': 'Baz'})]
```

## 通过字符递归分割

默认列表为：`["\n\n", "\n", " ", ""]`

```python
state_of_the_union = """

斗之力，三段！”

望着测验魔石碑上面闪亮得甚至有些刺眼的五个大字，少年面无表情，唇角有着一抹自嘲，紧握的手掌，因为大力，而导致略微尖锐的指甲深深的刺进了掌心之中，带来一阵阵钻心的疼痛…

“萧炎，斗之力，三段！级别：低级！”测验魔石碑之旁，一位中年男子，看了一眼碑上所显示出来的信息，语气漠然的将之公布了出来…

中年男子话刚刚脱口，便是不出意外的在人头汹涌的广场上带起了一阵嘲讽的骚动。

“三段？嘿嘿，果然不出我所料，这个“天才”这一年又是在原地踏步！”

“哎，这废物真是把家族的脸都给丢光了。”

“要不是族长是他的父亲，这种废物，早就被驱赶出家族，任其自生自灭了，哪还有机会待在家族中白吃白喝。”

“唉，昔年那名闻乌坦城的天才少年，如今怎么落魄成这般模样了啊？”

"""

from langchain.text_splitter import CharacterTextSplitter

from pprint import pprint

text_splitter = CharacterTextSplitter(

    chunk_size=128,

    chunk_overlap=10,

    length_function=len,

)

texts = text_splitter.create_documents([state_of_the_union])

pprint(texts[0].page_content)
```

```text
('斗之力，三段!”\n'

 '\n'

 '望着测验魔石碑上面闪亮得甚至有些刺眼的五个大字，少年面无表情，唇角有着一抹自嘲，紧握的手掌，因为大力，而导致略微尖锐的指甲深深的刺进了掌心之中，带来一阵阵钻心的疼痛…')

['斗之力，三段!”\n'

 '\n'

 '望着测验魔石碑上面闪亮得甚至有些刺眼的五个大字，少年面无表情，唇角有着一抹自嘲，紧握的手掌，因为大力，而导致略微尖锐的指甲深深的刺进了掌心之中，带来一阵阵钻心的疼痛…',

 '“萧炎，斗之力，三段！级别：低级！”测验魔石碑之旁，一位中年男子，看了一眼碑上所显示出来的信息，语气漠然的将之公布了出来…\n'

 '\n'

 '中年男子话刚刚脱口，便是不出意外的在人头汹涌的广场上带起了一阵嘲讽的骚动。']
```

## 通过tokens进行分割

我们知道，语言模型有一个令牌限制。不应该超过令牌的限制。因此，当你把你的文本分成几块时，计算标记的数量是一个好主意。有许多标记器。当你计算文本中的令牌时，你应该使用与语言模型中使用的相同的令牌器。

安装`tiktoken`安装包

```bash
pip install tiktoken
```

`python`代码实现如下：

```python
state_of_the_union = """

斗之力，三段！”

望着测验魔石碑上面闪亮得甚至有些刺眼的五个大字，少年面无表情，唇角有着一抹自嘲，紧握的手掌，因为大力，而导致略微尖锐的指甲深深的刺进了掌心之中，带来一阵阵钻心的疼痛…

“萧炎，斗之力，三段！级别：低级！”测验魔石碑之旁，一位中年男子，看了一眼碑上所显示出来的信息，语气漠然的将之公布了出来…

中年男子话刚刚脱口，便是不出意外的在人头汹涌的广场上带起了一阵嘲讽的骚动。

“三段？嘿嘿，果然不出我所料，这个“天才”这一年又是在原地踏步！”

“哎，这废物真是把家族的脸都给丢光了。”

“要不是族长是他的父亲，这种废物，早就被驱赶出家族，任其自生自灭了，哪还有机会待在家族中白吃白喝。”

“唉，昔年那名闻乌坦城的天才少年，如今怎么落魄成这般模样了啊？”

"""

from langchain.text_splitter import CharacterTextSplitter

from pprint import pprint

text_splitter = CharacterTextSplitter.from_tiktoken_encoder(

    chunk_size=128, chunk_overlap=10

)

texts = text_splitter.split_text(state_of_the_union)

pprint(texts)

for text in texts:

  print(len(text))
```

```text
WARNING:langchain.text_splitter:Created a chunk of size 184, which is longer than the specified 128

['斗之力，三段!”',

 '望着测验魔石碑上面闪亮得甚至有些刺眼的五个大字，少年面无表情，唇角有着一抹自嘲，紧握的手掌，因为大力，而导致略微尖锐的指甲深深的刺进了掌心之中，带来一阵阵钻心的疼痛…',

 '“萧炎，斗之力，三段！级别：低级！”测验魔石碑之旁，一位中年男子，看了一眼碑上所显示出来的信息，语气漠然的将之公布了出来…',

 '中年男子话刚刚脱口，便是不出意外的在人头汹涌的广场上带起了一阵嘲讽的骚动。',

 '“三段？嘿嘿，果然不出我所料，这个“天才”这一年又是在原地踏步！”\n\n“哎，这废物真是把家族的脸都给丢光了。”',

 '“要不是族长是他的父亲，这种废物，早就被驱赶出家族，任其自生自灭了，哪还有机会待在家族中白吃白喝。”',

 '“唉，昔年那名闻乌坦城的天才少年，如今怎么落魄成这般模样了啊？”']

8

83

61

37

55

50

32
```

直接使用`tiktoken`

```python
from langchain.text_splitter import TokenTextSplitter

text_splitter = TokenTextSplitter(chunk_size=128, chunk_overlap=0)

texts = text_splitter.split_text(state_of_the_union)

print(texts[0])
```

# 模型IO组件

模型包括LLM、聊天模型、文本嵌入模型。大型语言模型（LLM）是我们涵盖的第一种模型类型。这些模型接受一个文本字符串作为输入，并返回一个文本字符串作为输出。聊天模型是我们涵盖的第二种类型的模型。这些模型通常由一个语言模型支持，但它们的API更加结构化。具体来说，这些模型接受一个聊天信息的列表作为输入，并返回一个聊天信息。第三类模型是文本嵌入模型。这些模型将文本作为输入，并返回一个浮点数列表。

`LangChain`提供了连接任何语言模型的构建模块。

+ 提示：模板化、动态选择和管理模型输入

对模型进行编程的新方法是通过提示语。一个提示指的是对模型的输入。这种输入通常是由多个组件构成的。`LangChain`提供了几个类和函数，使构建和处理提示信息变得容易。常用的方法是：提示模板：对模型输入进行参数化处；与例子选择器：动态地选择要包含在提示中的例子。

一个提示模板可以包含：对语言模型的指示；一组少量的例子，以帮助语言模型产生一个更好的反应；一个对语言模型的问题。例如：

```python
from langchain import PromptTemplate

template = """ 
You are a naming consultant for new companies.

What is a good name for a company that makes {product}?

"""

prompt = PromptTemplate.from_template(template)

prompt.format(product="colorful socks")
```

```text
You are a naming consultant for new companies.

What is a good name for a company that makes colorful socks?
```

如果需要创建一个与角色相关的消息模板，则需要使用`MessagePromptTemplate`。

```python
template="You are a helpful assistant that translates {input_language} to {output_language}."

system_message_prompt = SystemMessagePromptTemplate.from_template(template)

human_template="{text}"

human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)
```

`MessagePromptTemplate`的类型：`LangChain`提供不同类型的`MessagePromptTemplate`。最常用的是`AIMessagePromptTemplate`、`SystemMessagePromptTemplate`和`HumanMessagePromptTemplate`，它们分别创建`AI`消息、系统消息和人类消息。

通用的提示模板：假设我们想让`LLM`生成一个函数名称的英文解释。为了实现这一任务，我们将创建一个自定义的提示模板，将函数名称作为输入，并对提示模板进行格式化，以提供该函数的源代码。

我们首先创建一个函数，它将返回给定的函数的源代码。

```python
import inspect

def get_source_code(function_name):

    # Get the source code of the function

    return inspect.getsource(function_name)
```
