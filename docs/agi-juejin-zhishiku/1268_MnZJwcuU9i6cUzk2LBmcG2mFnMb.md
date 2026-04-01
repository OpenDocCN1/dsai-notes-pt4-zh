# QLoRA：4-bit级别的量化+LoRA方法，用3090在DB-GPT上打造基于33B LLM的个人知识库

> 来源：[https://agijuejin.feishu.cn/docx/DUQ4dbkhKow6wsxYcsZchsrHnjd](https://agijuejin.feishu.cn/docx/DUQ4dbkhKow6wsxYcsZchsrHnjd)

> 作者：陆淳

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/634516004)hihu.com/p/634516...

## **一、背景**

大型语**言模型（LLM）的发展日新**月异，是近年来自然语言处理（NLP）领域的热门话题，LLM可以通过大规模的无监督预训练来学习丰富的语言知识，并通过微调来适应不同的下游任务，从而在各种NLP任务上取得了令人瞩目的性能。然而，LLM也带来了一些挑战，其中一个便是它们的 巨大规模和高昂的计算成本 。例如，微调LLaMA的65B模型需要超过780G的显存，在BLOOM-176B上进行推理，需要8个80GB的A100 gpu(每个约1.5万美元)。这远远超出了普通用户和研究者的可用资源。虽然最近出现的一些量化方法可以减少LLM的内存占用量，但是这些技术仅适用于推理，并不适合在训练过程中使用。因此，如何在保持或提高性能的同时，降低LLM的内存占用和训练时间，是一个急需解决的问题。

![](img/b0b0ab9ef456a95ee466435f3533abfa.png)

