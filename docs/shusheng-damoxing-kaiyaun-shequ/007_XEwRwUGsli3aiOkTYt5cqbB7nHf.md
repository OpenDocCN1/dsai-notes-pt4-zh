# OREAL 圆桌

> 来源：[https://aicarrier.feishu.cn/docx/TuX3dRpfeoeaTOxFflrcX1Rsn0c](https://aicarrier.feishu.cn/docx/TuX3dRpfeoeaTOxFflrcX1Rsn0c)

![](img/d73ec44b6cca2c1371c05a7237db9ec8.png)

**论文名称：**Exploring the Limit of Outcome Reward for Learning Mathematical Reasoning

![](img/d743216c5425cf486ddfdcf2d9d816ba.png)

论文中文名：探索基于 Outcome Reward 的方法下模型学习数学推理的极限

论文地址：https://huggingface.co/papers/2502.06781

#腾讯会议：758-227-145

推理能力，尤其是解决复杂数学问题的能力，是通用智能的重要组成部分。最近，如 OpenAI 的 o-series 模型等专有公司在这方面取得了显著进展。然而，技术细节尚未完全公开，被认为采用的技术只有强化学习（RL）和长链思考。本文提出了一种新的基于结果奖励的 RL 框架，称为 OREAL，旨在通过基于结果奖励的强化学习来追求数学推理任务所能达到的性能极限，其中仅二元结果奖励易于获取。我们理论上证明，从 best-of-N（BoN）采样中正轨迹的行为克隆足以在二元反馈环境中学习 KL 正则化的最优策略。这种表述进一步表明，负样本的奖励应被重塑以确保正负样本之间的梯度一致性。 为了缓解由稀疏奖励在强化学习中长期存在的困难，以及由于推理任务中长链思维的部分正确性而加剧的问题，我们进一步应用了一个令牌级奖励模型来采样推理轨迹中的重要令牌以进行学习。借助 OREAL，首次实现了一个 7B 模型在 MATH-500 上通过强化学习获得 94.0 的 pass@1 准确率，与 32B 模型相当。OREAL-32B 还超越了通过蒸馏训练的先前 32B 模型，在 MATH-500 上获得了 95.0 的 pass@1 准确率。我们的研究还表明初始策略模型和训练查询对于强化学习的重要性。代码、模型和数据将被发布以造福未来的研究 https://github.com/InternLM/OREAL。