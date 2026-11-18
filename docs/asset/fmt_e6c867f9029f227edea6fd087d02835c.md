
# 机制解释

> 来源：[https://deepwisdom.feishu.cn/docx/PjXLdvl2ioAognxN8kaculyhnMP](https://deepwisdom.feishu.cn/docx/PjXLdvl2ioAognxN8kaculyhnMP)

虽然用户可以编写几行代码来设置运行的`Role`，但描述内部机制是有益的，这样用户就能理解设置代码的含义，并对框架有一个整体的了解。

![](img/8fee0c2d4b4b854acec4ca30812d9a6b.png)

如图的右侧部分所示，`Role`将从`Environment`中`_observe` `Message`。如果有一个`Role` `_watch` 的特定 `Action` 引起的 `Message`，那么这是一个有效的观察，触发`Role`的后续思考和操作。在 `_think` 中，`Role`将选择其能力范围内的一个 `Action` 并将其设置为要做的事情。在 `_act` 中，`Role`执行要做的事情，即运行 `Action` 并获取输出。将输出封装在 `Message` 中，最终 `publish_message` 到 `Environment`，完成了一个完整的智能体运行。

在每个步骤中，无论是 `_observe`、`_think` 还是 `_act`，`Role`都将与其 `Memory` 交互，[通过添加或](https://deepwisdom.feishu.cn/wiki/HLjDwsk8Xi9VqzkNYn3cLzn9nkg)检索来[实现。此外](https://deepwisdom.feishu.cn/wiki/ZW4WwUvDViBMDpkh5fZcWb2fnlg)，`MetaGPT`提供了 `react` 过程的不同模式。这些部分的详细内容，请参阅 使用记忆 和 思考与行动。

当每个 `Role` 被适当设置时，我们可以看到与本教程中前面示例相应的`SOP`，如图的左侧部分所示。虚线框表明如果我们使 `SimpleTester` 同时 `_watch` `SimpleWriteCode` 和 `SimpleWriteReview`，则可以扩展 `SOP`。

我们鼓励对此[感兴](https://github.com/geekan/MetaGPT/blob/main/metagpt/roles/role.py)趣的开发人员查看 `Role` 的代码，因为它相当易读。可以 `run`、`_observe`、`react`、`_think`、`_act`、`publish_message`的内容，应该能够让你对其有一个相当不错的理解。
