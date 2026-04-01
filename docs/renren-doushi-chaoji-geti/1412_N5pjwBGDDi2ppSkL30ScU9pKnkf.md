# 2023-06-29低成本微调模型的"美好承诺"可以相信吗？ —— 解读Fine-tuning的原理、作用和局限

> 来源：[https://xuqiwei1986.feishu.cn/docx/RGsudxeWNoSmMjx1Lkjc3WTen7c](https://xuqiwei1986.feishu.cn/docx/RGsudxeWNoSmMjx1Lkjc3WTen7c)

> 🔗 原文链接： h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/XcU5SXrbL6A_KPV_M5LBvA)q.com/s/XcU5SXrb...

>

原创 何文斯 何文斯 2023-06-29 18:47 发表于 江苏

##

## **背景**

随着大模型开源世界的迅速发展，众多基于LLaMA（由Meta公司开源的大语言模型）进行Instruct-tuning（指令微调，Fine-tuning的一种方式）的模型，如Alpaca，Vicuna，Koala，GPT4ALL等都对外宣传：通过使用ChatGPT的真实对话数据（如ShareGPT）作为指令微调数据集之后，可以在基础模型参数量较小 （低至10b以下 ） ，算力成本要求较低的（8*A100）的情况下，通过指令微调使得这些低成本模型获得与ChatGPT相近的效果。

其中Vicuna 13b官方声称他们达到了ChatGPT的90%模型效果。

![](img/db93b9729d5970dd30fc0aee8875cfe3.png)

一时间，Fine-tuning成为了一种极具有性价比的技术方案，使得某些公司能将开源可商用的小模型（相比于GPT3.5减少了几十至上百倍的参数量），通过中英文指令微调，快速训练出一个『看起来』与ChatGPT差不多的模型，并且有能力回答各类问题。只需再起个新名字、换套皮肤，摇身一变就能发布一个新的大模型——"xxx公司xxx大模型重磅发布"。

除此之外，这类模型发布后往往也会同时公布经由某个Benchmark体系打分后的排名结果，通过对覆盖人类社会和各行业问题的评估测试，公开证明其模型出色的语言和任务能力。

![](img/4f03c633a110a4529b6f9f85b9f3a952.png)

但随着后来这类新闻看多了之后，我始终有种感觉，似乎这些『类ChatGPT』模型在公众视野内最高的热度就是在其发布时。换角度来思考，如果『便宜量又足』的方案真的可行，那OpenAI这类投入巨资训练千亿乃至万亿参数大模型的公司岂不是成了『冤大头』。

事**实上，我们团队之前也简单测试过Vicun**a13b的能力，将其在特定的代码生成方面与GPT3.5做了对比， 测试下来远远未达到其宣称的『接近ChatGPT的90%』，也许连10%都达不到 。

于是我随之产生了几个疑问：

1.  这些『低成本微调模型』实际上是什么水平？是否是以宣传为目编造的谎言？

1.

1.  **通**过Fine-tuning的方式能够让一个参数量远远小于GPT的模型获得和ChatGPT接近的语言和推理能力吗？

1.

1.  **市面上的这些人类/机器打分系统和Benc**hmark能否真实评价模型各方面的能力？

##

## **探究**

要寻找上面几个问题的答案，首先要先理解Fine-tuning的基本概念：

> The process of taking a pre-trained machine learning model that has already been trained on a large dataset and adapting it for a slightly different task or specific domain. During fine-tuning, the model’s parameters are further adjusted using a smaller, task-specific dataset, allowing it to learn task-specific patterns and improve performance on the new task.

微调的过程是采用一个已经在大数据集上预训练的模型，将其适应于稍微不同的任务或**特定领域。在微调过程中，使用较**小的、针对特定任务的数据集进一步调整模型的参数， 使其能够 学习任务特定的模式 ，并提高在新任务上的表现。

###

### 为什么需要Fine-tuning（微调）？

在**Deep Learnin**g发布的官方课程《ChatGPT Prompt Engineering for Developers》中，可以找到 关于指令微调模型的解释：

1.  微调可以使得大模型能够更好地遵循人类指令。

1.  微调可以使得大模型倾向于提供对人类更有益、诚实、无害的信息。

1.

![](img/d83fe3d077199eb5111343addacb758e.png)

1. 对未经指令微调的基础模型提问：

提问："法国的首都是哪里？"

回答1："法国最大的城市是哪里？"

回答2："法国的人口有多少?"

回答3："法国的货币是什么？"

模型根据自己已有的知识来尝试回答问题，但回答的方式都是错误的。

2. 对指令微调后的模型提问：

提问："法国的首都是哪里？"

回答："法国的首都是巴黎。"

指令微调后模型理解了人类回答这类问题的模式，因此给出了正确的答案。我们现在常用的ChatGPT，背后的GPT-3.5和GPT-4都属于指令微调模型。

###

### 再来看看OpenAI关于Fine-tuning的官方说明：

OpenAI在其官方开发文档中，向其社区开发者解释了Fine-tuning适用的范围：

