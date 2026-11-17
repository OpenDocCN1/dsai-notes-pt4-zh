
# 回调处理器

```python
        self, error: Union[Exception, KeyboardInterrupt], **kwargs: Any

    ) -> Any:

        """Run when LLM errors."""

    def on_chain_start(

        self, serialized: Dict[str, Any], inputs: Dict[str, Any], **kwargs: Any

    ) -> Any:

        """Run when chain starts running."""

    def on_chain_end(self, outputs: Dict[str, Any], **kwargs: Any) -> Any:

        """Run when chain ends running."""

    def on_chain_error(

        self, error: Union[Exception, KeyboardInterrupt], **kwargs: Any

    ) -> Any:

        """Run when chain errors."""

    def on_tool_start(

        self, serialized: Dict[str, Any], input_str: str, **kwargs: Any

    ) -> Any:

        """Run when tool starts running."""

    def on_tool_end(self, output: str, **kwargs: Any) -> Any:

        """Run when tool ends running."""

    def on_tool_error(

        self, error: Union[Exception, KeyboardInterrupt], **kwargs: Any

    ) -> Any:

        """Run when tool errors."""

    def on_text(self, text: str, **kwargs: Any) -> Any:

        """Run on arbitrary text."""

    def on_agent_action(self, action: AgentAction, **kwargs: Any) -> Any:

        """Run on agent action."""

    def on_agent_finish(self, finish: AgentFinish, **kwargs: Any) -> Any:

        """Run on agent end."""
```

LangChain 内置支持了一系列回调处理器，我们也可以按需求自定义处理器，以实现特定的业务。

## 内置处理器

`StdOutCallbackHandler` 是 LangChain 所支持的最基本的处理器。它将所有的回调信息打印到标准输出。这对于调试非常有用。

LangChain 链的基类 `Chain` 提供了一个 `callbacks` 参数来指定要使用的回调处理器。请参考 `Chain` 源码，其中代码片段为：

```python
class Chain(Serializable, ABC):

    """Abstract base class for creating structured sequences of calls to components.

    ...

    callbacks: Callbacks = Field(default=None, exclude=True)

    """Optional list of callback handlers (or callback manager). Defaults to None.

    Callback handlers are called throughout the lifecycle of a call to a chain,

    starting with on_chain_start, ending with on_chain_end or on_chain_error.

    Each custom chain can optionally call additional callback methods, see Callback docs

    for full details."""
```

用法如下：

```python
from langchain.callbacks import StdOutCallbackHandler

from langchain.chains import LLMChain

from langchain.llms import OpenAI

from langchain.prompts import PromptTemplate

handler = StdOutCallbackHandler()

llm = OpenAI()

prompt = PromptTemplate.from_template("Who is {name}?")

chain = LLMChain(llm=llm, prompt=prompt, callbacks=[handler])

chain.run(name="Super Mario")
```

> 这段代码首先从`langchain`库的几个模块中导入了一些类。`StdOutCallbackHandler`是一个处理回调并将结果输出到标准输出的类，`LLMChain`是一个创建和运行语言模型链的类，`OpenAI`是一个与`OpenAI` API进行交互的类，`PromptTemplate`是一个创建和管理提示模板的类。
>
> 接着，代码创建了一个`StdOutCallbackHandler`对象并将其存储在`handler`变量中。这个对象处理回调并将结果输出到标准输出。
>
> 然后，创建了一个`OpenAI`对象并将其存储在`llm`变量中。这个对象用于与`OpenAI` API进行交互。
>
> 之后，调用了`PromptTemplate`类的`from_template`方法，参数是一个字符串`"Who is {name}?"`，并将返回的对象存储在`prompt`变量中。这个对象是一个提示模板，用于创建包含了占位符的提示。
>
> 接着，创建了一个`LLMChain`对象，参数是`llm`、`prompt`和一个包含了`handler`的列表，并将这个对象存储在`chain`变量中。这个对象是一个语言模型链，用于运行一系列的操作。
>
> 最后，调用了`chain`对象的`run`方法，参数是一个关键字参数`name="Super Mario"`。这会运行语言模型链，并将结果输出到标准输出。这个结果是对`"Who is Super Mario?"`这个问题的回答。

你应该期望如下输出：

```
> Entering new LLMChain chain...

Prompt after formatting:

Who is Super Mario?

> Finished chain.

\n\nSuper Mario is the protagonist of the popular video game franchise of the same name created by Nintendo. He is a fictional character who stars in video games, television shows, comic books, and films. He is a plumber who is usually portrayed as a portly Italian-American, who is often accompanied by his brother Luigi. He is well known for his catchphrase "It's-a me, Mario!"
```

## 自定义处理器

我们可以通过继承 `BaseCallbackHandler` 来实现自定义的回调处理器。下面是一个简单的例子，`TimerHandler` 将跟踪 `Chain` 或 `LLM` 交互的起止时间，并统计每次交互的处理耗时。

