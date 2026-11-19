
# 多智能体入门

> 来源：[https://deepwisdom.feishu.cn/docx/BxBKdhEqloOrvbxC4lXcUHHbnhc](https://deepwisdom.feishu.cn/docx/BxBKdhEqloOrvbxC4lXcUHHbnhc)

在上一章中，我们简要讨论了单智能体的创建。虽然对许多情况来说，单智能体可能已经足够，但更复杂的任务通常需要协作和团队合作，这也就是多智能体为什么必不可少的原因。MetaGPT 的核心优势也在于轻松灵活地开发一个智能体团队。在 MetaGPT 框架下，用户可以通过少量代码实现智能体之间的交互。

完成本节，你将能够：

1. 理解智能体之间如何进行交互
1. 开发你的第一个智能体团队

## 运行“软件公司”示例

```
metagpt "write a function that calculates the product of a list"
```

## 开发你的第一个智能体团队

[希望你会发现](https://deepwisdom.feishu.cn/wiki/W6r7wzVQGiFLZekUax6cv0u0nTc)软件创业示例很有启发。也许现在你已经有了灵感，想要开发一个根据你的独特需求而定制的智能体团队。在本节中，我们将继续在智能体入门中的简单代码示例中添加更多角色，并引入智能体之间的交互协作。

让我们还雇佣一名测试人员和一名审阅人员携手与编码人员一起工作。这开始看起来像一个开发团队了，不是吗？总的来说，我们需要三个步骤来建立团队并使其运作：

1. 定义每个角色能够执行的预期动作
1. 基于标准作业程序（SOP）确保每个角色遵守它。通过使每个角色观察上游的相应输出结果，并为下游发布自己的输出结果，可以实现这一点。
1. 初始化所有角色，创建一个带有环境的智能体团队，并使它们之间能够进行交互。

完整的代码在本教程的末尾可用

### 定义动作和角色

与[智能体入门](https://deepwisdom.feishu.cn/wiki/W6r7wzVQGiFLZekUax6cv0u0nTc)相同的过程，我们可以定义三个具有各自动作的`Role`：

+ `SimpleCoder` 具有 `SimpleWriteCode` 动作，接收用户的指令并编写主要代码

+ `SimpleTester` 具有 `SimpleWriteTest` 动作，从 `SimpleWriteCode` 的输出中获取主代码并为其提供测试套件

+ `SimpleReviewer` 具有 `SimpleWriteReview` 动作，审查来自 `SimpleWriteTest` 输出的测试用例，并检查其覆盖范围和质量

通过上述概述，我们使得 SOP（标准作业程序）变得更加清晰明了。接下来，我们将详细讨论如何根据 SOP 来定义`Role`。

#### 定义动作

我们列举了三个 `Action`。

```
class SimpleWriteCode(Action):
    PROMPT_TEMPLATE: str = """
    Write a python function that can {instruction}.
    Return ```python your_code_here ``` with NO other texts,

    your code:

    """

    name: str = "SimpleWriteCode"

    async def run(self, instruction: str):

        prompt = self.PROMPT_TEMPLATE.format(instruction=instruction)

        rsp = await self._aask(prompt)

        code_text = parse_code(rsp)

        return code_text
```

```
class SimpleWriteTest(Action):

    PROMPT_TEMPLATE: str = """

    Context: {context}

    Write {k} unit tests using pytest for the given function, assuming you have imported it.

    Return ```python your_code_here ``` with NO other texts,
    your code:
    """

    name: str = "SimpleWriteTest"

    async def run(self, context: str, k: int = 3):
        prompt = self.PROMPT_TEMPLATE.format(context=context, k=k)

        rsp = await self._aask(prompt)

        code_text = parse_code(rsp)

        return code_text
```

```
class SimpleWriteReview(Action):
    PROMPT_TEMPLATE: str = """
    Context: {context}
    Review the test cases and provide one critical comments:
    """

    name: str = "SimpleWriteReview"

    async def run(self, context: str):
        prompt = self.PROMPT_TEMPLATE.format(context=context)

        rsp = await self._aask(prompt)

        return rsp
```

#### 定义角色

在许多多智能体场景中，定义`Role`可能只需几行代码。对于`SimpleCoder`，我们做了两件事：

1. 使用 `set_actions` 为`Role`配备适当的 `Action`，这与设置单智能体相同

1. 多智能体操作逻辑：我们使`Role` `_watch` 来自用户或其他智能体的重要上游消息。回想我们的 SOP，`SimpleCoder`接收用户指令，这是由 MetaGPT 中的`UserRequirement`引起的`Message`。因此，我们添加了 `self._watch([UserRequirement])`。

这就是用户需要做的全部。对于那些对[底层机制感兴趣](https://deepwisdom.feishu.cn/wiki/MZaYwBaJpinRlNkjvGkcC5EqnSf)的人，请参见本教程的本章中的机制解释。

```
class SimpleCoder(Role):
    name: str = "Alice"
    profile: str = "SimpleCoder"

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self._watch([UserRequirement])
        self.set_actions([SimpleWriteCode])
```

与上述相似，对于 `SimpleTester`，我们：

1. 使用 `set_actions` 为`SimpleTester`配备 `SimpleWriteTest` 动作

1. 使`Role` `_watch` 来自其他智能体的重要上游消息。回想我们的 SOP，`SimpleTester`从 `SimpleCoder` 中获取主代码，这是由 `SimpleWriteCode` 引起的 `Message`。因此，我们添加了 `self._watch([SimpleWriteCode])`。

1. 一个扩展的问题：想一想如果我们使用 `self._watch([SimpleWriteCode, SimpleWriteReview])` 会意味着什么，可以尝试这样做

此外，你可以为智能体定义自己的操作逻辑。这适用于`Action`需要多个输入的情况，你希望修改输入，使用特定记忆，或进行任何其他更改以反映特定逻辑的情况。因此，我们：

1. 重写 `_act` 函数，就像我们在[智能体入门](https://deepwisdom.feishu.cn/wiki/HoFPwNWsoiANZDk3WYlcx7XOnZg)中的单智能体设置中所做的那样。在这里，我们希望`SimpleTester`将所有记忆用作编写测试用例的上下文，并希望有 5 个测试用例。

```
class SimpleTester(Role):
    name: str = "Bob"
    profile: str = "SimpleTester"

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.set_actions([SimpleWriteTest])
        self._watch([SimpleWriteCode])
        # self._watch([SimpleWriteCode, SimpleWriteReview])  # feel free to try this too

    async def _act(self) -> Message:
        logger.info(f"{self._setting}: to do {self.rc.todo}({self.rc.todo.name})")
        todo = self.rc.todo

        # context = self.get_memories(k=1)[0].content # use the most recent memory as context
        context = self.get_memories()  # use all memories as context

        code_text = await todo.run(context, k=5)  # specify arguments
        msg = Message(content=code_text, role=self.profile, cause_by=type(todo))

        return msg
```

按照相同的过程定义 `SimpleReviewer`：

```
class SimpleReviewer(Role):
    name: str = "Charlie"
    profile: str = "SimpleReviewer"

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.set_actions([SimpleWriteReview])
        self._watch([SimpleWriteTest])
```

### 创建一个团队并添加角色

现在我们已经定义了三个 `Role`，是时候将它们放在一起了。我们初始化所有角色，设置一个 `Team`，并`hire` 它们。

运行 `Team`，我们应该会看到它们之间的协作！

```
import asyncio
import typer
from metagpt.logs import logger
from metagpt.team import Team
app = typer.Typer()

@app.command()
def main(
    idea: str = typer.Argument(..., help="write a function that calculates the product of a list"),
    investment: float = typer.Option(default=3.0, help="Dollar amount to invest in the AI company."),
    n_round: int = typer.Option(default=5, help="Number of rounds for the simulation."),
):
    logger.info(idea)

    team = Team()
    team.hire(
        [
            SimpleCoder(),
            SimpleTester(),
            SimpleReviewer(),
        ]
    )

    team.invest(investment=investment)
    team.run_project(idea)
    await team.run(n_round=n_round)

if __name__ = '__main__':
    app()
```

## 本教程的完整脚本

[https://github.com/geekan/MetaGPT/blob/main/examples/build_customized_multi_agents.py](https://github.com/geekan/MetaGPT/blob/main/examples/build_customized_multi_agents.py)

使用以下命令运行：

```
python3 examples/build_customized_multi_agents.py --idea "write a function that calculates the product of a list"
```