> 微调通过训练超出提示范围的更多示例来改进小样本学习，让您在大量任务上取得更好的结果。一旦模型经过微调，您将不再需要在提示中提供示例。这可以节省成本并实现更低延迟的请求。

在OpenAI cookbook内形象描述了Fine-tuning作为模型学习新知识训练方法的缺陷：

> 微调在事实回忆方面并不可靠。类比一下，把大模型比作一个参加考试的学生，模型权重就像长期记忆，当你对模型微调时，就像提前一周准备考试，当考试来临时，模型可能会遗漏掉之前学习过的知识或者错误地回答出从未教授给他的知识。

###

###

### **其他解释：**

油管上的一个大模型应用开发者David Shapiro，通俗形象地解释了有关微调和Embedding在Q&A应用中的区别。

![](img/27e8f57eb5b82c06c6230d7cfaa64d3b.png)

David Shapiro在模型微调方面有相当丰富的项目实践经验，在视频中，David Shapiro关于Fine-tuning的解释是这样的：

> 微调是一种迁移学习，它只解冻了模型的一小部分，它并没有真正的重新训练整个模型，更像是当你之前学会了系鞋带（基础模型预训练），通过Fine-tuning 你就能学会系上任何绳子（比如给两根绳子打个活结），你并没有学到任何新的基本知识。你只是把已经学会的知识应用在一个稍微不同的任务上。

**所以说，Fine-tuning可以教会模型一种新**模式， 比如ChatGPT聊天就是一种模式，即用户短提问——机器长回复，并且价值观遵循人类偏好 。写代码是一种模式，写邮件是一种模式，写小说一种模式。

**不少人都认为可以通过微调来教授模型新知识来使得模型完**成问答任务，这可能是对微调最大的误解。 微调只是教会了模型按某种模式（Pattern）来完成新任务，并无法教会模型新知识。从技术原理上来看，GPT类的大模型本身并不是认知模型，并不具备学习新知识的能力。

###

### **论文研究：**

这篇5月25日在发布的技术论文：The False Promise of Imitating Proprietary LLMs，研究团队将这类低成本指令微调模型称之为『模仿模型』，并对其有效性进行了深入的探讨和评估。

![](img/6db220386c8f145d759c756428228f39.png)

这篇文章肯定了模仿模型在一定条件下的输出效果——它们在遵循指示方面表现良好，但通过更严谨全面的测试，研究者发现了『模仿模型』与大模型之间在事实性问题方面存在的明显差距，也发现了当前对模型能力评估方法方面的欠缺（" 问题在于现在的模型评估人员没找到科学方法来综合评估模仿模型和GPT之间的差距"） 。

其结论是： 模仿模型仅仅擅长模仿ChatGPT的语言风格，而非其事实性。

此外，在6月9日刚发表的另一篇论文《Judging LLM-as-a-judge with MT-Bench and Chatbot Arena》的研究中发现，当前市面上常见的大模型打分评估方式——使用顶尖大语言模型（如GPT-4）作为裁判存在的局限性：

**1. 位置偏好**，几乎所有 LLM 都偏好排在第一位的回答（ 谁先来的选谁 ）；

**2. 冗长偏好，**LLM 倾向于选择更加冗长的回答（ 三短一长选最长 ）；

**3. 自我**偏好，LLM 会更倾向于选择相同模型生成的答案（ 血缘歧视 ）。

![](img/0387aab10b2a978c29a23f2f05d94260.png)

##

## **总结**

经过上述的事实探究，文章开头的三个疑问都得到了可信服的解释：

1. 微调只是让这些开源小模型『看起来』与ChatGPT更接近而已。

**2. 微调能够以较低成本让模型学习某**种新的任务模式或语言风格，而不是新知识。

**3. 现有的大模型评估体系仍存在**局限性，不能盲目轻信某个大模型打分系统。

当然，也并不是每个低成本微调模型开发团队都试图夸大宣传自家模型的能力，比如我之前在Colossal.AI（一个基于开源LLaMA的指令微调模型）的开发文档内就看到过这样一段关于Limitation的注释：

![](img/5174de8094d7111883c984d65047fd8f.png)

翻译成中文，基于LLaMA的微调模型的限制：很难弥补在预训练阶段缺失的知识。

**只不过，我猜对于那些正在热衷于跟风炒概念的大模型团**队来讲，可能更倾向于隐瞒以上的事实吧。

感谢阅读，欢迎分享。

参考引用：

1. https://lmsys.org/blog/2023-03-30-vicuna/

2. Judging LLM-as-a-judge with MT-Bench and Chatbot Arena：https://arxiv.org/abs/2306.05685

3. The False Promise of Imitating Proprietary LLMs：https://arxiv.org/abs/2305.15717

4. https://github.com/openai/openai-cookbook

5. https://platform.openai.com/docs/guides/fine-tuning

6. https://a16z.com/ai-glossary/

7. https://github.com/hpcaitech/ColossalAI/tree/main/applications/Chat

8. https://twitter.com/quinn_leng/status/1674170046636244992

9. https://www.youtube.com/watch?v=9qq6HTr7Ocw&t=8s