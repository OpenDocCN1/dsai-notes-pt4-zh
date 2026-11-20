
# LangGraph Studio: 首个agent IDE

> 来源：[https://agijuejin.feishu.cn/docx/N1fzdPDo3o44Qpx98CQc2odQnyc](https://agijuejin.feishu.cn/docx/N1fzdPDo3o44Qpx98CQc2odQnyc)

> 作者：lucas大叔

> 原文：[https://zhuanlan.zhihu.com/p/713379247](https://zhuanlan.zhihu.com/p/713379247)

LLM为开发新型智能体应用铺平了道路——随着LLM应用的进化，有效开发它们所需的工具也必须发展。Langchain官方发布了第一个专门为智能体开发设计的 集成开发环境 ——LangGraph Studio的 公测版 本。

LangGraph Studio提供了开发LLM应用的新方法，专门的智能体IDE用于可视化、交互和调试复杂的智能体应用程。本文将简要概述LangGraph，然后探索LangGraph Studio如何简化智能体应用的开发。

---

## LangGraph

2023年1月，Langchain官方推出了LangGraph，一个用于构建智能体应用的高度可控的低级编排框架。从那以后，我们看到团队为生产构建了更复杂的智能体应用；反过来，我们在LangGraph上投入了大量资金，今年6月发布了 stable 0.1 版本。

LangGraph具有持久层，可以实现“人在环路”交互，它擅长构建需要高度特定领域认知架构的复杂（即不止一个LLM调用）应用。我们在生产中看到的大多数智能体都符合这一描述。

LangGraph是完全开源的，提供 Python 和 Javascript 版本。无论有无 LangChain 它都能运行，并与LangSmith无缝集成。

## LangGraph Studio

虽然 LangGraph 为开发智能体应用提供了新的框架，但我们也坚信需要新的工具使开发过程更容易。构建 LLM 应用不同于传统的软件开发，需要传统代码编辑器之外的不同工具。

编码对于开发LLM应用仍然很重要——毕竟，生产就绪的LangGraph应用在创建的图的节点和边具有复杂的自定义逻辑。我们的目标不是取代 代码编辑器 ，而是用为LangGraph应用量身定制的工具来增强开发体验。

LangGraph Studio通过可视化和与图交互变得容易来达成这一点，即使开发仍然主要在代码中进行。 可视化图 有助于开发人员理解其结构。此外，你可以在智能体的轨迹中途修改智能体结果（或特定节点背后的逻辑），让你在那个时间点与状态迭代进行交互和操作。

## LangGraph Studio使用

LangGraph Studio是一款桌面应用，目前可用于Apple Silicon。你可以在此处 下载 。对更多平台的支持即将推出

下载并打开LangGraph Studio后，系统会提示你使用LangSmith 帐户登录 。LangSmith的所有用户（包括拥有免费帐户的用户）目前都可以在LangGraph Studio 测试版 期间访问它。

![](img/3d3398f502c64ad45a8b7fc8882880a6.png)

下载LangSmith后，你可以打开一个目录。该目录至少需要包含一个定义了图的Python文件。

接下来，你需要创建 `langgraph.json` 文件，其中包含已定义智能体的位置、安装的依赖项以及要加载的环境变量等详细信息。该文件可以在UI中创建，也可以作为文件存在于目录中。满足这些要求的示例存储库，请参阅此 GitHub repo 。

![](img/54b9996b1d89d49243855b3dbd403750.png)

打开目录后，为智能体构建 运行环境 。构建完成后，你应该会看到图的可视化以及用于与智能体交互的方框。

![](img/84966ce59007b611dd9a727c49f7b40f.png)

当与智能体交互时，你将获得有关正在发生步骤的实时信息流。可以看到智能体决定调用哪些工具，调用这些工具，然后继续循环。

如果偏离轨道，你可以随时中断智能体，或者可以中断智能体在“ 调试模式 ”下运行，它会在图的每一步之后暂停（因此你可以逐步遍历）。

如果你不喜欢智能体在特定步骤的响应，可以直接修改响应，然后基于新的响应继续。这对于模拟如果智能体或工具返回不同内容时会发生什么情况很有用。

你还可以修改底层代码，然后重新运行节点。LangGraph Studio 会检测到底层代码文件的更改，允许你在代码编辑器中更新提示，并在智能体响应不佳时重新运行节点。这可以使对长期运行的智能体进行迭代变得更容易。

## 总结

构建智能体应用与传统软件开发有所不同。虽然代码编辑器仍然重要，但也需要为智能体设计的新集成开发环境。LangGraph Studio 就是朝这个方向迈出的一步，我们 很期待 看到它如何提升你的工作流程。 有关 LangGraph Studio 的更多信息，请查阅 文档 。

## 参考文档

[https://blog.langchain.dev/langgraph-studio-the-first-agent-ide/](https://blog.langchain.dev/langgraph-studio-the-first-agent-ide/)

[GitHub - langchain-ai/langgraph-example at blog.langchain.dev](https://github.com/langchain-ai/langgraph-example?ref=blog.langchain.dev)

[https://github.com/langchain-ai/langgraph-studio?ref=blog.langchain.dev](https://github.com/langchain-ai/langgraph-studio?ref=blog.langchain.dev)
