
https://zhuanlan.zhihu.com/p/721098254?utm_psn=1821821015208116224

# 蒙特卡罗树搜索在数学推理中的应用

在现有技术中，蒙特卡罗树搜索（MCTS）是一个数据飞轮系统，它将答案正确性作为一个训练示例小数据集，并逐步改进未来的推理过程。本文讨论了MCTS在Deepseek团队和Ali团队在数学推理任务中的应用。总结了三篇关键论文，分别来自Deepseek和Qwen团队，介绍了其在自动化监督生成中的不同技术。

Deepseek提出的MATH-SHEPHERD系统，通过构建过程监督数据集，有效评估了中间步骤在推导正确答案中的重要性。这一方法自动生成数学推理任务的监督信号，特别是在验证和强化学习两个场景下，使用了步骤级别的Proximal Policy Optimization（PPO）算法，显著提高了Mistral-7B等模型的准确性。例如，在GSM8K数据集上，模型准确性从77.9%提升至89.1%。

相比之下，Qwen团队的AlphaMath采用了无标注过程监督的方式，结合多任务损失函数，同时优化策略和价值模型。他们还提出了一种步骤级别的值偏好优化（Step-level Value Preference Optimization, SVPO）算法，通过构建偏好对数据集，提升了多步骤推理问题的性能。

这两种方法都充分利用了自动化的过程标注技术，解决了依赖昂贵人工标注的问题。通过蒙特卡罗树搜索（MCTS）的迭代训练，Deepseek和Qwen分别展示了如何在数学推理中自动生成高效的监督信号，显著提高了大型语言模型（LLMs）在复杂任务中的表现。

![](img/467f74b0d84a8ecab193c730411f8aac.png)

## SFT loss 计算的那些坑（多轮合并/packing）

https://zhuanlan.zhihu.com/p/721652210?utm_psn=1821703631675727875

在监督微调（SFT）过程中，通过多轮合并和packing两种方法可以显著加速训练，同时也带来了loss计算中的一些技术问题。

多轮合并的思路是在对话任务中，将多轮对话合并为一个样本处理，而不是将每轮作为独立样本。这种做法避免了重复计算相同的前缀内容。通过因果注意力掩码，可以保证每个token只会看到前面的token，从而保持计算的等价性。然而，直接使用标准的`CrossEntropyLoss`进行loss计算会导致不同轮次之间的权重分配不均衡。由于`CrossEntropyLoss`默认是按token数量取平均，如果对话轮次的输出长度不一致，较短的轮次在计算loss时权重会被降低，较长的轮次权重会被提高，导致短轮次的数据训练不充分。因此，在多轮合并时需要调整loss的计算方法，使得每个token的权重被正确分配，以确保不同轮次的数据训练效果等价。

在packing技术中，为了减少训练时的padding浪费，多个对话样本会被拼接成一条长序列处理。通过设置适当的attention mask，确保每个样本只能看到自己的token，避免不同样本之间的相互干扰。这样做的好处是提高了计算效率，因为减少了无效的padding。然而，和多轮合并一样，不同样本的长度不同会导致loss计算不等价。解决这个问题的关键是禁用默认的三次平均（micro batch维度、数据并行维度、梯度累加维度），统一使用整个batch的对话轮次作为分母来计算全局loss。在实现中，通过开启`--calculate-per-token-loss`选项来禁用数据并行和梯度累加的平均操作。同时，loss计算函数需要根据每个batch中的轮次数返回正确的权重，确保全局loss的准确计算。

代码实现中，通过`losses * loss_mask / loss_token_num`的操作，为每个token设置了正确的权重。`torch.distributed.all_reduce`函数在所有GPU上同步计算loss，确保在多GPU环境下的计算结果一致性。这样可以保证每轮对话中的token都得到应有的权重，解决了不同轮次或样本长度不一致导致的训练不充分问题。

