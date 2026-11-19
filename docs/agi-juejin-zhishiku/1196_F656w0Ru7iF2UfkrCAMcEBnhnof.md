
# LLM（十四）：基于 LLM 的推荐系统重构

> 来源：[https://agijuejin.feishu.cn/docx/ENDMdG3kNoyDXtxPZptc1SEcnId](https://agijuejin.feishu.cn/docx/ENDMdG3kNoyDXtxPZptc1SEcnId)

> 作者：紫气东来

> 原文：[https://zhuanlan.zhihu.com/p/632686242](https://zhuanlan.zhihu.com/p/632686242)

## 一、风云际会：推荐系统遇上 LLM

在笔者之前的专栏中 [推荐系统及计算广告](https://www.zhihu.com/column/c_1611400569750257665) 介绍了推荐系统发展过程及主流算法，该类方法有些主要特点：模型规模不大、关注用户和商品数据、针对特定任务等。随着 LLM 的兴起，以及在多领域多场景的良好表现，也为推荐系统的提供新的工具和思路。

### 1.1 基于 LLM 的推荐系统的优点

根据 LLM 和 推荐系统 的特点，二者结合的优点可从以下方面考虑：

+ 缓解稀疏场景：LLM 的 `zero-shot` 和 `few-shot` 能力可用于解决冷启动问题和长尾问题。实验表明，在冷启动场景下，具有超过 100B 参数的 LLM 的效果，与基于启发式的 `baseline` 相当；

+ 庞大的信息存储：LLM 无需更改模型架构或重新训练即可适应新信息，像协同过滤这样的传统方法依赖于大量的训练数据。相比之下，LLM 需要的数据要少得多，因为他们已经拥有大量关于 `item` 的世界知识；

+ 更好的交互方式：LLM 可以让用户通过自然语言指令，通过基于聊天的界面自由、准确地表达他们的需求。而在传统方法中，用户只是被动地（通过隐式反馈）参与改进推荐；

+ 强大的泛化能力：现有的推荐算法是 `task-specific` 的，需要特定的 `user-item` 交互数据进行训练，这意味着它们缺乏泛化能力。另一方面，LLM 可以使用表示为序列的用户交互数据以补充其知识；

+ 建模及开发简单：当前的推荐算法使用复杂的特征处理方法，如预处理策略、`embedding` 方法、自定义架构等。`prompts` 在很大程度上简化了这些特征处理和建模步骤；

+ 使用便捷透明：作为 `CoT` 推理的副产品，LLM 可以用自然语言生成解释，以证明特定建议的合理性，这增加了推荐系统的透明度。

### 1.2 基于 LLM 的推荐系统的不足

尽管 LLM 在推荐系统上有上述诸多优点，但仍然存在以下不足和限制：

+ 生成过程的不可控性：由于 LLM 生成过程的随机性，LLM 可能会推荐候选集中不存在的 `item`。解决该问题的一个思路是将候选集输入 `prompt`，但这种方法也仅限于候选集较小的场景，同时需要面对有限的上下文长度等问题；

+ 生成结果的不可控性：LLM 可能会生成无效或完全出乎意料的答案，即使提示在上下文中明确指定可能的答案集（小样本学习）也是如此；

![](img/ce6610a289168002e535dffb55896423.png)

+ 要求很高的`prompt`技巧：LLM 可能对输入提示高度敏感，可能不会完全遵循说明提示；

+ 推荐可能高度依赖于数据集：例如，同一部电影在不同平台上的评分可能不同。在这种情况下进行的评估可能会认为 LLM 的性能对于某些数据集来说是不可接受的；

+ 数据安全及隐私问题：最近的研究表明，LLM 中的通用知识与私有域数据中用户行为模式之间可能存在巨大差距。因此，零样本和少样本方法可能不足以在私有域中部署 LLM 推荐系统，同时也可能存在数据安全问题；

+ LLM 与推荐任务之间存在GAP：当前流行的 LLM 的预训练目标不包括任何推荐目标。因此，在解决复杂的专门任务的零样本和少样本设置下，这些 LLM 通常比现有的任务特定模型表现更差；

+ LLM 训练成本昂贵：`GPT3` 训练一次的成本约为 140 万美元，对于一些更大的 LLM（大型语言模型），训练成本介于 200 万美元至 1200 万美元之间；

+ LLM 输入受限：LLM 中有限的上下文长度使得很难将大量用户行为序列和/或候选项目合并到提示中。

## 二、百花齐放：当前流行方案一览

随着相关研究的广泛开展，已出现一些将 LLM 用于推荐系统的案例。主要都是 `Instruction Tuning` 的方式，即通过对面向推荐的指令数据进行指令微调 LLM ，以便有效地将隐私领域的用户行为与通用知识相结合。下面便结合部分案例进行讨论。

### 2.1 NIR

在 [NIR (Zero-Shot Next-Item Recommendation)](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2304.03153.pdf) 论文中，作者提出了一种在零样本设置中执行下一项推荐的提示策略。他们的方法分为以下三个步骤：

+ 候选集生成：作者认为，像 `GPT-3` 这样的 `PLM` 在给出 `zero-shot` 提示时会表现不佳，例如：Based on the movies I have watched, including..., can you recommend 10 movies to me？因为推荐空间很大，因此，在他们的实验中，他们使用基于用户过滤或项目过滤的外部推荐系统组件来首先构建候选项目集；

+ 三步 `Prompt`：此步骤使用 `GPT3` 和三个 `Prompt`：

  + 首先，他们提示 `GPT3` 使用观看历史记录中的项目来总结用户的偏好；

  + 接下来，他们使用 `GPT3` 的响应和提示中的候选电影来请求 `GPT-3` 按偏好降序选择代表性电影；

  + 最后，他们创建了一个推荐提示来引导 `GPT3` 从候选集中推荐 10 部与代表性电影最相似的电影。

+ 答案提取：他们使用基于规则的提取方法从 `GPT3` 的最终响应中获取推荐项目，提取的项目可用于下游应用，或用于性能评估。

![](img/4466263aa8f215f67eefc102414e640d.png)

### 2.2 Chat-REC

在 [Chat-REC(Chat-GPT Augmented Recommender System)](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2303.14524.pdf) 中，作者使用 LLM 来增强他们的会话推荐系统。他们首先使用用户个人资料（如年龄、性别、位置、兴趣等）、历史交互（如点击、购买或评价项目）和对话历史（可选）创建提示，然后要求 LLM 总结用户偏好。接下来，使用传统的推荐系统生成大量候选项目集，最后，LLM 缩小候选集范围以生成最终的推荐集。

![](img/db03c5818d4207ebaa6ef21f6b97f9ea.png)

除了上述过程外，Chat-Rec 的工作还包括以下几点：

+ 候选集压缩：首先将用户的资料和历史交互转换为 `Prompt` 输入到 LLM，使 LLM 了解用户的偏好。同时推荐系统生成的候选集就会提供给 LLM，并根据用户的偏好进一步排序和过滤候选集。这种方法可以确保向用户展示一个更小、更相关的物品集，以更好地命中用户的需求；

+ 冷启动：为了解决冷启动问题，本文引入了 `new item` 的外部信息，利用 LLM 来生成相应的 `embedding` 并进行缓存。当遇到新的 `item` 推荐时，计算物品 `embedding` 与用户请求和偏好的 `embedding` 之间的相似性，然后根据相似性检索最相关的 `item` 信息，并构建一个 `prompt` 输入到 `ChatGPT` 进行推荐；

+ 跨域推荐：LLM 实际上可以作为多视角的知识库。除了一个领域的目标产品（如电影），LLM 还对许多其他领域的产品有广泛的了解（如音乐和书籍），并且了解上述各领域的产品之间的关系。这种跨领域的推荐能力有可能大大扩展推荐系统的范围和相关性。

### 2.3 TALLRec

[TALLRec(Tuning framework for Aligning LLMs with Recommendations)](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.00447.pdf) 使用推荐数据微调 LLM。该方法结合了指令调优和推荐调优，以增强模型的效果。首先构建一组指令，其中每条指令都由指令输入和指令输出对组成。`Instruction Input` 定义任务和相应的任务输入，`Instruction Output` 定义任务输出。

接下来，在两个调优阶段训练 LLM：`instruct-tuning` 和 `rec-tuning`。`Instruction Tuning`阶段是LLM的一个标准训练过程，增强了LLM的泛化能力。作者使用来自 Stanford Alpaca 的 `self-instruct` 数据进行此调整。对于推荐调优，可以将用户的历史行为改写成如下所示的指令：

```
User Preference: $item_1$, $item_4$, …, $item_n$. User Unpreferenced: $item_2$, $item_3$, …, $item_{n-1}$. Will the user enjoy the target movie/book: $item_{n+1}$. [yes/no]
```

![](img/63aaecac88c533b8b16b62437184caa8.png)

作者使用 `LoRA` 来促进高效的轻量级微调，这可以通过低 `GPU` 成本和少量调优样本（少于 100 个）来完成。他们选择了 `LLaMA` LLM 来进行他们的实验，并表明他们训练的模型在少镜头设置中优于以前的基线。他们训练的模型还展示了强大的跨域传输能力。

### 2.4 InstructRec

[InstructRec(Recommendation as Instruction Following)](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.07001.pdf) 将推荐任务视为 LLM 遵循/执行的指令。为了表达用户的偏好、意图以及任务形式和上下文，他们设计了一种用于调整 LLM 的通用指令格式，如下：

![](img/940e813226c67719c1d4920b4c72ec66.png)

`User Preference` 是指用户对物品属性的个性化喜好。`User Intention`是指用户对某类物品的更直接的需求，`Task`形式就是对应的推荐任务。 此外，还利用了时间和地点等上下文信息。 作者引入了 39 个粗粒度的指令模板，覆盖了推荐、检索、个性化搜索等所有交互场景。

![](img/cd771efb2799d46de09173444888b4f7.png)

受自动提示策略（如`self-instruct`）的启发，作者提示了具有用户历史交互和评论的 `teacher-LLM` (`GPT-3.5`)，以偏好/意图/任务格式自动生成 252K 细粒度指令。 为了进一步增加指令的多样性，他们提出了手动策略，例如翻转指令的输入和输出结构，加强偏好和意图之间的相关性，以及添加 `CoT` 。

![](img/ec93d6220a8ca480a20788398253e047.png)

对于指令调优，作者选择了 3B `Flan-T5-XL` 模型。 超过模型 512 个标记长度限制的输入长度被截断。 对于更长的上下文长度，作者建议尝试使用 `LLaMA` 模型。 根据计算效率和模型容量，LLM被用作`reranker`。 在推理时，选择适当的指令模板，通过使用连接、插入和角色转换等操作来创建指令。 然后用候选项目集对该指令进行补充，并提供给 LLM 进行排名。

### 2.5 RecLLM

[RecLLM(Leveraging Large Language Models in Conversational Recommender Systems)](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.07961.pdf) 提出了使用 LLM 构建端到端可控的大规模会话推荐系统 (Conversational Recommender Systems, `CRS`) 的路线图。 他们提出了几种将 LLM 与外部资源（例如用户资料数据库、推荐引擎等）集成的方法，以构建如下所示的 `CRS` 系统。

![](img/c37967fedd8c5890725bffe49ba50466.png)

`CRS` 通过参与实时多轮对话，让用户可以灵活地控制他们的推荐。 该系统学习根据用户的直接反馈改进推荐，由于缺乏现有的对话数据集，他们使用可控的基于 LLM 的用户模拟器来生成合成对话。 作者没有利用交互历史来理解用户偏好，而是使用 LLM，将用户配置文件作为可解释的自然语言使用，并使用它们来个性化会话。

![](img/75cd93278f2de70a27693cdd397b2bcd.png)

在本文中，推荐的检索方法之一是使用双编码器架构，其中 LLM 用作上下文编码器。 用于排名的 LLM 还执行中间推理，以生成自然语言解释为什么选择一个项目。 检索模型、排名模型以及对话管理器根据包含生成的合成会话的推荐数据进行微调。 作为概念验证，作者构建了一个 `CRS`（使用 `LaMDA` 作为基础 LLM），该 `CRS` 提供来自所有公共 YouTube 视频语料库的推荐。

## 三、见微知著：通过实践理解细节

本节将使用 [TALLRec 的开源项目](https://link.zhihu.com/?target=https%3A//github.com/SAI990323/TALLRec)来体验 LLM在推荐系统中的实践过程．

### 3.1 数据及模型

该项目提供了两份数据：Moive和Book数据，在此使用Moive数据(训练集共8000条)，数据样例如下：

```
{
    &quot;instruction&quot;: &quot;Given the user&#x27;s preference and unpreference, identify whether the user will like the target movie by answering \&quot;Yes.\&quot; or \&quot;No.\&quot;.&quot;,
    &quot;input&quot;: &quot;User Preference: \&quot;Paris, Texas (1984)\&quot;, \&quot;Rebel Without a Cause (1955)\&quot;, \&quot;Return of the Pink Panther, The (1974)\&quot;, \&quot;Ace Ventura: Pet Detective (1994)\&quot;, \&quot;Magnificent Seven, The (1954)\&quot;, \&quot;Star Trek: The Wrath of Khan (1982)\&quot;, \&quot;Cat People (1982)\&quot;, \&quot;Orlando (1993)\&quot;, \&quot;Dave (1993)\&quot;\nUser Unpreference: \&quot;Kalifornia (1993)\&quot;\nWhether the user will like the target movie \&quot;Perez Family, The (1995)\&quot;?&quot;,
    &quot;output&quot;: &quot;No.&quot;
},
```

选用7B的 `LLaMa` 模型，并从 `alpaca-lora` 加载 `Lora` 的 `checkpoint`，如下:

```
base_model=&#x27;decapoda-research/llama-7b-hf&#x27;
train_data=&#x27;./data/movie/train.json&#x27;
val_data=&#x27;./data/movie/valid.json&#x27;
instruction_model=&#x27;tloen/alpaca-lora-7b&#x27;
```

需要注意的是，源代码在多卡训练时效率较低，可采用 `Torch` 的数据并行方式以加快训练速度:

```
CUDA_VISIBLE_DEVICES=$1 torchrun --nproc_per_node=8 finetune_rec.py
```

### 3.2 训练与评估过程

在论文中, 作者采用小数据采样的方式验证模型训练的正确性, 如下图即是 `samples=64` (表示 `K-shot` 训练) 时的训练 loss

![](img/3ed146ff22ae4310b8429f942ce0920c.png)

全数据集训练的过程, 与小数据集基本一致

![](img/b7c73357dc448cf0ed7b501654454f0a.png)

训练过程中在验证集上的`eval` 得到的`AUC`曲线如下, 可看到逐步增大到收敛的过程

![](img/479cb22b947aefb34d7b4f25572114fe.png)

### 3.3 LLM 应用于推荐系统的展望

将 LLM 用于推荐任务的想法仍处于初级阶段。LLM 有几个问题，例如幻觉(`hallucinations`)和偏见仍然是一个悬而未决的问题。 下面列出了一些未来需要探索的潜在领域：

+ 自动提示设计(`Automated prompt design`)方法可以让系统更有效地设计提示；



+ 当前多数基于 LLM 的推荐系统都依赖于单轮指令跟随方法。将它们扩展到多轮对话场景可以提高这些推荐模型的泛化能力；
+ 随着 LLM 的研究继续支持更长的上下文长度，将使得能够对长行为序列进行建模；
+ LLM 可以扩展对多模式指令和反馈的支持，以更深入地了解用户意图和偏好；
+ AI 生成的内容 (AIGC) 还可以补充人类生成的内容，为 LLM 创建更高效的训练和微调策略。

## 参考资料

[1] [Tuning Large Language Models for Recommendation Tasks](https://link.zhihu.com/?target=https%3A//blog.reachsumit.com/posts/2023/05/tuning-llm-for-recsys/)

[2] [Zero and Few Shot Recommender Systems based on Large Language Models](https://link.zhihu.com/?target=https%3A//blog.reachsumit.com/posts/2023/04/llm-for-recsys/)

[3] [`arxiv.org/pdf/2303.14524.pdf`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2303.14524.pdf)

[4] [`arxiv.org/pdf/2305.00447.pdf`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.00447.pdf)

[5] GitHub - `SAI990323/TALLRec`

[6] [`Chat-REC`、`InstructRec`（LLM 大模型用于推荐系统）_上杉翔二的博客-CSDN博客](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_39388410/article/details/130000501)

[7] [`arxiv.org/pdf/2305.07961.pdf`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2305.07961.pdf)

[8] `2305.02182.pdf` (arxiv.org)

[9] [`WLiK/LLM4Rec`: The repo of Large Lanuage Model (LLM) for recommendation system.](https://github.com/WLiK/LLM4Rec)

> 离愁渐远渐无穷，迢迢不断如春水。—— 欧阳修 《踏莎行·候馆残梅》
