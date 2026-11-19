
# LangChain大模型应用开发指南-从最基础的链式结构到ReAct对话结构

> 来源：[https://agijuejin.feishu.cn/docx/BB9tdCsk8oRSFRxYLHcc1wTKn7b](https://agijuejin.feishu.cn/docx/BB9tdCsk8oRSFRxYLHcc1wTKn7b)
> 作者：AI小智
> 原文：https://mp.weixin.qq.com/s/GdYBnj2pILK5S8rIhpLWSw

![](img/5501f58d2dd1389132444f5e3fef52c0.png)

上[节课，我介绍](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=Mzg2ODA5NTM1OA==&action=getalbum&album_id=3115055723522015235#wechat_redirect)了如何使用`LangChain`的六大核心概念和组件，构建一个基于语言模型的问答系统。没有看过的小伙伴可以点击链接查看：AI课程合集

> 今天我将拆解上节课讲授的问答系统案例，将其中涉及到`Modal I/O`组件的部分拆解出来进行深度精讲，基于最基本且最典型的`IO`链式结构作为引入及过渡后讲解问答系统中使用到的`ReAct`对话模型，并最终教会你是什么样的化学作用，使得最简单模板套用衍生出如此复杂的应用模式。

![](img/8c1f7e2c91ea8d65c1fe73a2de417fff.png)

## 案例回顾与分析

首先我们将上节课的问答系统进行深度简化，仅保留与`Modal I/O`组件相关的部分，代码如下

```
from langchain.llms import OpenAI

from langchain import hub
from langchain.output_parsers import StructuredOutputParser, ResponseSchema
from langchain.prompts import PromptTemplate

# 大模型定义
api_key = ""
api_url = ""
modal = "Qwen"
llm = OpenAI(model_name=modal, openai_api_key=api_key, openai_api_base=api_url, temperature=0.0)

# 定义输出解析器
response_schemas = [
    ResponseSchema(name="answer", description="answer to the user's question"),
    ResponseSchema(name="source", description="source used to answer the user's question, should be a website.")
]
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)
format_instructions = output_parser.get_format_instructions()

# 定义对话模板
prompt = PromptTemplate(
    template="answer the users question as best as possible.\n{format_instructions}\n{input}",
    input_variables=["input"],
    partial_variables={"format_instructions": format_instructions}
)

# 定义qa链
qachain = prompt | llm | output_parser

question_1 = qachain.invoke({"input": "What is Lu Xun's real name?"})
print("执行结果：" + str(question_1))

print("输出类型为：" + str(type(question_1)))

print("参考来源为：" + question_1['source'])
```

代码案例运行结果如下：

![](img/7e488fa8947f5c16c103179289e87696.png)

### 基础Modal I/O链式结构分析

如以上案例所示，上节课复杂的`Retriever`工具等配置被去除，剩余的链式结构只剩余`prompt`，`llm`，`outputParser`三个部分。这些组件可以分为三类：输入组件，模型组件，和输出组件。

```
PromptTemplate / ChatPromptTemplate -> LLM / ChatModel -> OutputParser
```

这是最基本的链式构成，它适用于大多数简单的语言模型应用，如文本生成，文本摘要，文本翻译等。它包含三个组件：`PromptTemplate`或`ChatPromptTemplate`，`LLM`或`ChatModel`，和`OutputParser`。

+ 首先通过`OpenAI`对象，示例构建了大语言模型的访问入口

+ 其次代码中使用`ResponseSchema`规范了输出的格式，并通过`StructuredOutputParser`创建了案例中`OutputParser`

+ 最后结合输出格式要求，创建了提问的`prompt`

### ReAct对话链式构成

运行过上节课案例代码的同学应该还有印象，没有映像的同学可以参考如下的截图。代理链在运行过程中不是仅仅进行了一次对话交互就结束了，而是通过特定的对话结构实现了多轮思考，并最终推理得出了答案。对于大模型应用有过一定了解的应该知道这种对话模型- `ReAct`对话。

![](img/e1bad1ad4445f869e903ec659a84c4c6.png)

`ReAct`文如其名，模型推理分成了两个部分，`Reason`和`Action`。`Reason`生成分析步骤，`Action`生成工具调用请求，二者交替进行直到得到最终的结果。

上节课的QA系统使用的`ReAct`对话模型是基于以下链式作用的

```
hwchase17/react-chat(prompt) -> LLM -> ReActSingleInputOutputParser
```

![](img/9d2a593ff1fbecc91e06aaae467c8c85.png)

+ `hwchase17/react-chat`是一个适配`ReAct`对话的预制模板，相关参数可参考如下截图

![](img/fe933d7efd3a39d43cd0758f78881bf3.png)

+ `LLM`或`ChatModel`是模型组件，它们负责调用语言模型并生成输出。`LLM`或`ChatModel`与上一节中的相同，不再赘述。

+ `ReActSingleInputOutputParser`是输出组件，它负责识别模型返回的`Thought`，`Action`，`Observation`等关键字将模型的回答按类别结构化，并将解析出来的模型对后续任务的指定输出到链中，以进行下一轮的思考

## 复杂对话结构的深度剖析

上文我们讲解了`LangChain`中的最基本的链式结构，以及基于这个链式结构演化出来的`ReAct`对话链模型。我们不难看出，虽然使用的基础对象和概念都是一样的，但是通过内置参数的调整以及不同组件以链的形式进行有机组合，我们能在大模型底座上衍生出众多花样的的使用方式。如下是我在使用过程中总结的三点心得：

### 通过prompts预设输出要求，减少黑盒模型回答的不确定性

我们知道，语言模型是一个基于概率的生成器，它会根据输入的文本和自身的知识库，生成一个最有可能的输出。但是这个输出并不一定是我们想要的，有时候甚至会出现一些无关或错误的信息。为了避免这种情况，我们可以通过`prompts`来给模型一些提示或约束，让它按照我们的要求来生成输出。例如，在上节课的QA系统中，我们使用了`hwchase17/react-chat`这个预制模板，它就是一个专门为`ReAct`对话设计的`prompt`，它包含了一些关键字和格式规范，让模型知道如何生成`Reason`和`Action`，并且如何区分`Thought`和`Observation`。这样一来，我们就可以减少模型生成一些无用或错误的输出的可能性，提高应用的效率和准确性。

### 通过积攒prompts来沉淀语言模型使用经验，形成知识库

`prompts`不仅可以作为模型生成输出的指导，也可以作为我们使用语言模型的经验和技巧的记录。我们可以把自己编写或收集到的`prompts`保存起来，形成一个知识库，方便我们在不同的场景和应用中快速地调用和复用。例如，`LangChain`提供了一个`prompt`库，里面收录了许多优秀的`prompt`模板，涵盖了不同的语言模型应用领域，如文本生成，文本摘要，文本翻译，文本分类，文本情感分析等。我们可以根据自己的需求，从`prompt`库中选择合适的模板，或者修改和定制自己的模板，并将其保存到自己的账户中。这样一来，我们就可以积累自己的语言模型使用经验，并且方便地分享和交流。

### prompts与OutputParser配对以实现单次调用多轮模型思考

`prompts`不仅可以影响模型生成输出的内容和形式，也可以影响模型生成输出的次数和顺序。通过与`OutputParser`配合，我们可以实现单次调用多轮模型思考的效果。例如，在上节课的QA系统中，我们使用了`ReActSingleInputOutputParser`这个输出组件，它负责识别模型返回的`Thought`，`Action`，`Observation`等关键字，并将解析出来的模型对后续任务的指定输出到链中，以进行下一轮的思考。这样一来，我们就可以让模型在一次调用中完成多个步骤，如检索信息，分析问题，生成答案等。这样不仅可以提高应用的效率和灵活性，也可以让模型展现出更高级的推理能力。

## 总结

这篇文章，我为您介绍了`LangChain`中最基本的链式结构，以及基于这个链式结构演化出来的`ReAct`对话链模型。基于二者的对比，我给大家展示了如何使用`LangChain`的基础组件来实现复杂的对话模式，并最终总结了自己使用`Modal I/O`链式结构的三点经验。希望您能从中受益，并且在`LangChain`上开发出更多有趣和有用的语言模型应用。谢谢您的阅读！
