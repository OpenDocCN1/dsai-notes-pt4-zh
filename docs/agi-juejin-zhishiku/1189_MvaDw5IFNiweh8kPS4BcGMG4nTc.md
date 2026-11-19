
# LLM（七）：RLHF——当语言模型遇上强化学习

> 来源：[https://agijuejin.feishu.cn/docx/JPlydmsR8oGRUVxijOYc3WFdnUb](https://agijuejin.feishu.cn/docx/JPlydmsR8oGRUVxijOYc3WFdnUb)
>
> 作者：紫气东来
>
> 原文：[https://zhuanlan.zhihu.com/p/613315873](https://zhuanlan.zhihu.com/p/613315873)

过去几年里，以`chatGPT`为代表的基于`prompt`范式的大型语言模型（`Large Language Model`，`LLM`）取得了巨大的成功。然而，对生成结果的评估是主观和依赖上下文的，这些结果难以用现有的基于规则的文本生成指标（如`BLUE`和`ROUGE`）来衡量。除了评估指标，现有的模型通常以预测下一个单词的方式和简单的损失函数（如交叉熵）来建模，没有显式地引入人的偏好和主观意见。

因此，训练阶段，如果直接用人的偏好（或者说人的反馈）来对模型整体的输出结果计算`reward`或`loss`，显然是要比上面传统的“给定上下文，预测下一个词”的损失函数合理的多。基于这个思想，便引出了本文要讨论的对象——`RLHF`（`Reinforcement Learning from Human Feedback`）：即使用强化学习的方法，利用人类反馈信号直接优化语言模型。

## 一、`RLHF` 的基本原理

`RLHF`的训练过程可以分解为三个核心步骤：

+ 多种策略产生样本并收集人类反馈

+ 训练奖励模型

+ 训练强化学习策略，微调 `LM`

![](img/85c2d957f709f635079eef3b11b90141.png)

### Step 1: 收集人类反馈

这部分用来产生`summaries` 的模型主要有两类：

+ 预训练模型，即只经过预料库训练而不经过 `fine-tune` 的模型

+ 监督基线模型，即在预训练模型基础上使用测试数据集 `fine-tune` 的模型

以上模型有以下4个规模：

![](img/3b1f94e45718f2cabd132d6dae7b0e17.png)

对于上述模型产生的结果，由专门的研究人员 `labeler` 去进行相对好坏的的评价。

### Step 2: 训练奖励模型

一个奖励模型（`Reward Model`，`RM`）的目标是刻画模型的输出是否在人类看来表现不错。即，输入 [`prompt`，模型生成的文本] ，输出一个刻画文本质量的标量数字。用公式表示如下：

\operatorname{loss}\left(r_\theta\right)=-E_{\left(x, y_0, y_1, i\right) \sim D}\left[\log \left(\sigma\left(r_\theta\left(x, y_i\right)-r_\theta\left(x, y_{1-i}\right)\right)\right)\right]

其中 `x`, `y` 分别表示 `post` 和 `summary`， `r_\theta` 表示参数为 `\theta` 的奖励模型的值， `\sigma` 表示`sigmod`函数。

![](img/272baaa71d7295650d34aee9cad4b9c4.png)

奖励模型接收一系列文本并返回一个标量奖励，数值上对应人的偏好。我们可以用端到端的方式用 `LM` 建模，或者用模块化的系统建模（比如对输出进行排名，再将排名转换为奖励）。这一奖励数值将对后续无缝接入现有的 `RL` 算法至关重要。

### Step 3: 训练策略模型

首先将初始语言模型的微调任务建模为强化学习（`RL`）问题，因此需要定义策略（`policy`）、动作空间（`action space`）和奖励函数（`reward function`）等基本要素。

显然，策略就是基于该语言模型，接收`prompt`作为输入，然后输出一系列文本（或文本的概率分布）；而动作空间就是词表所有`token`在所有输出位置的排列组合（单个位置通常有50k左右的`token`候选）；观察空间则是可能的输入`token`序列（即`prompt`），显然也相当大，为词表所有`token`在所有输入位置的排列组合；而奖励函数（`reward`）则是基于上一章节我们训好的`RM`模型计算得到初始`reward`，再叠加上一个约束项来。整个过程如下所示：

![](img/2021cd93a763b4f4739dc0462d74c631.png)

而对于强化学习的算法，常见的可行方案是使用策略梯度强化学习（`Policy Gradient RL`）算法、近端策略优化（`Proximal Policy Optimization`，`PPO`）微调初始 `LM` 的部分或全部参数。关于策略梯度强化学习的原理，可参考笔者之前的专栏文章，在此不予赘述：

[  ](https://zhuanlan.zhihu.com/p/450690041)

**语言模型的强化学习建模**

设词表为 `\Sigma` ，语言模型为 `\rho` ，那么对于长度为 `n` 的序列的概率分布可表示为

\rho\left(x_0 \cdots x_{n-1}\right)=\prod_{0 \leq k<n} \rho\left(x_k \mid x_0 \cdots x_{k-1}\right)

输入空间 `X=\Sigma ^{\leq m}` ，输出空间 `X=\Sigma ^{n}` ，对于输入 `x \in X` 可能长度1000的文章， `y \in Y` 可能是长度为100的总结。那么由文章 `x` 生成总结 `y` 的概率可表示为 `\rho(y|x) = \rho(xy)/\rho(x)` 。

初始化策略 `\pi=\rho` ，然后使用 `PPO` 算法更新策略 `\pi` ，奖励函数定义为 `r` ，则奖励的期望值可表示为：

\mathbb{E}_\pi[r]=\mathbb{E}_{x \sim \mathcal{D}, y \sim \pi(\cdot \mid x)}[r(x, y)]

接下来，`PPO` 算法优化奖励函数计算步骤如下：

+ 将提示 `x` 输入初始 `LM` 和当前微调的 `LM`，分别得到了输出文本 `y_1, y_2` ，将来自当前策略的文本传递给 `RM` 得到一个标量的奖励 `r_\theta` 。

+ 将两个模型的生成文本进行比较计算差异的惩罚项，通常设计为输出词分布序列之间的 `Kullback–Leibler` (`KL`) 散度的缩放，即 `r=r_\theta-\beta r_{KL}` ，其中 `r_{KL} = \log [\pi^{RL}(y \mid x) / \pi^{SFT}(y \mid x)]` 。

+ 这一项被用于惩罚 `RL` 策略在每个训练批次中生成大幅偏离初始模型，以确保模型输出合理连贯的文本。如果去掉这一惩罚项可能导致模型在优化中生成乱码文本来愚弄奖励模型提供高奖励值。

![](img/23b1767f49e01815d514229c47a41f39.png)

最后根据 `PPO` 算法，我们按当前批次数据的奖励指标进行优化（来自 `PPO` 算法 `on-policy` 的特性）。`PPO` 算法是一种信赖域优化（`Trust Region Optimization`，`TRO`）算法，它使用梯度约束确保更新步骤不会破坏学习过程的稳定性，另外也可以使用 `A2C` (`synchronous advantage actor-critic`) 算法来优化梯度。

## 二、结果及分析

OpenAI请了一些研究者来做`labeler`，每次给他们一对摘要，让他们判断哪个比较好一些。可以想见，如果每次都给一对同样是人力生成的摘要，那么大概50%的时间会是第一篇比第二篇更好。这就是下图黑色虚线的位置。纵坐标"Fraction preferred to ref"，指的是有多大比例的第一篇比第二篇文章要好。

如果每次第一篇摘要都是模型生成的，那么我们可以比较出模型和人力生成摘要的差距（或优越性）。最上面黄色的线说明`RLHF`模型摘要的质量远超人工摘要。且模型越大，质量越好。

![](img/0ea7ecc41d40b2d1119e869b5e1b3b90.png)

下面的左图显示了模型在`CNN/DM`数据集上的效果，这算是`transfer learning`效果的一个展示，毕竟模型只是用`TL;DR`训练的，从来没有见过`CNN/DM`的数据。从效果上看毫无疑问，传统的`Supervised`和`Pretrain Only`的`GPT3`模型最弱。

看上去黄线即作者训练出的模型要弱于著名的`T5`在`CNN/DM`上`fine tunning`的效果。但是模型是用`summary`在24到48之间的`posts`训练出来的。 如果区分不同的`summary`长度，我们在右图可以看到，`transfer learning`状态下的模型和`fine tune`过的`T5`表现相当接近。

![](img/53b1628e504703b52fbeb924ff0718cf.png)

下面看一下 `reward model`。 左图比较了在不同的 `KL penalty` 权重下模型的表现。我们看到当 `KL penalty` 小即在图的左端，模型的实际表现很糟。这也说明了`KL`项的必要性。随着惩罚项增大，`RM`预测的效果开始提升，但当惩罚项过大时，`RM` 预测不再提升，即与`SFT`保持一致。右图说明了对于`reward model`，模型越大，数据越多效果越好。

![](img/0f9b04484d8444cd3a5330a618c73302.png)

文章还从覆盖度、准确度、流畅性、整体质量四个方面对各模型的结果进行了对比。发现`HF`模型在每一个方面都有优势，而且在覆盖度方面很突出。

![](img/dad5cd1fc6d1fd001481573bfda12572.png)

![](img/7060d3f1c1f34612eb8f0e43cbeaa0cf.png)

### `RLHF` 的局限性

虽然以`ChatGPT`为代表的`RLHF`技术非常有影响力，引发了巨大的关注，但仍然存在若干局限性：

1. `RLHF` 范式训练出来的这些模型虽然效果更好，但仍然可能输出有害或事实上不准确的文本。这种不完美则是 `RLHF` 的长期挑战和优化目标。

2. 在基于 `RLHF` 范式训练模型时，人工标注的成本是非常高昂的，而 `RLHF` 性能最终仅能达到标注人员的知识水平。此外，这里的人工标注主要是为`RM`模型标注输出文本的排序结果，而若想要用人工去撰写答案的方式来训练模型，那成本更是不可想象的。

3. `RLHF`的流程还有很多值得改进的地方，其中，改进 `RL` 优化器显得尤为重要。`PPO` 是一种基于信赖域优化的相对较旧的`RL`算法，但没有其他更好的算法来优化 `RLHF` 了。

## 三、`RLHF` 的实践

首先介绍一些相关的开源工作：

[关于 `RLHF` 的第一个项目，来自 `OpenAI`](https://link.zhihu.com/?target=https%3A//github.com/openai/lm-human-preferences)

一些 `PyTorch` 的 `repo`：

+ https://github.com/lvwerra/trl

+ https://github.com/CarperAI/trlx

+ [https://github.com/allenai/RL4LMs](https://link.zhihu.com/?target=https%3A//github.com/allenai/RL4LMs)

接下来以`trlx`的 [`rlhf案例`](https://link.zhihu.com/?target=https%3A//github.com/CarperAI/trlx/tree/main/examples/summarize_rlhf)为例，深入了解整个过程

### 3.1 奖励模型(`RM`) 的训练

**数据集加载与预处理**

使用开源 [`数据集`](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/CarperAI/openai_summarize_comparisons/viewer/CarperAI--openai_summarize_comparisons/train)，创建一个由字典的组成的列表，每一个字典有两个`key`，分别是 `chosen` 和 `rejected` ，存储的分别是 `prompt` 加上对应的`summary`。

```
def create_comparison_dataset(path="CarperAI/openai_summarize_comparisons", split="train"):
    dataset = load_dataset(path, split=split)
    pairs = []
    for sample in tqdm(dataset):
        pair = {}
        prompt = sample["prompt"]
        chosen_summary = sample["chosen"]
        rejected_summary = sample["rejected"]
        if chosen_summary == rejected_summary:
            continue
        if len(chosen_summary.split()) < 5 or len(rejected_summary.split()) < 5:
            continue
        pair["chosen"] = prompt + "\n" + chosen_summary
        pair["rejected"] = prompt + "\n" + rejected_summary
        pairs.append(pair)
    return pairs 
```

针对处理完的 `pair` 对，进行分词处理，并构造成可供训练的数据集形式

```
class PairwiseDataset(Dataset):
    def __init__(self, pairs, tokenizer, max_length):
        self.chosen_input_ids = []
        self.chosen_attn_masks = []
        self.rejected_input_ids = []
        self.rejected_attn_masks = []
        for pair in tqdm(pairs):
            chosen, rejected = pair["chosen"], pair["rejected"]
            chosen_encodings_dict = tokenizer(
                "<|startoftext|>" + chosen + "<|endoftext|>",
                truncation=True,
                max_length=max_length,
                padding="max_length",
                return_tensors="pt",
            )
            rejected_encodings_dict = tokenizer(
                "<|startoftext|>" + rejected + "<|endoftext|>",
                truncation=True,
                max_length=max_length,
                padding="max_length",
                return_tensors="pt",
            )
            self.chosen_input_ids.append(chosen_encodings_dict["input_ids"])
            self.chosen_attn_masks.append(chosen_encodings_dict["attention_mask"])
            self.rejected_input_ids.append(rejected_encodings_dict["input_ids"])
            self.rejected_attn_masks.append(rejected_encodings_dict["attention_mask"])

    def __len__(self):
        return len(self.chosen_input_ids)

    def __getitem__(self, idx):
        return (
            self.chosen_input_ids[idx],
            self.chosen_attn_masks[idx],
            self.rejected_input_ids[idx],
            self.rejected_attn_masks[idx],
        )
```

上述数据不便于同时输入模型，需要进一步整理数据，构造成如下形式：



## 3.1 奖励模型（RM）的训练

### 数据预处理

+ `input_ids`: 将 `input_ids` 的 `chosen` 和 `rejected` 在0维度上 concat

+ `attention_mask`: 将 `attention_mask` 的 `chosen` 和 `rejected` 在0维度上 concat

+ `labels`: 将 `chosen` 部分置0，`rejected` 部分置1，并在0维度上concat

需要说明的是，经过上述处理，`batch size` 变为原来的2倍

```python
class DataCollatorReward:
    def __call__(self, data):
        batch = {}
        batch["input_ids"] = torch.cat([f[0] for f in data] + [f[2] for f in data])
        batch["attention_mask"] = torch.cat([f[1] for f in data] + [f[3] for f in data])
        batch["labels"] = torch.tensor([0] * len(data) + [1] * len(data))
        return batch
```

### 构建奖励模型

`RM` 的结构相对简单，即 `transformer` 结构+线性分类头，比较复杂的部分是`loss`公式的计算

`\operatorname{loss}\left(r_\theta\right)=-E_{\left(x, y_0, y_1, i\right) \sim D}\left[\log \left(\sigma\left(r_\theta\left(x, y_i\right)-r_\theta\left(x, y_{1-i}\right)\right)\right)\right]`

```python
class GPTRewardModel(nn.Module):
    def __init__(self, model_path):
        super().__init__()
        model = AutoModelForCausalLM.from_pretrained(model_path)
        self.config = model.config
        # `gpt-neo(x)` models use `hidden_size` attribute names instead of `n_embd``
        self.config.n_embd = self.config.hidden_size if hasattr(self.config, "hidden_size") else self.config.n_embd
        self.transformer = model.transformer
        self.v_head = nn.Linear(self.config.n_embd, 1, bias=False)
        self.tokenizer = AutoTokenizer.from_pretrained("EleutherAI/gpt-j-6B")
        self.tokenizer.pad_token = self.tokenizer.eos_token
        self.PAD_ID = self.tokenizer(self.tokenizer.pad_token)["input_ids"][0]

    def forward(
        self,
        input_ids=None,
        past_key_values=None,
        attention_mask=None,
        token_type_ids=None,
        position_ids=None,
        head_mask=None,
        inputs_embeds=None,
        mc_token_ids=None,
        labels=None,
        return_dict=False,
        output_attentions=False,
        output_hidden_states=False,
    ):
        loss = None
        transformer_outputs = self.transformer(
            input_ids,
            past_key_values=past_key_values,
            attention_mask=attention_mask,
            token_type_ids=token_type_ids,
            position_ids=position_ids,
            head_mask=head_mask,
            inputs_embeds=inputs_embeds,
        )

        hidden_states = transformer_outputs[0]

        rewards = self.v_head(hidden_states).squeeze(-1)
        chosen_end_scores = []
        rejected_end_scores = []

        # Split the inputs and rewards into two parts, chosen and rejected
        assert len(input_ids.shape) == 2
        bs = input_ids.shape[0] // 2
        chosen = input_ids[:bs]
        rejected = input_ids[bs:]
        chosen_rewards = rewards[:bs]
        rejected_rewards = rewards[bs:]

        loss = 0
        inference = False
        for i in range(bs):
            if torch.all(torch.eq(chosen[i], rejected[i])).item():
                c_inds = (chosen[i] == self.PAD_ID).nonzero()
                c_ind = c_inds[0].item() if len(c_inds) > 0 else chosen.shape[1]
                chosen_end_scores.append(chosen_rewards[i, c_ind - 1])
                inference = True
                continue

            # Check if there is any padding otherwise take length of sequence
            c_inds = (chosen[i] == self.PAD_ID).nonzero()
            c_ind = c_inds[0].item() if len(c_inds) > 0 else chosen.shape[1]
            r_inds = (rejected[i] == self.PAD_ID).nonzero()
            r_ind = r_inds[0].item() if len(r_inds) > 0 else rejected.shape[1]
            end_ind = max(c_ind, r_ind)

            # Retrieve first index where trajectories diverge
            divergence_ind = (chosen[i] != rejected[i]).nonzero()[0]
            assert divergence_ind > 0

            # Index into the correct rewards
            c_truncated_reward = chosen_rewards[i][divergence_ind:end_ind]
            r_truncated_reward = rejected_rewards[i][divergence_ind:end_ind]

            # Append the last rewards to the list of end scores
            chosen_end_scores.append(c_truncated_reward[-1])
            rejected_end_scores.append(r_truncated_reward[-1])

            # Compute loss based on truncated rewards (ignore padding)
            loss += -torch.log(torch.sigmoid(c_truncated_reward - r_truncated_reward)).mean()
        loss = loss / bs

        if not inference:
            chosen_end_scores = torch.stack(chosen_end_scores)
            rejected_end_scores = torch.stack(rejected_end_scores)

        if inference:
            chosen_end_scores = torch.stack(chosen_end_scores)
            return {"chosen_end_scores": chosen_end_scores}

        return {
            "loss": loss,
            "chosen_end_scores": chosen_end_scores,
            "rejected_end_scores": rejected_end_scores,
        }
```

将以上部分组合起来，即可以训练`RM`

```python
# Initialize the reward model from the (supervised) fine-tuned GPT-J
model = GPTRewardModel("CarperAI/openai_summarize_tldr_sft")

# Freeze the first 70% of the hidden layers of the reward model backbone
layers = model.transformer.h
num_layers = len(layers)
num_unfrozen = int(0.3 * num_layers)
for layer in layers[:-num_unfrozen]:
    layer.requires_grad_(False)

# Create the comparisons datasets
data_path = "CarperAI/openai_summarize_comparisons"
train_pairs = create_comparison_dataset(data_path, "train")
val_pairs = create_comparison_dataset(data_path, "test")

# Make pairwise datasets for training
max_length = 550
train_dataset = PairwiseDataset(train_pairs, tokenizer, max_length=max_length)
val_dataset = PairwiseDataset(val_pairs, tokenizer, max_length=max_length)

# Create the collator to gather batches of pairwise comparisons
data_collator = DataCollatorReward()

Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    compute_metrics=compute_metrics,
    eval_dataset=val_dataset,
    data_collator=data_collator,
).train()
```

下面分别是训练集和验证集上的`loss`，以及验证集上的精度。

![](img/b38471d7a69b2cbb7df285ecf91f78b9.png)

### 3.2 策略模型（PPO）的训练

由于`PPO`算法的值函数可以是一个深度学习模型，在本例中则是一个 `transformer` 模型，而策略梯度方法的基本思想将值函数表示为策略参数的某个函数，然后可以根据`RM`的反馈值进行更新。

#### 标准化（Normalization）

由于 `reward scores` 方差较大，因此需要根据人为结果做差以实现标准化，即

`f(x)=r(x)−r(x_{ref})`

其中 `r(x)`, `r(x_{ref})` 分别表示模型得分和人为得分。代码实现如下：

```python
def reward_fn(samples: List[str]):
    # get humans summarizes
    posts = [sample.split('TL;DR')] for sample in samples]
    ref_samples = [post + 'TL;DR' + post_summ_dict[post] for post in post]
    samples_encodings = reward_tokenizer(samples)
    samples_scores = reward_model(**samples_encodings) # get scores from reward model for samples
    ref_samples_encodings = reward_tokenizer(ref_samples) # get scores from reward model corresponding references samples
    ref_samples_scores = reward_model(**ref_samples_encodings)
    norms_rewards = samples_scores - ref_samples_scores
    return norms_rewards
```

#### KL 散度（KL Divergence）

当使用 `PPO` 做 `fine-tuning` 的时候，`summary` 由策略(`LLM`)生成。生成的 `summary` 传到奖励模型生成奖励分，进而更新策略。由于上述操作是 `batch-wise` 的，同时由于 `RL` 训练的噪声很大，特别是在初始阶段，这些可能会导致策略偏移过大。为防止这个问题，引进KL散度作为惩罚项，以避免策略模型偏差过大。

`r=r_\theta-\beta r_{\mathrm{KL}}`

`r_{KL} = \log \left[\frac{\pi^{\mathrm{RL}}(y \mid x)}{\pi^{\mathrm{SFT}}(y \mid x)}\right]`

其中 `r_{\theta}` 表示奖励模型的输出分数，`\beta` 表示系数，`\pi^{\mathrm{RL}} {(y \mid x)}` 表示策略模型，`\pi^{\mathrm{SFT}} {(y \mid x)}` 表示监督模型。

下图为`PPO`对 `SFT` 模型做 `fine-tuning` 过程的`loss`曲线。

![](img/ebf251d2a357cc3dae9188f4647d409b.png)

`PPO` 的目标在于最大化 `reward` 值，下图是训练过程中 `reward` 值的变化曲线

![](img/91e5ba0b7340c9ebf48ca9a3396c5f1b.png)

笔者复现过主流的`RLHF`方案，包括 `ColossalAI-Chat`, `DeepSpeed-Chat`, `safe-RLHF`，复现过程的部分细节可参考：

[  ](https://zhuanlan.zhihu.com/p/621391363?)

## 参考资料

[1] [https:// arxiv.org/](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2009.01325.pdf)pdf/2009.0132 5.pdf



[2] [`https://wandb.ai/ayush-thakur/RLHF/reports/Understanding-Reinforcement-Learning-from-Human-Feedback-RLHF-Part-1--VmlldzoyODk5MTIx`](https://link.zhihu.com/?target=https%3A//wandb.ai/ayush-thakur/RLHF/reports/Understanding-Reinforcement-Learning-from-Human-Feedback-RLHF-Part-1--VmlldzoyODk5MTIx)

[3] [`Illustrating Reinforcement Learning from Human Feedback (RLHF)`](https://link.zhihu.com/?target=https%3A//huggingface.co/blog/rlhf)

[4] [`抱抱脸：ChatGPT背后的算法——RLHF | 附12篇RLHF必刷论文 - 知乎 (zhihu.com)`](https://zhuanlan.zhihu.com/p/592671478)

[5] [`https://wandb.ai/carperai/summarize_RLHF/reports/Implementing-RLHF-Learning-to-Summarize-with-trlX--VmlldzozMzAwODM2#dataset`](https://link.zhihu.com/?target=https%3A//wandb.ai/carperai/summarize_RLHF/reports/Implementing-RLHF-Learning-to-Summarize-with-trlX--VmlldzozMzAwODM2%23dataset)

[6] [`Fine-Tuning Language Models from Human Preferences (arxiv.org)`](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1909.08593.pdf)

[7] [`The Full Story of Large Language Models and RLHF`](https://link.zhihu.com/?target=https%3A//www.assemblyai.com/blog/the-full-story-of-large-language-models-and-rlhf/)

> 细雨斜风作晓寒，淡烟疏柳媚晴滩。—— 苏轼 《浣溪沙》
