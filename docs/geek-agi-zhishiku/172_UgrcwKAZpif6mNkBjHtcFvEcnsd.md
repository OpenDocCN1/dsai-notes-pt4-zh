
# DeepSeek 的创新点及优势、实操建议

> 来源：[https://geekbang.feishu.cn/docx/OeKadEjoqou1zTxyijscto23nFc](https://geekbang.feishu.cn/docx/OeKadEjoqou1zTxyijscto23nFc)

## 1. 概念梳理

### 大模型训练流程

[大模型训练全流程](https://geek-agi.feishu.cn/wiki/Jqi1w5AG3iR5X8kFyobc6mZgnmf)

详细流程: [https://mubu.com/app/edi](https://mubu.com/app/edit/home/7h1f7CinhsC#m)

![](img/edb87a5e5fdce48f13d12eb2a63cfa88.png)

### DeepSeek相关概念

![](img/bad56852f42e24dd766a2d4d0e2c65fc.png)

### 推理模型 VS 传统模型

![](img/a2f71a5a1e1f90cbb8e4d06034fb9088.png)

### 蒸馏技术

让厉害的大模型把自己的知识一点点地教给小模型。大模型会告诉小模型，遇到各种问题的时候应该怎么去想，怎么去做，就像老师教学生一样。通过这种方式，小模型就能学到大模型的很多本事，虽然它还是没有大模型那么厉害，但是也能变得比以前厉害很多的一种方法。

+ 白盒: 参考权重

+ 黑盒: 问答结果+精度参考

### Token

![](img/5691aa044f2e7cd7bebd387b7b62148f.png)

## 对比其他模型

![](img/caa9857671ff625f4231d29180c78bfe.png)

![](img/7ad148e8bd9a5d68d56b6eb3104d992c.png)

### 结论:

DeepSeek V3 超过了 GPT-4o

DeepSeek R1 基本上相当于 OpenAI o1 的水平, 部分场景测试领先

## 技术论文 创新+优势 分析

### DeepSeek-V3

论文地址: https://arxiv.org/abs/2412.19437

![](img/6ed074b8baa66baaa76a6a2076211d4f.png)

全文链接: [https://mubu.com/app/edi](https://mubu.com/app/edit/home/4xn2lD8OK4C#m)

### DeePSeek-R1

论文地址: https://arxiv.org/abs/2501.12948

![](img/52b117710fc93cabfd7853ae75b3cca1.png)

全文地址: [https://mubu.com/app/ed](https://mubu.com/app/edit/home/8dbbRN2UAC#m)

训练流程优化: [https://mubu.com/app/edi](https://mubu.com/app/edit/home/76gO55G6f4C#m)

+ 使用 RL (强化学习) 而不是 传统的微调(SFT) -> Zero(没有 SFT)

+ 用少量的标注数据训练模型, 用模型生成训练数据, +旧数据 一起训练新模型(自己增强自己)

+ pre-train 大量的token 训练基础

+ post-train SFT(OpenAI 早期使用人工) , DeepSeek 省略了这一步

+ RL alignment 增强 reasoning -> 多头注意力机制 + Cold start 数据

+ r1 + r1 zero 都开源, + 6 个蒸馏小模型(qwen+llama)

+ post-training 很重要, openai 不开放 reasoning capabilities

+ 使用纯 RL, 不使用 SFT 数据, GRPO

+ 为了解决, 模型乱说+语言混杂

+ 使用 cold start 有标注数据

+ multi-stage training

+ 使用 MIT 协议开源

### 开源:

### 可以基于这个模型去自由定制, 开源协议 MIT（是几种开源协议中最宽松的，允许任何人自由地使用、复制、修改、合并、出版发行、散布、再授权及贩售软件及软件的副本）

+ 首个开源的推理模型, 之前的都是大语言模型(LLM)

+ 针对私有化部署, 技术实现上区别不大, 多了一种选择, 而且需求的算力相比同水平更低

+ 蒸馏极客大模型成为可能

### 训练成本: 相当于 llama 405B 的十分之一

![](img/f61ca89d2265a14cc5515f571135f1c5.png)

### 推理成本: 性能超过 Claude3.5, 但是成本非常低

![](img/14c98ddd637007ed49a844298e118cbb.png)

## 实操建议

1. Prompt 以前怎么写现在怎么写, 甚至不按照 prompt 也可以, 通过多轮对话来补充信息。

把 DeepSeek 想象成一个高智商的陌生朋友，重点是陌生，所以找他帮忙的时候得把背景信息说清楚。

1. 提示词只是指导原则, 口语中只要包含那些要素, 效果也都差不多

1. 使用平台，推荐秘塔和纳米。

![](img/149547fa033fe963bc8dbbd06d74ec12.png)

4. 万能公式：4步提问法

“明确身份+具体任务+细节约束+输出格式”

公式拆解：

1. 身份：你是谁？（学生/打工人/新手妈妈…）

1. 任务：要解决什么问题？（写报告/做计划/分析数据…）

1. 细节：限制条件是什么？（时间/场景/禁忌…）

1. 格式：想要什么形式的结果？（表格/分段/口语化…）

## 补充说明

1. 蒸馏技术的简单解释：让厉害的大模型把自己的知识一点点地教给小模型。大模型会告诉小模型，遇到各种问题的时候应该怎么去想，怎么去做，就像老师教学生一样。通过这种方式呀，小模型就能学到大模型的很多本事，虽然它还是没有大模型那么厉害，但是也能变得比以前厉害很多的一种方法。

1. 600 万美元的训练成本是最终版本的训练成本，不包括研发成本，硬件购置成本，前期测试和迭代的训练成本，只是最终版本的训练成本，或者说你拿着开源代码，你有对方完整语料，去做完整复现的算力成本

1. 是算法和工程层面的奇技淫巧（褒义词），针对H800的内存局限做各种优化，如果公司有很多 H100，不一定会有这个产品的诞生。

H800 是在 2022 年美国对华半导体出口管制规定的背景下，作为 H100 的中国特供版推出。

H200 的推理速度是 H100 的两倍。

1. 训练成本低的负面影响：如果一些黑灰产领域在没有合规约束的情况下进行训练和使用，暂时还没有什么好办法应对，很可怕。

1. 给其它闭源模型带来的挑战，要不要继续坚持自己的闭源策略，比如豆包等，因为开源模型比你训练成本低效果还好，闭源那么高的训练成本如果效果不能领先一大截，何去何存？

1. 只是暂时的领先，能不能持续领先，未知。

1. 不要过度神话，也不要无脑贬低。

## 深度揭秘 DeepSeek

如果想对 DeepSeek 了解更多，请关注 AI 前线和 Kevin 视频号，12 场直播给你深度揭秘，今晚8 点第一场，赶紧扫码预约，别错过。

![](img/3ab8e4a5e7c54ab02eb93ab862b27640.png)
