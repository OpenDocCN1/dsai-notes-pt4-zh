
# ActionNode 详解与使用

## 概述

`ActionNode`可以被视为一组动作树，根据类内定义，一个动作树的父节点可以访问所有的子动作节点；也就是说，定义了一个完整的动作树之后，可以从父节点按树的结构顺序执行每一个子动作节点。因此，动作的执行也可以突破0.4版本框架中，需要在`Role`的`_react`内循环执行的限制，达到更好的CoT效果。

`ActionNode`仍然需要基于`Action`类进行构建，在定义一个`ActionNode`动作树之后，需要将该动作树作为参数赋予一个`Action`子类，并将其输入到`Role`中作为其动作。在这个意义上，一个`ActionNode`动作树可以被视为一个内置CoT思考的`Action`。

同时，在`ActionNode`基类中，也配置了更多格式检查和格式规范工具，让CoT执行过程中，内容的传递更加结构化。这也服务于让`MetaGPT`框架生成更好、更长、更少`Bug`的代码这一目的。

## 如何使用 `ActionNode`

定义一个包含一系列`ActionNode`的`Action`，首先需要定义一个动作树的父节点和一系列子节点，将子节点与父节点建立联系；

```
# 定义单个子节点ActionNode
UI_DESIGN_DESC = ActionNode(
    key="UI Design Desc",
    expected_type=str,
    instruction="place the design objective here",
    example="Snake games are classic and addictive games with simple yet engaging elements. Here are the main elements"
    " commonly found in snake games",
)

# 定义完所有ActionNode之后，将其存放在一个List里面
NODES = [
    UI_DESIGN_DESC,
    SELECTED_ELEMENTS,
    HTML_LAYOUT,
    CSS_STYLES,
    ANYTHING_UNCLEAR,
]

# 将上述List的所有子节点传入父节点UI_DESIGN_NODE中
UI_DESIGN_NODE = ActionNode.from_children("UI_DESIGN", NODES)
```

之后，在`Action.run`方法中，调用父节点的执行方法，获取所有子节点按顺序执行之后的结果

```
ui_describe = await UI_DESIGN_NODE.fill(prompt)
```

## `ActionNode` 支持的基础功能

`ActionNode`中定义了一系列强大的基础功能，可以实现丰富的效果。以下的代码均截取于`ActionNode`类，方便大家更加深入地了解这一个类所支持的基础功能。

`ActionNode`支持从一个列表中初始化所有的子动作节点，仅需调用类内定义的方法，输入一个子节点组成的`List`

```
def add_child(self, node: "ActionNode"):
    """增加子ActionNode"""
    self.children[node.key] = node

def add_children(self, nodes: List["ActionNode"]):
    """批量增加子ActionNode"""
    for node in nodes:
        self.add_child(node)

@classmethod
def from_children(cls, key, nodes: List["ActionNode"]):
    """直接从一系列的子nodes初始化"""
    obj = cls(key, str, "", "")
    obj.add_children(nodes)
    return obj
```

`ActionNode.fill`是其执行动作的方法，通过这个方法，实现执行传入的`prompt`并获取结果返回，并将结果存储在自身中

```
# 获取自身的context信息（保存着所有与动作执行有关的上下文）作为prompt
# 执行prompt，并获取返回，保存到自身的content中
async def simple_fill(self, schema, mode):
    prompt = self.compile(context=self.context, schema=schema, mode=mode)

    if schema != "raw":
        mapping = self.get_mapping(mode)
        class_name = f"{self.key}_AN"
        
        # 这里获取的content是llm返回的源文本，scontent则是结构化处理的文本；
        # _aask_v1 函数会检查llm返回的内容的结构，如果不符合用户期望的格式，则会调用llm重新生成内容
        content, scontent = await self._aask_v1(prompt, class_name, mapping, schema=schema)
        self.content = content
        self.instruct_content = scontent
    else:
        self.content = await self.llm.aask(prompt)
        self.instruct_content = None

    return self

# 根据执行模式的输入，选择只执行当前节点的simple_fill或者执行所有子节点的simple_fill
async def fill(self, context, llm, schema="json", mode="auto", strgy="simple"):
    self.set_llm(llm)
    self.set_context(context)
    if self.schema:
        schema = self.schema

    if strgy == "simple":
        return await self.simple_fill(schema=schema, mode=mode)
    elif strgy == "complex":
        # 这里隐式假设了拥有children
        tmp = {}
        for _, i in self.children.items():
            child = await i.simple_fill(schema=schema, mode=mode)
            tmp.update(child.instruct_content.dict())
        cls = self.create_children_class()
        self.instruct_content = cls(**tmp)
        return self
```

`ActionNode`执行动作的基础是以下函数，向`llm`发送消息获取返回内容，同时对特定的格式需要做特定`prompt`输入和格式约束。

