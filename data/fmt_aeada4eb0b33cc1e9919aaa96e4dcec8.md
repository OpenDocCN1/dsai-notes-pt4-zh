
# 知识图谱 Top15 被引盘点

> 来源：[https://v11enp9ok1h.feishu.cn/docx/YG9rdgrXSoAad6xiiCpcKnennyb](https://v11enp9ok1h.feishu.cn/docx/YG9rdgrXSoAad6xiiCpcKnennyb)

本文精选了 [EMNLP 2024 论文集](https://aclanthology.org/events/emnlp-2024/)中与知识图谱相关的、被引量最高的15篇论文。后续我们还会继续陆续发布不同领域的 EMNLP 2024 高引盘点，在机智流👸号后台对话框回复“盘点”，加入顶会论文盘点交流群。

*注：引用数据来自谷歌学术，数据统计截止 2024 年 11 月 13 日

![](img/ed20fdab28d925b7cf760e1f3bb90682.png)

1. `C-ICL`: 用于信息抽取的对比式上下文学习（10次被引）
1. 抽取、定义、规范化：基于大语言模型的知识图谱构建框架（8次被引）
1. `DALK`: 回答基于科学文献的阿尔茨海默症问题背景下，大语言模型与知识图谱的动态联合增强（8次被引）
1. 知识图谱增强的大语言模型编辑（7次被引）
1. `InfuserKI`: 通过知识图谱和 `Infuser` 引导的知识整合以增强大语言模型（4次被引）
1. 通过知识图谱提升检索增强型大语言模型的规划能力（4次被引）
1. `TRACE` the Evidence: 面向检索增强生成的知识驱动型推理链构建方法（4次被引）
1. 合理推理即正确：用于可验证常识型知识图谱问答的大语言模型（3次被引）
1. 面向知识增强零样本问答的以证据为中心的事实摘要（3次被引）
1. 基于图的生成：同时将大型语言模型作为智能体和知识图谱处理不完整的知识图谱问答（3次被引）
1. 以使用为中心的电商意图理解方法（2次被引）
1. 上下文的重要性：利用图结构化知识上下文推动开放式回答生成的边界（2次被引）
1. `FactGenius`: 结合零样本提示和模糊关系挖掘提升基于知识图谱的事实验证（2次被引）
1. 历史文本中的对比式实体共指解析和消歧（1次被引）
1. 利用语言模型提取结构化实体（1次被引）

## `C-ICL`：用于信息抽取的对比式上下文学习（`C-ICL`: Contrastive In-context Learning for Information Extraction）

`https://aclanthology.org/2024.findings-emnlp.590.pdf`([2])

论文指出，在信息提取（IE）领域利用大型语言模型（LLMs）进行小样本信息提取时，研究人员大多仅使用正确示例演示，忽视了错误示例的潜在价值。本文提出一种新的小样本技术 `c-ICL`，通过构建正确与错误样本创建上下文学习演示，利用正样本及其背后推理过程的提示提高`LLMs`实体和关系提取能力，还能识别和纠正潜在接口错误。该方法利用硬负样本（hard negative samples）和测试最近正邻的固有上下文及有价值信息，基于`LLMs`进行上下文学习演示。在多个数据集的实验表明，`c-ICL` 性能优于之前小样本上下文学习方法，在多种相关任务中有显著提升，展现其通用性。

![](img/a4ab6c19b0e60302fe3beba52cabf7db.png)

## 抽取、定义、规范化：基于大语言模型的知识图谱构建框架 (Extract, Define, Canonicalize: An LLM-based Framework for Knowledge Graph Construction)

`https://aclanthology.org/2024.emnlp-main.548.pdf`([3])

该论文关注从输入文本自动创建知识图谱（KGC）的方法。大语言模型（LLM）虽已被应用于KGC，但面临在大规模文本应用中的难题，如KG模式需包含在LLM提示中，复杂模式易超LLM上下文窗口长度，且有时缺乏固定预定义模式。为解决这些问题，论文提出提取 - 定义 - 规范化（`EDC`）三阶段框架，该框架灵活，可应用于有或无预定义目标模式的情况；后一种情况可自动构建模式并自我规范化。此外还引入了检索相关模式元素的训练组件以提升性能。经三个KGC基准测试证明，`EDC`无需参数调整就能提取高质量三元组且模式规模大于之前的成果。`EDC`代码可通过网址（`https://github.com/clear-nus/edc`）获取。

![](img/8e41606e5430becb368b036001884096.png)

## `DALK`：回答基于科学文献的阿尔茨海默症问题背景下，大语言模型与知识图谱的动态联合增强 (`DALK`: Dynamic Co-Augmentation of LLMs and KG to answer Alzheimer’s Disease Questions with Scientific Literature)

`https://aclanthology.org/2024.findings-emnlp.119.pdf`([4])

大型语言模型（LLMs）虽在多应用中表现良好，但长尾知识整合的挑战阻碍其在专业领域的应用。本文提出名为`DALK`（LLMs与知识图谱动态协同增强）的方法，以解决此局限，并在研究阿尔茨海默病（AD，生物医学的专业子领域、全球健康优先事项）上展示其能力。通过LLM与知识图谱（KG）相互增强的协同框架，先用LLM构建源自AD相关科学文献、不断发展的AD特定知识图谱，再用从粗到细采样法与新型自我感知知识检索法从图谱中选合适知识以增强LLM推理能力。在构建的AD问答基准上的实验结果突显了`DALK`的有效性，还进行了系列详细分析为KG和LLM相互增强这一新兴主题提供见解和指导，代码和数据将在指定网址发布。

![](img/b62910bcb11b9824096e119884cb9f46.png)

## 知识图谱增强的大语言模型编辑 (Knowledge Graph Enhanced Large Language Model Editing)

`https://aclanthology.org/2024.emnlp-main.1261.pdf`([5])

大型语言模型（LLMs）对自然语言处理（NLP）任务很重要，但存在知识不准确和过时的问题，模型编辑是一种解决方案，不过现有编辑方法难以跟踪和整合编辑相关的知识变化而限制了编辑后模型处理编辑知识的泛化能力。本文提出一种利用知识图谱增强LLM编辑的新模型编辑方法`GLAME`，包括知识图谱扩充模块挖掘编辑相关的知识及其在LLM中的内部表示，基于图的知识编辑模块整合结构化知识到模型编辑中，在`GPT-J`和`GPT-2 XL`上的实验表明`GLAME`显著提高了编辑后LLMs在运用编辑知识时的泛化能力。

![](img/4b0f1b02b425d6f5611094d3f8c1ad02.png)

## `InfuserKI`: 通过知识图谱和 `Infuser` 引导的知识整合以增强大语言模型 (`InfuserKI`: Enhancing Large Language Models with Knowledge Graphs via `Infuser`-Guided Knowledge Integration)

`https://aclanthology.org/2024.findings-emnlp.209.pdf`([6])

大型语言模型（LLMs）在知识密集型任务上存在困难，虽有知识整合方法用外部模块增强LLMs，但存在数据效率低的问题。本文研究有效整合未知知识且避免已知知识不必要重叠的新问题，注入新知识可能导致遗忘已有知识，为此提出`InfuserKI`框架，利用变换器内部状态决定是否用额外信息增强原始LLM输出以减轻知识遗忘。在`UMLS - 2.5k`和`MetaQA`领域知识图谱评估表明，`InfuserKI`能有效获取新知识且在减少知识遗忘方面分别比最先进的基准方法高出9%和6%。

![](img/702fd4e9de163cadaa49883a7815f32e.png)

## 通过知识图谱提升检索增强型大语言模型的规划能力（Learning to Plan for Retrieval-Augmented Large Language Models from Knowledge Graphs）

`https://aclanthology.org/2024.findings-emnlp.459.pdf`([7])

提升大型语言模型（LLMs）在复杂问答（QA）场景中的性能一直是研究重点。近期研究试图通过将逐步规划与外部检索相结合来提高LLMs的性能，但较小的LLMs在分解复杂问题时面临挑战，需要监督微调。以往工作依赖手动标注和从教师LLMs中进行知识蒸馏，既耗时又不够准确。本文提出一个新框架，利用从知识图谱（KGs）得出的规划数据增强LLMs的规划能力，经此数据微调后的LLMs规划能力提高，更能处理涉及检索的复杂QA任务，对多个数据集（包括新提出的基准）的评估凸显了该框架的有效性和KG衍生规划数据的优势。

![](img/833b7ce6b4e272dc4bac9ee2a7dfd1c4.png)

## `TRACE`证据链：面向检索增强生成的知识驱动型推理链构建方法 (`TRACE` the Evidence: Constructing Knowledge-Grounded Reasoning Chains for Retrieval-Augmented Generation)

`https://aclanthology.org/2024.findings-emnlp.496.pdf`([8])

检索增强生成（RAG）可用于问答（QA）任务，但RAG模型中检索器的不完善常导致检索到无关信息，影响性能，尤其是处理多跳推理问题时。为提高RAG模型多跳推理能力，提出`TRACE`，它构建基于知识的推理链（一系列逻辑相连的知识三元组）来从检索文档中识别和整合证据以回答问题，具体是利用KG生成器从检索文档构建知识图谱，再用自回归推理链构造器构建推理链。三个多跳QA数据集的实验结果显示，与使用所有检索文档相比，`TRACE`平均性能提升达14.03%，且结果表明用推理链而非整个文档作上下文通常就足以正确回答问题。

![](img/99cc918f32b6a3fa84a9e6cab360ff1b.png)

## 合理推理即正确：用于可验证常识型知识图谱问答的大语言模型 (Right for Right Reasons: Large Language Models for Verifiable Commonsense Knowledge Graph Question Answering)

`https://aclanthology.org/2024.emnlp-main.378.pdf`([9])

知识图谱问答（KGQA）方法旨在利用知识图谱（KGs）中的关系信息回答自然语言问题。随着大型语言模型（LLMs）的发展及其卓越推理能力的展现，利用其进行KGQA成为趋势，但现有方法仅关注事实性问题，对常识推理问题未涉及。本研究发现基于LLM的KGQA方法在处理常识推理问题时存在幻觉问题，尤其针对长尾实体的查询，且推理过程不易验证，阻碍实际应用。因此提出“基于正确理由的正确答案（`R3`）”这一常识性KGQA方法，通过公理化呈现LLMs的内在常识知识，并将每个事实推理步骤基于KG三元组，实现可验证的推理过程。经三项任务（问答、声明验证、偏好匹配）实验评估，`R3`表现更优，减少幻觉和推理错误。

![](img/5da36266615e0bcd8b0ecff7df7fb997.png)

## 面向知识增强零样本问答的以证据为中心的事实摘要 (Evidence-Focused Fact Summarization for Knowledge-Augmented Zero-Shot Question Answering)

`https://aclanthology.org/2024.emnlp-main.594.pdf`([10])

近期研究利用知识图谱（KGs）提升大型语言模型（LLMs）的问答（QA）性能，但结构化的KG表述仍具挑战性。现有方法存在诸如因实体或关系重复导致证据密度降低、无法强调关键证据使证据清晰度下降等问题。为解决这些问题，提出`EFSum`（以证据为中心的事实总结框架）用于增强知识增强型LLMs的QA，通过蒸馏和偏好对齐优化开源LLM作为事实总结器，实验表明`EFSum`提高了LLMs的零样本QA性能且能确保总结的有效性和准确性。

![](img/e6cdf5ba858ef51ee55bdb0f42a98cbe.png)

## 基于图的生成：同时将大型语言模型作为智能体和知识图谱处理不完整的知识图谱问答（`Generate-on-Graph`: Treat LLM as both Agent and KG for Incomplete Knowledge Graph Question Answering）

`https://aclanthology.org/2024.emnlp-main.1023.pdf`([11])

为解决大型语言模型（LLMs）知识不足和产生幻觉的问题，许多研究探索将LLMs与知识图谱（KGs）整合，但以往方法多是在完整知识图谱的常规知识图谱问答（KGQA）中评估。在此类情况下，LLMs主要是在KG内寻找答案实体，而非有效整合内部知识与外部知识源。实际上，KGs常不完整。为模拟现实场景并评估LLMs整合内外知识的能力，提出利用LLMs进行不完备知识图谱下的问答（`IKGQA`）并构建相关数据集。为处理`IKGQA`，提出无需训练的`Generate - on - Graph`（`GoG`）方法，其能在探索KGs时生成新的事实三元组，通过思考 - 搜索 - 生成框架推理，将LLM视作`IKGQA`中的代理和KG。两个数据集的实验结果表明`GoG`优于之前所有方法。

![](img/14fcbedb4f7d864a66890b8b4030c987.png)

## 以使用为中心的电商意图理解方法（A Usage-centric Take on Intent Understanding in E-Commerce）

`https://aclanthology.org/2024.emnlp-main.14.pdf`([12])

在电子商务中，识别和理解用户意图至关重要，但意图理解缺乏一致定义和准确基准。本文将预测性用户意图定义为“客户如何使用产品”，将其视为自然语言推理任务。指出电子商务意图知识图谱`FolkScope`存在类别僵化和属性模糊两个弱点，这些弱点限制了它将用户意图与具有最理想属性的产品紧密结合以及跨不同类别推荐有用产品的能力。还介绍了一个产品恢复基准（包含新评估框架和示例数据集），并在此基准上验证`FolkScope`的弱点，相关代码和数据集可通过指定链接（`https://github.com/stayones/Usage-Centric-Intent-Understanding`）获取。

![](img/c227e24252dba1f18d2ed6241a4aafc2.png)

## 上下文的重要性：利用图结构化知识上下文推动开放式回答生成的边界（Context Matters: Pushing the Boundaries of Open-Ended Answer Generation with Graph-Structured Knowledge Context）

`https://aclanthology.org/2024.emnlp-industry.23.pdf`([13])

在不断发展的人工智能领域，通过大型语言模型（LLMs）生成语境丰富且有意义的回答已成为关键。然而，研究人员逐渐意识到参数较少的LLMs在回答开放式问题时面临的挑战。为了解决这些难题，将先进策略与丰富的外部领域知识相结合并应用于LLMs，能够显著提升其性能。本文提出了一种新颖的框架，结合基于图的上下文检索和知识图谱增强方法，旨在提升LLMs的能力，特别是在面向领域特定的社区问答平台（如AskUbuntu、Unix和ServerFault）时的表现。我们对多种具有不同参数规模的LLMs进行实验，评估其在知识溯源和回答开放式问题时的事实准确性。我们的方法`GraphContextGen`在性能上持续优于主流的基于文本的检索系统，展现了其在更多应用场景中的鲁棒性和适应性。这一进步凸显了将语境丰富的数据检索与LLMs结合的重要性，为知识获取和生成提供了一种全新的AI系统方法。此外，我们的研究表明，由于语境丰富的数据检索，关键实体以及生成的答案能够与标准答案保持事实一致性。

![](img/b7d31390f00cfd5584be8705f0b159fd.png)

## `FactGenius`: 结合零样本提示和模糊关系挖掘提升基于知识图谱的事实验证（`FactGenius`: Combining Zero-Shot Prompting and Fuzzy Relation Mining to Improve Fact Verification with Knowledge Graphs）

`https://aclanthology.org/2024.fever-1.30.pdf`([14])

事实核查是一项重要的自然语言处理任务，传统方法受数据管理和基于规则的方式所限。本文提出`FactGenius`方法，它将大型语言模型的零样本提示与知识图谱上的模糊文本匹配相结合来加强事实核查，利用来自维基百科的`DBpedia`数据集，通过相似性度量优化大型语言模型生成的连接以确保准确性。在事实核查基准数据集`FactKG`上的评估表明，`FactGenius`显著优于现有基准，尤其在微调`RoBERTa`作为分类器时。过滤和验证连接的两阶段方法很关键，使`FactGenius`在各种推理类型中表现卓越，是强大事实核查的有潜力工具，代码和材料可通过相关网址（`https://github.com/SushantGautam/FactGenius`）获取。

![](img/b2ea75794f9286fee92f51cf0d750cb0.png)
