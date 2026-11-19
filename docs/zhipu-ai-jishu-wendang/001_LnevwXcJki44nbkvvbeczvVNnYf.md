
# 1.1 模型 · GLM 系列开源模型

> 来源：[https://ai.feishu.cn/docx/GZpydBc2UoUAU4x1lzhcP51enQe](https://ai.feishu.cn/docx/GZpydBc2UoUAU4x1lzhcP51enQe)

## 简介

GLM开源系列模型从`ChatGLM3`不断迭代，在模型结构与技术社区上不断采纳优秀案例，目前最新版本`GLM4`，可以完成诸多文档解析任务，我们在模型能力做了诸多探索，目前存在三个方向上的能力，在长文本模型上我们可以输出长度超过 1 万字的文本，非常适合写作，特别是论文、公文等。在视觉模型上我们具备 `1120 * 1120` 高分辨率下的中英双语多轮对话能力。在基座`9B`的开源模型上我们拥有语义、数学、推理、代码和知识等多方面表现出超越 `Llama-3-8B` 的卓越性能，同时基于此基座模型，你可以轻松构建更加高级的功能如网页浏览、代码执行、自定义工具调用（`Function Call`）等

我们整理了全面详细的技术报告、入门指南、众多解决方案，除了这些，你可以在我们生态社区中发表自己的见解，我们将持续不断地接纳来自众多开发者优秀的建议

## 里程碑

我们提出了通用语言模型（`GLM`）架构，该架构具有`autoregressive blank infilling objective`，通过`GLM-10B`模型到年底预训练 `GLM-130B`，经过一年的努力，我们完成了指令对齐的（`SFT` 和 `RLHF` 对齐）模型 `ChatGLM-130B`

为了吸收更加多的关注，我们持续开源了`ChatGLM`系列、`CodeGeeX`系列、`CharacterGLM`系列，在不断迭代中我们收集了更多的反馈，`ChatGLM2-6B` 于 6 月发布，经过预训练并与更高质量的数据保持一致，在这个版本中我们采用了`FlashAttention`技术，其上下文长度可以扩展到`32K`。更进一步，我们的第二代代码模型`CodeGeeX2-6B`是通过在另外 6000 亿个代码令牌上进行预训练而开发的。`CharacterGLM`通过`LLMs`特殊字符自定义在角色扮演和心理分析方向允许使用者有效且安全地进行交互。

多样化的训练数据集、更充足的训练步骤和更优化的训练策略。从这里开始，`GLM`为往更加工程化的目标进发，我们推出了开放平台、智谱清言等多个线上业务

在吸取了所有教训和经验之后，我们开始了`GLM-4`的训练。然后，在基础`checkpoint`我们通过一些多阶段的对齐训练过程（例如，`SFT`、`RLHF`、`安全校准`），目前形成了在两个不同的开发版本：`GLM-4` 和 `GLM-4 All Tools`，都支持 `128K` 上下文长度。我们将持续输出：”让机器像人一样思考。“的价值

## 最新动态

+ `LongCite` 让大模型精准找到 `{引用}`

+ 在长上下文的模型（例如 `GLM-4-9B`）的基础上，让大模型在回复中加入引用信息，提出了 `CoF`（`Coarse to Fine`）的方法，分别得到 `LongCite-9B` 和 `LongCite-8B`。

+ `LongWriter`长文本模型

+ 在长上下文的模型（例如 `GLM-4-9B`）的基础上，我们用 `6000+` 条输出长度在 `2k-32k` 之间的数据进行 `SFT` 微调以及 `DPO`。

+ `GLM模型框架`

+ `2024/8/12`: `GLM-4-9B-Chat` 模型依赖的`transformers`版本升级到 `4.44.0`以及以上版本, 请更新模型配置文件并参考 `basic_demo/requirements.txt` 更新依赖。

> 如果该版本`transformers`微调出现
>
> `AttributeError: 'NoneType' object has no attribute 'to'`
>
> 报错，需修改 `transformers` 源文件 `tokenization_utils_base.py` 对应代码为:
>
```
self.data = {k: v.to(device=device) for k, v in self.data.items() if v is not None}
```
>
> 该bug已经在 `transformers` 库最新源代码修复，可以通过源代码安装 `transformers` 库(`4.43.0dev`)或者修改上述代码解决问题。推理不受影响

+ `GLM-4-9B`开源动态

+ `2024/08/12`: `GLM-4-9B-Chat` 模型依赖的`transformers`版本升级到 `4.44.0`，请重新拉取除模型权重（`*.safetensor` 文件 和 `tokenizer.model`）外的文件并参考 `basic_demo/requirements.txt` 严格更新依赖。

+ `2024/07/24` [我们](https://medium.com/@ChatGLM/glm-long-scaling-pre-trained-model-contexts-to-millions-caa3c48dea85) 发布了与长文本相关的最新技术解读，关注 这里 查看我们在训练 `GLM-4-9B` 开源模型中关于长文本技术的技术报告。

+ `2024/7/16`: `GLM-4-9B-Chat` 模型依赖的`transformers`版本升级到 `4.42.4`, 请更新模型配置文件并参考 `basic_demo/requirements.txt` 更新依赖。

+ `2024/7/1`: 我们更新了 `GLM-4V-9B` 的微调，您需要更新我们的模型仓库的运行文件和配置文件， 以支持这个功能，更多微调细节（例如数据集格式，显存要求）。

+ `2024/6/28`: 我们与英特尔技术团队合作，改进了 `GLM-4-9B-Chat` 的 `ITREX` 和 `OpenVINO` 部署教程。您可以使用英特尔 `CPU/GPU` 设备高效部署 `GLM-4-9B` 开源模型。

<https://zhipu-ai.feishu.cn/sync/I447dldoQsFFP6bcGUYcAaOVnQc>
