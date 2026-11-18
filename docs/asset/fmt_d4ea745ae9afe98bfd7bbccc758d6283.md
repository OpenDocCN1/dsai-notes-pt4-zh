
# LangChain 核心组件

## 自定义提示模板

接下来，我们将创建一个自定义的提示模板，将函数名称作为输入，并格式化提示模板以提供函数的源代码。

```python
from langchain.prompts import StringPromptTemplate

from pydantic import BaseModel, validator


class FunctionExplainerPromptTemplate(StringPromptTemplate, BaseModel):
    """A custom prompt template that takes in the function name as input, and formats the prompt template to provide the source code of the function."""

    @validator("input_variables")
    def validate_input_variables(cls, v):
        """Validate that the input variables are correct."""
        if len(v) != 1 or "function_name" not in v:
            raise ValueError("function_name must be the only input_variable.")
        return v

    def format(self, **kwargs) -> str:
        # Get the source code of the function
        source_code = get_source_code(kwargs["function_name"])

        # Generate the prompt to be sent to the language model
        prompt = f"""

        Given the function name and source code, generate an English language explanation of the function.

        Function Name: {kwargs["function_name"].__name__}

        Source Code:

        {source_code}

        Explanation:

        """

        return prompt

    def _prompt_type(self):
        return "function-explainer"
```

## 语言模型

语言模型：通过通用接口调用语言模型

LLMs和聊天模型有微妙但重要的区别。LangChain中的`LLM`指的是纯文本完成模型。它们所包含的API将一个字符串提示作为输入并输出一个字符串完成。`OpenAI`的`GPT-3`是作为`LLM`实现的。聊天模型通常由`LLM`支持，但专门为进行对话而进行调整。而且，至关重要的是，他们的提供者API暴露了一个与纯文本完成模型不同的接口。它们不接受单一的字符串，而是接受一个聊天信息的列表作为输入。通常这些信息都标有说话者（通常是 "系统"、"AI "和 "人类 "中的一个）。它们会返回一个（"AI"）聊天信息作为输出。`GPT-4`和`Anthropic`的`Claude`都是作为聊天模型实现的。

为了使`LLM`和聊天模型的交换成为可能，两者都实现了基础语言模型接口。这暴露了常见的方法 `predict` 和 `pred messages`，前者接受一个字符串并返回一个字符串，后者接受消息并返回一个消息。如果你使用一个特定的模型，建议你使用该模型类的特定方法（例如，`LLM`的 "预测 "和聊天模型的 "预测消息"），但如果你正在创建一个应该与不同类型的模型一起工作的应用程序，共享接口会有帮助。

使用`LLM`的最简单方法是可调用：传入一个字符串，得到一个字符串完成。

### generate: batch calls, richer outputs

`generate`让你可以用一串字符串调用模型，得到比文本更完整的响应。这个完整的响应可以包括像多个顶级响应和其他`LLM`提供者的特定信息，例如：

```python
llm_result = llm.generate(["Tell me a joke", "Tell me a poem"] * 15)
len(llm_result.generations)
# 30
llm_result.generations[0]
# [Generation(text='\n\nWhy did the chicken cross the road?\n\nTo get to the other side!'),
#  Generation(text='\n\nWhy did the chicken cross the road?\n\nTo get to the other side.')]
llm_result.generations[-1]
# [Generation(text="\n\nWhat if love neverspeech\n\nWhat if love never ended\n\nWhat if love was only a feeling\n\nI'll never know this love\n\nIt's not a feeling\n\nBut it's what we have for each other\n\nWe just know that love is something strong\n\nAnd we can't help but be happy\n\nWe just feel what love is for us\n\nAnd we love each other with all our heart\n\nWe just don't know how\n\nHow it will go\n\nBut we know that love is something strong\n\nAnd we'll always have each other\n\nIn our lives."),
#  Generation(text='\n\nOnce upon a time\n\nThere was a love so pure and true\n\nIt lasted for centuries\n\nAnd never became stale or dry\n\nIt was moving and alive\n\nAnd the heart of the love-ick\n\nIs still beating strong and true.')]
```

你也可以访问提供商返回的特定信息。这些信息在不同提供商之间不是标准化的。

```python
llm_result.llm_output
# {'token_usage': {'completion_tokens': 3903,
#   'total_tokens': 4023,
#   'prompt_tokens': 120}}
```

LongChain具体支持什么模型，不同模型是怎么使用的可以看官方文档： Language models | ️ Langchain