```
@retry(
    wait=wait_random_exponential(min=1, max=20),
    stop=stop_after_attempt(6),
    after=general_after_log(logger),
)
async def _aask_v1(
    self,
    prompt: str,
    output_class_name: str,
    output_data_mapping: dict,
    system_msgs: Optional[list[str]] = None,
    schema="markdown",  # compatible to original format
) -> (str, BaseModel):
    """Use ActionOutput to wrap the output of aask"""
    content = await self.llm.aask(prompt, system_msgs)
    logger.debug(f"llm raw output:\n{content}")
    output_class = self.create_model_class(output_class_name, output_data_mapping)

    if schema == "json":
        parsed_data = llm_output_postprecess(output=content, schema=output_class.schema(), req_key=f"[/{TAG}]")
    else:  # using markdown parser
        parsed_data = OutputParser.parse_data_with_mapping(content, output_data_mapping)

    logger.debug(f"parsed_data:\n{parsed_data}")
    instruct_content = output_class(**parsed_data)
    return content, instruct_content
```

## 快速掌握 `ActionNode` 的用法：打印斐波那契数列

上一章节我们已经学会了`ActionNode`支持的基础功能，但是，想要让`ActionNode`实例实现更多的功能，还需要使用一个`Action`类来执行`ActionNode`。在这个章节中，我将带领大家，通过一个简单的小训练：打印前10个斐波那契数列的数字，来掌握`ActionNode`的基本用法。

### 需求分析

斐波那契数列是一个数列，每个数都是前两个数的和，通常以0和1开始。更多关于斐波那契数列的知识可以查看：[baike.baidu.com](https://baike.baidu.com/item/%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97/99145)

在数学上，斐波那契数列被定义为：

```
F(0) = 0, F(1) = 1
F(n) = F(n-1) + F(n-2) for n > 1
```

很明显，`LLM`了解斐波那契数列是什么东西，通过简单的对话也能快速满足我们的需求。但在一些特殊的情况下，`LLM`通过简单对话返回的字符串数据无法满足我们的要求。在此之上，我提出了更高的要求：`LLM`要能以特定的可解析的格式来返回斐波那契数列，并通过格式解析实现逐个打印数字的效果。

在这里，我们需要利用`ActionNode`来固定返回的格式，同时，我们也复习一下，实现技术文档助手中，我们学会的一些技巧，来实现逐个打印的效果。

### 返回特定格式的斐波那契数列

根据需求分析，我们需要返回特定格式的斐波那契数列前10个数字，这里我们有两个动作需要执行：

+ 思考斐波那契数列前10个数字是什么；
+ 将思考得到的前10个数字转化为特定格式；

我们可以使用两个`ActionNode`，形成一个简单的CoT来执行上述的动作。

首先，我们定义好两个`ActionNode`，可以实现上述的功能。

```
# 将思考斐波那契数列的10个数字作为prompt输入，在这里我们将“思考需要生成的数字列表”作为命令（instruction）写入
# 将期望返回格式（expected_type）设置为str，无需设置例子（example）
SIMPLE_THINK_NODE = ActionNode(
    key="Simple Think Node",
    expected_type=str,
    instruction="""
            Think about what list of numbers you need to generate
            想一想您需要生成哪些数字列表
            """,
    example=""
)

# 在这里通过命令（instruction）来规定需要生成的数字列表格式，提供例子（example）来帮助LLM理解
SIMPLE_CHECK_NODE = ActionNode(
    key="Simple CHECK Node",
    expected_type=str,
    instruction="""
            Please provide the number list for me, strictly following the following requirements:
            1. Answer strictly in the list format like [1,2,3,4]
            2. Do not have extra spaces or line breaks.
            Return the list here:
            请严格按照以下要求为我提供号码清单：
            1. 严格按照 [1,2,3,4] 这样的列表格式作答
            2. 不要有额外的空格或换行。
            在此返回列表：
            """,
    example="[1,2,3,4]"
            "[4,5,6]",
 )
```

设置好`ActionNode`之后，我们需要为这两个动作节点设置一个父节点。

为什么需要设置父节点？还记得基础功能里面的`ActionNode.fill`方法吗，当我们输入`complex`参数时，这个方法会按顺序执行每一个子节点，并将上一个子节点返回的内容作为下一个子节点的`prompt`。为了将两个动作节点串联起来，形成一个简单的CoT效果，我们需要设置一个父节点。

我们需要编写一个`ActionNode`的子类，通过修改其中部分方法，来达到更加稳定的效果。

```
class THINK_NODES(ActionNode):
    def __init__(self, name="Think Nodes", expected_type=str, instruction="", example=""):
        super().__init__(key=name, expected_type=str, instruction=instruction, example=example)
        self.add_children([SIMPLE_THINK_NODE, SIMPLE_CHECK_NODE])    # 初始化过程，将上面实现的两个子节点加入作为THINK_NODES类的子节点

    async def fill(self, context, llm, schema="raw", mode="auto", strgy="complex"):
        self.set_llm(llm)
        self.set_context(context)
        if self.schema:
            schema = self.schema

        if strgy == "simple":
            return await self.simple_fill(schema=schema, mode=mode)
        elif strgy == "complex":
            # 这里隐式假设了拥有children
            child_context = context    # 输入context作为第一个子节点的context
            for _, i in self.children.items():
                i.set_context(child_context)    # 为子节点设置context
                child = await i.simple_fill(schema=schema, mode=mode)
                child_context = child.content    # 将返回内容（child.content）作为下一个子节点的context

            self.content = child_context    # 最后一个子节点返回的内容设置为父节点返回内容（self.content）
            return self
```
