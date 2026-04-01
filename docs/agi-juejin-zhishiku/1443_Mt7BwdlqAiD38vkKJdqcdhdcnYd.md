# Langchain Agent - 自定义工具

> 来源：[https://agijuejin.feishu.cn/docx/JgH8dlqv1oC2JxxG9qmc77Qnnhf](https://agijuejin.feishu.cn/docx/JgH8dlqv1oC2JxxG9qmc77Qnnhf)

> 作者：[lucas大叔](https://www.zhihu.com/people/lucasda-shu)

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/694458241)hihu.com/p/694458...

在构建自定义agent时，需要提供可以使用的工具列表。除了实际调用的函数外，工具还包括以下部分：

+   name（str）：必填项，提供给agent的工具集合中的唯一名称

+   description（str）：可选项，但建议提供。因为agent根据描述决定工具使用

+   args_schema（Pydantic BaseModel）：可选项，但建议提供。用于提供更多的信息（例如，few-shot样例）或期望参数的验证

langchain提供了多种方式定义工具，本文将针对以下两个函数介绍定义工具的方法：

1.  总是返回字符串"LangChain"的虚构搜索函数

1.  将两个数字相乘的乘法函数

这两个函数最大的区别是，第一个函数只有一个输入，第二个函数需要多个输入。许多agent只对单个输入的函数奏效，因此了解使用这些函数非常重要。大多数情况下，定义这些自定义工具是一样的，但也有一些不同。

```
# Import things that are needed generically
from langchain.pydantic_v1 import BaseModel, Field
from langchain.tools import BaseTool, StructuredTool, tool
```

## @tool修饰符

@tool修饰符是自定义工具最简单的方法。默认情况下修饰符用函数的名字作为工具名称，但传字符串作为第一个参数可以重命名工具名称。除此之外，修饰符将用函数的docstring作为工具描述，因此docstring必须提供。

```
@tool
def search(query: str) -&gt; str:
    &quot;&quot;&quot;Look up things online.&quot;&quot;&quot;
    return &quot;LangChain&quot;

print(search.name)
print(search.description)
print(search.args)
```

打印结果

```
search
search(query: str) -&gt; str - Look up things online.
{&#x27;query&#x27;: {&#x27;title&#x27;: &#x27;Query&#x27;, &#x27;type&#x27;: &#x27;string&#x27;}}
```

乘法函数

```
@tool
def multiply(a: int, b: int) -&gt; int:
    &quot;&quot;&quot;Multiply two numbers.&quot;&quot;&quot;
    return a * b

print(multiply.name)
print(multiply.description)
print(multiply.args)
```

打印结果

```
multiply
multiply(a: int, b: int) -&gt; int - Multiply two numbers.
{&#x27;a&#x27;: {&#x27;title&#x27;: &#x27;A&#x27;, &#x27;type&#x27;: &#x27;integer&#x27;}, &#x27;b&#x27;: {&#x27;title&#x27;: &#x27;B&#x27;, &#x27;type&#x27;: &#x27;integer&#x27;}}
```

在工具修饰符中可以自定义工具名称和JSON参数。

```
class SearchInput(BaseModel):
    query: str = Field(description=&quot;should be a search query&quot;)


@tool(&quot;search-tool&quot;, args_schema=SearchInput, return_direct=True)
def search(query: str) -&gt; str:
    &quot;&quot;&quot;Look up things online.&quot;&quot;&quot;
    return &quot;LangChain&quot;

print(search.name)
print(search.description)
print(search.args)
print(search.return_direct)
```

可以看到，工具名称和参数已经重写。

```
search-tool
search-tool(query: str) -&gt; str - Look up things online.
{&#x27;query&#x27;: {&#x27;title&#x27;: &#x27;Query&#x27;, &#x27;description&#x27;: &#x27;should be a search query&#x27;, &#x27;type&#x27;: &#x27;string&#x27;}}
True
```

## BaseTool子类

用户也可以基于BaseTool类构造子类显式自定义工具。这为工具定义提供了最大限度的把控，但需要做更多的工作。

```
from typing import Optional, Type

from langchain.callbacks.manager import (
    AsyncCallbackManagerForToolRun,
    CallbackManagerForToolRun,
)


class SearchInput(BaseModel):
    query: str = Field(description=&quot;should be a search query&quot;)


class CalculatorInput(BaseModel):
    a: int = Field(description=&quot;first number&quot;)
    b: int = Field(description=&quot;second number&quot;)


class CustomSearchTool(BaseTool):
    name = &quot;custom_search&quot;
    description = &quot;useful for when you need to answer questions about current events&quot;
    args_schema: Type[BaseModel] = SearchInput

    def _run(
        self, query: str, run_manager: Optional[CallbackManagerForToolRun] = None
    ) -&gt; str:
        &quot;&quot;&quot;Use the tool.&quot;&quot;&quot;
        return &quot;LangChain&quot;

    async def _arun(
        self, query: str, run_manager: Optional[AsyncCallbackManagerForToolRun] = None
    ) -&gt; str:
        &quot;&quot;&quot;Use the tool asynchronously.&quot;&quot;&quot;
        raise NotImplementedError(&quot;custom_search does not support async&quot;)


class CustomCalculatorTool(BaseTool):
    name = &quot;Calculator&quot;
    description = &quot;useful for when you need to answer questions about math&quot;
    args_schema: Type[BaseModel] = CalculatorInput
    return_direct: bool = True

    def _run(
        self, a: int, b: int, run_manager: Optional[CallbackManagerForToolRun] = None
    ) -&gt; str:
        &quot;&quot;&quot;Use the tool.&quot;&quot;&quot;
        return a * b

    async def _arun(
        self,
        a: int,
        b: int,
        run_manager: Optional[AsyncCallbackManagerForToolRun] = None,
    ) -&gt; str:
        &quot;&quot;&quot;Use the tool asynchronously.&quot;&quot;&quot;
        raise NotImplementedError(&quot;Calculator does not support async&quot;)

search = CustomSearchTool()
print(search.name)
print(search.description)
print(search.args)
```

打印结果

```
custom_search
useful for when you need to answer questions about current events
{&#x27;query&#x27;: {&#x27;title&#x27;: &#x27;Query&#x27;, &#x27;description&#x27;: &#x27;should be a search query&#x27;, &#x27;type&#x27;: &#x27;string&#x27;}}
```

乘法函数

```
multiply = CustomCalculatorTool()
print(multiply.name)
print(multiply.description)
print(multiply.args)
print(multiply.return_direct)
```

打印结果

```
Calculator
useful for when you need to answer questions about math
{&#x27;a&#x27;: {&#x27;title&#x27;: &#x27;A&#x27;, &#x27;description&#x27;: &#x27;first number&#x27;, &#x27;type&#x27;: &#x27;integer&#x27;}, &#x27;b&#x27;: {&#x27;title&#x27;: &#x27;B&#x27;, &#x27;description&#x27;: &#x27;second number&#x27;, &#x27;type&#x27;: &#x27;integer&#x27;}}
True
```

## StructuredTool数据类

StructuredTool数据类是前两种方法的混合，比从BaseTool类继承更方便，但提供了比只使用修饰符更多的功能。

```
def search_function(query: str):
    return &quot;LangChain&quot;


search = StructuredTool.from_function(
    func=search_function,
    name=&quot;Search&quot;,
    description=&quot;useful for when you need to answer questions about current events&quot;,
    # coroutine= ... &lt;- you can specify an async method if desired as well
)

print(search.name)
print(search.description)
print(search.args)
```

打印结果

```
Search
Search(query: str) - useful for when you need to answer questions about current events
{&#x27;query&#x27;: {&#x27;title&#x27;: &#x27;Query&#x27;, &#x27;type&#x27;: &#x27;string&#x27;}}
```

也可以自定义args_schema提供输入的更多信息。

```
class CalculatorInput(BaseModel):
    a: int = Field(description=&quot;first number&quot;)
    b: int = Field(description=&quot;second number&quot;)


def multiply(a: int, b: int) -&gt; int:
    &quot;&quot;&quot;Multiply two numbers.&quot;&quot;&quot;
    return a * b


calculator = StructuredTool.from_function(
    func=multiply,
    name=&quot;Calculator&quot;,
    description=&quot;multiply numbers&quot;,
    args_schema=CalculatorInput,
    return_direct=True,
    # coroutine= ... &lt;- you can specify an async method if desired as well
)

print(calculator.name)
print(calculator.description)
print(calculator.args)
```

打印结果

```
Calculator
Calculator(a: int, b: int) -&gt; int - multiply numbers
{&#x27;a&#x27;: {&#x27;title&#x27;: &#x27;A&#x27;, &#x27;description&#x27;: &#x27;first number&#x27;, &#x27;type&#x27;: &#x27;integer&#x27;}, &#x27;b&#x27;: {&#x27;title&#x27;: &#x27;B&#x27;, &#x27;description&#x27;: &#x27;second number&#x27;, &#x27;type&#x27;: &#x27;integer&#x27;}}
```

## 处理工具错误

当工具遇到错误且未捕获异常时，agent将停止执行。如果希望agent继续执行，可以引发 ToolException 并相应地设置 handle_tool_error 。

当抛出 ToolException 时，agent不会停止工作，而是会根据工具的 handle_tool_error 变量处理异常，处理结果会作为observation返回给agent，并以红色打印。

可以设置 handle_tool_error 为 True ，将其设置为统一字符串值，或将其设置成函数。如果它被设置为函数，那么该函数应该以 ToolException 作为参数并返回 str 值。

需要注意的是仅引发 ToolException 是无效的。需要先设置该工具的 handle_tool_error ，因为它的默认值为 False 。

```
from langchain_core.tools import ToolException


def search_tool1(s: str):
    raise ToolException(&quot;The search tool1 is not available.&quot;)
```

首先，让我们看下如果不设置 handle_tool_error 会发生什么

```
search = StructuredTool.from_function(
    func=search_tool1,
    name=&quot;Search_tool1&quot;,
    description=&quot;A bad tool&quot;,
)

search.run(&quot;test&quot;)
```

执行结果

```
ToolException: The search tool1 is not available.
```

现在将 handle_tool_error 设为True

```
search = StructuredTool.from_function(
    func=search_tool1,
    name=&quot;Search_tool1&quot;,
    description=&quot;A bad tool&quot;,
    handle_tool_error=True,
)

search.run(&quot;test&quot;)
```

执行结果

```
&#x27;The search tool1 is not available.&#x27;
```

我们也可以自定义处理工具错误的方式

```
def _handle_error(error: ToolException) -&gt; str:
    return (
        &quot;The following errors occurred during tool execution:&quot;
        + error.args[0]
        + &quot;Please try another tool.&quot;
    )


search = StructuredTool.from_function(
    func=search_tool1,
    name=&quot;Search_tool1&quot;,
    description=&quot;A bad tool&quot;,
    handle_tool_error=_handle_error,
)

search.run(&quot;test&quot;)
```

执行结果

```
&#x27;The following errors occurred during tool execution:The search tool1 is not available.Please try another tool.&#x27;
```

## 参考文档

[https:// python.langchain.com/do](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/how_to/custom_agent/) cs/modules/agents/how_to/custom_agent/

发布于 2024-04-30 20:08 ・IP 属地江苏