在SFT的加速过程中，关键在于保证不同样本和不同轮次的等价性。无论是多轮合并还是packing，都需要特别注意loss计算方法的调整，为每个token设置合适的权重。同时，关闭数据并行和梯度累加中的平均操作，确保在分布式训练环境中，所有样本的对话轮次都被正确考虑，是保证训练正确性的必要步骤。

![](img/ab2afe4f32f1ea0c8db53a3136c467a1.png)

# 信号

### Retrieval Augmented Generation (RAG) and Beyond: A Comprehensive Survey on How to Make your LLMs use External Data More Wisely

贡献人：

链接：https://arxiv.org/abs/2409.14924

这篇文章提出了一种 RAG 任务分类方法，根据所需外部数据类型和任务的主要重点将用户查询分为四个级别：显性事实查询、隐性事实查询、可解释的原理查询和隐藏的原理查询。文章定义了这些查询级别，提供了相关数据集，并总结了解决这些挑战的关键挑战和最有效的技术。最后，文章讨论了将外部数据集成到 LLM 中的三种主要形式：上下文、小模型和微调，重点介绍了它们各自的优势、局限性以及它们适合解决的问题类型。这项工作旨在帮助读者彻底理解和分解构建 LLM 应用程序中的数据需求和关键瓶颈，为不同的挑战提供解决方案，并为系统地开发此类应用程序提供指南。

![](img/326df5a142259ddf2952b95c93679268.png)

### Minstrel: Structural Prompt Generation with Multi-Agents Coordination for Non-AI Experts

贡献人：

链接：https://arxiv.org/abs/2409.13449

受结构化可重用编程语言的启发，文章提出了结构提示设计框架 `LangGPT`。此外，引入了 `Minstrel`，这是一个具有反射功能的多生成代理系统，用于自动生成结构提示。实验和案例研究表明，由 `Minstrel` 生成或手动编写的结构提示可显著提高 LLM 的性能。此外，通过在线社区中的用户调查分析了结构提示的易用性。

![](img/0b01e0cb1465bfb90c8e0eb710e7ddc4.png)

### A Preliminary Study of o1 in Medicine: Are We Closer to an AI Doctor?

贡献人：

链接：https://arxiv.org/abs/2409.15277

