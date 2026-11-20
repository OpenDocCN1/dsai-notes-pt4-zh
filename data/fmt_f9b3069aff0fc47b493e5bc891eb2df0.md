
# ChatGPT Papers

> 来源：[https://learning.feishu.cn/docx/BTRYdaNlBotNpSxbz3BcEDFsnnc](https://learning.feishu.cn/docx/BTRYdaNlBotNpSxbz3BcEDFsnnc)

| Title | year | Author | Notes |
| --- | --- | --- | --- |
| Coding |  |  |  |
| Evaluating Large Language Models Trained on Code | 2021.07 | Wojciech Zaremba | 把 GPT 在 code 上训练了一下，模型没有创新，但解决了新的问题。参考性主要是怎么处理数据。 |
| Competition-Level Code Generation with AlphaCode | 2022.02 | Yujia Li | 数据集上，进一步 finetune 了数据本身。 |
| ChatGPT |  |  |  |
| Training language models to follow instructions with human feedback | 2022.03 | Long Ouyang | 20 页正文，48 页附录 |
|  |  |  |  |
| TODO |  |  |  |
| Visual ChatGPT: Talking, Drawing and Editing with Visual Foundation Models | 2023.03 | Chenfei Wu | Code https://github.com/microsoft/visual-chatgpt |
|  |  |  |  |
| Multimodal Chain-of-Thought Reasoning in Language Models | 2023.02 | Zhuosheng Zhang |  |
| Sparks of Articial General Intelligence: Early experiments with GPT-4 | 2023-03 |  |  |

## GPT3 的相关文章 & 脉络

gpt-2 和 gpt-3 的引用，都不算高。

![](img/2140b5041f524fee66e2390e4de650df.png)

GPT 1-3 系列，模型输入是一句话，输出也是一句话。这就是对话模式。输入也叫 `prompt`。

https://mp.weixin.qq.com/s/mYhSFtZZMRv5dDMnYcruJg

AI中经常遇到的挑战之一，就是建立能够进行多步骤推理的系统，将复杂的问题分解成较小的任务，并结合这些任务的解决方案，解决更大的问题。谷歌最近在思维链提示方面的工作，就鼓励模型在解决新问题时「展示工作」，这样就能帮助语言模型遵循逻辑思维链，并产生更有条理、有组织和准确的响应。就像四年级的数学老师会鼓励学生展示解决问题的步骤，而不是仅仅写下答案一样，这种方法不仅使解决问题的方法更具有可解释性，而且对于需要多个推理步骤的复杂问题，也更有可能找到正确的答案。

![](img/14f887a073b2aae53118308a480e1e5f.png)

谷歌提出了Minerva模型，它以通用的PaLM语言模型为基础，在来自arXiv的大量数学文档和论文的语料库中对其进行微调，然后使用思维链提示和自洽解码。在各自数学推理和科学问题的基准套件上，Minerva都展示出了SOTA。

![](img/660e21b048ec0cb85fd4c739969fa5ab.png)

思维链提示（chain of thought prompting）是一种向模型更好地表达自然语言提示和示例的方法，能够显著提高模型处理新任务的能力。

类似的提示微调（`prompt tuning`），即在问题领域特定文本的语料库上对大型语言模型进行微调，也显示出了巨大的前景。
