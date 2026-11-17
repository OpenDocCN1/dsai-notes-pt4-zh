
# 如果LLM大模型是巫师，那么代码就是魔杖

> 来源：`https://agijuejin.feishu.cn/docx/ZgRldVsNsoUBBaxZeyZcld39nEc`

> 作者：西红柿牛腩

> 原文：`https://zhuanlan.zhihu.com/p/675744120`

人工智能（AI）领域在未来几年将迎来一场革命性的变革。

[全世界为之瞩目](https://www.zhihu.com/search?q=GPT3.5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A675744120%7D) [的LLMs](https://www.zhihu.com/search?q=%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A675744120%7D)大模型，包括Llama2、GPT3.5 和GPT-4，它们的规模庞大，经过了正式语言、代码和自然语言的综合训练。代码是一种强大的媒介，起到了连接人类意图与机器执行的作用。它将抽象、逻辑一致、标准语法和模块化转化为可执行过程。

[个人认为本篇介绍](https://www.zhihu.com/search?q=%E8%BD%AF%E4%BB%B6%E5%B7%A5%E5%B7%A5%E7%A8%8B%E5%B8%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A675744120%7D)的综述是具有跨时代意义的。一般关于代码的大模型的综述，要么关注评估和比较代码大模型的生成能力（除了[软件工程师](https://www.zhihu.com/search?q=%E6%96%B9%E6%B3%95%E8%AE%BA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A675744120%7D)，很少人关注这个领域）；或者列出关于智能体Intelligent Agents(IA)的论文，来自于伊利诺大学这篇综述主要讲代码如何辅助LLM大模型，以及代码在哪些方面可以给智能体带来收益。值得研究Agent以及跨学科方法论值得借鉴。

![](img/1df5904862b30da5e64b4208b4eed34b.png)

作为一个代码大模型的从业者，都没意识到代码对大模型的质变，新技术新方法总可以给个人和组织带来新的机会和亮点，这就是科技和AI的魅力。其实这也给代码生成大模型带来思路，Agent和Code能力是相互促进的。{\color{red}{作为一个代码大模型的从业者，都没意识到代码对大模型的质变，新技术新方法总可以给个人和组织带来新的机会和亮点，这就是科技和AI的魅力。其实这也给代码生成大模型带来思路，Agent和Code能力是相互促进的。}}

## 代码的定义

代码是既可由机器执行又可为人类解读的形式化语言。例如，人类可阅读的编程语言来论文讨论范围内，而由于缺乏人类可解释性，低级语言（如基于二进制指令的机器语言）被排除在外。

作为人类与计算机之间的媒介，代码将高级目标转化为可执行的步骤，具有语法标准化、逻辑一致性、抽象和模块化等特点。代码整合到LLMs训练数据中，除了增强LLMs在代码生成方面的能力外，还具备一下独特优势：

1. 将代码添加到LLM训练中，可以通过扩展它们的推理能力，使这些模型接受更广泛的具有挑战性的自然语言任务的训练。

2. 接受代码训练的LLMs可以生成精确且有组织的中间阶段。通过 [函数调用](https://www.zhihu.com/search?q=%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A675744120%7D)，这些阶段可以顺利地与外部执行目标相连接，展示出更好的连贯性和组织性。

3. 通过集成代码，LLMs可以从代码编译和执行环境中受益，该环境提供了多种反馈渠道以增强模型。

此外，代码可以赋能LLM大模型，在需要理解指令、分解目标、计划和执行动作以及从反馈中改进的情境中，成为智能代理（intelligent agents IAs）。最后，论文提出了几个关键挑战和赋予LLMs代码能力的未来方向。

## 代码相关的下游任务

虽然传统的LLM擅长处理常规自然语言任务，如文档分类和问答，但通过人类可解释和机器可执行的代码对LLM进行进一步的预训练或微调，就像为巫师配备增加魔力的魔杖一样，起到了额外的增强作用。这通过复杂编织的操作步骤显著提高了它们作为IA的性能。

![](img/ec22ab64419f6e085ff1e0b3b9da103f.png)

基于代码训练的大模型，首先增强代码能力

提升大模型的编程和代码执行能力：通过代码预训练，LLM能够编写代码、评估代码，并利用协作编码来解决复杂任务。

提升大模型的推理能力：代码的逐步性质有利于结构化的CoT思维链能力。LLM可以利用代码来帮助实现某些能力，如数学推理。

增强大模型结构化知识：代码可用于表达复杂结构，某些编程语言特性（如逻辑表达式和类继承）将特别有用。

### 链接大模型和功能终端

![](img/749cdd8a18b04997369235729318cf0e.png)

代码为中心的工具调用范式充当了LLM与大量功能终端之间的统一接口，从而实现了许多跨模态和跨领域任务。代码比自然语言更明确、更清晰，因此可以更好地表达连接到任何其他功能终端的明确指令。

### 代码大模型和环境的交互

![](img/6c469119c1cdd70176d45c0a4540dac3.png)

LLM可以嵌入到代码执行环境中，在那里它们收集真实、自动和可定制的反馈以实现自我改进。代码执行结果可以作为反馈来进一步微调LLM，从而使它们的性能更加理想。

### [改善智能](https://www.zhihu.com/search?q=%E8%87%AA%E6%88%91%E6%8F%90%E5%8D%87&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A675744120%7D)体决策制定、行为执行和自我提升

1. 代码预训练增强了代理对结构性知识的感知能力，代码的逐步特性改善了CoT思维链的规划，逻辑表达式和嵌套有助于更好地控制流程。代码的机器可执行有助于解决数学任务。

2. 明确的函数调用有助于实现动作基础。

3. 代理代码执行结果反映了潜在的环境变化，并可用于自我提升。

`https:// arxiv.org/abs/2401.0081 2`
