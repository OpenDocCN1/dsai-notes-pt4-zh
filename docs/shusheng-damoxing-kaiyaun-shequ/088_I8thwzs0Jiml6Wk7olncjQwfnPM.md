
# L2 书生大模型强化学习 RL 实践

> 来源：[https://aicarrier.feishu.cn/docx/Ko7FdpWESoRDGGxNjEhcLt77nFh](https://aicarrier.feishu.cn/docx/Ko7FdpWESoRDGGxNjEhcLt77nFh)

本文带你使用`xturner`，在`gsm8k`数据集上使用GRPO算法进行对于`InternLM`的训练

## 原理介绍：PPO,DPO,GRPO

RLVR是大模型训练的重要组成部分，包含监督微调（SFT），建立奖励模型（RM）和强化学习（RL）。其中强化学习阶段主要使用PPO,DPO和GRPO三种算法进行优化。

### ①PPO

PPO 是 OpenAI 在 2017 年提出的一种强化学习算法，其设计的初衷是为了解决传统策略梯度算法（Policy Gradient）中训练不稳定、更新步长难以确定的问题。在 RLHF 的背景下，PPO 的稳定性和可靠性使其成为优化语言模型的首选。

PPO的损失函数为：

其中核心的损失函数为： ，其中两个核心项与核心机制分别为优势函数 和概率比

优势函数 ：代表在状态下，采取动作相较于平均水平有多好。在 RLHF 中，它通常由奖励模型（RM）的输出减去一个基线（Baseline，通常是另一个叫做“价值模型”的网络的输出）来估计。

概率比 ：表示新策略和旧策略对于同一个动作的输出概率之比。

### ②DPO

DPO是斯坦福大学和CZ Biohub在2023年提出的创新性偏好优化方法，通过重新推导RLHF的数学形式，将强化学习问题转化为简单的监督学习问题，从而绕过了复杂的奖励模型训练和强化学习优化过程。

DPO 的核心思想是通过 Bradley-Terry 偏好模型，将偏好概率建模为语言模型输出概率的对数比值，并引入温度参数 (β) 来控制 KL 惩罚强度。

DPO的损失函数为：

其中， 分别表示输入提示、偏好回答和非偏好回答； 是待优化的策略模型； 是参考模型（通常是SFT后的模型），用于防止优化后的模型偏离原始分布过远；β是温度参数，控制偏好强度；σ 是`sigmoid`函数。

### ③GRPO

GRPO是DeepSeek在2024年提出的创新算法，专门针对数学推理等复杂任务设计。GRPO巧妙地结合了PPO的策略优化思想和DPO的简化架构，通过群组采样和相对优势计算，在提升训练效率的同时保持了优秀的优化效果。

GRPO 可以看作是 PPO 的一个变种，它通过一种新颖的方式来估计优势函数，从而省去了 PPO 中昂贵的价值模型（Critic Model）。

GRPO 的核心是对 PPO 中优势函数的计算方式进行了修改。其步骤如下：

1. 组采样对于一个给定的指令 `q`，使用当前的策略模型 生成一个包含 `G`个回答的组 `o1`，`o2`，…，`oG`。

2. 组评估使用一个奖励函数（可以是一个训练好的奖励模型，也可以是某种可计算的启发式规则，例如代码的执行结果、数学题的答案是否正确等）为组内的每一个回答 打分，得到奖励 。

3. 组内优势计算计算组内所有回答的平均奖励  和标准差 `σr`。对于组内的每一个回答 `oi`，其优势被定义为其归一化后的奖励：

这种方法被称为组内奖励归一化。它直接用组内的统计量（均值和标准差）来替代了 PPO 中需要专门训练的价值模型（Critic Model）所扮演的角色。

4. 策略更新一旦计算出了每个样本的优势 `Ai`，接下来的步骤就和 PPO 非常相似。GRPO 同样使用 Clipped Surrogate Objective 来更新策略模型：

![](img/400ab98c4a45775d0d6cd1f1da8b9b07.png)

## XTurner

`XTuner V1` 是一个专为超大规模 MoE 模型打造的新一代大模型训练引擎。与传统 3D 并行训练架构相比，`XTuner V1` 针对当前学术界主流的 MoE 训练场景进行了深度优化。

### 环境准备

确保显卡驱动正确安装即可，例如在 NVIDIA GPU 设备上，`nvidia-smi` 的 `Driver Version` 需要大于 550.127.08

### XTurner安装

```
git clone https://github.com/InternLM/xtuner.git
cd xtuner
pip install -e .
```

不同的任务`xtuner`依赖会有所不同，比如想训练`gpt-oss`模型，需要强制安装安装 `torch2.8`。

训练 MoE 模型建议额外安装 `GroupedGEMM`。

```
pip install git+https://github.com/InternLM/GroupedGEMM.git@main
```

如果需要训练 FP8 MoE 模型，除了安装上述 `GroupedGEMM` 外需要额外安装 `AdaptiveGEMM`。

```
pip install git+https://github.com/InternLM/AdaptiveGEMM.git@main
```

此外，`XTuner` 推荐安装 [`flash-attn`](https://github.com/Dao-AILab/flash-attention)，RL 推荐安装 `flash-attn-3`，能够显著提升训练速度。可以参考官网文档进行安装。

如果想抢先体验 RL 相关功能，则需要执行下述命令来安装 RL 部分依赖。除此之外，需要安装你选择的推理引擎。以[`LMDeploy`](https://github.com/InternLM/lmdeploy/)为例，可参考官网文档进行安装。

```
pip install -r requirements/rl.txt
# 或者直接安装
# pip install -e '.[rl]'
```

## 进行GRPO训练
