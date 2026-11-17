
# Triplex vs. GPT-4：将`Graph RAG`成本降低98%的革命性模型、知识图谱构建的 SOTA LLM！

> 来源：[https://agijuejin.feishu.cn/docx/A2PwdbB0To3bY7x0c5HcDwpnnof](https://agijuejin.feishu.cn/docx/A2PwdbB0To3bY7x0c5HcDwpnnof)

> 作者：Aitrainee | AI进修生

> 原文链接： [https://mp.weixin.](https://mp.weixin.qq.com/s/T-MxvP5jg3hcUWHlSMk87A)qq.com/s/T-MxvP5j...

Hi，这里是Aitrainee，欢迎阅读本期新文章。

知识图谱（例如 Microsoft 的 `Graph RAG` ）增强了 `RAG` 方法，但构建成本高昂。`Triplex` 可将知识图谱创建成本降低 98%，性能优于 `GPT-4`，成本仅为 `GPT-4` 的 1/60。

并且可以使用 `SciPhi.AI` 的 `R2R` 框架快捷构建本地图谱。

![](img/842fdb7a4e16bce849d9409e81b3c675.png)

▲ 知识图谱结构图解

`Triplex` 是 `SciPhi.AI` 开发的 `Phi3-3.8B` 的微调版本，用于从非结构化数据创建知识图谱。它的工作原理是从文本或其他数据源中提取三元组（由主语、谓语和宾语组成的简单语句）。

![](img/02f0627805b7447f2bf26397c4ebe162.png)

## GraphRAG 与 Triplex

知识图谱擅长回答传统搜索方法通常难以回答的查询，尤其是人口级关系查询。例如，“提供一份就读技术学校的 AI 员工名单”。微软发布 `GraphRAG` 后，人们对知识图谱的兴趣愈演愈烈。

但是，知识图谱构建过程传统上非常复杂且资源密集，限制了其广泛应用。最近的估计表明，微软的 `GraphRAG` 程序成本特别高，要求为每个输入标记生成至少一个输出标记。这种高成本使其对于大多数应用程序来说不切实际。

`Triplex` 的目标是通过将知识图谱的生成成本降低十倍来彻底颠覆这一模式。这一成本降低是通过 `Triplex` 将非结构化文本转换为“语义三元组”——知识图谱的构建块——的有效方法实现的。

为了演示 `Triplex` 如何创建这些图谱，观察一下它如何处理简单的句子：

![](img/94685afcaaf4f894abc987ba6a7f7d28.png)

对于更复杂的输入：

![](img/0020d0de973dfd2dc6c68df2d68d24a0.png)

性能测量表明，`Triplex` 在成本和性能方面明显优于 `gpt-4o`。

### 准确度比较

![](img/6c3878ff69f968296229459313bbba80.png)

### 价格比较

`Triplex` 模型实现了与 `GPT-4` 相当的结果，但成本却仅为其一小部分。`Triplex` 的模型尺寸更小，并且无需少量样本上下文即可运行，因此成本显著降低。

在 `SFT` 模型的基础上，`Triplex`使用多数投票和拓扑排序生成了额外的基于偏好的数据集，以进一步使用 `DPO` 和 `KTO` 训练 `Triplex`。这些额外的训练步骤显著提高了模型性能。

为了准确评估这些细微的增强，他们使用 Claude-3.5 Sonnet 进行了严格的评估。`Triplex`的评估涉及三个模型之间的直接比较：`triplex-base`、`triplex-kto` 和 `GPT-4o`。结果如下表所示：

![](img/e0380d9aeae10c2489698d0e0e7fa5f9.png)

`Triplex` 的出色表现源于对多样化和综合性数据集的广泛训练。`Triplex`模型利用了来自权威来源（例如 `DBPedia` 和 `Wikidata`）以及基于网络的文本来源和合成生成的数据集生成的专有数据集。

这一广泛的基础确保了 `Triplex` 在各种应用中的多功能性和稳健性。

### 用法

开发团队设计了`R2R` `RAG` 引擎，并与`Neo4J`一起，以便立即利用 `Triplex` 进行本地知识图谱构建，这一用例现在由于这项工作才变得更加可行。在此处：`https://kg.sciphi.ai/`直接试用`Triplex`。

![](img/413759b48d691c1a11bccaece795cc3b.png)

▲ Triplex 演示站点

## 使用 R2R 构建和使用知识图谱（ `Triplex` ）

`R2R` 提供灵活而强大的配置系统，可定制您的检索增强生成 (`RAG`) 应用程序。

它是一个完整的平台，可帮助你快速构建和启动可扩展的 `RAG` 解决方案。`R2R` 围绕简单的`RESTful API`构建，易于使用且可快速实施。

![](img/ae99eb28c362f7dcdc9f4903775acd34.png)

`R2R`构建知识图谱是使用新发布的`Triplex` 模型与本地系统一起创建的

![](img/7a73bee29bd7e0c39c3b3a883dbb01ec.png)

他还支持本地`Ollama`，你必须在本地拥有 `Triplex` 模型。将其拉出并刷新其他相关模型，然后启动 `Ollama` 服务器

![](img/905e28355ac7d0c9f069f33137b14a6f.png)

如果你对这个模型感兴趣的话，请以 分享 / 点赞 / 在看 的方式告诉我，以便让我评估是否需要创作部署教程文章，比如：`Triplex` + `LangChain` + `Ollama` 构建 `RAG` ？

🌟希望这篇文章对你有帮助，感谢阅读！

参考链接：

[1] 演示： `kg.sciphi.ai`

[2] huggingface ：`https://huggingface.co/SciPhi/Triplex`

[3] 博客： `https://www.sciphi.ai/blog/triplex`

知音难求，自我修炼亦艰

抓住前沿技术的机遇，与我们一起成为创新的超级个体

（把握AIGC时代的个人力量）

![](img/ef88b72a0196e0769395782ea25a2334.png)

点这里 👇 关注我，记得标星哦～

一键三连「分享」、「点赞」和「在看」

科技前沿进展日日相见 ~
