
# HIT-SCIR发布首个中文扩词表增量预训练混合专家模型Chinese-Mixtral-8x7B

> 来源：[https://agijuejin.feishu.cn/docx/AL9Bd59JvoXK6CxBro9ctIc0nFe](https://agijuejin.feishu.cn/docx/AL9Bd59JvoXK6CxBro9ctIc0nFe)

> 哈工大SCIR｜[阅读原文](https://mp.weixin.qq.com/s/hpoLYcJzPL_ujyIa67ZPtA)

> 转载请联系原作者取得授权

![](img/290acfb25c124bd53a16be3149fec74b.png)

## 一、介绍

大规模语言模型（ LLM ）在通用自然语言处理领域已取得了巨大成功。在各种应用场景下，这种技术均展示了强大的潜力，学术界和工业界的兴趣也持续升温。通常，在充足的计算资源、数据集以及参数量的支持下，进行大规模训练是获取性能强大模型的有效途径，因此性能强大的模型往往具有相当庞大的参数量。然而仅仅扩大参数量将降低模型的推理速度，这不利于将模型应用于实际场景。混合专家模型（ MoE ）由于其稀疏激活的特性，在每次推理时仅激活部分参数，从而在可接受的推理速度和计算效率下显著扩大模型规模，进而提高模型性能。

近期， Mistral AI 发布了首个开源 MoE 大模型： Mixtral-8x7B ，其比肩 LLaMA2-70B 和 GPT-3.5 的强大性能引起了社区的广泛关注。由于 Mixtral-8x7B 词表不支持中文，因此对中文的编解码效率较低，限制了中文场景下的实用性。哈工大社会计算与信息检索研究中心（ HIT-SCIR ）基于 Mixtral-8x7B 进行了中文扩词表增量预训练，希望进一步促进中文自然语言处理社区对 MoE 模型的研究。我们扩充后的词表显著提高了模型对中文的编解码效率，并通过大规模开源语料对扩词表模型进行增量预训练，使模型具备了强大的中文生成和理解能力。

我们的模型发布于：`https://github.com/HIT-SCIR/Chinese-Mixtral-8x7B` 。

## 二、模型性能

针对大模型综合能力评价，我们分别使用以下评测数据集对 Chinese-Mixtral-8x7B 进行评测：

• C-Eval [1] ：一个全面的中文基础模型评估套件。它包含了 13948 个多项选择题，涵盖了 52 个不同的学科和四个难度级别。

• CMMLU [2] ：一个综合性的中文评估基准，专门用于评估语言模型在中文语境下的知识和推理能力，涵盖了从基础学科到高级专业水平的 67 个主题。

• MMLU [3] ：一个包含 57 个多选任务的英文评测数据集，涵盖了初等数学、美国历史、计算机科学、法律等，难度覆盖高中水平到专家水平，是目前主流的 LLM 评测数据集之一。

• HellaSwag [4] ：一个极具挑战的英文 NLI 评测数据集，每一个问题都需要对上下文进行深入理解，而不能基于常识进行回答。

根据 Mistral 发布的技术报告 [5] ， Mixtral-8x7B 在推理时将激活 13B 参数。下表为 Chinese-Mixtral-8x7B 与其他 13B 规模的中文扩词表模型在各个评测数据集上的 5-shot 结果：

![](img/55ad912ec85609d5d9f5c67d9a84c5ca.png)

表1： 各个中文扩词表模型在评测数据集上的性能

在中文知识和理解方面，我们的 Chinese-Mixtral-8x7B 与 TigerBot-13B-Base-v3 性能相当。由于 Chinese-Mixtral-8x7B 的训练数据量仅为 TigerBot-13B-Base-v3 的 8% ，我们的模型仍有进一步提升的空间。与此同时，得益于原版 Mixtral-8x7B 模型强大的性能，我们的 Chinese-Mixtral-8x7B 达到了各个扩词表模型的最强英文水平。

下表为各个扩词表模型的生成效果。由于部分模型的预训练语料未使用 `eos_token` 进行分隔，我们采用了 `max_tokens = 100` 对生成文本进行截断。我们的采样参数为 `temperature = 0.8, top_p = 0.9` 。

![](img/9067ff843bee025a1d35ebc430b6e8cd.png)

表2： 各个中文扩词表模型的生成效果

针对中文编解码效率，我们使用各个扩词表模型的分词器对 SkyPile 数据集的一个切片（ `2023-06_zh_head_0000.jsonl` ）进行编码，对比了各个分词器输出的中文文本 Token 量：

![](img/0ece396dcc1c404325a7b14bf460f600.png)

表3： 各个中文扩词表模型及原模型对中文的编解 码效率比较

在约 1.4GB 的测试文本中，我们的 Chinese-Mixtral-8x7B 中文编解码效率仅次于 TigerBot-13B-Base-v3 ，较原模型提高了 41.5% 。这有利于加速中文文本的推理速度，并在 In-Context Learning 、 Chain-of-Thought 等场景中节省序列长度，有利于提高复杂推理任务的性能 [6] 。

## 三、词表扩充

我们使用 `sentencepiece` 在 12G 知乎数据和 2G 悟道数据上训练中文 BPE 词表。我们在训练词表时分别枚举了中文单字 Token 数量以及中文总 Token 数量，并对二者进行组合，得到了数百个大小、内容各异的词表。为了得到最适合的词表，我们通过 Zheng Bo 等人提出的 ALP [7] 计算这些词表的中文词汇能力。 ALP 通过计算特定语言的子词切分粒度，并对词表的中低频子词进行惩罚，是一种方便快捷的衡量特定语言词汇能力的指标。

我们在书籍和百科语料上评估了不同词表的 ALP 值。图示中，四条曲线分别代表四种中文单字 Token 数量的词表（ 4451 、 5435 、 6414 和 7434 ）。为了避免词表过小导致中文压缩率过低，以及词表过大导致 `embedding` 层过于稀疏，我们选取 ALP 曲线的拐点，对应向词表中新增 25000 个中文 Token 。在此基础上，我们选择了四条曲线中 ALP 最大者，即新增 6414 个中文单字 Token 的词表，作为最终 Chinese-Mixtral-8x7B 选用的词表。

![](img/65dd67e354004a349abd1f96e8a15d70.png)

图1： 书籍语料库上各词表的 ALP 值

![](img/1a3df5ad80aea2e17696ee0a4ee32f37.png)

图2：百科语料库上各词表的 ALP 值

在获得新词表后，我们需要对 `embedding` 和 `lm_head` 层进行扩充和初始化。我们使用新 Token 在旧 `embedding` 层中的词嵌入平均值对扩充部分进行初始化。在我们的前期实验中，这种方法略优于 HuggingFace 的默认实现，即使用固定的正态分布进行初始化。

## 四、训练策略与训练数据

Mixtral-8x7B 模型参数量为 46.7B ，全参数训练需要同时使用多种并行策略，在训练资源受限的情况下时间成本过高。因此我们采用 HuggingFace 官方推荐的方法 [8] ，使用 `QLoRA`[9] 对模型进行训练。 `QLoRA` 在 `LoRA` 低秩分解的基础上，通过引入 4 位量化、双重量化和利用 NVIDIA 统一内存进行分页，进一步减少了训练所需显存，同时保持了与全参数训练相当的性能。

我们参考 Yiming Cui 等人对 `LoRA` 的设置 [10] ，对原模型所有 `Linear` 层应用低秩分解，并将扩增后的 `embedding` 和 `lm_head` 层的参数设置为可训练。对于模型主体，我们采用 `NF4` 格式进行量化，这种格式可以使得量化后的数据与量化前具有同等的数据分布，模型的权重信息损失更少。

我们基于现有的开源数据集训练了 Chinese-Mixtral-8x7B ，数据集包括：

![](img/19354d53e68978901930f79b65e1c0d0.png)

表4： Chinese-Mixtral-8x7B 增量训练数据集

## 五、结语

Chinese-Mixtral-8x7B 是哈工大社会计算与信息检索研究中心（ HIT-SCIR ）在自然语言处理领域的最新进展。本项目旨在助力中文自然语言处理社区深入了解和研究混合专家模型。请注意， Chinese-Mixtral-8x7B 仍然可能生成包含事实性错误的误导性回复或包含偏见 / 歧视的有害内容，请谨慎鉴别和使用生成的内容，请勿将生成的有害内容传播至互联网。最后，诚邀您访问我们的 GitHub 项目页面，并共同探讨中文自然语言处理的未来发展。

如果您觉得本项目对您的研究有所帮助或使用了本项目的代码，请引用本项目：

```
@misc{Chinese-Mixtral-8x7B, author = {HIT-SCIR}, title = {Chinese-Mixtral-8x7B: An Open-Source Mixture-of-Experts LLM}, year = {2024}, publisher = {GitHub}, journal = {GitHub repository}, howpublished = {\url{https://github.com/HIT-SCIR/Chinese-Mixtral-8x7B}}}
```

### 参考文献

[1] Huang Y, Bai Y, Zhu Z, et al. C-eval: A multi-level multi-discipline chinese evaluation suite for foundation models[J]. arXiv preprint arXiv:2305.08322, 2023.

[2] Li H, Zhang Y, Koto F, et al. CMMLU: Measuring massive multitask language understanding in Chinese[J]. arXiv preprint arXiv:2306.09212, 2023.

[3] Hendrycks D, Burns C, Basart S, et al. Measuring massive multitask language understanding[J]. arXiv preprint arXiv:2009.03300, 2020.

[4] Zellers R, Holtzman A, Bisk Y, et al. Hellaswag: Can a machine really finish your sentence?[J]. arXiv preprint arXiv:1905.07830, 2019.

[5] Jiang A Q, Sablayrolles A, Roux A, et al. Mixtral of Experts[J]. arXiv preprint arXiv:2401.04088, 2024.

[6] Jin M, Yu Q, Zhao H, et al. The Impact of Reasoning Step Length on Large Language Models[J]. arXiv preprint arXiv:2401.04925, 2024.

[7] Zheng B, Dong L, Huang S, et al. Allocating large vocabulary capacity for cross-lingual language model pre-training[J]. arXiv preprint arXiv:2109.07306, 2021.

[8] HuggingFace. Welcome Mixtral - a SOTA Mixture of Experts on Hugging Face[EB/OL]. [2024-01-16]. https://huggingface.co/blog/mixtral.

[9] Dettmers T, Pagnoni A, Holtzman A, et al. Qlora: Efficient finetuning of quantized llms[J]. arXiv preprint arXiv:2305.14314, 2023.

[10] Cui Y, Yang Z, Yao X. Efficient and effective text encoding for chinese llama and alpaca[J]. arXiv preprint arXiv:2304.08177, 2023.

本期责任编辑：车万翔

本期编辑：孙洲浩