本报告对 `o1` 在不[同医疗场景中的表现进行](https://ucsc-vlaa.github.io/o1_medicine/)了全面探索，研究了 3 个关键方面：理解、推理和多语言性。具体来说，评估涵盖了 6 个任务，使用来自 37 个医学数据集的数据，其中包括两个基于《新英格兰医学杂志》(NEJM) 和《柳叶刀》的专业医学测验新构建的更具挑战性的问答 (QA) 任务。与 MedQA 等标准医学 QA 基准相比，这些数据集具有更高的临床相关性，可以更有效地转化为现实世界的临床效用。文章对 `o1` 的分析表明，LLM 的推理能力增强可能会（显著）有利于他们理解各种医疗指令和推理复杂临床场景的能力。值得注意的是，在 19 个数据集和两个新创建的复杂 QA 场景中，`o1` 的准确率平均比之前的 GPT-4 高出 6.2% 和 6.6%。但与此同时，文章发现模型能力和现有评估协议都存在一些弱点，包括幻觉、多语言能力不一致以及评估指标不一致。文章在此 https URL上发布原始数据和模型输出，以供将来研究。

![](img/d2fbdcf873ee5bfe2a212e979902e3bf.png)

### LLMs Still Can't Plan; Can LRMs? A Preliminary Evaluation of OpenAI's o1 on PlanBench

贡献人：

链接：https://arxiv.org/abs/2409.13373

文章在 2022 年 GPT3 发布后不久开发了可扩展基准 `PlanBench` ，它仍然是评估 LLM 规划能力的重要工具。尽管自 GPT3 以来出现了大量新的私有和开源 LLM，但该基准的进展却出人意料地缓慢。OpenAI 声称，他们最近的 `o1`（Strawberry）模型经过专门构建和训练，可以摆脱自回归 LLM 的正常限制——使其成为一种新型模型：大型推理模型 (LRM)。利用这一发展作为催化剂，本文全面研究了当前 LLM 和新 LRM 在 `PlanBench` 上的表现。我们将看到，虽然 `o1` 的性能在基准上有了很大的改进，超过了竞争对手，但它还远远没有达到饱和状态。这种改进也带来了关于准确性、效率和保证的问题，这些问题在部署此类系统之前必须考虑。

![](img/0386daef5799e0b5f6bfbf9e1b805584.png)

### Creative and Strategic Capabilities of Generative Al: Evidence from Large-Scale Experiments

贡献人：

链接：https://docs.iza.org/dp17302.pdf

本文将 `AI` 在创造性和战略性任务中的能力与美国成年人的代表性人群进行了比较。人工智能聊天机器人产生的创意比人类创造的创意更有创意。此外，`ChatGPT` 比人类更有创造力，而 `Bard` 则落后了。

用 `AI` 增强人类可以提高人类的创造力，尽管不如 `ChatGPT` 单独创造的想法那么多。

来自 `AI` 的竞争不会显著降低男性的创造力，但会降低女性的创造力。

对文本进行评分的人无法很好地区分 `AI` 或其他人创造的想法，但他们认为 `AI` 产生的反应得分较低。

至于战略能力，虽然 `ChatGPT` 显示出在战略游戏中根据对手的打法调整动作的明显能力，但平均而言，人类在这种适应方面更成功。

![](img/bc773a02d94cce63495399e6206e9e0f.png)

### Training Language Models to Self-Correct via Reinforcement Learning

贡献人：

链接：https://arxiv.org/abs/2409.12917

作者们开发了一种多轮在线强化学习（RL）方法 `SCoRe`，它使用完全自生成的数据显著提高了 LLM 的自我纠正能力。为了构建 `SCoRe`，文章首先表明，对离线模型生成的校正轨迹进行监督微调（SFT）的变体不足以灌输自我纠正行为。具体而言，观察到通过 SFT 进行的训练要么存在训练数据与模型自身响应之间的分布不匹配，要么隐式地只偏向某种在测试时通常无效的校正行为模式。 `SCoRe` 通过在模型自身自生成校正轨迹分布下进行训练并使用适当的正则化来引导学习过程学习在测试时有效的自我校正策略，而不是简单地为给定的提示拟合高奖励响应，从而解决了这些挑战。此正则化规定在基础模型上运行 RL 的第一阶段以生成不易崩溃的策略初始化，然后使用奖励奖励在训练期间放大自我校正。当应用于 `Gemini 1.0 Pro` 和 `1.5 Flash` 模型时，文章发现 `SCoRe` 实现了最先进的自我校正性能，在 `MATH` 和 `HumanEval` 基准上分别将基础模型的自我校正提高了 15.6% 和 9.1%。

![](img/755d9315da87f11418c14573afcbd065.png)

# 招聘公告

---

![](img/45128a5a84879ed3535f367d8333511a.png)

**奇绩创坛**由陆奇博士于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建`GenAI`时代独特的跨界人才社群。

## 🚀F24实习岗位

##### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

##### 行研-GenAI研究岗位-战略分析（偏产品）

##### Web / AI 全栈开发工程师实习生

## 火热开放中🚀

##### 行研-GenAI研究岗位-前沿学术探索

##### 行研-GenAI研究岗位-战略分析（偏咨询）

##### 创新活动产品运营 （大模型系列活动产品运营）

> 点击相关岗位可查看对应岗位需求【JD】；相关岗位投递请添加🛰️💻【`l_ivresse29`】

> ‼️欢迎推荐/自荐，期待在奇绩创坛遇见更好的你‼️

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给`AI`日报小组点个赞👍吧！）

`ChatGPT`；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（`HuggingFace`）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> 【日报知识库】这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/f0bbf6bf13c25a6e0f432019fe2b37ef.png)