5月24日华盛顿大学在《 [QLORA: Efficient Finetunin](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.14314.pdf)g of **Quantized LLM**s [》这篇文章中提出了一种针](https://link.zhihu.com/?target=https%3A//huggingface.co/timdettmers)对LLM的低精度量化和高效微调技术，可以在保证完整的fp16的微调任务性能的同时，减少内存使用，从而能够 在单个48[GB显存的](https://zhida.zhihu.com/search?q=%E5%85%AC%E5%BC%80%E5%8F%91%E5%B8%83&zhida_source=entity&is_preview=1) GPU上微调65B参数模型 。作者 Tim Dettmers 在 huggingface 上已经公布了他们利用QLoRA方法训练的系列模型Guanaco，其中33b和65b的模型可以直接下载使用，7b和13b的模型需要和对应的LLaMA模型进行参数融合。同时作者表明Guanaco在Vicuna基准测试中的表现超过了所有以前 公开发布 的模型，在24 小时内微调的Guanaco 65B大模型甚至能够 达到chatgpt性能水平的99.3% 。以下是华盛顿大学的作者所公布的数据：

![](img/cbaf72e01b95e813aac901443ebf8225.png)

以下是我自己在40GB显存，500GB磁盘空间的A100上对Guanaco模型的推理实测：

| MODEL | GPU RAM（括号内为未使用qlora的显存） | CPU RAM | DISK |
| --- | --- | --- | --- |
| Guanaco 7b merged | 4.8GB（14.7GB） | 3.6GB | 36.4GB |
| Guanaco 13b merged | 8.4GB（28.7GB） | 5.9GB | 60.2GB |
| Guanaco 33b merged | 18.3GB（OOM） | 8.4GB | 122GB |
| Guanaco 65b merged | 38.7GB（OOM） | 13.1GB | 434GB |

看到这里相信有很多同学已经按捺不住准备去尝试了[，但如果](https://zhida.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&zhida_source=entity&is_preview=1)我告诉你 只要5元就可以在DB-GPT上部署 自己的Guanaco 33b模型呢 在安全隐私的环境下拥有如此一个强大的模型将对个人 知识库 将带来怎样的改变？不妨先耐着性子看完以下介绍

## **二、QLoRA**

总的来说QLoRA 使用一种低精度的存储数据类型（NF4）来压缩预训练的语言模型。通过冻结 LM 参数，将相对少量的可训练参数以 Low-Rank Adapters 的形式添加到模型中，LoRA 层是在训练期间更新的唯一参数，使得模型体量大幅压缩同时推理效果几乎没有受到影响。从QLoRA的名字可以看出，QLoRA实际上是Quantize+LoRA技术。

[  ](https://link.zhihu.com/?target=https%3A//github.com/artidoro/qlora)

我们首先简单了解一些微调方法。

### **1、Fine-Tuning**

在 [大语言模型 ](https://zhida.zhihu.com/search?q=%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B&zhida_source=entity&is_preview=1)对特定任务或领域进行微调任务时，重新训练所有模型参数将会带来昂贵的训练成本，因此出现了各种优化的微调方案：

### **1.1、 **[Adapters-Tuning](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1902.00751.pdf)

在[预训练模型每](https://zhida.zhihu.com/search?q=%E9%9D%9E%E7%BA%BF%E6%80%A7%E5%87%BD%E6%95%B0&zhida_source=entity&is_preview=1)一层(或某些层)中添加Adapter（bottleneck）模块，可以理解为在attention外面整体包一个复杂的 非线性函数 进行空间变换。微调时冻结预训练模型主体，由Adapter模块学习特定下游任务的知识。每个Adapter模块由两个前馈子层组成，第一个前馈子层将Transformer块的输出作为输入，将原始输入维度d投影到m，通过控制m的大小来限制Adapter模块的参数量，通常情况下m<<d。在输出阶段，通过第二个前馈子层还原输入维度，将m重新投影到d，作为Adapter模块的输出。

![](img/5d0bda8ec8eb482c228f3f95ab7c7a93.png)

Adapter方法不需要微调预训练模型的全部参数，通过引入少量针对特定任务的参数，来存储有关该任务的知识，降低了对模型微调的算力要求。论文指出adapter规模超参数在为8、256的时候， 该方案微调时每个任务仅增加3.6%的训练参数量 。

![](img/473dd9ef1ce26b936d500233fae01000.png)

然而，基于Adapter的微调方[法需要在预](https://zhida.zhihu.com/search?q=%E7%9B%B8%E5%85%B3%E5%8F%82%E6%95%B0&zhida_source=entity&is_preview=1)训练模型中添加针对下游任务的 相关参数 。虽然提高了模型的训练效率，但也会降低模型推理时的性能。将模型在实际应用中部署时，速度下降会非常明显。

以下是相关代码：

```
from transformers import AutoModelWithHeads

model = AutoModelWithHeads.from_pretrained(&quot;bert-base-uncased&quot;)
adapter_name = model.load_adapter(&quot;AdapterHub/bert-base-uncased-pf-imdb&quot;, source=&quot;hf&quot;)
model.active_adapters = adapter_name
```

### 1.2、prompt tuning (P-tuning v2)

要把一篇电影评论x="了不起的电影！"分类为正面或负面，自然会想到在评论中附加一个提示 "[它是[MA](https://zhida.zhihu.com/search?q=%E6%9D%A1%E4%BB%B6%E6%A6%82%E7%8E%87&zhida_source=entity&is_preview=1)SK]"，并生成mask token被预测为 "好 "或 "坏 "的 条件概率 作为分类。在这种情况下，提示token{"它"、"是"、"[MASK]"}都属于模型的单词表V，而输入嵌入序列将为

![](img/5eb5ac7790112189cffac610a5439e75.png)

然而，由于模型M本质上是连续的，从优化的角度来看，用离散的自然提示语永远不可能达到最优。相反，P-tuning建议用可训练的连续嵌入[h0，...，hi]代替提示token，并将输入序列变成

![](img/5ca192cd5bfecaf41b6f4b3ab257d2f6.png)

P-tuning v2可以被看作是prefix tuing的优化版本。prefix tuing是在输入的embedding层最前端插入prompt. P-tuing V2为了提升模型解决NLU任务的能力，是除了在输入层的embedding加入promt之外,其他的Transformer层也加了可训练的独立的连续提示。 该方案会增加0.1%~3%的训练参数量 。

![](img/66f30d006286779073aac55ae42508aa.png)

p-tuning的缺点是promt所构造的连续embedding很难优化，它在可训练参数中呈非单调变化，为保留自适应的最佳序列长度必然会减少用于处理下游任务的序列长度，这会导致性能下降。

相关代码如下：

```
from transformers import AutoModelForCausalLM, AutoTokenizer, default_data_collator, get_linear_schedule_with_warmup
from peft import get_peft_config, get_peft_model, PromptTuningInit, PromptTuningConfig, TaskType, PeftType
import torch
from datasets import load_dataset
import os
from torch.utils.data import DataLoader
from tqdm import tqdm

device = &quot;cuda&quot;
model_name_or_path = &quot;bigscience/bloomz-560m&quot;
tokenizer_name_or_path = &quot;bigscience/bloomz-560m&quot;
peft_config = PromptTuningConfig(
    task_type=TaskType.CAUSAL_LM,
    prompt_tuning_init=PromptTuningInit.TEXT,
    num_virtual_tokens=8,
    prompt_tuning_init_text=&quot;Classify if the tweet is a complaint or not:&quot;,
    tokenizer_name_or_path=model_name_or_path,
)
```

### **1.3、LoRA**

t**ransf**ormer的attention网络结构中的参数通常是冗余的，它们可以精简到一个低维中完成各种NLP任务。 低秩分解 便是一种将高维稠密参数向量降维分解为稀疏的低维向量的方法。

[低秩自适应](https://zhida.zhihu.com/search?q=%E5%88%86%E8%A7%A3%E7%9F%A9%E9%98%B5&zhida_source=entity&is_preview=1)（LoRA）方法冻结了预训练的模型权重，并将可快速迭代的秩 分解矩阵 注入Transformer attention架构的Q、V 中，从而大大减少了下游任务的可训练参数数量。如图，训练阶段pretrained weights直接freeze, 只更新右边的AB参数即可，模型的输入输出维度不变，输出时将BA与PW叠加即可。

> 关于秩的理解：一个教师里面有50个学生，每个学生代表一个权重，每个学生都要完成学习任务和练习来更新自己的知识，教师独立的让每个学生都是完成作业，那么这个班级的学习水平是满秩的；由于不同学生的基础水平不同，如果50个学生中有10个学生先掌握了知识，这10个学生把作业借给别人抄袭，这样班级也按时完成了作业，这时候班级学习水平便是低秩的。

![](img/73beebf10978229e3c169931fbaccbda.png)

神经网络包含许多执行 [矩阵乘法 ](https://zhida.zhihu.com/search?q=%E7%9F%A9%E9%98%B5%E4%B9%98%E6%B3%95&zhida_source=entity&is_preview=1)**的密集层，**对于预训练的权重矩阵，将  W_0∈R^{d*k} 用 低秩分解 ，即

h=W_0x+∆W_x=W_0x+BAx

来更新权重，W_0是预训练模型初始化的参数，ΔW就是需要更新的参数，  B∈R^{d*r},A∈R^{r*k}** ，而秩  r≪**min(d,k**) 。在训**练期间， W_0被冻结并且不接收梯度更新，而A和B包含可训练参数。对 A 使用 随机高斯初始化 ，对B 使用 零初始化 ，然后用 \frac{a}{r}缩放∆W_x，其中α是r中的常数，可设置为尝试的第一个r，因此当改变r时便有助于减少重新调整超参数的需要。这意味着，将LoRA应用于所有权重矩阵并训练所有权重时，通过设置秩r将会使模型大致恢复到完全微调时的能力。

![](img/a6fecf68c92e2ade430b4a195b50938a.png)

LoRA的优势很明显：

+   [预训练模型参](https://link.zhihu.com/?target=https%3A//www.pnas.org/doi/10.1073/pnas.1611835114)数可以共享并保持冻结，因此模型不容易发生 灾难性遗忘 。

+   秩分解矩阵的参数明显少于原始模型，根据不同**的任务可以构**建不同的小型LoRA模块， 移植性很强 。我们可以通过替换矩阵A和B来冻结共享模型并有效地切换任务，从而显著降低存储需求和任务切换开销。

+   当使用adapter时，因为我们不需要计算梯度或维护大多数参数的优化器状态，LoRA使显存开销下降。

+   LoRA简单的线性设计允许我们在输出时将可训练矩阵与冻结权重合并即可，通过构造与完全微调的模型相比，LoRA不会引入推理延迟。

+   LoRA与许多先前的方法正交，并且可以与其中的许多方法组合，例如p-tuning。

LoRA的也有一些缺点：

+   [LORA进](https://zhida.zhihu.com/search?q=%E6%B3%9B%E5%8C%96%E8%83%BD%E5%8A%9B&zhida_source=entity&is_preview=1)行低秩分解时候可能会损失一些模型的表达能力和 泛化能力 。

+   LORA微调方法可能会受到初始化和超参数的影响较大，需要进行适当的调整。

以下是代码部分：

```
from transformers import AutoModelForSeq2SeqLM
from peft import get_peft_config, get_peft_model, LoraConfig, TaskType
model_name_or_path = &quot;bigscience/mt0-large&quot;
tokenizer_name_or_path = &quot;bigscience/mt0-large&quot;

peft_config = LoraConfig(
    task_type=TaskType.SEQ_2_SEQ_LM, inference_mode=False, r=8, lora_alpha=32, lora_dropout=0.1
)

model = AutoModelForSeq2SeqLM.from_pretrained(model_name_or_path)
model = get_peft_model(model, peft_config)
model.print_trainable_parameters()
# output: trainable params: 2359296 || all params: 1231940608 || trainable%: 0.19151053100118282
```

### **2、Quantize**

模型的大小通常由其参数的数量及其精度决定，常见的精度有全精度float32(FP32)、半精度float16(FP16)和bfloat16(BF16)。

+   **FP32 **： [单精度浮点数 ](https://zhida.zhihu.com/search?q=%E5%8D%95%E7%B2%BE%E5%BA%A6%E6%B5%AE%E7%82%B9%E6%95%B0&zhida_source=entity&is_preview=1)，用8bit 表示指数，23bit 表示小数。使用此数据类型，可以表示各种浮点数，并且支持大多硬件。

+   **FP16 **：半精度浮点数，用5bit 表示指数，10bit 表示小数。FP16 表示整数范围较小，但是尾数精度较高。

+   **BF16 **：是对FP32单精度浮点数截断数据，用8bit 表示指数，7bit 表示小数。BF16 可表示的整数范围与FP32一样广泛。但只有新的硬件(A100\3090\4090等)才支持，V100/昇腾910等不支持

![](img/a74c9b46fdb7a2202495960824339ff5.png)

在A**RM NEON**指令集中，一条指令最多load 128bit 数据，则对于FP32的数据，一次性最多支持4个数据的并行计算；对FP16/BF16数据，一次性最多支持8个数据的并行计算，那么在计算中， FP16/BF16的性能峰值应该是FP32 的两倍 。而且，实验表明FP16/BF16往往能够在 模型大小减半 的情况下还能获得和FP32模型几乎相同的推理结果，这种惊人的效果吸引着众多研究人员前往更深的领域——FP8、FP4、FP2、FP1。许多量化的技术也逐渐引入了大模型的研究中。

### **2.1、LLM.int8 量化**

**量化的**本质实际是从一种数据类型 舍入 到另一种数据类型，通常包含量化和反量化两步，假如我们有两组数据类型A、B，A可以表示的数值为[0, 1, 2, 3, 4, 5]，B可以表示的数值为[0, 2, 4]。我们要做的便是：

+   将数据范围从A标准化为B。数据类型A表示的向量为[3, 1, 2, 3]。

+   找到向量[3, 1, 2, 3]的最大绝对值3

+   向量[3, 1, 2, 3]除以最大值3：[3, 1, 2, 3]->[1, 0.33, 0.66, 1.0]

+   将向量[1, 0.33, 0.66, 1.0]与B的数据范围4相乘：[1, 0.33, 0.66, 1.0]->[4.0, 1.33, 2.66, 4.0]

+   将向量[4.0, 1.33, 2.66, 4.0]中的每个值用B中最接近的数值表示：[4.0, 1.33, 2.66, 4.0] -> [4, 2, 2, 4]。

+   用B中最接近的数值表示A 。

+   [4, 2, 2, 4]除以4->[1.0, 0.5, 0.5, 1.0]

+   乘以量化过程中找到的最大的绝对值：[1.0, 0.5, 0.5, 1.0] -> [3.0, 1.5, 1.5, 3.0]

+   近似表示：[3.0, 1.5, 1.5, 3.0] -> [3, 2, 2, 3]

![](img/8c99271a775109a1656153e983a114eb.png)

经过量化、反量化，原来的[3, 1, 2, 3][变成了[3](https://zhida.zhihu.com/search?q=%E9%87%8F%E5%8C%96%E8%AF%AF%E5%B7%AE&zhida_source=entity&is_preview=1)[, 2, ](https://zhida.zhihu.com/search?q=%E9%87%8F%E5%8C%96%E7%B2%BE%E5%BA%A6&zhida_source=entity&is_preview=1)2, 3]，产**生了 量化误差 。这**样的误差在模型中传播、积累，最终会影响模型性能。提高 量化精度 的最佳方法是 使用更多的量化参数 。例如，我们有一个二维的张量：[[3, 1, 2, 3], [0, 1, 1, 0]]。如果分别为[3, 1, 2, 3]和[0, 1, 1, 0]设置独立的量化参数为各自的最大绝对值3和1，那么量化的精度会比为整个[[3, 1, 2, 3], [0, 2, 2, 0]]只设置一个量化参数3更高。

[同](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2208.07339.pdf)样是Tim Dettmers，他在 《LLM.int8(): 8-bit Matrix Multiplication for Transformers at Scale》 这篇文章中提出将 vector-wise量化与混合精度分解 结合的方法LLM.int8，这里简单介绍，具体细节见原文：

+   vector-wise量化：为矩阵内积中独立的行、列向量设置各自的scaling constant。这种量化方式引入了更多的量化参数，量化的结果也更加精确。

+   混合精度分解：Transformer的hidd[en s](https://zhida.zhihu.com/search?q=%E7%A6%BB%E7%BE%A4%E5%80%BC&zhida_source=entity&is_preview=1)tate中存在一些绝对值很大的 离群值 ，这种离群值对于数据分布的影响是重大的。混合精度分解将包含了离群值的几个维度从矩阵中分离出来，对其做高精度的矩阵乘法；其余部分进行量化。

![](img/9170e8ba528dbe20ed6cfdd25f69cbf8.png)

以下是int8和FP16的OPT-175B模型生成质量的对比结果:

| benchmarks | metric | value - int8 | value - fp16 | std err - fp16 | difference - value |
| --- | --- | --- | --- | --- | --- |
| hellaswag | acc_norm | 0.7849 | 0.7849 | 0.0041 | 0 |
| hellaswag | acc | 0.5921 | 0.5931 | 0.0049 | 0.001 |
| piqa | acc | 0.7965 | 0.7959 | 0.0094 | 0.0006 |
| piqa | acc_norm | 0.8101 | 0.8107 | 0.0091 | 0.0006 |
| lambada | ppl | 3.0142 | 3.0152 | 0.0552 | 0.001 |
| lambada | acc | 0.7464 | 0.7466 | 0.0061 | 0.0002 |
| winogrande | acc | 0.7174 | 0.7245 | 0.0125 | 0.0071 |

以下是在BLLOM 176B模型，batch size为1、8、32的情况下，BF16和LLM.int8生成一个token所需的毫秒级的时间表现。

![](img/9f0d0f1ef59ecb4f4f259c1dfba60791.png)

### **2.2 、NF4**

**Q**LoRA的量化核心技术有三个： 4-bit NormalFloat (NF4)量化、Double Quantization和Paged Optimizers 。首先介绍一下NF4。

\text{4-bit Nor[**malF**](https://zhida.zhihu.com/search?q=%E5%88%86%E4%BD%8D%E6%95%B0&zhida_source=entity&is_preview=1)**loat（**NF4）} 是建立在 分位数 量化技术 的基础之上的一种信息理论上最优的数据类型。

由于预训练的神经网络权值通常具有标准差为0的正态分布性质，因此我们可以通过缩放系数将所有的权值转换为固定期望值，从而使该分布完全适合我们的数据类型范围。一旦权重范围和数据类型范围匹配，我们就可以像往常一样进行量化。

分位数量化技术的主要思想便是将数值尽量落到均值为0，标准差为[-1,1]的正态分布的固定期望值上。前面我们知道离群值对于模型量化的影响极其重要，而由于分位数估计算法的近似性质，精度量化对于离群值又有很大的误差。分位数量化技术使得每个量化分区中具有相等的期望值，相等的期望值可以避免昂贵的分位数估计和近似误差，使得精确的分位数估计在计算上可行。

分位数量化技术步骤如下：

+   估计N(0,1)分布的  2^{k+1} 个分位数，得到正态分布的k-bit位量化数据类型。

+   将其值归一化到[- 1,1]范围内

+   通过absmax去重新缩放权重张量的标准差，以获得k-bit的数据形式。

以下是分位数q_i的计算过程：

![](img/0b23827758dbaef76cef5b50218dd0ab.png)

[它在量化过程中保](https://zhida.zhihu.com/search?q=bit%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B&zhida_source=entity&is_preview=1)留了零点，并使用所有  2^k 位来表示k- bit数据类型 。这种数据类型通过估计两个范围的分位数q_i来创建一个非对称的数据类型，这两个范围分别是负数部分[-1,0]的  r≪min(d,k) 和正数部分[0,1]的  2^{k-1}+1 。然后，它统一了这两组分位数q_i，并从两组中都出现的两个零中移除一个。这种结果数据类型在每个量化bin中都有相等的期望值数量，因此被称为  2^{k-1}+1 。

以NF4，即k=4为例， [标准正态分布 ](https://zhida.zhihu.com/search?q=%E6%A0%87%E5%87%86%E6%AD%A3%E6%80%81%E5%88%86%E5%B8%83&zhida_source=entity&is_preview=1)量化函数把[-1, 0]分成7份，然后生成[-1, ..., 0]共8个分位数, 把[0, 1]分成8份，然后生成[0, ..., 1]共9个分位数，两个合起来去掉一个0就生成全部的16个分位数了。

q_i 创建了NF4数据类型的  2^4 =16 个值，并用零填**充，以便在8位量化函数中使**用（256个值，其中包括256-16个零）。该函数在 bitsandbytes 库中使用8位量化方法来“模拟”NF4。以下是更直观的解释：

> 我们**的目标是找**到等面积的量化区间，使得量化区间左右两侧的面积相等。这意味着我们不从正态分布的0和1量化区间开始，而是从一个偏移量量化区间开始。代码片段中称之为"offset"，其值为1-1/(2 15)。如果我们有一个非对称的 数据类型 ，其中一侧的间隔等于每个量化区间周围的16个“半个”，而另一侧只有15个“半个”。因此，平均偏移量为(1-1/(2* 15) + 1-1/(2*16))/2 = 0.9677083。

我们使用norm.ppf函数获取标准正态分布（N(0, 1)）的量化区间。然后，通过将这些量化区间的值除以绝对最大值来重新缩放它们。

### 2.3、Double Quantization

Double Quantization是将额外的量化常数进行二次量化以减小内存开销的过程。例如每64个参数块共享一个32bit的量化常数， 这样的话相当于每一个参数的量化额外开销为32/64 = 0.5 bit。这个总体来说也是比较大的一个开销，所以为了进一步优化这个量化开销，我们对其进行二次量化( Double Quantization )，即把第一次32bit量化的输出作为第二次量化的输入，我们采用256的块大小对量化常数进行FP8量化，这样的话，我们可以把每个参数的量化额外开销降低到：

![](img/13562d72a979f92e389f297bf46fdef4.png)

### 2.4、Paged Optimizers

使用NVIDIA统一内存功能，该功能在CPU和GPU之间进行自动page对page传输，以便在GPU偶尔OOM的情况仍然下进行模型训练和微调。 可以理解成显存偶发OOM时，QLoRA会将优化器状态自动的驱逐到CPU RAM，当在优化器更新步骤中需要内存时，它们会被分页回GPU内存，从而保证训练正常训练下去。

### **3、总结**

QLoRA 的量化过程如下：QLoRA有一个用于基本模型权重的存储数据类型（NF4）和一个用于执行计算的计算数据类型（BF16）。QLoRA 将权重从存储数据类型反量化为计算数据类型以执行向前和向后传递， 但在传递过程中仅计算使用 BF16 的 LoRA 参数的权重梯度。权重仅在需要时解压缩，因此在训练和推理期间内存使用量保持较低。

![](img/548cfc5076ccb05553841377e668a4ad.png)

文中还有几个观点值得关注：

+   NF4量化 优于FP4和int4量化的性能 ，并且NF4+Double Quantization减少存储的同时，精度并未下降。

![](img/77096aeb735b08e78b98d8355e5b1e04.png)

+   单卡24G/48G训练33B/65B模型的时候，Paged optimziers 表现很好。

![](img/630429b47e03ae64ce902f07c6c655ab.png)

+   使用默认的LoRA参数时，NF4训练达不到BF16的评估指标，QLoRA(4bit)需要LoRA应用到所有的transformer层才会有好的效果。

![](img/bc06b2d74bbdd719550ce097462ab914.png)

+   NF4+Double Quantizatio**n的方案finetun**e的模型精度可以 媲美BFloat 16

**同时作者也**在文章中留下了Guanaco模型的一些 不足之处 ：

+   在模型评估过程中：人为的主观性影响着模型评估，在自动评估系统中，评估方式存在明显的偏见，系统对首次出现的结果给予更高的分数，并且GPT-4对其自身的输出给予的分数高于人类评分。

+   在数据准备过程中：训练Guanaco模型的OASST1数据集是多语言的，而Vicuna13B模型仅在英语数据上训练，这表明Vicuna13B与Guanaco 模型的测试评估是不太公平的。

+   [在模型训](https://zhida.zhihu.com/search?q=%E4%BA%A4%E5%8F%89%E7%86%B5&zhida_source=entity&is_preview=1)练过程中：Guanaco仅使用 交叉熵 损失(监督学习)进行训练，而不依赖于来自人类反馈的强化学习(RLHF)

总的来说，QLoRA方法有三个意义：

+   QLoRA是第一种能够在单个消费者GPU上微调33B参数模型和在单个专业GPU上微调65B参数模型的方法，同时相比于full finetuning baseline并不会降低性能。这有助于大模型微调方法的普及，让更多的普通人和小团队参与到LLM研究中来

+   另一个是QLoRA为LLM在手机端微调训练带来了可能，作者估计在iPhone 12 Plus上, QLoRA可以在手机充电状态下每晚微调300万个token。

+   QLoRA还可以帮助实现LLM的隐私保护，用户可以拥有和管理他们自己的数据和模型，同时LLM更容易部署。这一点会放在DB-GBT中讲述。

### **4、代码**

[QLoRA ](https://link.zhihu.com/?target=https%3A//github.com/artidoro/qlora)的相关代码已经开源，下面简单介绍一下安装过程：

```
git clone https://github.com/artidoro/qlora.git

conda create -n qlora python=3.10
conda activate qlora
cd qlora
pip install -r requirements.txt

# 安装 bitsandbytes，transformers, accelerate， peft
pip install -q -U bitsandbytes
pip install -q -U git+https://github.com/huggingface/transformers.git
pip install -q -U git+https://github.com/huggingface/peft.git
pip install -q -U git+https://github.com/huggingface/accelerate.git

#这四个库的版本号应该为：
bitsandbytes 0.39.0
transformers 4.30.0.dev0
accelerate 0.20.0.dev0
peft 0.4.0.dev0

#若官方安装方法不成功可以试试手动安装：
#transformers
git clone https://github.com/huggingface/transformers.git
cd transformers
python -m pip install -e . -i https://mirror.baidu.com/pypi/simple

#bitsandbytes
git clone https://github.com/TimDettmers/bitsandbytes.git
cd bitsandbytes
# CUDA_VERSIONS in {110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 120}
# make argument in {cuda110, cuda11x, cuda12x}
# 看好自己的cuda版本, cuda11.8 对应 118和cuda11x . 
CUDA_VERSION=118 make cuda11x
python setup.py install

#peft
git clone https://github.com/huggingface/peft.git
cd peft &amp;&amp; pip install -e . -i https://mirror.baidu.com/pypi/simple

#accelerate
git clone https://github.com/huggingface/accelerate.git
cd accelerate &amp;&amp; pip install -e . -i https://mirror.baidu.com/pypi/simple
```

在scripts文件夹中有模型微调和合成脚本，可以根据自身需要进行修改参数和模型，然后一键运行即可

![](img/207cd0c4b43de514e4e5727685de72d0.png)

模型量化部分代码如下：

```
model = AutoModelForCausalLM.from_pretrained(
        model_name_or_path=&#x27;/name/or/path/to/your/model&#x27;,
        load_in_4bit=True, #模型压缩的关键
        device_map={&quot;&quot;: 0},  #调用GPU，并使用accelerate加载模型
        max_memory=max_memory,
        torch_dtype=torch.bfloat16,
        quantization_config=BitsAndBytesConfig(
            load_in_4bit=True,
            bnb_4bit_compute_dtype=torch.bfloat16, #计算时采用BF16最快，默认为FP32
            bnb_4bit_use_double_quant=True, #二次量化
            bnb_4bit_quant_type=&#x27;nf4&#x27; #量化时使用nf4最优，也可以使用fp4
        )
    )


# 若想使用Paged Optimizer，则在qlora.py中调用 --optim paged_adamw_32bit
```

官方也给出了llama-7b模型和guanaco-7b[ ad](https://link.zhihu.com/?target=https%3A//github.com/artidoro/qlora/blob/main/examples/guanaco_7B_demo_colab.ipynb)apter modules的合成 示例 ：

```
import torch
from peft import PeftModel    
from transformers import AutoModelForCausalLM, AutoTokenizer, LlamaTokenizer, StoppingCriteria, StoppingCriteriaList, TextIteratorStreamer

model_name = &quot;decapoda-research/llama-7b-hf&quot;
adapters_name = &#x27;timdettmers/guanaco-7b&#x27;

print(f&quot;Starting to load the model {model_name} into memory&quot;)

m = AutoModelForCausalLM.from_pretrained(
    model_name,
    #load_in_4bit=True,
    torch_dtype=torch.bfloat16,
    device_map={&quot;&quot;: 0}
)
m = PeftModel.from_pretrained(m, adapters_name)
m = m.merge_and_unload()
tok = LlamaTokenizer.from_pretrained(model_name)
tok.bos_token_id = 1

stop_token_ids = [0]

print(f&quot;Successfully loaded the model {model_name} into memory&quot;)
```

QLoRA方法在实际使用中要注意以下几点：

+   load_in_4bit=True的情况下模型推理能力较慢。4bit推理还未能与4bit矩阵乘法结合

+   [bnb_4](https://zhida.zhihu.com/search?q=%E9%87%8F%E5%8C%96%E6%A8%A1%E5%9E%8B&zhida_source=entity&is_preview=1)bit_compute_type='fp16'会导致 量化模型 训练不稳定。

+   要设置 tokenizer.bos_token_id = 1

## 三、 Guanaco+DB-GPT的部署：

[DB-GPT ](https://link.zhihu.com/?target=https%3A//github.com/csunny/DB-GPT/tree/main)是一个在数据库领域与LLM大模型结合的项目，核心理念是 技术的绝对开放、环境的绝对私有 自从5月6号发布第一个版本以来， 不到一个月就突破了3.3k star，在半个月内数次占领 GitHub Trending 之后，又登上了 Hacker News 首页，项目仍然在不断开发中，后续我们会不断将最前沿的技术逐渐集成到项目中来，大家可以持续关注~

[  ](https://link.zhihu.com/?target=https%3A//github.com/csunny/DB-GPT)

下面讲讲我在DB-GPT上是如何部署Guanaco 33b大模型，打造自己的知识库

### **1、安装DB-GPT**

[这里 ](https://zhuanlan.zhihu.com/p/629467580)有在阿里云上部署的详细教程，大家可以看看。但是阿里[云的价格还是有](https://link.zhihu.com/?target=https%3A//www.autodl.com/)点小贵，今天教大家怎么在 AutoDL 上部署。

首先在算力市场中选择适合我们的GPU，这里根据我自己的需要选择3090显卡：

![](img/bdeb571710f3c71f2756b8a7dbe3db18.png)

由于后续会存储各种大模型，所以我会扩容一下硬盘，注意这里系统会根据硬盘扩容大小按日收费。然后选择好要安装的基础镜像。

![](img/74018ed25e86e5d8420b5ac016b23fda.png)

点击立即创建后进入控制台就可以看到我们创建的实例啦，然后开机进入jupyter **lab即可。这**里有个小技巧，如果仅仅是部署环境而不涉及到模型训练，我们可以使用 无卡开机模式 ，每小时只要0.1元，可以说相当划算了。

![](img/e0bc5d750ca5bfcca965466989486538.png)

进入jupyter lab的终端环境后，我们就可以开始配置环境了，首先我们要安装git lfs方便下载hf repo的大模型，

```
curl [-s http](https://zhida.zhihu.com/search?q=apt-get&amp;zhida_source=entity&amp;is_preview=1)s://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
sudo apt-get install git-lfs
```

我们所购买的扩容数据盘都是放在auto-tmp目录下的，所以我们要cd到auto-tmp目录下，再进行后续安装。AutoDL内置了学术资源加速方法，可以提高github的访问速度，在帮助文档里有详细说明，这里我们也加载一下。

```
#设置学术加速
source /etc/network_turbo
cd auto-tmp
# 下载项目代码
git clone https://github.com/csunny/DB-GPT.git

# 更新bashrc中的环境变量
conda init bash &amp;&amp; source /root/.bashrc

#构建并激活conda环境
conda create -n dbgpt_env python=3.10 
conda activate dbgpt_env

#安装环境依赖，这里保险起见我加入了信任的镜像源下载
cd DB-GPT
pip install -r requirements.txt -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

#若后续使用guanaco，还需安装QLoRA的bitsandbytes，transformers，accelerate，peft，安装方法在上面
#这四个库的版本号应该为：
bitsandbytes 0.39.0
transformers 4.30.0.dev0
accelerate 0.20.0.dev0
peft 0.4.0.dev0
```

环境安装完成后，我们要在DB-GPT里新建文件夹models，然后我们把huggingface上下载的模型了放在这个目录里面即可

```
#若想体验完整的DB-GPT项目，建议也下载vicuna-13b和all-MiniLM-L6-v2
git clone https://huggingface.co/Tribbiani/vicuna-13b 
git clone https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2

#guanaco-33b-merged是量化后可以直接使用的33b模型，如需使用其他量化模型，可自行选择合成或下载
git clone https://huggingface.co/timdettmers/guanaco-33b-merged
```

模型文件很大，下载时间会比较长。在下载期间，我们来配置一下.env文件，.env文件需要从.env.template复制创建，我们需要将模型名称修改为要使用的guanaco-33b-merged，即LLM_MODEL=guanaco-33b-merged，

由于我们的项目因为要直接连接数据库，这里我们以MySQL作为样例，安装mysql。

```
# 首先更新apt源
sudo apt update

#下载mysql-server
sudo apt install mysql-server

#查看mysql的状态，开启mysql
sudo service mysql status
sudo service mysql start

#进入mysql终端
sudo mysql

#设置root密码，注意这里的密码应该和DB-GPT中的.env文件保持一致
ALTER USER &#x27;root&#x27;@&#x27;localhost&#x27; IDENTIFIED WITH mysql_native_password BY &#x27;aa12345678&#x27;;

#登录mysql，这里会提示输入密码，可以查看自己密码创建是否正确
mysql -u root -p
```

模型下载好以后，我们需要切换到DB-GPT最新的dev分支，Guanaco 33b这个模型可以跑起来了，但目前还不是流式，后续我们会持续处理一下

```
#切换分支
git checkout dev
#更新分支
git pull origin dev

# 运行命令启动服务端：
python pilot/server/llmserver.py 
```

在这里我们的前后端均放在AutoDL上，因此我们再开一个新的终端

```
cd autodl-tmp/DB-GPT
conda activate [dbgp](https://zhida.zhihu.com/search?q=%E7%AB%AF%E5%8F%A3%E6%98%A0%E5%B0%84&amp;zhida_source=entity&amp;is_preview=1)t_env

#由于AutoDL的实例无独立公网IP，因此不能任意开启额外的端口。但是AutoDL为每个实例都预留了一个可对外暴露的端口，技术实现为将实例中的6006端口映射到公网可供访问的ip:port上，而ip:port可在「自定义服务」入口获取，这里我们使用6006开启客户端
python pilot/server/webserver.py --port 6006
```

然后我们访问自定义服务便可以开启我们的DB-GPT啦

![](img/47dab78dd21fd1fe6a92026661720866.png)

![](img/d12f5d3502ce5a7d38a4e36c4b94fa9a.png)

### **2、Guanaco相关**

模型的加载代码如下：

```
class GuanacoAdapter(BaseLLMAdaper):
    &quot;&quot;&quot;TODO Support guanaco&quot;&quot;&quot;

    def match(self, model_path: str):
        return &quot;guanaco&quot; in model_path

    def loader(self, model_path: str, from_pretrained_kwargs: dict):
        tokenizer = LlamaTokenizer.from_pretrained(model_path)
        model = AutoModelForCausalLM.from_pretrained(
            model_path, load_in_4bit=True, device_map={&quot;&quot;: 0}, **from_pretrained_kwargs
        )
        return model, tokenizer
```

模型的生成代码如下：

```
import torch
import copy
from threading import Thread
from transformers import TextIteratorStreamer, StoppingCriteriaList, StoppingCriteria


def guanaco_generate_output(model, tokenizer, params, device, context_len=2048):
  

    print(params)
    stop = params.get(&quot;stop&quot;, &quot;###&quot;)
    prompt = params[&quot;prompt&quot;]
    query = prompt
    print(&quot;Query Message: &quot;, query)

    input_ids = tokenizer(query, return_tensors=&quot;pt&quot;).input_ids
    input_ids = input_ids.to(model.device)

    streamer = TextIteratorStreamer(
        tokenizer, timeout=10.0, skip_prompt=True, skip_special_tokens=True
    )
    stop_token_ids = [0]

    class StopOnTokens(StoppingCriteria):
        def __call__(
            self, input_ids: torch.LongTensor, scores: torch.FloatTensor, **kwargs
        ) -&gt; bool:
            for stop_id in stop_token_ids:
                if input_ids[0][-1] == stop_id:
                    return True
            return False

    stop = StopOnTokens()

    generate_kwargs = dict(
        input_ids=input_ids,
        max_new_tokens=512,
        temperature=1.0,
        do_sample=True,
        top_k=1,
        streamer=streamer,
        repetition_penalty=1.7,
        stopping_criteria=StoppingCriteriaList([stop]),
    )

    t1 = Thread(target=model.generate, kwargs=generate_kwargs)
    t1.start()

    generator = model.generate(**generate_kwargs)
    for output in generator:
        # new_tokens = len(output) - len(input_ids[0])
        decoded_output = tokenizer.decode(output)
        if output[-1] in [tokenizer.eos_token_id]:
            break

        out = decoded_output.split(&quot;### Response:&quot;)[-1].strip()

        yield out
```

### 3、Guanaco 33B与vicuna 13B在SQL生成方面的对比

请根据提供的内容生成对应的sql，内容如下： Here are the birth dates of all guests with the letter M in their first or last name schema:Guests(guest_id,gender_code,guest_first_name,guest_last_name,date_of_birth)

guanaco:

```
SELECT guest_id, gender_code, guest_first_name, guest_last_name, date_of_birth FROM Guests WHERE guest_first_name LIKE &#x27;M%&#x27; OR guest_last_name LIKE &#x27;M%&#x27;
```

vicuna

```
SELECT guest_id, gender_code, guest_first_name, guest_last_name, date_of_birth
FROM schema:Guests
WHERE (guest_first_name || guest_last_name) LIKE &#x27;%M%&#x27;
这个 SQL 查询语句将返回包含字母 M 的所有客人的 ID、性别码、名字、姓氏和出生日期。其中的条件的运算符是 SQL 的模糊匹配符，它会匹配字母 M 的任何位置，并且在名字中出现次数不限。
```

---

请根据提供的内容生成对应的sql，内容如下： In what year was Lindenwood University founded? schema:table_10082596_1(School,Location,Founded,Affiliation,Enrollment,Team_Nickname,Primary_conference)

guanaco:

```
SELECT Founded FROM table_10082596_1 WHERE School = &quot;Lindenwood University&quot;
```

vicuna：

```
SELECT School, Location, Founded, Affiliation, Enrollment, Team_Nickname, Primary_conference
FROM schema:table_10082596_1
WHERE School = &#x27;Lindenwood University&#x27;
这个 SQL 查询语句将返回 Lindenwood University 所在的所有信息，包括学校、所在地、成立年份、联接机构、学生人数和主要竞技队名称，以及主要会议联盟。请注意，如果您不知道该数据库中包含的其他列的含义，请确保在查询结果中使用了正确的列名。
```

---

请根据提供的内容生成对应的sql，内容如下： Which customers placed orders before George schema:Customers(customer_id,customer_name,customer_details),Orders(order_id,customer_id,order_status,date_order_placed,order_details)

guanaco:

```
SELECT customer_id FROM Customers AS c
LEFT JOIN Orders AS o ON c.customer_id = o.customer_id AND order_status = &#x27;Pending&#x27;
WHERE customer_name = &#x27;George&#x27;
```

vicuna：

```
SELECT Orders.order_id, Customers.customer_id, Orders.order_status, Orders.date_order_placed, Orders.order_details
FROM Orders
INNER JOIN Customers ON Orders.customer_id = Customers.customer_id
WHERE Orders.date_order_placed &lt; (SELECT MIN(date_order_placed) FROM Orders WHERE customer_id = Customers.customer_id AND order_status = &#x27;completed&#x27;)
AND Orders.order_status &lt;&gt; &#x27;completed&#x27;
AND Customers.customer_name = &#x27;George&#x27;
这个 SQL 查询语句将返回在 George 客户名称的所有早于 George 最早的订单的所有订单信息，包括订单 ID、客户 ID、订单状态、订单日期和订单详细信息。它使用了内连接（INNER JOIN）将 Orders 和 Customers 表连接起来，以便查询客户名称为 George 的所有订单。WHERE 子句使用了一个严格比较符（&lt;）和子查询，以确保只返回在 George 客户名称的所有订单，而不是其他客户的订单。此外，使用了一个条件表达式（AND Customers.customer_name = &#x27;George&#x27;）来筛选出只有 George 客户名称的订单。最后，使用了一个简单的 SQL 子句（SELECT MIN(date_order_placed)）来查询客户名称为 George 且订单状态为 &#x27;completed&#x27; 的最早订单的日期。
```

---

请根据提供的内容生成对应的sql，内容如下： what is the highest point in states bordering colorado schema:highlow(state_name,highest_elevation,lowest_point,highest_point,lowest_elevation) 以下是根据提供的内容生成的 SQL 代码：

guanaco:

```
SELECT state_name, highest_elevation, lowest_point, highest_point, lowest_elevation FROM highlow(state_name, highest_elevation, lowest_point, highest_point, lowest_elevation);
```

vicuna：

```
SELECT states.state_name, highest_point.highest_elevation, lowest_point.lowest_point, highest_point.highest_point, lowest_point.lowest_elevation
FROM states
LEFT JOIN (
  SELECT state_name, highest_point.highest_elevation
  FROM states
  JOIN highest_point ON states.state_name = highest_point.state_name
  WHERE states.state_name IN (&#x27;Colorado&#x27;)
) AS highest_point_states ON states.state_name = highest_point_states.state_name
LEFT JOIN (
  SELECT state_name, lowest_point.lowest_point, lowest_point.lowest_point
  FROM states
  JOIN lowest_point ON states.state_name = lowest_point.state_name
  WHERE states.state_name IN (&#x27;Colorado&#x27;)
) AS lowest_point_states ON states.state_name = lowest_point_states.state_name
这个 SQL 查询语句将返回所有与 Colorado 共有边界的州名称、最高点的高度、最低点的海拔高度和最高点的海拔高度，以及最低点的海拔高度。它使用了左连接（LEFT JOIN）将 states 表与最高点和最低点表连接起来，并使用了一个子查询来筛选出与 Colorado 共有边界的州名称。使用了一个子查询可以避免使用 WHERE 子句执行过多的连接，从而提高查询效率。最后，使用了一个表达式排序（ORDER BY）来将结果按照海拔高度从高到低排序。
```

---

请根据提供的内容生成对应的sql，内容如下： What are the names of the singers and number of concerts for each person? schema:singer_in_concert(concert_ID,Singer_ID),singer(Singer_ID,Name,Country,Song_Name,Song_release_year,Age,Is_male)

guanaco:

```
SELECT s.Name AS Singer_Name, COUNT(*) AS Concert_Count
FROM
INNER JOIN singer_in_concert si ON s.Singer_ID = si.Singer_ID
GROUP BY s.Name
```

vicuna：

```
SELECT s.Name AS Singer_Name, COUNT(*) AS Concert_Count
FROM singer_in_concert si
INNER JOIN singer s ON si.Singer_ID = s.Singer_ID
GROUP BY s.Name
这个 SQL 查询语句将返回每个歌手的名字和演唱会数量，并且使用了内连接（INNER JOIN）将与演唱会表（singer_in_concert）连接起来，以便查询歌手的演出信息。使用了一个子查询来分组结果，并使用了 COUNT(*) 来统计每个歌手在演唱会中的次数。最后，使用了 AS 来为结果列的列名添加上假名，以使其与查询语句中的列名匹配
```

---

请根据提供的内容生成对应的sql，内容如下： 注册课程最多的学生的详细情况是什么? schema:Students(student_id,student_details),Student_Course_Registrations(student_id,course_id,registration_date)

guanaco:

```
SELECT * FROM Students WHERE student_id IN (
SELECT student_id FROM Student_Course_Registrations GROUP BY student_id HAVING count() = (SELECT count() FROM Student_Course_Registrations GROUP BY student_id ORDER BY count(*) DESC LIMIT 1)
);
```

vicuna：

```
SELECT s.student_id, s.student_details, c.course_name, s.registration_date
FROM Students s
JOIN Student_Course_Registrations sc ON s.student_id = sc.student_id
JOIN Courses c ON sc.course_id = c.course_id
WHERE s.student_id = (SELECT student_id FROM Students ORDER BY student_id DESC LIMIT 1)
这个 SQL 查询语句将返回注册课程最多的学生的详细情况，包括学生 ID、学生详细信息、课程名称和注册日期。它使用了内连接（JOIN）将三个表连接起来，并使用了一个子查询来筛选出有最多注册的学生 ID。使用了一个 ORDER BY 子句来按照学生 ID 降序排序，并使用了 LIMIT 子句来选择只有一个学生 ID。最后，使用了 WHERE 子句来筛选出最多注册的学生。
```

---

请根据提供的内容生成对应的sql，内容如下： 查找有不止一个跟随者的用户的姓名和电子邮件. schema:user_profiles(uid,name,email,partitionid,followers),follows(f1,f2)

guanaco:

```
SELECT UP.name, UP.email FROM user_profiles UP, follows F WHERE UP.uid IN (SELECT f1 FROM F) AND UP.uid IN (SELECT f2 FROM F)
```

vicuna:

```
以下是根据提供的内容生成的 SQL 代码：

SELECT p.name AS User_Name, p.email AS User_Email, COUNT(f.uid) AS Followers
FROM user_profiles p
LEFT JOIN follows f ON p.uid = f.f1
GROUP BY p.uid, p.name, p.email, p.partitionid
HAVING COUNT(f.uid) &gt; 1
这个 SQL 查询语句将返回每个用户的姓名、电子邮件以及其邻居人数，并且使用了左连接（LEFT JOIN）将用户背景信息表（user_profiles）与粉丝表（follows）连接起来。使用了一个子查询来筛选出有多个邻居的用户，并使用了 COUNT(*) 来统计每个用户的邻居人数。最后，使用了 HAVING 子句来筛选出只有多个邻居的用户。此外，使用了 GROUP BY 子句来分组结果，以便按照用户 ID、姓名、电子邮件和粉丝 ID 进行统计。
```

## **四、 **[**参考文献**](https://zhida.zhihu.com/search?q=%E5%8F%82%E8%80%83%E6%96%87%E7%8C%AE&zhida_source=entity&is_preview=1)

[A G](https://link.zhihu.com/?target=https%3A//huggingface.co/blog/hf-bitsandbytes-integration)entle Introduction to 8-bit Matrix Multiplication for transformers at scale using transformers, accelerate and bitsandbytes (huggingface.co)

[LLM.int8()——在大模型上使用int](https://zhuanlan.zhihu.com/p/586406082)8量化 - 知乎 (zhihu.com)

[⁣](https://link.zhihu.com/?target=https%3A//readpaper.feishu.cn/docx/CrMGdSVPKow5d1x1XQMcJioRnQe)⁤⁢⁤⁢⁢⁢⁣⁣⁢⁣⁣⁣⁢⁤⁡⁡⁢⁢QLoRA: 训练更大的GPT - 飞书云文档 (feishu.cn)

QLoRA的实测记录 - 知乎 (zhihu.com)

编辑于 2023-06-07 22:11 ・IP 属地四川