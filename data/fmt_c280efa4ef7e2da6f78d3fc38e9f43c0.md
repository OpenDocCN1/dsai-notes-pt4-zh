
# “记忆”使用

> 来源：[https://deepwisdom.feishu.cn/docx/L0ewd7rYboUd8AxE84QcGccxnRl](https://deepwisdom.feishu.cn/docx/L0ewd7rYboUd8AxE84QcGccxnRl)

如[新手任务](https://deepwisdom.feishu.cn/wiki/Pn0OwFQhoiwgbdkXsT7cD17gn9f)中所讨论的，记忆是智能体的核心组件之一。智能体需要记忆来获取做出决策或执行动作所需的基本上下文，还需要记忆来学习技能或积累经验。在本教程中，我们将介绍记忆的基本使用方法。

完成本节，你将能够：

1. 理解`MetaGPT`中记忆的概念
1. 如何添加或检索记忆

## 什么是记忆

在`MetaGPT`中，`Memory`类是智能体的记忆的抽象。当初始化时，`Role`初始化一个`Memory`对象作为`self.rc.memory`属性，它将在之后的`_observe`中存储每个`Message`，以便后续的检索。简而言之，`Role`的记忆是一个含有`Message`的列表。

## 检索记忆

当需要获取记忆时（获取LLM输入的上下文），你可以使用`self.get_memories`。函数定义如下：

```
def get_memories(self, k=0) -> list[Message]:
    """A wrapper to return the most recent k memories of this role, return all when k=0"""
    return self.rc.memory.get(k=k)
```

例如，在[多智能体入门](https://deepwisdom.feishu.cn/wiki/HoFPwNWsoiANZDk3WYlcx7XOnZg)中，我们调用此函数为测试人员提供完整的历史记录。通过这种方式，如果审阅人员提供反馈，测试人员可以参考其先前版本修改测试用例。片段如下

```
async def _act(self) -> Message:
        logger.info(f"{self._setting}: ready to {self.rc.todo}")
        todo = self.rc.todo

        # context = self.get_memories(k=1)[0].content # use the most recent memory as context
        context = self.get_memories() # use all memories as context

        code_text = await todo.run(context, k=5) # specify arguments

        msg = Message(content=code_text, role=self.profile, cause_by=todo)

        return msg
```

## 添加记忆

可以使用`self.rc.memory.add(msg)`添加记忆，其中`msg`必须是`Message`的实例。请查看上述的代码片段以获取示例用法。

建议在定义`_act`逻辑时将`Message`的动作输出添加到`Role`的记忆中。通常，`Role`需要记住它先前说过或做过什么，以便采取下一步的行动。
