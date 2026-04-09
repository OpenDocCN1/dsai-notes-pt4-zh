# 2312.16171v2_优化大型语言模型的提问和提示（Prompting）设计

> 来源：[https://aithinkup.feishu.cn/docx/IyJ3dMgBxocNnLx1McycQlr3nJg](https://aithinkup.feishu.cn/docx/IyJ3dMgBxocNnLx1McycQlr3nJg)

> 文档AI含量：100%。没有读过计算机科学的论文，使用AI工具的一次试验。欢迎大家给意见，我来改进。

源文件下载地址：

https://sat2tjmapq6.feishu.cn/record/RJLcrjkX2eH3ZDci64ucAUjTnUb

## **基础文档分析：**

1.  📄 文件名: 2312.16171v2.pdf

1.  📚 主题: 优化大型语言模型的提问和提示（Prompting）设计

1.  💎 关键词: LLaMA, GPT-3.5/4, 提示工程（Prompt Engineering）, 指导原则（Guiding Principles）, 大型语言模型（Large Language Models, LLMs）

1.  🔍 内容摘要:

该论文介绍了26条指导原则，旨在简化查询和提示大型语言模型（LLMs）的过程。研究目标是简化为不同规模的大型语言模型制定问题的概念，检查它们的能力，并增强用户对不同规模的大型语言模型在不同提示下的行为的理解。

通过在LLaMA-1/2 (7B, 13B, 70B)和GPT-3.5/4上进行广泛的实验，验证了这些原则在指令和提示设计上的有效性。希望这项工作能为研究大型语言模型提示的研究者提供更好的指导。

1.  🗂️ 文章大纲:

+   引言

+   大型语言模型的发展

+   提示工程的相关研究

+   26条指导原则的详细说明

+   实验设置和实施细节

+   模型和评估指标

+   实验结果

+   结论

+   局限性和讨论

+   参考文献

1.  📍 重要段落:

+   "Prompt engineering is the art of communicating with a generative large language model."

+   "Our goal is to simplify the underlying concepts of formulating questions for various scales of large language models, examining their abilities, and enhancing user comprehension on the behaviors of different scales of large language models when feeding into different prompts."

+   "Extensive experiments are conducted on LLaMA-1/2 (7B, 13B and 70B), GPT-3.5/4 to verify the effectiveness of the proposed principles on instructions and prompts design."

+   "The specialized prompts we introduced have enhanced both the quality and accuracy of the LLM responses by an average of 57.7% and 36.4%, respectively, when applied to GPT-4."

1.  📃 参考文献:

+   [1] Simran Arora et al. "Ask me anything: A simple strategy for prompting language models," 2022.

+   [2] Tom Brown et al. "Language models are few-shot learners," 2020.

+   [3] Jacob Devlin et al. "BERT: pre-training of deep bidirectional transformers for language understanding," 2018.

+   ...（其他参考文献按照文中列出的顺序排列）

1.  📅 文件日期: 18 Jan 2024

## **内容理解与分析：**

📜 历**史和背景:**

+   大型语言模型（LLMs）的发展是自然语言处理（NLP）领域的一个关键进展，从BERT的双向训练方法到GPT系列模型的不断扩展和优化。

+   提示工程作为与LLMs互动的一个独特方面，已经发展成为一个复杂的研究领域，涉及用户输入和LLMs响应之间复杂的关系。

+   本研究建立在之前关于如何通过精心设计的提示来显著影响语言模型性能的研究基础之上，并进一步探索了提示的语义和上下文细微差别。

+   研究背景还包括了对LLMs在不同规模上的性能和效率的考量，以及如何通过优化提示来充分利用预训练模型的能力。

🚫 限**制与假设:**

+   **限制**: **提**出的26条原则虽然在实验中显示出提升LLMs性能的潜力，但它们在处理极为复杂或高度专业化的问题时的有效性可能会降低。这主要取决于每个模型的推理能力和训练情况。此外，评估改进和准确性的百分比是基于有限数量的问题选择，未来研究扩大问题集可能会得到更普遍的发现。

+   **假设**: 研究假设这些原则普遍适用于不同的LLMs，但实际效果可能因模型架构的不同而异。研究还假设通过优化提示可以显著提高模型的输出质量，而不是通过改变模型的内部结构或进行微调。

+   **评估限制**: 人类评估者用于衡量回答质量的提升，但不同的评估者可能会有不同的评估标准，这可能会影响结果的一致性。

+   **模型多样性**: 尽管在多种规模的模型上进行了测试，但研究可能没有涵盖所有类型的LLM架构，这可能限制了原则的普遍适用性。

+   **数据集限制**: 实验使用的是手动设计的ATLAS基准测试，可能存在选择偏差，未来的研究可以在更广泛的数据集上进行测试。

+   **实施复杂性**: 将这些原则实际应用于LLMs的操作中可能会遇到技术和资源的限制。

+   **长期影响**: 研究没有深入探讨长期应用这些原则对模型性能和用户交互的影响。

+   **伦理和社会影响**: 虽然提到了伦理考虑，但研究可能没有全面评估这些原则在不同文化和社会背景下的伦理和社会影响。

🧐 结**果解释:**

1.  **提升效果**: 实验结果显示，应用提出的指导原则后，大型语言模型（LLMs）在回答质量上平均提升了57.7%，在准确性上提升了36.4%。这表明原则在改善模型响应方面是有效的。

1.  **模型规模影响**: 研究观察到，随着模型规模的增大，原则带来的性能提升更为显著。例如，从LLaMA-2-7B到GPT-4，性能增益超过了20%，说明大型模型在利用这些原则时能够获得更大的改进。

1.  **原则有效性**: 某些原则如原则14在所有应用的问题上都显示出了改进效果，而其他原则则在特定情况下更为有效。这表明不同原则可能适用于不同类型的问题或任务。

1.  **准确性评估**: 在绝对准确性方面，模型在应用原则后的平均准确率在20%到40%之间，大型模型的准确率可以超过40%。这说明原则有助于提高模型输出的准确性。

1.  **相对准确性提升**: 研究还发现，应用原则后，模型的相对准确性平均提升了超过10%，对于更大型的模型，这一提升可以超过20%。

1.  **具体示例**: 文档中提供了一些具体的例子，如解释气候变化、数学问题的解答等，展示了原则应用前后模型回答的对比，进一步证实了原则的有效性。

1.  **人类评估**: 所有的评估都是通过人类评估完成的，这可能带来主观性，但提供了对模型输出质量的直观理解。

1.  **未来方向**: 结果鼓励未来的研究进一步探索这些原则的应用，并考虑将它们集成到LLMs的标准操作中。

🛑 结**论和建议:**

+   **本文提出的指导原则为研究人员在设计和**优化LLMs的提示时提供了有价值的参考。

+   研究建议未来的工作可以进一步探索这些原则在不同任务和领域中的应用。

+   建议可以通过微调、强化学习、直接偏好优化或使用生成的数据集进行不同的提示方法来进一步改进模型。

+   研究还指出，这些原则可以集成到标准LLM操作中，例如通过微调原始/原则性提示作为输入，以及经过润色的原则性响应作为训练目标。