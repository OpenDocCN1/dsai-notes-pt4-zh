
虽然本白皮书探讨了智能体的核心组件，但构建生产级应用程序需要将它们与用户界面、评估框架和持续改进机制等其他工具集成。Google 的 Vertex AI 平台通过提供一个完全托管的环境（包含前面所述的所有基本要素）简化了这一过程。使用自然语言界面，开发人员可以快速定义其智能体的关键要素——目标、任务指令、工具、用于任务委派的子智能体以及示例——以轻松构建所需的系统行为。此外，该平台还提供了一组开发工具，用于测试、评估、衡量智能体性能、调试以及提高已开发智能体的整体质量。这使开发人员能够专注于构建和改进他们的智能体，而基础设施、部署和维护的复杂性则由平台自身管理。

图15展示了一个使用 Vertex AI 平台上的各种功能（例如 Vertex Agent Builder、Vertex Extensions、Vertex Function Calling 和 Vertex Example Store 等）构建的智能体的示例架构。该架构包含生产就绪应用程序所需的许多不同组件。

图15: Vertex AI平台端到端智能体架构示例

![](img/4e0b15e40d7f297090f1c9c3f8d68cf2.png)

## 7.总结

在本白皮书中，我们探讨了生成式 AI 智能体的基本构建模块、它们的组成以及以认知架构形式有效实施它们的方法。本白皮书的一些主要结论包括：

1. 智能体通过利用工具访问实时信息、提出实际操作建议以及自主规划和执行复杂任务，扩展了语言模型的功能。智能体可以利用一个或多个语言模型来决定何时以及如何转换状态，并使用外部工具来完成模型自身难以或无法完成的任意数量的复杂任务。

2. 智能体运行的核心是编排层，这是一种认知架构，用于构建推理、规划、决策并指导其行动。诸如 ReAct、思维链(CoT)和思维树(ToT)等各种推理技术为编排层提供了框架，使其能够接收信息、执行内部推理并生成明智的决策或响应。

3. 扩展、函数和数据存储等工具是智能体通往外部世界的关键，使它们能够与外部系统交互并访问超出其训练数据的知识。扩展提供了智能体和外部API之间的桥梁，从而能够执行API调用并检索实时信息。函数通过分工为开发人员提供了更精细的控制，允许智能体生成可在客户端执行的函数参数。数据存储为智能体提供了对结构化或非结构化数据的访问，从而支持数据驱动的应用程序。

智能体的未来充满令人兴奋的进步，我们才刚刚开始探索其可能性。随着工具变得更加复杂且推理能力得到增强，智能体将能够解决日益复杂的问题。此外，“智能体链接(agent chaining)”的战略方法将继续获得发展势头。通过结合专门的智能体——每个智能体都擅长特定的领域或任务——我们可以创建一种“智能体专家混合”方法，能够在各个行业和问题领域提供卓越的成果。

重要的是要记住，构建复杂的智能体架构需要迭代方法。实验和改进是为特定业务案例和组织需求找到解决方案的关键。由于构成其架构基础的基础模型的生成性质，没有两个智能体是完全相同的。然而，通过利用这些基本组件的优势，我们可以创建影响深远的应用，扩展语言模型的功能并驱动实际价值。

## 参考资料

1. Shafran, I., Cao, Y. et al. (2022). ReAct: Synergizing Reasoning and Acting in Language Models. `https://arxiv.org/abs/2210.03629`

2. Wei, J., Wang, X. et al. (2023). Chain-of-Thought Prompting Elicits Reasoning in Large Language Models. `https://arxiv.org/pdf/2201.11903.pdf`

3. Wang, X. et al. (2022). Self-Consistency Improves Chain of Thought Reasoning in Language Models. `https://arxiv.org/abs/2203.11171`

4. Diao, S. et al. (2023). Active Prompting with Chain-of-Thought for Large Language Models. `https://arxiv.org/pdf/2302.12246.pdf`

5. Zhang, H. et al. (2023). Multimodal Chain-of-Thought Reasoning in Language Models. `https://arxiv.org/abs/2302.00923`

6. Yao, S. et al. (2023). Tree of Thoughts: Deliberate Problem Solving with Large Language Models. `https://arxiv.org/abs/2305.10601`

7. Long, X. (2023). Large Language Model Guided Tree-of-Thought. `https://arxiv.org/abs/2305.08291`

8. Google. Google Gemini Application. `http://gemini.google.com`

9. Swagger. OpenAPI Specification. `https://swagger.io/specification/`

10. Xie, M. (2022). How does in-context learning work? A framework for understanding the differences from traditional supervised learning. `https://ai.stanford.edu/blog/understanding-incontext/`

11. Google Research. ScaNN (Scalable Nearest Neighbors). `https://github.com/google-research/google-research/tree/master/scann`

12. LangChain. LangChain. `https://python.langchain.com/v0.2/docs/introduction/`

`https://www.kaggle.com/whitepaper-agent-companion`
