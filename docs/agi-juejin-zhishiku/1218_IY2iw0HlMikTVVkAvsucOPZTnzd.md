# Skeleton-of-Thought: 并行解码加速和改进LLM输出

> 来源：[https://agijuejin.feishu.cn/docx/CfMwdZuB5oRrwSxwLAwcpv8Kn3e](https://agijuejin.feishu.cn/docx/CfMwdZuB5oRrwSxwLAwcpv8Kn3e)

> 作者：lucas大叔

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/695498429)hihu.com/p/695498...

[LLM如LL](https://www.zhihu.com/search?q=GPT-4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)aMA和OpenAI的 GPT-4 正在改变技术。然而，关于LLM的常见抱怨之一是它们的速度，或者缺乏速度。在许多情况下，从LLM获得答案需要很长时间。这限制了LLM的应用和它们在时延敏感功能中的有用性，例如聊天机器人、copilot和工业控制器。

为了[解决这个](https://www.zhihu.com/search?q=%E9%BB%91%E7%9B%92%E5%AD%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)问题，来自微软研究院和清华大学的研究人员提出了Skeleton-of-Thought（SoT），这是一[种加速LLM生成](https://www.zhihu.com/search?q=GPT-3.5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)的新方法。与大多数需要修改LLM模型、系统或硬件的现有方法不同，SoT将LLM视为 黑盒子 ，因此可以应用于任何现成的开源（例如LLaMA）甚至API-based（例如OpenAI的GPT-4）模型。评估表明SoT不仅极大加速了12个LLM中的内容生成，而且在某些情况下还可能提高答案质量。例如，在OpenAI的 GPT-3.5 和GPT-4上，SoT提供了2倍的加速，同时在基准数据集上提高了答案质量。

代码和demo已开源在 [https:// github.com/imaginat](https://link.zhihu.com/?target=https%3A//github.com/imagination-research/sot/)ion- research/sot/  。

![](img/fec3d6582c30396af694553ba956ea65.png)

## SoT：鼓励LLM中的 [结构化思维](https://www.zhihu.com/search?q=%E7%BB%93%E6%9E%84%E5%8C%96%E6%80%9D%E7%BB%B4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)

SoT的思想源于LLM和人类处理信息的方式的差异。LLM按顺序生成答案。例如，为了回答上图（左）中的“ How can I improve my time management [techn](https://www.zhihu.com/search?q=%E7%9B%B8%E5%85%B3%E6%97%B6%E9%97%B4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)iques ”，LLM完成了一点，然后再转到下一点。相比之下，人类可能并不总是按顺序思考问题并写出答案。在许多情况下，人类首先推导出答案的骨架，然后添加细节来解释每一点。例如，为了回答上图中的同一个问题，在深入研究每种技术的细节之前，人们可能首先考虑 相关时间 管理技术的列表。提供咨询、参加考试、写论文等练习尤其如此。

我们能[让LLM](https://www.zhihu.com/search?q=%E7%8B%AC%E7%AB%8B%E7%82%B9&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)更动态地处理信息，而不是线性地处理信息吗？如下图所示，SoT做到了这一点。SoT不是按顺序生成答案，而是将生成分解为两个阶段：（1）SoT首先要求LLM导出答案的骨架，然后（2）要求LLM向骨架中的每个点提供答案。这种方法为加速提供了一个新的机会，因为第2阶段中的 独立点 的答案可以并行生成。这既可以用于权重可由用户访问的本地模型（例如LLaMA），也可以用于只能通过API访问的基于API的模型（例如OpenAI的GPT-4）。

+   对于基于API的模型，我们可以针对每个点发出并行的API请求。

+   [对于本地运行](https://www.zhihu.com/search?q=batch&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)的[模型，我们](https://www.zhihu.com/search?q=%E6%9D%83%E9%87%8D%E5%8A%A0%E8%BD%BD&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)可以同时回答在一个 batch 中的所有问题。请注意，在许多场景中（例如，本地服务、非饱和查询期内的集中式服务），LLM的解码阶段通常受到 权重加载 而不是激活加载或计算的限制，因此未充分利用可用硬件。在这些情况下，增加batch大小运行LLM推理可以提高硬件计算利用率，并且不会显著增加时延。

![](img/92e115c3d7472d65dba14ea6f88b3ae6.png)

因此，如果答案中有  B 个点，那么与当前LLM中的顺序生成相比，理论上，如在So[T中并行生](https://www.zhihu.com/search?q=%E9%AA%A8%E6%9E%B6%E9%98%B6%E6%AE%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)成这些点可以得到  B\times 的加速。然而在实践中，由于额外的 骨架阶段 、不平衡的点长度和其他开销，实际的加速可能会小一些。

我[们在12个最近发布的模型上](https://www.zhihu.com/search?q=Vicuna-80%E6%95%B0%E6%8D%AE%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)测试了SoT，包括九个开源模型和三个基于API的模型，如下表所示。我们使用 Vicuna-80数据集 ，该数据集包含80个问题，涵盖了编码、数学、写作、角色扮演等九个类别。

![](img/ec7327eda37bd805ffdd592385e47121.png)

下图显示SoT在所有模型上均提供了显著的加速。特别是，在12个模型中，SoT在8个模型上获得了大于2倍的加速（最高达2.39倍）。此外，SoT实现了这一加速水平同时没有出现显著的答案质量下降。

![](img/4045da9a7f6da0870bd1bbf40d9195f9.png)

图4显示了SoT的胜/平/负率（定义为SoT提供比正常的顺序生成更好答案的问题的比例；“更好”是由FastChat和LLMZoo提出的指标定义的，并由GPT-4判断评估）。我们可以看到，SoT的答案质量与顺序生成的相当。在论文中，我们进一步展示了，由于骨架阶段要求LLMs提前规划答案结构，SoT在问题相关性和全面性方面提高了答案质量。

![](img/cdc666df9f4ba908c47edd7a4a002907.png)

## SoT-R：自适应触发SoT

SoT进行独立且并行的 [点扩展 ](https://www.zhihu.com/search?q=%E7%82%B9%E6%89%A9%E5%B1%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)。因[此，它不适](https://www.zhihu.com/search?q=%E8%B7%AF%E7%94%B1%E6%A8%A1%E5%9D%97&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)用于需要逐步推理的问题，例如数学和编码。为此，我们提出了SoT的扩展，称为带路[由的SoT](https://www.zhihu.com/search?q=%E6%8F%90%E7%A4%BA%E8%B7%AF%E7%94%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)（SoT-R），它只在适当时自适应地触发SoT。具体来说，我们提出了一个 路由模块 ，该模块决定是否应用SoT来处理用户请求，然后根据情况调用SoT或正常的顺序解码。路由模块可以通过提示OpenAI的GPT-4而无需进行任何模型训练（称为“ 提示路由 ”）或训练指定的RoBERTa模型（称为“训练路由”）来实现。我们表明，SoT-R提高了SoT在不同问题类别中的普适性，同时保持了显著的加速效果。

![](img/f38b9b8d22b5d13f8d0b062fe4aee6b7.png)

![](img/9f14e792199f0378942c38360c092e54.png)

## 下一步研究方向

### 减少SoT成本

[SoT相较于正](https://www.zhihu.com/search?q=prompt&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)常的顺序解码使用更长的 prompt[ ，这可能](https://www.zhihu.com/search?q=%E6%9C%8D%E5%8A%A1%E7%B3%BB%E7%BB%9F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)导致在以提示长度计费的LLM API上产生更高的成本，并潜在地降低 服务系统 的吞吐量。需要进一步研究以探索降低SoT成本的方法，包括压缩SoT提示或调整LLM，使其在必要时自动触发SoT。

### 提升LLM能力

SoT受人类结构化思维启发。因此，其成功需要进一步审视人类思维过程，以促进更有效和高效的人工智能。

考虑到人类思维的复杂性，我们可以将其想象成[更像是一](https://www.zhihu.com/search?q=%E6%80%9D%E7%BB%B4%E5%9B%BE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)张图，而SoT可以被视为通向“ 思维图 ”（GoT）的中间步骤。GoT是一个旨在表示更复杂思维的新框架，更接近人类解决问题的方式。GoT呈现了多个概念以图结构相连，其中图的边表示依赖关系，每个点根据其原始点的内容进行解码。此外，我们预期需要动态的思维图，而不是遵循静态图，其中高层次的思维结构由LLM动态调整，试图模仿人类思维方式。这可以结合SoT的效率和全局思考优势，同时捕捉了思维过程中更多的复杂性。

另一个要考虑的问题是如何利用SoT更结构化的答案微调LLM，以增强其生成组织良好且全面的答案的能力。

### 以数据为中心的效率优化

与现有的旨在提升LLM效率的模型和系统级努力相比，SoT采用了一种新颖的“数据级”路径，通过让LLM组织其输出内容来实现。由于最先进的LLM的不断发展的能力，这种观点变得可行且日益相关。我们希望这项工作能够在数据中心优化效率的领域激发更多的研究。

## langchain实战

感兴趣的同学可以根据SoT Github中的引导安装、测试SoT效果。这里[介绍lang](https://www.zhihu.com/search?q=%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)chain template的实现方式，首先安装LangChain 命令行界面 （CLI）。

```
pip install -U langchain-cli
```

创建新的LangChain项目并将其安装为唯一的软件包，执行以下操作：

```
langchain app new my-app --package skeleton-of-thought
```

如果想将软件包添加到现有项目中，可以直接运行：

```
langchain app add skeleton-of-thought
```

并将以下代码添加到 server.py 文件中：

```
from skeleton_of_thought import chain as skeleton_of_thought_chain

add_routes(app, skeleton_of_thought_chain, path=&quot;/skeleton-of-thought&quot;)
```

（可选）现在配置LangSmith，它将帮助我们追踪、监视[和调试La](https://www.zhihu.com/search?q=%E8%AE%BF%E9%97%AE%E6%9D%83%E9%99%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A695498429%7D)ngChain应用。如果没有 访问权限 ，可以跳过这部分。

```
export LANGCHAIN_TRACING_V2=true
export LANGCHAIN_API_KEY=&lt;your-api-key&gt;
export LANGCHAIN_PROJECT=&lt;your-project&gt;  # if not specified, defaults to &quot;default&quot;
```

如果你在这个目录里，那么可以直接通过以下方式启动一个LangServe实例：

```
langchain serve
```

这将启动FastAPI应用程序，服务器在本地运行的地址是 http://localhost:8000 。

我们可以在 [h](https://link.zhihu.com/?target=http%3A//127.0.0.1%3A8000/skeleton-of-thought/playground)ttp:// 127.0.0.1:8000/docs  查看所有template，也可以在 http:// 127.0.0.1:8000/skeleton -of-thought/playground  访问playground。

通过以下代码访问template：

```
from langserve.client import RemoteRunnable

runnable = RemoteRunnable(&quot;http://localhost:8000/skeleton-of-thought&quot;)
```

## 参考文档

[S](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2307.15337)keleton-of-Thought: Prompting LLMs for Efficient Parallel Generation

[S](https://link.zhihu.com/?target=https%3A//www.microsoft.com/en-us/research/blog/skeleton-of-thought-parallel-decoding-speeds-up-and-improves-llm-output/)keleton-of-Thought: Parallel decoding speeds, improves LLM output

skeleton-of-thought | ️ LangChain

[GitHub - imagination-research/sot: [ICLR 2024] Skeleto](https://link.zhihu.com/?target=https%3A//github.com/imagination-research/sot)n-of-Thought: Large Language Models Can Do Parallel Decoding