```python
from langchain.callbacks.base import BaseCallbackHandler

import time

class TimerHandler(BaseCallbackHandler):

    def __init__(self) -> None:

        super().__init__()

        self.previous_ms = None

        self.durations = []

    def current_ms(self):

        return int(time.time() * 1000 + time.perf_counter() % 1 * 1000)

    def on_chain_start(self, serialized, inputs, **kwargs) -> None:

        self.previous_ms = self.current_ms()

    def on_chain_end(self, outputs, **kwargs) -> None:

        if self.previous_ms:

          duration = self.current_ms() - self.previous_ms

          self.durations.append(duration)

    def on_llm_start(self, serialized, prompts, **kwargs) -> None:

        self.previous_ms = self.current_ms()

    def on_llm_end(self, response, **kwargs) -> None:

        if self.previous_ms:

          duration = self.current_ms() - self.previous_ms

          self.durations.append(duration)

llm = OpenAI()

timerHandler = TimerHandler()

prompt = PromptTemplate.from_template("What is the HEX code of color {color_name}?")

chain = LLMChain(llm=llm, prompt=prompt, callbacks=[timerHandler])

response = chain.run(color_name="blue")

print(response)

response = chain.run(color_name="purple")

print(response)

timerHandler.durations
```

> 这段代码首先从`langchain.callbacks.base`模块导入了`BaseCallbackHandler`类，并导入了Python的内置`time`模块。
>
> 然后，定义了一个名为`TimerHandler`的类，该类继承自`BaseCallbackHandler`。这个类的目的是为了测量和记录某些操作的执行时间。
>
> 在`TimerHandler`类中，定义了一个`__init__`方法，用于初始化类的实例。在这个方法中，调用了父类的`__init__`方法，并定义了两个实例变量`previous_ms`和`durations`。`previous_ms`用于存储上一次操作开始的时间，`durations`用于存储所有操作的执行时间。
>
> 接着，定义了一个`current_ms`方法，用于获取当前的时间，以毫秒为单位。
>
> 然后，定义了四个方法：`on_chain_start`、`on_chain_end`、`on_llm_start`和`on_llm_end`。这些方法在语言模型链的不同阶段被调用，用于测量和记录操作的开始和结束时间。
>
> 接着，创建了一个`OpenAI`对象并将其存储在`llm`变量中。
>
> 然后，创建了一个`TimerHandler`对象并将其存储在`timerHandler`变量中。
>
> 之后，调用了`PromptTemplate`类的`from_template`方法，参数是一个字符串`"What is the HEX code of color {color_name}?"`，并将返回的对象存储在`prompt`变量中。
>
> 接着，创建了一个`LLMChain`对象，参数是`llm`、`prompt`和一个包含了`timerHandler`的列表，并将这个对象存储在`chain`变量中。
>
> 然后，调用了`chain`对象的`run`方法，参数是一个关键字参数`color_name="blue"`，并将返回的结果存储在`response`变量中。这个结果是对`"What is the HEX code of color blue?"`这个问题的回答。接着，打印了`response`的值。
>
> 之后，再次调用了`chain`对象的`run`方法，参数是一个关键字参数`color_name="purple"`，并将返回的结果存储在`response`变量中。这个结果是对`"What is the HEX code of color purple?"`这个问题的回答。接着，打印了`response`的值。
>
> 最后，打印了`timerHandler`对象的`durations`属性的值，这个值是一个包含了所有操作的执行时间的列表。

你应该期望如下输出：

```
The HEX code for blue is #0000FF.

The HEX code of the color purple is #800080.

[1589, 1097]
```

## 回调处理器的适用场景

通过 `LLMChain` 的构造函数参数设置 `callbacks` 仅仅是众多适用场景之一。接下来我们简明地列出其他使用场景和示例代码。

对于 `Model`，`Agent`，`Tool`，以及 `Chain` 都可以通过以下方式设置回调处理器：

### 1. 构造函数参数 `callbacks` 设置

关于 `Chain`，以 `LLMChain` 为例，请参考本讲上一部分内容。注意在 `Chain` 上的回调器监听的是 `chain` 相关的事件，因此回调器的如下函数会被调用：

+   `on_chain_start`
+   `on_chain_end`
+   `on_chain_error`

`Agent`，`Tool`，以及 `Chain` 上的回调器会分别被调用相应的回调函数。

下面分享关于 `Model` 与 `callbacks` 的使用示例：

```python
timerHandler = TimerHandler()

llm = OpenAI(callbacks=[timerHandler])

response = llm.predict("What is the HEX code of color BLACK?")

print(response)

timerHandler.durations
```

> 在这段代码中，首先创建了一个`TimerHandler`对象并将其存储在`timerHandler`变量中。这个对象用于测量和记录操作的执行时间。
>
> 接着，创建了一个`OpenAI`对象并将其存储在`llm`变量中。在创建这个对象时，传入了一个包含了`timerHandler`的列表作为`callbacks`参数。这意味着`timerHandler`对象将被用于处理`OpenAI`对象的回调。
>
> 然后，调用了`llm`对象的`predict`方法，参数是一个字符串`"What is the HEX code of color BLACK?"`，并将返回的结果存储在`response`变量中。这个结果是对`"What is the HEX code of color BLACK?"`这个问题的预测结果。接着，打印了`response`的值。
>
> 最后，打印了`timerHandler`对象的`durations`属性的值，这个值是一个包含了所有操作的执行时间的列表。
>
> 这段代码的目的是使用`OpenAI`对象来预测对一个问题的答案，并测量和记录这个操作的执行时间。

你应该期望看到类似如下的输出：

```
['What is the HEX code of color BLACK?']
```
