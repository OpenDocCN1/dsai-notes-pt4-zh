
# 谷歌发布《Agents》白皮书

> 来源：[https://gotoaiworld.feishu.cn/docx/UB4jdsHVZoP5RLxmCeJcHr91n0e](https://gotoaiworld.feishu.cn/docx/UB4jdsHVZoP5RLxmCeJcHr91n0e)

> 作者：Julia Wiesinger, Patrick Marlow and Vladimir Vuskovic

1. 引言
1. 什么是Agent?
1. Agent的组成部分
1. 模型:AI界的"大脑"
1. 工具:Agent的"瑞士军刀"
1. 编排层:Agent的"指挥中心"
1. Agent vs 模型:不只是"长得像"的表亲
1. 认知架构:Agent如何运作
1. 工具:我们通往外部世界的钥匙
1. 扩展
1. 函数
1. 数据存储
1. 用定向学习提升模型表现
1. 用[`LangChain`](https://zhida.zhihu.com/search?content_id=252360031&content_type=Article&match_order=1&q=LangChain&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjIxMzI4NDIsInEiOiJMYW5nQ2hhaW4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNTIzNjAwMzEsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.1lgtWg2652vTKQx4dON1Q35l-0TQ4GoLIhaCsS5VLZg&zhida_source=entity)快速入门Agent开发
1. 用Vertex AI agents构建生产级应用
1. 总结
1. 参考文献

---

> 这种推理、逻辑和访问外部信息相结合，并与生成式 AI 模型相连接，引出了Agents的概念

## 简介

人类非常擅长处理混乱的模式识别任务。然而，他们通常依赖工具 —— 如书籍、Google 搜索或计算器 —— 来补充他们先前的知识，然后才能得出结论。

就像人类一样，生成式 AI 模型可以被训练来使用工具访问实时信息或建议现实世界的行动。例如，模型可以利用数据库检索工具来访问特定信息，例如客户的购买记录，以便生成量身定制的购物推荐。

或者，根据用户的查询，模型可以进行各种 API 调用，向同事发送电子邮件回复或代表您完成金融交易。为此，模型不仅必须访问一组外部工具，还需要具备以自我指导的方式计划和执行任何任务的能力。

这种推理、逻辑和访问外部信息的结合都与生成式 AI 模型相连接，引出了智能体或程序的概念，该程序扩展了生成式 AI 模型的独立功能。本白皮书更详细地探讨了所有这些和相关方面。

## 什么是智能体？

在其最基本的形式中，生成式 AI 智能体（Agents）可以被定义为一个应用程序，它通过观察世界并使用其可用的工具对其采取行动来尝试实现目标。智能体是自主的，可以独立于人类干预而行动，特别是当为其提供适当的目标或其要实现的意图时。智能体在实现其目标的方法上也可以是主动的。即使在没有人类明确指令集的情况下，智能体也可以推理下一步应该做什么以实现其最终目标。虽然人工智能中智能体的概念相当普遍和强大，但本白皮书侧重于在发布时生成式 AI 模型能够构建的特定类型的智能体。

为了理解智能体的内部运作，让我们首先介绍驱动智能体行为、行动和决策的基础组件。这些组件的组合可以被描述为认知架构，并且存在许多这样的架构，可以通过这些组件的混合和匹配来实现。关注核心功能，智能体的认知架构中有三个基本组件，如图 1 所示。

![](img/db00273d8f5a576b9526fefcbb32129d.png)

图 1. 通用智能体架构和组件

### 模型

在智[能体的](https://zhida.zhihu.com/search?content_id=252360031&content_type=Article&match_order=1&q=%E6%80%9D%E7%BB%B4%E9%93%BE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjIxMzI4NDIsInEiOiLmgJ3nu7Tpk74iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNTIzNjAwMzEsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.yDpj7jfHuwHAgQCqz0jGlLmF1feLekTwh3zLWrJc1Mo&zhida_source=entity)范围内，模型[是指将](https://zhida.zhihu.com/search?content_id=252360031&content_type=Article&match_order=1&q=%E6%80%9D%E7%BB%B4%E6%A0%91&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjIxMzI4NDIsInEiOiLmgJ3nu7TmoJEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNTIzNjAwMzEsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.vDBqU-lXnqxz03zRUOb6FGi3wNkqImX8V7uFTzM98Ao&zhida_source=entity)用作智能体流程的集中决策者的语言模型 (LM)。智能体使用的模型可以是一个或多个任何大小 (小型/大型) 的 LM，它们能够遵循基于指令的推理和逻辑框架，如 `ReAct`、思维链（`CoT`）或思维树（`ToT`）。模型可以是通用的、多模态的或根据您的特定智能体架构的需求进行微调的。为了获得最佳生产结果，您应该利用最适合您的预期最终应用程序的模型，并且理想情况下，该模型已在与您计划在认知架构中使用的工具相关联的数据签名上进行了训练。重要的是要注意，该模型通常没有使用智能体的特定配置设置 (即工具选择、编排/推理设置) 进行训练。但是，可以通过向其提供展示智能体功能的示例来进一步优化模型以执行智能体的任务，包括智能体在各种上下文中使用特定工具或推理步骤的实例。

### 工具

基础模型尽管具有令人印象深刻的文本和图像生成能力，但仍然受限于无法与外部世界交互。工具弥合了这一差距，使智能体能够与外部数据和服务进行交互，同时解锁超越底层模型本身的更广泛的操作范围。工具可以采取多种形式，并且具有不同的复杂性深度，但通常与常见的 Web API 方法（如 `GET`、`POST`、`PATCH` 和 `DELETE`）保持一致。例如，工具可以更新数据库中的客户信息或获取天气数据以影响智能体提供给用户的旅行推荐。借助工具，智能体可以访问和处理现实世界的信息。这使他们能够支持更专业的系统，如检索增强生成 (RAG)，这显著扩展了智能体的能力，使其超越了基础模型自身可以实现的范围。我们将在下面更详细地讨论工具，但最重要的是要理解工具弥合了智能体内部功能和外部世界之间的差距，从而释放了更广泛的可能性。

### 编排层

编排层描述了一个循环过程，该过程控制智能体如何获取信息，执行一些内部推理，并使用该推理来通知其下一步行动或决策。通常，此循环将持续到智能体达到其目标或停止点。编排层的复杂性可能会因智能体及其执行的任务而有很大差异。有些循环可以是带有决策规则的简单计算，而另一些循环可能包含链式逻辑，涉及额外的机器学习算法，或实现其他概率推理技术。我们将在认知架构部分详细讨论智能体编排层的实现。

### 智能体与模型

为了更清楚地了解智能体和模型之间的区别，请考虑以下图表：

### 认知架构：智能体如何运作

想象一下繁忙厨房里的厨师。他们的目标是为餐厅顾客制作美味的菜肴，这涉及到计划、执行和调整的循环。

- 他们收集信息，例如顾客的订单以及食品储藏室和冰箱中的配料。
- 他们根据刚刚收集的信息，对可以制作的菜肴和风味进行一些内部推理。
- 他们采取行动制作菜肴：切菜、混合香料、煎肉。

在流程的每个阶段，厨师都会根据需要进行调整，在食材用完或收到顾客反馈时完善他们的计划，并使用之前的一组结果来确定下一步的行动计划。这种信息摄入、计划、执行和调整的循环描述了厨师为实现其目标而采用的独特认知架构。

就像厨师一样，智能体可以通过迭代处理信息、做出明智的决策并根据以前的输出来完善后续行动，从而使用认知架构来实现其最终目标。智能体认知架构的核心在于编排层，负责维护记忆、状态、推理和计划。它使用快速发展的提示工程领域和相关框架来指导推理和计划，使智能体能够更有效地与其环境交互并完成任务。语言模型的提示工程框架和任务计划领域的研究正在迅速发展，产生了各种有前途的方法。虽然不是一个详尽的清单，但以下是截至本白皮书发布时一些最流行的框架和推理技术：

- `ReAct`，一种提示工程框架，为语言模型提供了一种思维过程策略，使其能够对用户查询进行推理并采取行动，无论有没有上下文示例。`ReAct` 提示已被证明优于多个 SOTA 基线，并提高了人类对大语言模型的可操作性和可信度。
- 思维链（`CoT`），一种提示工程框架，通过中间步骤实现推理能力。`CoT` 有各种子技术，包括自洽性、主动提示和多模态 `CoT`，它们各自根据具体应用具有优势和劣势。
- 思维树（`ToT`），一种提示工程框架，非常适合探索或战略前瞻任务。它概括了思维链提示，并允许模型探索各种思维链，这些思维链充当使用语言模型进行一般问题解决的中间步骤。

智能体可以利用上述推理技术之一或许多其他技术来为给定的用户请求选择下一个最佳操作。例如，让我们考虑一个被编程为使用 `ReAct` 框架为用户查询选择正确操作和工具的智能体。事件的顺序可能是这样的：

1. 用户向智能体发送查询
1. 智能体开始 `ReAct` 序列
1. 智能体向模型提供提示，要求它生成下一个 `ReAct` 步骤之一及其相应的输出：

   a. 问题：来自用户查询的输入问题，随提示一起提供
   b. 思考：模型关于下一步应该做什么的思考
   c. 操作：模型关于下一步采取什么行动的决定（1）这是可以选择工具的地方；（2）例如，一个操作可以是 [`航班`、`搜索`、`代码`、`无`]，其中前 3 个代表模型可以选择的已知工具，最后一个代表“无工具选择”
   d. 操作输入：模型关于向工具提供哪些输入的决定（如果有）
   e. 观察：操作/操作输入/观察序列的结果：（1）这种 `思考`/`操作`/`操作输入`/`观察` 可以根据需要重复 `N` 次
   f. 最终答案：模型提供的原始用户查询的最终答案

1. [`ReAct` 循环结束，](https://link.zhihu.com/?target=https%3A//www.aisharenet.com/react-2/)并向用户提供最终答案 PS：`ReAct`实现逻辑实操

图 2. 在编排层中使用 `ReAct` 推理的示例智能体

如图 2 所示，模型、工具和智能体配置协同工作，以根据用户的原始查询向用户提供一个有根据的、简洁的响应。虽然模型可以根据其先验知识猜测答案（产生幻觉），但它却使用了工具（`航班`）来搜索实时外部信息。这些额外的信息被提供给模型，使其能够根据真实的事实数据做出更明智的决策，并将这些信息汇总反馈给用户。

总而言之，智能体响应的质量可以直接关系到模型对这些各种任务进行推理和操作的能力，包括选择正确工具的能力，以及工具定义的完善程度。就像厨师用新鲜食材精心制作菜肴并关注顾客的反馈一样，智能体依靠合理的推理和可靠的信息来提供最佳结果。在下一节中，我们将深入探讨智能体连接新鲜数据的各种方式。

## 工具：我们通往外部世界的钥匙

虽然语言模型擅长处理信息，但它们缺乏直接感知和影响现实世界的能力。这限制了它们在需要与外部系统或数据交互的情况下的实用性。这意味着，在某种意义上，语言模型的好坏取决于它从训练数据中学到的东西。但是，无论我们向模型输入多少数据，它们仍然缺乏与外部世界交互的基本能力。那么，我们如何才能使我们的模型具有与外部系统进行实时、上下文感知的交互的能力呢？函数、扩展、数据存储和插件都是为模型提供这种关键能力的方法。

虽然它们有许多名称，但工具是在我们的基础模型和外部世界之间建立联系的纽带。这种与外部系统和数据的连接使我们的智能体能够执行各种各样的任务，并且更加准确和可靠。例如，工具可以使智能体能够调整智能家居设置、更新日历、从数据库中获取用户信息或根据一组特定的指令发送电子邮件。

截至本白皮书发布之日，Google 模型能够与之交互的主要工具类型有三种：扩展、函数和数据存储。通过为智能体配备工具，我们释放了它们不仅可以理解世界而且可以对其采取行动的巨大潜力，为无数新的应用和可能性打开了大门。

### 扩展

理解扩展的最简单方法是将它们视为以标准化方式弥合 API 和智能体之间差距的桥梁，允许智能体无缝执行 API 而不管其底层实现如何。假设您构建了一个智能体，其目标是帮助用户预订航班。您知道您想使用 Google Flights API 来检索航班信息，但您不确定如何让您的智能体调用此 API 端点。

![](img/7c6ef402c633b65dd490cec188ce1166.png)

图 3. 智能体如何与外部 API 交互

一种方法可能是实现自定义代码，该代码将接收传入的用户查询，解析查询以获取相关信息，然后进行 API 调用。例如，在航班预订用例中，用户可能会说“我想预订从奥斯汀到苏黎世的航班。”在这种情况下，我们的自定义代码解决方案将需要从用户查询中提取“奥斯汀”和“苏黎世”作为相关实体，然后才能尝试进行 API 调用。但是，如果用户说“我想预订飞往苏黎世的航班”而没有提供出发城市，会发生什么情况呢？在没有所需数据的情况下，API 调用将失败，并且需要实现更多代码才能捕获此类边缘和极端情况。这种方法不可扩展，并且在任何超出已实现自定义代码范围的情况下都可能轻易失败。

更可靠的方法是使用扩展。扩展通过以下方式弥合了智能体和 API 之间的差距：

1. 使用示例教导智能体如何使用 API 端点。
1. 教导智能体成功调用 API 端点需要哪些参数。

![](img/6e13e04f2a3170a4088515076b7031cf.png)

图 4. 扩展将智能体连接到外部 API

扩展可以独立于智能体进行构建，但应作为智能体配置的一部分提供。智能体在运行时使用模型和示例来决定哪个扩展（如果有）适合解决用户的查询。这突出了扩展的一个关键优势，即它们的内置示例类型，允许智能体动态选择最适合该任务的扩展。

![](img/6e13e04f2a3170a4088515076b7031cf.png)

图 5. 智能体、扩展和 API 之间的 1 对多关系

可以这样理解：软件开发人员在解决用户问题时决定使用哪个 API 端点。如果用户想预订航班，开发人员可能会使用 Google Flights API。如果用户想知道相对于[他们所在位置](https://link.zhihu.com/?target=https%3A//www.aisharenet.com/geminibardgubu/)最近的咖啡店在哪里，开发人员可能会使用 Google Maps API。以同样的方式，智能体/模型堆栈使用一组已知的扩展来决定哪一个最适合用户的查询。如果您想查看扩展的实际应用，您可以在 Gemini 应用程序上试用它们，方法是转到“设置”>“扩展”，然后启用任何您想测试的扩展。例如，您可以启用 Google Flights 扩展，然后询问 Gemini“显示下周五从奥斯汀飞往苏黎世的航班”。

### 示例

为了简化扩展的使用，Google 提供了一些开箱即用的扩展，可以快速导入到您的项目中，并且只需最少的配置即可使用。例如，下面的代码片段 1中的代码解释器扩展允许您从自然语言描述中生成和运行 Python 代码



```python
import vertexai
import pprint

PROJECT_ID = "YOUR_PROJECT_ID"
REGION = "us-central1"
vertexai.init(project=PROJECT_ID, location=REGION)
from vertexai.preview.extensions import Extension
extension_code_interpreter = Extension.from_hub("code_interpreter")
CODE_QUERY = """Write a python method to invert a binary tree in O(n) time."""
response = extension_code_interpreter.execute(operation_id="generate_and_execute", operation_params={"query": CODE_QUERY})
print("Generated Code:")
pprint.pprint({response['generated_code']})
# The above snippet will generate the following code.
# Generated Code:
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

def invert_binary_tree(root):
    """
    Inverts a binary tree.
    Args:
    root: The root of the binary tree.
    Returns:
    The root of the inverted binary tree.
    """
    if not root:
        return None
    # Swap the left and right children recursively
    root.left, root.right = \
        invert_binary_tree(root.right), invert_binary_tree(root.left)
    return root

# Example usage:
# Construct a sample binary tree
#     4
#    / \
#   2   7
#  / \ / \
# 1  3 6  9
root = TreeNode(4)
root.left = TreeNode(2)
root.right = TreeNode(7)
root.left.left = TreeNode(1)
root.left.right = TreeNode(3)
root.right.left = TreeNode(6)
root.right.right = TreeNode(9)

# Invert the binary tree
inverted_root = invert_binary_tree(root)
```

（代码解释器扩展可以生成和运行 Python 代码）

总而言之，扩展为智能体提供了一种感知、交互和影响外部世界的多种方式。这些扩展的选择和调用由示例的使用指导，所有这些都定义为扩展配置的一部分。

## 函数

在软件工程领域，函数被定义为自包含的代码模块，用于完成特定任务，并且可以根据需要重复使用。当软件开发人员编写程序时，他们通常会创建许多函数来执行各种任务。他们还将定义调用 `function_a` 与 `function_b` 的逻辑，以及预期的输入和输出。

函数在智能体领域的工作方式非常相似，但我们可以用模型代替软件开发人员。模型可以采用一组已知函数，并根据其规范决定何时使用每个函数以及函数需要哪些参数。函数与扩展在几个方面有所不同，最显著的是：

1. 模型输出函数及其参数，但不进行实时的 API 调用。
2. 函数在客户端执行，而扩展在智能体端执行。

再次使用我们的 Google Flights 示例，函数的简单设置可能如图 7 所示。

![](img/03fcad08024f95ddd4f4bcbdf47b5d81.png)

图 7. 函数如何与外部 API 交互？

请注意，这里的主要区别在于函数和智能体都不会直接与 Google Flights API 交互。那么 API 调用实际上是如何发生的呢？

使用函数时，调用实际 API 端点的逻辑和执行被从智能体转移到客户端应用程序，如下面的图 8 和图 9 所示。这为开发人员提供了对应用程序中数据流的更精细的控制。开发人员可能选择使用函数而不是扩展的原因有很多，但一些常见的用例是：

+ 需要在应用程序堆栈的另一个层级调用 API，而不是直接在代理架构流程中调用（例如，使用中间件系统、前端框架等）。
+ 安全性或身份验证的限制，导致代理无法直接调用 API（例如，API 未暴露于互联网，或者代理基础设施无法访问 API）。
+ 时间或操作顺序的限制，导致代理无法实时调用 API（例如，批量操作、人工审核流程等）。
+ 需要对 API 响应进行额外的数据转换逻辑，而代理无法执行。例如，某些 API 端点不提供限制返回结果数量的过滤机制。在客户端使用函数为开发人员提供了额外的机会来完成这些转换。
+ 开发人员希望在不为 API 端点部署额外基础设施的情况下迭代代理开发（例如，函数调用可以作为 API 的“桩模拟”）。

虽然从内部架构上看，两种方法的差异如图 8 所示较为细微，但函数调用提供了额外的控制能力，并减少了对外部基础设施的依赖，使其成为开发人员的一个有吸引力的选项。

![](img/ec70aea42e477ea64d8bc7c7bed5c336.png)

图 8. 描述扩展和函数调用的客户端与智能体端控制

## 用例

模型可以用来调用函数，以便为最终用户处理复杂的客户端执行流程，其中智能体开发人员可能不希望语言模型管理 API 执行（就像使用扩展一样）。让我们考虑以下示例，其中一个智能体被训练成一个旅行礼宾员，与想要预订度假旅行的用户进行交互。目标是让智能体生成一个城市列表，我们可以在我们的中间件应用程序中使用这些城市来下载图像、数据等，以供用户进行旅行计划。用户可能会说：

我想和家人一起去滑雪旅行，但不确定去哪里。

在模型的典型提示中，输出可能如下所示：

当然，以下是可以考虑进行家庭滑雪旅行的城市列表：

+ 美国科罗拉多州克雷斯特德比特
+ 加拿大不列颠哥伦比亚省惠斯勒
+ 瑞士采尔马特

虽然上面的输出包含我们需要的数据（城市名称），但该格式并不适合解析。通过函数调用，我们可以教会模型以结构化样式（如 JSON）格式化此输出，这更方便另一个系统解析。给定来自用户的相同输入提示，来自函数的示例 JSON 输出可能如下所示。

```
function_call {
name: "display_cities"
args: {
"cities": ["Crested Butte", "Whistler", "Zermatt"],
"preferences": "skiing"
}
}
```

用于显示城市列表和用户偏好的示例函数调用负载此 JSON 负载由模型生成，然后发送到我们的客户端服务器，以执行我们希望对其执行的任何操作。在这种特定情况下，我们将调用 Google Places API 获取模型提供的城市并查找图像，然后将它们作为格式化的丰富内容提供给我们的用户。请参考图 9 中的序列图，其中详细显示了上述交互的步骤。

![](img/097b9ab8103432b1aa068d8bdd9a7384.png)

图 9. 显示函数调用生命周期的序列图

图 9 中示例的结果是，该模型被用来“填空”，其中包含客户端 UI 调用 Google Places API 所需的参数。客户端 UI 使用模型在返回的函数中提供的参数来管理实际的 API 调用。这只是函数调用的一个用例，但还有许多其他场景需要考虑，例如：

+ 您希望语言模型建议一个可以在代码中使用的函数，但您不想在代码中包含凭据。由于函数调用不会运行该函数，因此您无需在代码中包含带有函数信息的凭据。
+ 您正在运行可能需要几秒钟以上才能完成的异步操作。这些场景适用于函数调用，因为它是异步操作。
+ 您希望在与生成函数调用及其参数的系统不同的设备上运行函数。

关于函数，需要记住的一件关键事情是，它们旨在为开发人员提供对 API 调用的执行以及应用程序中整个数据流的更多控制。在图 9 的示例中，开发人员选择不将 API 信息返回给智能体，因为它与智能体将来可能采取的操作无关。但是，根据应用程序的架构，将外部 API 调用数据返回给智能体可能是有意义的，以影响未来的推理、逻辑和操作选择。最终，由应用程序开发人员来选择适合特定应用程序的内容。

## 函数示例代码

为了从我们的滑雪度假场景中获得上述输出，让我们构建每个组件，以使其与我们的 `gemini-1.5-flash-001` 模型一起使用。首先，我们将 `display_cities` 函数定义为一个简单的 Python 方法。

```
def display_cities(cities: list[str], preferences: Optional[str] = None):
    """根据用户的搜索查询和偏好提供城市列表。
    Args:
    preferences (str): 用户的搜索偏好, 例如滑雪、
    海滩、餐馆、烧烤等。
    cities (list[str]): 推荐给用户的城市列表。
    Returns:
    list[str]: 推荐给用户的城市列表。
    """
    return cities
```

（代码片段. 用于显示城市列表的函数的示例 Python 方法）

接下来，我们将实例化我们的模型，构建工具，然后将用户的查询和工具传递给模型。执行下面的代码将产生如代码片段底部所示的输出。

```
from vertexai.generative_models import GenerativeModel, Tool, FunctionDeclaration
model = GenerativeModel("gemini-1.5-flash-001")
display_cities_function = FunctionDeclaration.from_func(display_cities)
tool = Tool(function_declarations=[display_cities_function])
message = "I'd like to take a ski trip with my family but I'm not sure where to go."
res = model.generate_content(message, tools=[tool])
print(f"Function Name: {res.candidates[0].content.parts[0].function_call.name}")
print(f"Function Args: {res.candidates[0].content.parts[0].function_call.args}")
> Function Name: display_cities
> Function Args: {'preferences': 'skiing', 'cities': ['Aspen', 'Vail', 'Park City']}
```

（代码片段. 构建工具，将其与用户查询一起发送到模型，并允许进行函数调用）

总之，函数提供了一个简单的框架，使应用程序开发人员能够精细控制数据流和系统执行，同时有效地利用智能体/模型进行关键输入生成。开发人员可以选择性地选择是否通过返回外部数据来保持智能体“参与循环”，或者根据特定的应用程序架构要求将其省略。

## 数据存储

将语言模型想象成一个庞大的图书馆，其中包含其训练数据。但与不断获取新书的图书馆不同，这个图书馆保持静态，仅保存最初训练时的知识。这是一个挑战，因为现实世界的知识在不断发展。数据存储通过提供对更动态和最新信息的访问来解决此限制，并确保模型的响应始终基于事实和相关性。

考虑一个常见的场景，其中开发人员可能需要向模型提供少量额外数据，可能是电子表格或 PDF 的形式。

![](img/fdc4b629ce21a45dede2415535e93db1.png)

图 10. 智能体如何与结构化和非结构化数据交互？

数据存储允许开发人员以原始格式向智能体提供额外数据，从而消除了耗时的数据转换、模型重新训练或微调的需要。数据存储将传入文档转换为一组向量数据库嵌入，智能体可以使用这些嵌入来提取所需信息以补充其下一步操作或对用户的响应。

![](img/5f3098b75b3aaa4d5178af98289e97a2.png)

图 11. 数据存储将智能体连接到各种类型的新实时数据源。

### 实现和应用

在生成式 AI 智能体的上下文中，数据存储通常被实现为向量数据库，开发人员希望智能体在运行时可以访问该数据库。虽然我们不会在此处深入介绍向量数据库，但需要理解的关键点是，它们以向量嵌入的形式存储数据，向量嵌入是一种高维向量或所提供数据的数学表示。近年来，将数据存储与语言模型一起使用的最典型的例子之一是检索增强生成 (RAG) 的应用。这些应用程序试图通过向模型提供对各种格式数据的访问来扩展模型知识的广度和深度，例如：

+ 网站内容
+ PDF、Word 文档、CSV、电子表格等格式的结构化数据。
+ HTML、PDF、TXT 等格式的非结构化数据。

![](img/b51141607fb13f06fc60a94e1aac73ad.png)

图 12. 智能体和数据存储之间的 1 对多关系，可以表示各种类型的预索引数据

每个用户请求和智能体响应循环的基本过程通常如图 13 所示建模。

1. 用户查询被发送到嵌入模型，以生成查询的嵌入
2. 然后使用匹配算法（如 `ScaNN`）将查询嵌入与向量数据库的内容进行匹配
3. 从向量数据库中以文本格式检索匹配的内容并将其发送回智能体
4. 智能体接收用户查询和检索到的内容，然后制定响应或操作
5. 将最终响应发送给用户

![](img/57d07c38d08ce512f2c2026b1310ee09.png)

图 13. 基于 RAG 的应用程序中用户请求和智能体响应的生命周期

最终结果是一个应用程序，该应用程序允许智能体通过向量搜索将用户的查询与已知数据存储进行匹配，检索原始内容，并将其提供给编排层和模型以进行进一步处理。下一步可能是向用户提供最终答案，或执行额外的向量搜索以进一步优化结果。与实现带有 `ReAct` 推理/计划的 RAG 的智能体进行交互的示例如图 14 所示。

![](img/bb6521e6732a4d8faeedefae90cdd773.png)

图 14. 使用 `ReAct` 推理/计划的示例 RAG 应用程序

## 工具回顾

总而言之，扩展、函数和数据存储构成了智能体可在运行时使用的几种不同工具类型。每种工具都有自己的用途，并且可以根据智能体开发人员的判断一起使用或单独使用。

开发人员希望使用以下任何数据类型实现检索增强生成 (RAG)：

## 增强模型性能与针对性学习

有效使用模型的一个关键方面是它们在生成输出时选择正确工具的能力，尤其是在生产中大规模使用工具时。虽然一般训练可以帮助模型发展这项技能，但现实世界的场景通常需要超出训练数据的知识。可以将其想象成基本烹饪技能和掌握特定菜系之间的区别。两者都需要基本的烹饪知识，但后者需要有针对性的学习才能获得更细致的结果。

为了帮助模型获得此类特定知识，存在几种方法：

+ 上下文学习：此方法在推理时为通用模型提供提示、工具和少样本示例，使其能够“即时”学习如何以及何时将这些工具用于特定任务。`ReAct` 框架是自然语言中这种方法的一个示例。
+ 基于检索的上下文学习：此技术通过从外部存储器中检索最相关的信息、工具和相关示例，动态填充模型提示。这方面的一个示例是 `Vertex AI` 扩展中的“示例存储”或前面提到的数据存储 RAG 架构。
+ 基于微调的学习：此方法涉及在推理之前使用更大的特定示例数据集训练模型。这有助于模型在收到任何用户查询之前了解何时以及如何应用某些工具。

为了进一步说明每种学习方法的目标，我们可以回到烹饪类比进行探讨。

+ 想象一下，一位厨师收到了一份特定的食谱（提示）、一些关键食材（相关工具）和一些来自顾客的示例菜肴（少样本示例）。基于这些有限的信息和厨师的一般烹饪知识，他们需要弄清楚如何“即时”准备最符合食谱和顾客偏好的菜肴。这就是上下文学习。
+ 现在让我们想象一下我们的厨师在一个备有充足的食品储藏室（外部数据存储）的厨房里，里面装满了各种食材和食谱（示例和工具）。厨师现在能够从食品储藏室中动态选择食材和食谱，并更好地与顾客的食谱和偏好保持一致。这使得厨师可以利用现有的和新的知识来制作更明智和精致的菜肴。这就是基于检索的上下文学习。
+ 最后，让我们想象一下，我们将厨师送回学校学习新的菜系或菜系（在更大的特定示例数据集上进行预训练）。这使得厨师能够以更深入的理解来处理未来未知的顾客食谱。如果我们希望厨师在特定菜系（知识领域）中表现出色，这种方法是完美的。这就是基于微调的学习。

每种方法在速度、成本和延迟方面都具有独特的优势和劣势。但是，通过在智能体框架中结合这些技术，我们可以利用各种优势并最大程度地减少其劣势，从而实现更强大和适应性更强的解决方案。



# 使用 LangChain 快速启动智能体

为了提供一个可执行的智能体实际运行示例，我们将使用 LangChain 和 LangGraph 库构建一个快速原型。这些流行的开源库允许用户通过将逻辑、推理序列“链接”在一起来构建客户智能体，以及调用工具来回答用户的查询。我们将使用我们的 `gemini-1.5-flash-001` 模型和一些简单的工具来回答来自用户的多阶段查询，如代码片段 8 所示。

我们使用的工具是 `SerpAPI` (用于 Google 搜索) 和 Google Places API。执行我们在代码片段 8 中的程序后，可以在下面的代码片段 中看到示例输出。

```
from langgraph.prebuilt import create_react_agent
from langchain_core.tools import tool
from langchain_community.utilities import SerpAPIWrapper
from langchain_community.tools import GooglePlacesTool
os.environ["SERPAPI_API_KEY"] = "XXXXX"
os.environ["GPLACES_API_KEY"] = "XXXXX"
@tool
def search(query: str):
"""Use the SerpAPI to run a Google Search."""
search = SerpAPIWrapper()
return search.run(query)
@tool
def places(query: str):
"""Use the Google Places API to run a Google Places Query."""
places = GooglePlacesTool()
return places.run(query)
model = ChatVertexAI(model="gemini-1.5-flash-001")
tools = [search, places]
query = "Who did the Texas Longhorns play in football last week? What is the address of the other team's stadium?"
agent = create_react_agent(model, tools)
input = {"messages": [("human", query)]}
for s in agent.stream(input, stream_mode="values"):
message = s["messages"][-1]
if isinstance(message, tuple):
print(message)
else:
message.pretty_print()
```

（代码片段. 使用工具的示例 LangChain 和 LangGraph 智能体）

```
=============================== 用户消息 ================================
德克萨斯长角牛队上周的橄榄球赛对手是谁？对方球队体育场的地址是什么？
================================= AI 消息 =================================
工具调用：搜索
参数：
查询：德克萨斯长角牛队橄榄球赛程
================================ 工具消息 ================================
名称：搜索
{...结果：“NCAA 一级橄榄球联赛，佐治亚州，日期...”}
================================= AI 消息 =================================
德克萨斯长角牛队上周与佐治亚斗牛犬队进行了比赛。
工具调用：地点
参数：
查询：佐治亚斗牛犬队体育场
================================ 工具消息 ================================
名称：地点
{...桑福德体育场地址：桑福德路 100 号...}
================================= AI 消息 =================================
佐治亚斗牛犬队体育场的地址是佐治亚州雅典市桑福德路 100 号，邮编 30602。
```

（代码片段. 程序的输出）

虽然这是一个相当简单的智能体示例，但它演示了模型、编排和工具的所有基本组件协同工作以实现特定目标。在最后一节中，我们将探讨这些组件如何在 Google 规模的托管产品 (如 Vertex AI 智能体和 Generative Playbooks) 中结合在一起。

## 使用 Vertex AI 智能体的生产应用

虽然本白皮书探讨了智能体的核心组件，但构建生产级应用程序需要将它们与其他工具 (如用户界面、评估框架和持续改进机制) 集成。Google 的 Vertex AI 平台通过提供一个包含前面介绍的所有基本元素的完全托管的环境来简化此过程。使用自然语言界面，开发人员可以快速定义其智能体的关键元素 - 目标、任务说明、工具、用于任务委派的子智能体和示例 - 以轻松构建所需的系统行为。此外，该平台还提供了一组开发工具，可用于测试、评估、衡量智能体性能、调试和提高已开发智能体的整体质量。这使得开发人员可以专注于构建和优化其智能体，而基础架构、部署和维护的复杂性则由平台本身管理。

在图 15 中，我们提供了一个在 Vertex AI 平台上构建的智能体的示例架构，该架构使用了诸如 Vertex Agent Builder、Vertex Extensions、Vertex 函数调用和 Vertex 示例存储等各种功能。该架构包括许多生产就绪应用程序所需的各种组件。

图 15. 在 Vertex AI 平台上构建的端到端智能体示例架构

可以从官方文档中尝试此预构建智能体架构的示例。

## 总结

在本白皮书中，讨论了生成式 AI 智能体的基本构建块、它们的组成以及以认知架构的形式实现它们的有效方法。本白皮书的一些主要要点包括：

+ 智能体通过利用工具访问实时信息、建议实际操作以及自主规划和执行复杂任务来扩展语言模型的功能。智能体可以利用一个或多个语言模型来决定何时以及如何转换状态并使用外部工具来完成任意数量的复杂任务，这些任务对于模型本身来说是难以或不可能完成的。

+ 智能体操作的核心是编排层，这是一种构建推理、计划、决策并指导其行动的认知架构。各种推理技术 (如 ReAct、思维链和思维树) 为编排层提供了一个框架，用于接收信息、执行内部推理并生成明智的决策或响应。

+ 工具 (如扩展、函数和数据存储) 充当智能体通往外部世界的钥匙，使它们能够与外部系统交互并访问超出其训练数据的知识。扩展提供了智能体和外部 API 之间的桥梁，从而可以执行 API 调用和检索实时信息。函数通过责任分工为开发人员提供了更细致的控制，允许智能体生成可以在客户端执行的函数参数。数据存储为智能体提供对结构化或非结构化数据的访问，从而支持数据驱动的应用程序。

智能体的未来拥有令人兴奋的进步，我们才刚刚开始触及可能的表面。随着工具变得越来越复杂，推理能力得到增强，智能体将能够解决日益复杂的问题。此外，“智能体链”的战略方法将继续保持势头。通过结合专业智能体 - 每个智能体都在特定领域或任务中表现出色 - 我们 够在各个行业和问题领域提供卓越的结果。

重要的是要记住，构建复杂的智能体架构需要迭代方法。实验和改进是为特定业务案例和组织需求找到解决方案的关键。由于支持其架构的基础模型的生成性质，没有两个智能体是完全相同的。但是，通过利用每个基础组件的优势，我们可以创建扩展语言模型能力并驱动实际价值的有影响力的应用程序。

## 参考

+ Shafran, I., Cao, Y. et al., 2022, 'ReAct: Synergizing Reasoning and Acting in Language Models'. Available at:
https://arxiv.org/abs/2210.03629

+ Wei, J., Wang,[ X. et al., 2023](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2201.11903.pdf), 'Chain-of-Thought Prompting Elicits Reasoning in Large Language Models'.
Available at: https://arxiv.org/pdf/2201.11903.pdf.

+ Wang, X. et al., 2022, 'Self-Consistency Improves Chain of Thought Reasoning in Language Models'.
Available at: https://arxiv.org/abs/2203.11171.

+ [Diao, S. et al.,](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2302.12246.pdf) 2023, 'Active Prompting with Chain-of-Thought for Large Language Models'. Available at:
https://arxiv.org/pdf/2302.12246.pdf.

+ Zhang, H. et al., 2023, 'Multimodal Chain-of-Thought Reasoning in Language Models'. Available at:
https://arxiv.org/abs/2302.00923.

+ Yao, S. et al., 2023, 'Tree of Thoughts: Deliberate Problem Solving with Large Language Models'. Available at:
https://arxiv.org/abs/2305.10601.

+ Long, X., 2023, 'Large Language Model Guided Tree-of-Thought'. Available at:
https://arxiv.org/abs/2305.08291.

+ Google. 'Google Gemini Application'. Available at: http://gemini.google.com.

+ Swagger. 'OpenAPI Specification'. Available at: https://swagger.io/specification/.

+ Xie, M., 2022, 'How does in-[c](https://link.zhihu.com/?target=https%3A//ai.stanford.edu/blog/understanding-incontext/)ontext learning work? A framework for understanding the differences from
traditional supervised learning'. Available at: https://ai.stanford.edu/blog/understanding-incontext/.

+ [G](https://link.zhihu.com/?target=https%3A//github.com/google-research/google-research/tree/master/scann)oogle Research. 'ScaNN (Scalable Nearest Neighbors)'. Available at:
https://github.com/google-research/google-research/tree/master/scann.

+ LangChain. 'LangChai[n](https://link.zhihu.com/?target=https%3A//python.langchain.com/v0.2/docs/introduction/)'. Available at: https://python.langchain.com/v0.2/docs/introduction/.

参考：https://www.aisharenet.com/gugeagentsyujiai/

https://zhuanlan.zhihu.com/p/16690009802
