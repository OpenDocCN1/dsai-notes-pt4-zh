
# Langchain Agent - 自定义工具

> 来源：[https://agijuejin.feishu.cn/docx/JgH8dlqv1oC2JxxG9qmc77Qnnhf](https://agijuejin.feishu.cn/docx/JgH8dlqv1oC2JxxG9qmc77Qnnhf)

> 作者：[lucas大叔](https://www.zhihu.com/people/lucasda-shu)

> 原文：https://zhuanlan.zhihu.com/p/694458241

在构建自定义agent时，需要提供可以使用的工具列表。除了实际调用的函数外，工具还包括以下部分：

+ `name`（`str`）：必填项，提供给agent的工具集合中的唯一名称

+ `description`（`str`）：可选项，但建议提供。因为agent根据描述决定工具使用

+ `args_schema`（`Pydantic BaseModel`）：可选项，但建议提供。用于提供更多的信息（例如，few-shot样例）或期望参数的验证

`langchain`提供了多种方式定义工具，本文将针对以下两个函数介绍定义工具的方法：

1. 总是返回字符串"LangChain"的虚构搜索函数
2. 将两个数字相乘的乘法函数

这两个函数最大的区别是，第一个函数只有一个输入，第二个函数需要多个输入。许多agent只对单个输入的函数奏效，因此了解使用这些函数非常重要。大多数情况下，定义这些自定义工具是一样的，但也有一些不同。

```
# Import things that are needed generically
from langchain.pydantic_v1 import BaseModel, Field
from langchain.tools import BaseTool, StructuredTool, tool
```

## `@tool`修饰符

`@tool`修饰符是自定义工具最简单的方法。默认情况下修饰符用函数的名字作为工具名称，但传字符串作为第一个参数可以重命名工具名称。除此之外，修饰符将用函数的`docstring`作为工具描述，因此`docstring`必须提供。

```
@tool
def search(query: str) -> str:
    """Look up things online."""
    return "LangChain"

print(search.name)
print(search.description)
print(search.args)
```

打印结果

```
search
search(query: str) -> str - Look up things online.
{'query': {'title': 'Query', 'type': 'string'}}
```

### 乘法函数

```
@tool
def multiply(a: int, b: int) -> int:
    """Multiply two numbers."""
    return a * b

print(multiply.name)
print(multiply.description)
print(multiply.args)
```

打印结果

```
multiply
multiply(a: int, b: int) -> int - Multiply two numbers.
{'a': {'title': 'A', 'type': 'integer'}, 'b': {'title': 'B', 'type': 'integer'}}
```

在工具修饰符中可以自定义工具名称和JSON参数。

```
class SearchInput(BaseModel):
    query: str = Field(description="should be a search query")


@tool("search-tool", args_schema=SearchInput, return_direct=True)
def search(query: str) -> str:
    """Look up things online."""
    return "LangChain"

print(search.name)
print(search.description)
print(search.args)
print(search.return_direct)
```

可以看到，工具名称和参数已经重写。

```
search-tool
search-tool(query: str) -> str - Look up things online.
{'query': {'title': 'Query', 'description': 'should be a search query', 'type': 'string'}}
True
```

## `BaseTool`子类

用户也可以基于`BaseTool`类构造子类显式自定义工具。这为工具定义提供了最大限度的把控，但需要做更多的工作。

```
from typing import Optional, Type

from langchain.callbacks.manager import (
    AsyncCallbackManagerForToolRun,
    CallbackManagerForToolRun,
)


class SearchInput(BaseModel):
    query: str = Field(description="should be a search query")


class CalculatorInput(BaseModel):
    a: int = Field(description="first number")
    b: int = Field(description="second number")


class CustomSearchTool(BaseTool):
    name = "custom_search"
    description = "useful for when you need to answer questions about current events"
    args_schema: Type[BaseModel] = SearchInput

    def _run(
        self, query: str, run_manager: Optional[CallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool."""
        return "LangChain"

    async def _arun(
        self, query: str, run_manager: Optional[AsyncCallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool asynchronously."""
        raise NotImplementedError("custom_search does not support async")


class CustomCalculatorTool(BaseTool):
    name = "Calculator"
    description = "useful for when you need to answer questions about math"
    args_schema: Type[BaseModel] = CalculatorInput
    return_direct: bool = True

    def _run(
        self, a: int, b: int, run_manager: Optional[CallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool."""
        return a * b

    async def _arun(
        self,
        a: int,
        b: int,
        run_manager: Optional[AsyncCallbackManagerForToolRun] = None,
    ) -> str:
        """Use the tool asynchronously."""
        raise NotImplementedError("Calculator does not support async")

search = CustomSearchTool()
print(search.name)
print(search.description)
print(search.args)
```

打印结果

```
custom_search
useful for when you need to answer questions about current events
{'query': {'title': 'Query', 'description': 'should be a search query', 'type': 'string'}}
```

### 乘法函数

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
{'a': {'title': 'A', 'description': 'first number', 'type': 'integer'}, 'b': {'title': 'B', 'description': 'second number', 'type': 'integer'}}
True
```

## `StructuredTool`数据类

`StructuredTool`数据类是前两种方法的混合，比从`BaseTool`类继承更方便，但提供了比只使用修饰符更多的功能。

```
def search_function(query: str):
    return "LangChain"


search = StructuredTool.from_function(
    func=search_function,
    name="Search",
    description="useful for when you need to answer questions about current events",
    # coroutine= ... <- you can specify an async method if desired as well
)

print(search.name)
print(search.description)
print(search.args)
```

打印结果

```
Search
Search(query: str) - useful for when you need to answer questions about current events
{'query': {'title': 'Query', 'type': 'string'}}
```

也可以自定义`args_schema`提供输入的更多信息。

```
class CalculatorInput(BaseModel):
    a: int = Field(description="first number")
    b: int = Field(description="second number")


def multiply(a: int, b: int) -> int:
    """Multiply two numbers."""
    return a * b


calculator = StructuredTool.from_function(
    func=multiply,
    name="Calculator",
    description="multiply numbers",
    args_schema=CalculatorInput,
    return_direct=True,
    # coroutine= ... <- you can specify an async method if desired as well
)

print(calculator.name)
print(calculator.description)
print(calculator.args)
```

打印结果

```
Calculator
Calculator(a: int, b: int) -> int - multiply numbers
{'a': {'title': 'A', 'description': 'first number', 'type': 'integer'}, 'b': {'title': 'B', 'description': 'second number', 'type': 'integer'}}
```

## 处理工具错误

当工具遇到错误且未捕获异常时，agent将停止执行。如果希望agent继续执行，可以引发`ToolException`并相应地设置`handle_tool_error`。

当抛出`ToolException`时，agent不会停止工作，而是会根据工具的`handle_tool_error`变量处理异常，处理结果会作为observation返回给agent，并以红色打印。

可以设置`handle_tool_error`为`True`，将其设置为统一字符串值，或将其设置成函数。如果它被设置为函数，那么该函数应该以`ToolException`作为参数并返回`str`值。

需要注意的是仅引发`ToolException`是无效的。需要先设置该工具的`handle_tool_error`，因为它的默认值为`False`。

```
from langchain_core.tools import ToolException


def search_tool1(s: str):
    raise ToolException("The search tool1 is not available.")
```

首先，让我们看下如果不设置`handle_tool_error`会发生什么

```
search = StructuredTool.from_function(
    func=search_tool1,
    name="Search_tool1",
    description="A bad tool",
)

search.run("test")
```

执行结果

```
ToolException: The search tool1 is not available.
```

现在将`handle_tool_error`设为`True`



# 工具错误处理

```
search = StructuredTool.from_function(
    func=search_tool1,
    name="Search_tool1",
    description="A bad tool",
    handle_tool_error=True,
)

search.run("test")
```

执行结果

```
'The search tool1 is not available.'
```

我们也可以自定义处理工具错误的方式

```
def _handle_error(error: ToolException) -> str:
    return (
        "The following errors occurred during tool execution:"
        + error.args[0]
        + "Please try another tool."
    )

search = StructuredTool.from_function(
    func=search_tool1,
    name="Search_tool1",
    description="A bad tool",
    handle_tool_error=_handle_error,
)

search.run("test")
```

执行结果

```
'The following errors occurred during tool execution:The search tool1 is not available.Please try another tool.'
```

## 参考文档

[https://python.langchain.com/docs/modules/agents/how_to/custom_agent/](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/modules/agents/how_to/custom_agent/)

发布于 2024-04-30 20:08 ・IP 属地江苏
