
# Llama分子嵌入优于GPT，LLM能理解分子吗？这一局Meta赢了OpenAI

> 来源：[https://agijuejin.feishu.cn/docx/Rh50d1qX2ovWaIx93ozchrwsnxb](https://agijuejin.feishu.cn/docx/Rh50d1qX2ovWaIx93ozchrwsnxb)

> 机器之心 ScienceAI｜[阅读原文](https://www.jiqizhixin.com/articles/2024-07-11-2)

> 转载请联系原作者取得授权

![](img/d1c118940c5000f6f7854a2f13793a44.png)

编辑 | 萝卜皮

## 引言

OpenAI 的 GPT 和 Meta AI 的 Llama 等大型语言模型（LLM），因其在化学信息学领域的潜力而日益受到认可，特别是在理解简化分子输入行输入系统（SMILES）方面。这些 LLM 还能够将 SMILES 字符串解码为矢量表示。

加拿大温莎大学（University of Windsor）的研究人员比较了 GPT 和 Llama 与 SMILES 上的预训练模型在下游任务中嵌入 SMILES 字符串的性能，重点关注两个关键应用：分子特性预测和药物-药物相互作用预测。

该研究以「 Can large language models understand molecules? 」为题，于 2024 年 6 月 25 日发布在《 BMC Bioinformatics 》。

![](img/ef5e732ec42854f35edf2a621bac340e.png)

## 分子嵌入技术

分子嵌入是药物发现中的重要任务，在分子性质预测、药物-靶标相互作用（DTI）预测和药物-药物相互作用（DDI）预测等相关任务中得到广泛应用。

分子嵌入技术可以从编码分子结构连接信息的分子图或其结构的线注释中学习特征，例如学界比较流行的 SMILES 表示。

通过 SMILES 字符串进行分子嵌入不断发展并与语言建模的进步同步，从静态词嵌入开始，到情境化的预训练模型。这些嵌入技术旨在以紧凑的数值表示形式捕获相关的结构和化学信息。

![](img/70be9c3db64b40f0d613ff9442e81f24.png)

图示：药物化学表示。（来源：论文）

基本假设认为结构相似的分子行为方式相似。这使得机器学习算法能够处理和分析分子结构，以进行属性预测和药物发现任务。

## LLM 与分子理解

随着 LLM 的突破，一个突出的问题是 LLM 是否能够理解分子并根据分子数据进行推断？

更具体地说，LLM 能否产生高质量的语义表示？

温莎大学的 Shaghayegh Sadeghi、Alioune Ngom Jianguo Lu 等人进一步探索了这些模型有效嵌入 SMILES 的能力。目前，这一能力尚未得到充分探索，部分原因可能是 API 调用的成本。

研究人员发现，使用 Llama 生成的 SMILES 嵌入在分子特性和 DDI 预测任务中的表现都优于 GPT 生成的 SMILES 嵌入。

![](img/252f6f3d672494c5d15c7de4b5fa59ce.png)

图示：分类和回归任务的结果。（来源：论文）

值得注意的是，基于 Llama 的 SMILES 嵌入在分子预测任务中表现出与 SMILES 上的预训练模型相当的结果，并且在 DDI 预测任务中优于预训练模型。

## 研究结论

因此该团队得到以下几个结论：

（1）LLM 的表现确实优于传统方法。

（2）性能取决于任务，有时也取决于数据。

（3）LLM 的新版本确实比旧版本有所改进，即使它们是针对更通用的任务进行训练的。

（4）他们观察到 Llama 的嵌入总体上优于 GPT 嵌入。

（5）还有个现象是 Llama 和 Llama2 在嵌入性能方面非常接近。

![](img/52804624d8e6d4c9e4e922d309e9c0a5.png)

图示：Llama 与 Llama2 性能比较。（来源：论文）

## 总结与展望

总之，这项研究凸显了 GPT 和 Llama 等 LLM 在分子嵌入方面的潜力。

该团队特别推荐 Llama 模型而不是 GPT，因为它们在从 SMILES 字符串生成分子嵌入方面具有卓越的性能。这些发现表明 Llama 在预测分子特性和药物相互作用方面可能特别有效。

虽然 Llama 和 GPT 等模型并非专门为 SMILES 字符串嵌入而设计（与 ChemBERTa 和 MolFormer-XL 等专业模型不同），但它们仍然表现出了竞争力。该工作为将来改进 LLM 分子嵌入奠定了基础。

## 未来工作

未来，该团队将侧重于提高由自然语言句子嵌入技术启发而来的 LLM 分子嵌入的质量，例如对 Llama 标记化的微调和修改。

## 参考链接

GitHub：`https://github.com/sshaghayeghs/LLaMA-VS-GPT`

论文链接：`https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-024-05847-x`
