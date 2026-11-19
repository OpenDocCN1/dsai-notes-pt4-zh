
# 强化微调

> 来源：[https://v11enp9ok1h.feishu.cn/docx/PGnsdc01BoSDENxNnKrce8wun2c](https://v11enp9ok1h.feishu.cn/docx/PGnsdc01BoSDENxNnKrce8wun2c)

强化[微调](https://github.com/modelscope/ms-swift/tree/main/examples/train/rft/rft.py)是目前模型训练非常重要的功能之一，它本身的实现是多种多样的，SWIFT目前已经支持了强化微调所需要的原子能力，如采样、强化学习和微调。目前我们提供了拒绝采样微调的一个具体示例，可以查看这里。

## 强化微调的概念

强化微调是从2022年开始（甚至更早）就被提出的概念。其方式一般有下列流程：

1. 使用某个模型生成数据，或进行原始数据扩充
2. 使用数据训练目标模型
3. 如果有必要，重复上述过程

### 步骤1

+ 如果生成数据的模型是更大的模型，如`Qwen-Max`、`DeepSeek-V3/R1`等，则该强化微调可以理解为蒸馏
+ 如果生成数据的模型是本模型，则可以理解为自我提升（`self-improvement`）微调
+ 如果采样过程是采样一个`batch`，然后通过KL散度和`reward`进行拟合训练并不断循环，则可以理解为PPO、GRPO等`on-policy`算法
+ 采样数据的算法包含蒙特卡洛采样、`do_sample`采样、`group beam search`、`dvts`等
+ 采样过程可以引入ORM（结果判断），PRM（过程打分），多样性过滤，语种过滤等

### 步骤2

+ 如果使用SFT，则称为拒绝采样微调
+ 如果是强化学习，则称为强化学习微调

### 步骤3

+ 如果使用更大的模型蒸馏，例如更大模型的蒙特卡洛采样蒸馏，一般不会有循环
+ 如果使用本模型进行采样，或者PPO等算法，则会有循环

泛泛来说，常见强化微调的方式有下面几种：

1. 蒸馏：使用蒙特卡洛、`do_sample`等方式从超大模型中采样大量优质数据，训练小模型
2. 自我提升：从本模型中采样部分优质数据，筛选后训练本模型，循环执行
3. `on-policy` RL：使用PPO、GRPO等方式循环训练

采样过程一般很漫长，比训练过程漫长的多。如果使用`GPT`等模型蒸馏数据，则需要购买`token`。因此，强化微调的时间成本和花费成本比较高，所以一般作为微调的补充机制出现，当然也有特例，例如最近的`DeepSeek-R1`。

De[epSe](https://zhuanlan.zhihu.com/p/197149872727)ek-R1使用了GRPO算法从零使`base`模型涌现CoT能力，该方法需要大规模集群支持，且模型需要足够大才能发生能力涌现，在本文中不详细讨论。如果需要了解该过程，请查看论文解析。

有关强化微调的一些论文：

+ 拒绝采样微调：https://arxiv.org/pdf/2308.01825
+ ReST：https://arxiv.org/pdf/2308.08998
+ B-STAR：https://arxiv.org/pdf/2412.17256
+ DeepSeekMath：https://arxiv.org/pdf/2402.03300
+ Qwen-math-PRM：https://arxiv.org/pdf/2501.07301
+ DeepSeek-R1：[h](https://github.com/deepseek-ai/DeepSeek-R1/tree/main)ttps://github.com/deepseek-ai/DeepSeek-R1/tree/main

## 什么时候使用强化微调

在`LLaMA3`之后，我们发现一个非常明显但却是不常被提及的特点：使用某个含有CoT的`train`数据集训练`Instruct`模型，再通过对应的`test`集进行评测，会发现`test`集评测效果变差。例如，使用`gsm8k`训练集训练`llama3.1-8b-instruct`，对生成的`ckpt`使用`test`集进行评测，会发现掉点。

这个[特性主要来源于模](https://zhuanlan.zhihu.com/p/19269451950)型的知识遗忘问题。在模型厂商的微调中，会加入非常多的CoT数据集，模型在解决数学任务的时候，用到的能力很有可能不是来自于`math`数据集，而是来自`arc`数据集，这个推论有一些工作可以证明。在继续训练通用任务后，知识遗忘破坏了模型原有能力，导致了掉点。

然而，优先使用微调方式训练模型总是正确的。微调可以使模型快速适应数据集的分布，并且微调的成本很低。当有如下条件之一时使用强化微调：

1. 已经微调过模型，能力不满足需求
2. 需要更强的CoT能力
3. 对基模型训练通用能力，而原始数据集已经导致模型效果无法提升
4. 对应`query`的输出结果可以相对准确地评估好坏，例如结果清晰（数学，代码），过程清晰（翻译，风格）等

强化微调非常依赖于`reward`评估是否准确。如果评估结果不准确，可能导致模型训练原地震荡，甚至越训越差。

## SWIFT的实现

SWIFT支持`sample`命令，该命令就是用于模型采样。目前支持的采样方式有：

+ `do_sample`：`sample`方式对模型进行采样，该方式支持对开源模型进行采样，后续会支持模型蒸馏
+ `sample`方式后续会支持URL采样，用于大模型蒸馏
+ `mcts`：蒙特卡洛采样，该方式在`PR`中，后续会支持
+ `dvts`：调研中

目前我们给出了一个较为通用的[RFT脚本](https://github.com/modelscope/ms-swift/tree/main/examples/train/rft/rft.py)。该脚本适用于自我提升方式的训练，且支持动态调整采样温度值、PRM阈值等超参数，并且训练方式灵活可变（微调、DPO等；或者每次迭代重新训练原模型或继续训练上个迭代的模型，甚至加载上个迭代的所有训练状态等）。开发者可以在该脚本中增加其他数据过滤（生成的数据集中，`id`相同的行来自同一个`query`），例如多样性判断、语种判断等。

## 实验结果

我们对该RFT脚本针对数学领域使用`competition_math`数据集进行了训练和评测，结果如下：

可以看到，使用`competition_math`直接SFT后，`instruct`模型的掉点十分严重。而RFT后模型能力有提升，即使对`Qwen2.5_math_7b_instruct`这个SOTA的`math`模型也同样有一定提升空间。

特别地，针对`Qwen2.5_math_7b_instruct`我们测试了`gsm8k`的指标：

可以看到，RFT训练后`gsm8k`指标变化不大，并没有出现前述的掉点现象。

## 未来计划

1. 更多的采样方式，如`MCTS`
2. 超大模型蒸馏训练
3. 以`PPO`为主的`on-policy`训练