## 输出分析器

输出分析器：从模型输出中提取信息

输出解析器是帮助结构化语言模型响应的类。有两个主要的方法是输出解析器必须实现的：

- "获取格式说明"：该方法返回一个字符串，包含语言模型的输出应该如何被格式化的指示。
- "解析"： 该方法接收一个字符串（假定是来自语言模型的响应）并将其解析为某种结构。
- "带提示的解析"：该方法是可选方法，它接收一个字符串（假定是来自语言模型的响应）和一个提示（假定是产生这种响应的提示），并将其解析为一些结构。提示主要是在`OutputParser`想要重试或以某种方式修复输出的情况下提供的，并且需要从提示中获得信息来这样做。

```python
from langchain.prompts import PromptTemplate, ChatPromptTemplate, HumanMessagePromptTemplate
from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI
from langchain.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field, validator
from typing import List

model_name = 'text-davinci-003'
temperature = 0.0
model = OpenAI(model_name=model_name, temperature=temperature)

# Define your desired data structure.
class Joke(BaseModel):
    setup: str = Field(description="question to set up a joke")
    punchline: str = Field(description="answer to resolve the joke")

    # You can add custom validation logic easily with Pydantic.
    @validator('setup')
    def question_ends_with_question_mark(cls, field):
        if field[-1] != '?':
            raise ValueError("Badly formed question!")
        return field

# Set up a parser + inject instructions into the prompt template.
parser = PydanticOutputParser(pydantic_object=Joke)
prompt = PromptTemplate(
    template="Answer the user query.\n{format_instructions}\n{query}\n",
    input_variables=["query"],
    partial_variables={"format_instructions": parser.get_format_instructions()}
)

# And a query intented to prompt a language model to populate the data structure.
joke_query = "Tell me a joke."
_input = prompt.format_prompt(query=joke_query)
output = model(_input.to_string())
parser.parse(output)
# Joke(setup='Why did the chicken cross the road?', punchline='To get to the other side!')
```

具体参见： Output parsers | ️ Langchain

## Chain链组件

更复杂的应用需要将`LLM`串联起来——要么相互串联，要么与其他组件串联。这时就需要用到`Chain`链组件。

`LangChain`为这种 "链式 "应用提供了`Chain`接口。其基本接口很简单：

```python
class Chain(BaseModel, ABC):
    """Base interface that all chains should implement."""
    memory: BaseMemory
    callbacks: Callbacks

    def __call__(
        self,
        inputs: Any,
        return_only_outputs: bool = False,
        callbacks: Callbacks = None,
    ) -> Dict[str, Any]:
        ...
```

使用不同链功能的演示，可参见官方文档： How to | ️ Langchain

最受欢迎的链可以参见： Popular | ️ Langchain

链允许我们将多个组件结合在一起，创建一个单一的、连贯的应用程序。例如，我们可以创建一个链，接受用户输入，用`PromptTemplate`格式化，然后将格式化的响应传递给`LLM`。我们可以通过将多个链组合在一起，或将链与其他组件组合在一起，建立更复杂的链。`LLMChain`是最基本的构建块链。它接受一个提示模板，用用户输入的格式化它，并从`LLM`返回响应。

我们首先要创建一个提示模板：

```python
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate

llm = OpenAI(temperature=0.9)
prompt = PromptTemplate(
    input_variables=["product"],
    template="What is a good name for a company that makes {product}?",
)
```

然后，创建一个非常简单的链，它将接受用户的输入，用它来格式化提示，然后将其发送到`LLM`。

```python
from langchain.chains import LLMChain

chain = LLMChain(llm=llm, prompt=prompt)

# Run the chain only specifying the input variable.
print(chain.run("colorful socks"))
# Colorful Toes Co.
```

如果有多个变量，可以用一个字典一次输入它们。

```python
prompt = PromptTemplate(
    input_variables=["company", "product"],
    template="What is a good name for {company} that makes {product}?",
)
chain = LLMChain(llm=llm, prompt=prompt)
print(chain.run({
    'company': "ABC Startup",
    'product': "colorful socks"
}))
# Socktopia Colourful Creations.
```

也可以在`LLMChain`中使用一个聊天模型：

```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts.chat import (
    ChatPromptTemplate,
    HumanMessagePromptTemplate,
)

human_message_prompt = HumanMessagePromptTemplate(
    prompt=PromptTemplate(
        template="What is a good name for a company that makes {product}?",
        input_variables=["product"],
    )
)
```
