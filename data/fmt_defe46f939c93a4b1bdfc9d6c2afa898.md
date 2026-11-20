
# 2024-01-08使用微调定制属于自己的大模型 - 掘金

> 来源：[https://xuqiwei1986.feishu.cn/docx/T7xvdfXjyo4P8ExsC3yc4Dbonsf](https://xuqiwei1986.feishu.cn/docx/T7xvdfXjyo4P8ExsC3yc4Dbonsf)

> 🔗 原文链接：h[ttps://juejin.cn/p](https://juejin.cn/post/7321558573518241818)ost/732155857351...

## 背景

我们日常所认识的「大模型」都是经过 微调 之后得到的。大模型只是一个「文本续写的模型」。在未经过任何微调或者基于人类反馈的强化学习（RLHF）之前，模型只能实现基本的文本接龙，并没有直接理解用户任何指令或意图的能力。

下面是开源大模型零一万物 Yi-34b 未经过微调之前，模型的输出：

![](img/37d2f8d1d0e3b0b171af569f7579e783.png)

上面的输出其实从语法结构什么来说都是对的，但是它的输出并不是「对话」，而是像继续编写某个文本里的片段。

经过 Chat 数据微调训练之后，模型才学会如何和人类进行正常交流：

![](img/20c448521b0e445dcc6039a00de7ea7f.png)

用来微调的 Chat 对话模型使用的训练数据为下图所示，由符合预期的「一问一答」数据组成：

![](img/d01bedae0379009d545bfd59f42e68df.png)

社区上的大模型，无论是 GPT 还是开源大模型，基本都遵循先进行 预训练 然后 微调 ，得到一个对话或者其他用途的模型。

![](img/eec017225aaf59747737afc7872e3574.png)

## 什么是预训练？

在预训练阶段，模型通过学习大量的数据来提取特征、理解语义和推理能力，从而对通识知识，代码知识获得一般性认识。质量较高的预训练数据可以增强模型的泛化能力，减少在后续特定任务上的训练调优时间和资源消耗。

+ 预训练是一种无监督学习方式，是指使用随机文本片段在大规模数据集上对神经网络模型进行初始训练，以便模型能够学习广泛的特征和知识。

+ 预训练使用的训练数据格式，「只有输出」，没有输入的标签。大模型使用这种大量没有标记的数据来自己学习训练数据中数据的规律（中英文语法，代码语法，通识知识等）

## 预训练适合什么场景？

+ 从 0 开始训练一个新的通用大模型。

+ 基座大模型 LLaMA2 中文数据非常少，只占 0.13%，有必要使用更多比例中文数据 继续预训练 使大模型学会中文表达。

+ 基座大模型 LLaMA2 模型代码内容的占比同样也不高（4%），使用预训练添加更多比例的代码数据，强化大模型代码能力（Code LLaMA代码模型）

+ 出现了一门新的编程语言，为了让模型能够学到最新的编程语言结构，可以考虑预训练。

## 什么是微调？

微调使用 「一问一答」的格式，即有标注的训练数据，在基于原有模型参数上，来让模型更清楚地知道什么样的输入下他应该给予什么样的输出，按照训练数据里的模式进行学习。

大模型先通过预训练学习基础知识，再通过微调让模型学会 基于它通过预训练已经学会的知识，学习如何去根据用户指令输出正确适当的内容。

## 我能用微调定制什么？

+ 设置风格、语气、格式等定性方面：

+ **场景举例：创建一个语音对话机器人**，不通过提示词的方式，每次让模型输出的内容尽可能精简如 50 字以内。

+ 大模型智能 Code Review，通过打标过的优质数据使得大模型 Review 的结果输出更加有效和高质量。

+ 提高生成预期输出的可靠性：

+ 将天气查询的需求转换为 JSON 请求参数，通过大量示例作为微调训练数据提升输出 JSON 的信息抽取的效果，并降低输出非法 JSON 内容的概率。

+ 要求大模型只输出代码块，即使通过提示词告诉模型只能一次性输出代码块且不添加任何解释，但大模型偶尔也会输出解释和多个代码块。此时用一些少量微调数据可以改善这些问题。

+ 提升效果

+ 使用大模型生成 Pandas 数据分析和可视化代码。大模型本身理解 Pandas 代码的编写，但是编写准确率在一些场景下可能不是特别高，通过一系列经过打标正确的训练数据提升大模型理解用户需求编写 Pandas 代码的效果和正确率。

+ 比较复杂的，有大量任务说明的提示词。

+ 比如将用户的一段描述转换为一个甚至多个接口请求参数，需要在提示词里添加大量说明文档和样例。

+ 减少 Token 占用

+ 就像上面的例子，在提示词里添加大量文档说明，使用按 Token 计费的模型如 GPT4 会显得很贵。同时较少的 Token 能获得更快的推理速度。

## 可以使用微调来让大模型新增知识吗？

不推荐，在需要有可信依据的场景上，比如构建智能客服机器人，通常会使用 RAG 的方式配合向量搜索等方式从文档库搜寻与用户询问问题最为相关的内容，并将匹配到的文档段落作为知识内容添加到提示词中，大模型使用提示词中的知识来回答用户的问题。

微调改善的是大模型在某种模式下的表现（如风格，准确度，幻觉问题等）。虽然微调也能一定程度上记忆新增的知识，但由于微调是改变模型的参数结构，使得模型在生成新 token 时输出与训练数据模式更相似的内容。从输出准确度上来说是不如在提示词中添加的知识内容。

## 微调方式

在大模型预训练参数上进行参数微调训练，有三种方式：

+ 全参数微调，即完全监督微调，在所有参数层上根据标注数据去调整原始预训练模型中的 QKV 参数层。

+ LoRA，即 LLM 的低秩适配（Low-Rank Adaptation），通过两个较小的矩阵来拟合调整后的参数层，这个过程可以理解为 `X + Z = Y`，其中 `X` 为原始参数，`Y` 为训练之后的参数，训练过程中就是寻找可以将 `X` 拟合为 `Y` 的 `Z` 矩阵。`Z` 矩阵由 两个较小的 `Wa` 矩阵 和 `Wb` 矩阵组成。

![](img/57d9e8f79dc58fb6bce2223eacd8b15d.png)

+ QLoRA, 与 LoRA 方式类似，也是训练两个拟合参数层来达到对原始模型的调整。区别在于为了节省训练硬件资源，QLoRA 会先将原始模型参数 量化至 4-bit 并冻结，然后添加一小组可学习的低秩适配器权重（Low-rank Adapter weights），这些权重通过量化权重的反向传播梯度进行调优，在量化之后的参数上进行 LoRA 训练，这将大幅下降显存的占用（33b 的模型 以 FP16 全量加载需消耗 80GB 显存，量化至 4 bit之后模型加载仅需要 20 GB 左右显存的占用）

+ 除了量化并冻结原始参数，QLoRA 还支持分页优化器：使用NVIDIA统一内存特性，将部分显存溢出的部分 offload 到内存中实现分页，来进一步避免 OOM 的问题。（即下图紫色箭头部分）

![](img/c1209651e79201706c9bb1a16349d2fb.png)

关于全量参数微调 和 LoRA 方式效果的对比，以下以 SQL 生成场景举例，柱状图从深到浅依次是未经过微调训练、LoRA 方式训练和全参数训练后模型生成的准确度：

![](img/812268d7fd2ee8ac40f7e23613301d12.png)

在 SQL 数据集上，根据模型大小和微调方法预测准确率，LoRA微调模型的表现几乎可与全参数微调模型相当。需要注意的是，LoRA 微调的 13B 模型的表现略优于全参数微调的 7B 模型。

关于 QLoRA 训练的效果：

![](img/b0dc6377cb2cd1cf108feb3dd88af2df.png)

从上图中可以看到，在5-shot MMLU 场景下，QLoRA 效果与常规 LoRA 效果接近，甚至能反超常规 LoRA 微调效果。

关于资源消耗：

![](img/43fb18395b962cd1bd92ea2b5c7641d3.png)

在上表中：

+ LLaMA-13B+LoRA（2M）表示使用 LLaMA-13B作为基本模型和 LoRA 训练方法在 200 万指令数据上训练的模型。

+ 可以看到全量参数的训练时间约为 LoRA 方式的 2~3 倍。

+ 从效果上来看，LoRA 与全参数微调效果差距在1 ~ 9%左右。但是在特定场景上（math 数据集）仅相差 1% 左右，因此在垂直领域训练上 LoRA 能保持较好的效果，同时显著降低训练时长。

以 6b 参数量的模型分别以不同方式启动训练，显存占用情况：

| 训练方式 | Full 全参数微调 | LoRA | 4bit-QLoRa |

| --- | --- | --- | --- |

| 占用 | 68450 MB | 15226 MB | 8422 MB |

从上表中可以看到：

+ 全参数训练显存占用约为 LoRA 的 4.5倍

+ 使用 QLoRA 方式显存占用相对于 LoRA 又可以省一倍。

+ 6B 参数的微调，一张 24G 显存的显卡运行LoRA微调绰绰有余。

因此，结合训练效果看，LoRA 或者 QLoRA 的高效微调的方式成本较低效果相对也较好，因此以LoRA 的方式来对基座模型进行预训练是最为合适。

## 微调训练框架的选择

2023 年各家推出的大模型浩如烟海，如 GPT4、Llama、ChatGLM、Baichuan、RWKV、Stable-Diffusion等。每个模型的训练方法可能都有略微区别，且业界提出了众多高效微调的方法， 例如Adapter-Tuning、Prompt-Tuning、LoRA、QLoRA等。不同的基座模型，不同的训练方法需要不同的代码去适配，造成了较高的上手门槛。

ModelScope 魔搭是阿里推出的下一代开源的模型即服务共享平台， 魔搭 ModelScope 开源社区推出了一套完整的轻量级训练和推理工具 SWIFT (Scalable lightWeight Infrastructure for Fine-Tuning)，通过框架代码、 CLI 的封装，可以使得普通开发者以几行代码就能启动模型训练、推理。

![](img/c5808ed964a66a786596aeefddc2d2d7.png)

[ModelScope 魔搭社区](https://link.juejin.cn/?target=https%3A%2F%2Fmodelscope.cn%2Fhome)

SWIFT 启动训练代码，主要需要指定以下内容:

+ 基座模型id，用什么基座模型，使用多大参数的模型进行训练

+ 训练数据集

+ 训练评估数据集

+ 输入，输出长度

详细环境搭建，使用方法可参考: `github.com/modelscope/…`

本地微调开源大模型推荐有一张 24G 显存的显卡。如果没有的话，那么试试 GPT 官方的微调接口吧：`platform.openai.com/docs/guides…`

其他的还有可以调整的参数如训练模式，训练几个 `epoch`，`checkpoint` 记录步长，`lora rank` 等，不清楚的话可以先使用默认参数。

CLI 启动训练示例代码，最主要的部分就是设置`custom_train_dataset_path`和`custom_val_dataset_path`配置训练数据:

```
PYTHONPATH=../../.. \
CUDA_VISIBLE_DEVICES=0 \
python llm_sft.py \
    --model_id_or_path 01ai/Yi-6B \
    --model_revision master \
    --sft_type lora \
    --tuner_backend swift \
    --template_type default-generation \
    --dtype fp16 \
    --output_dir output \
    --train_dataset_sample -1 \
    --num_train_epochs 5 \
    --max_length 2048 \
    --max_new_tokens 2048 \
    --check_dataset_strategy warning \
    --lora_rank 8 \
    --lora_alpha 32 \
    --lora_dropout_p 0.05 \
    --lora_target_modules ALL \
    --gradient_checkpointing true \
    --batch_size 1 \
    --weight_decay 0.01 \
    --learning_rate 1e-4 \
    --gradient_accumulation_steps 16 \
    --max_grad_norm 0.5 \
    --warmup_ratio 0.03 \
    --eval_steps 100 \
    --save_steps 100 \
    --save_total_limit 2 \
    --logging_steps 10 \
    --quantization_bit 4 \
    --bnb_4bit_comp_dtype fp16 \
    --custom_train_dataset_path /root/train.jsonl \
    --custom_val_dataset_path /root/train_eval.jsonl
```

以下是魔搭有标注的训练数据格式，使用 `query` 和 `response` 两部分组成：

```
{&quot;query&quot;: &quot;11111&quot;, &quot;response&quot;: &quot;22222&quot;}
{&quot;query&quot;: &quot;aaaaa&quot;, &quot;response&quot;: &quot;bbbbb&quot;}
{&quot;query&quot;: &quot;AAAAA&quot;, &quot;response&quot;: &quot;BBBBB&quot;}
```

## 数据可视化场景的微调

数据分析中可视化是一个非常常见的场景。在各类可视化系统中，非常常见的逻辑就是先将数据源分类为一系列指标（或者叫度量）或者维度（类别）。通过指标和维度之间的组合加上适当的可视化类型，如折线图，柱图，饼图等，来生成可视化图表。

以 QuickBI 举例，如果需求为「展示每个产品类型的单价和利润情况」，那么需要从已知的维度列表中选择product_type，度量选择「利润金额」和「单价」，最后再选择柱图或者线图进行可视化。对于不那么熟悉指标维度概念的非专业数据分析人员，或者移动端场景来说，快速选择合适的指标和维度操作不是那么容易。

![](img/9d2bd8a545bba3813073cc7e0950f2eb.png)

而利用 ChatGPT 或同类大语言模型，可以实现理解用户数据分析的意图并结合已知的指标维度列表，抽取其中的指标维度信息，并将抽取生成的配置映射为真实的可视化展示，通过自然语言的方式大大降低了用户操作的门槛。

![](img/9330f450b6c26825d8dbd26e7cd57b88.png)

**设计以下的提示词：**

![](img/432f16d1fee856b7b77a9e406a709fa4.png)

上面提示词中，绿色部分为固定说明，橙色部分为根据已知可使用的可视化字段列表动态进行替换的地方，而蓝色部分则替换为为用户实际的输入问题。

来测试一下，将以上提示词直接带入某开源大模型，得到以下输出：

```
{
  &quot;chartType&quot;: &quot;CHART_LINE&quot;,
  &quot;chartFields&quot;: {
    &quot;dimensions&quot;: [&quot;时间&quot;],
    &quot;metrics&quot;: [&quot;利润率&quot;]
  },
  &quot;chartTimeFilter&quot;: {
    &quot;granularity&quot;: &quot;day&quot;,
    &quot;dayjsScript&quot;: [&quot;subtract&quot;, 6, &quot;months&quot;]
  },
  &quot;dimensionFilters&quot;: [
    {
      &quot;field&quot;: &quot;时间&quot;,
      &quot;values&quot;: [
        &quot;2022-01-01&quot;,
        &quot;2022-01-02&quot;,
        &quot;2022-01-03&quot;,
        ...
      ]
    }
  ],
  &quot;metricFilters&quot;: [],
  &quot;mainAxis&quot;: &quot;时间&quot;
}
```

提取字段基本没问题，但是有一些问题，首先我们要求将时间范围输出 `dayjsScript` 来通过 JS 运行解析获取准确的近半年时间，但是这里没有这么生成。其次 `dimensionFilters` 这里错误地列出所有近半年具体日期，且今年也不是2022年。上面这些错误的输出其实就是典型的模型幻觉问题。

这样的效果肯定是没有办法接入到我们的应用流程中的。要优化大模型输出，我们只需要给大模型一个 例子 即可：

![](img/ea751b3b0a747c2ab307d123363ffa86.png)

上面紫色框添加了一个示例，这样大模型可以基于已有描述和输出范例，更好地输出正确的内容。添加了示例之后的输出：
