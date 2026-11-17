
# OpenAI o1-mini 模型测评

> 来源：[https://feicaiclub.feishu.cn/docx/G7RId0I4coUBbJxEmyacG2YvnJe](https://feicaiclub.feishu.cn/docx/G7RId0I4coUBbJxEmyacG2YvnJe)

## 引言

我们发布了 OpenAI o1-mini，这是一个具有成本效益的推理模型。o1-mini 在 STEM 领域表现出色，尤其是在数学和编程方面——在 AIME 和 Codeforces 等评估基准上，其表现几乎与 OpenAI o1 相当。我们预计 o1-mini 将成为在不需要广泛世界知识的应用场景中，一个更快速、成本更低的模型。

今天，我们将 o1-mini 推向第 5 层 API 用户（新窗口打开），其成本比 OpenAI o1-preview 便宜 80%。ChatGPT Plus、团队版、企业版和教育版用户可以将 o1-mini 作为 o1-preview 的替代选择，享受更高的速率限制和更低的延迟（详见模型速度）。

## 针对 STEM 推理的优化

像 o1 这样的大型语言模型在预训练时使用了庞大的文本数据集。虽然这些高容量模型具有广泛的世界知识，但在实际应用中可能昂贵且较慢。相比之下，o1-mini 是一个较小的模型，在预训练期间针对 STEM 推理任务进行了优化。通过与 o1 相同的高计算量强化学习 (RL) 管道训练，o1-mini 在许多实用推理任务中实现了与 o1 相当的表现，同时具有显著的成本效益。

在需要智能和推理能力的基准测试中，o1-mini 的表现优于 o1-preview 和 o1。然而，在需要非 STEM 事实性知识的任务中，o1-mini 的表现较差。

![](img/e18b82012a558c2980395a3a140c5888.png)

### 数学

在高中 AIME 数学竞赛中，o1-mini 的得分（70.0%）与 o1（74.4%）相当，且成本显著降低，表现优于 o1-preview（44.6%）。o1-mini 的得分（约 11/15 道题目）使其大致位列全美前 500 名高中生。

### 编程

在 Codeforces 编程竞赛网站上，o1-mini 达到了 1650 的 Elo 分数，与 o1（1673）接近，且高于 o1-preview（1258）。这个 Elo 分数使该模型处于 Codeforces 平台上约 86% 的程序员之上。o1-mini 同样在 HumanEval 编程基准测试和高中水平的网络安全夺旗赛（CTF）中表现优异。

![](img/94cc9b5d014c3281fcd450cd2c67e6fe.png)

### STEM

在一些需要推理的学术基准测试上，例如 GPQA（科学）和 MATH-500，o1-mini 的表现优于 GPT-4o。但在 MMLU 等任务上，o1-mini 的表现不及 GPT-4o，并且由于缺乏广泛的世界知识，在 GPQA 上落后于 o1-preview。

![](img/f1e8cfef715399b46efc23622f26cd38.png)

### 人类偏好评估

我们让人类评估员根据各种领域中具有挑战性、开放式的提示，比较了 o1-mini 和 GPT-4o，使用的评估方法与 o1-preview 对比 GPT-4o 的方法相同。与 o1-preview 类似，o1-mini 在推理密集型领域比 GPT-4o 更受青睐，但在语言处理为主的领域不如 GPT-4o。

![](img/19d7a75ea1d8c4888b64612af81762e7.png)

### 模型速度

作为一个具体的例子，我们比较了 GPT-4o、o1-mini 和 o1-preview 在一个文字推理问题上的响应。虽然 GPT-4o 没有给出正确答案，但 o1-mini 和 o1-preview 都做到了，而且 o1-mini 比 o1-preview 快了大约 3-5 倍。

![](img/10620accc56ea9f648017f84ce60b48b.png)

### 安全性

o1-mini 使用与 o1-preview 相同的对齐和安全技术进行训练。与 GPT-4o 相比，o1-mini 在内部版本的 StrongREJECT 数据集上具有 59% 更高的“越狱”攻击防御能力。在部署之前，我们使用与 o1-preview 相同的准备措施、外部红队测试和安全评估方法，仔细评估了 o1-mini 的安全风险。我们在系统卡中发布了这些评估的详细结果。

### 局限性及未来发展方向

由于 o1-mini 专注于 STEM 推理能力，其在非 STEM 话题（如日期、传记和问答题目）上的事实性知识表现与小型语言模型（如 GPT-4o mini）相当。我们将在未来版本中改进这些局限性，并尝试将模型扩展到 STEM 以外的其他领域和模式。

4o
