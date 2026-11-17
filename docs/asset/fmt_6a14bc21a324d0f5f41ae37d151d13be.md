
# 又遇到「GPT写的review」了？看看北大&密歇根的这个研究工作

> 来源：[`https://agijuejin.feishu.cn/docx/GicNdBolFoJxdKx0UptcvouJnq6`](https://agijuejin.feishu.cn/docx/GicNdBolFoJxdKx0UptcvouJnq6)

> 机器之心｜[阅读原文](`https://www.jiqizhixin.com/articles/2024-07-10-4`)

> 转载请联系原作者取得授权

本文是对发表在计算经济学顶级会议ACM Conference on Economics and Computation (EC'24)上的论文Eliciting Informative Text Evaluations with Large Language Models的解读。该论文由北京大学前沿计算研究中心孔雨晴课题组与密歇根大学Grant Schoenenbeck课题组合作完成，共同一作为北京大学计算机学院博士生陆宇暄和密歇根大学博士生许晟伟。

![](img/78aa6beb73c91d9f5b980420deea7cfc.png)

![](img/93be80820bf0523eb804cfe49742fd20.png)

+   论文链接：`https://arxiv.org/abs/2405.15077`

+   现场交流 Oral at EC’24：
    Yale School of Management, New Haven, Room 2400  
    Wednesday, July 10. 14:30–16:00 (EDT)

## 介绍

你遇到过“ChatGPT”味道十足的审稿意见吗？质量如何呢？

低质量的审稿是一直让研究者头疼的事情。大语言模型（LLMs）的流行给审稿质量带来转机了吗？

不妨看看下面这两个例子，你能够分辨其中哪个是由ChatGPT-4o生成的吗？

例1:

… The paper is engaging and addresses a highly pertinent issue: information elicitation in the context of Large Language Models (LLMs). The concept of computing conditional probability using an LLM is both elegant and innovative. …

… A primary concern is the robustness of the method used to estimate conditional probability with an LLM, which may require additional experimentation and methodological refinement to ensure reliability and applicability across diverse scenarios. …

例2:

… The paper presents a novel application of LLMs to enhance peer prediction mechanisms, which is a significant step forward from traditional methods that focus on simpler report types. …

… While the mechanisms are theoretically sound, their practical implementation, especially in real-world settings with diverse and complex textual inputs, might pose significant challenges. …

当你仔细品读这两个例子的语法使用和遣词造句后，不知道有没有分辨出来二者之间的区别。

现在公布答案：第一个例子为人类审稿人撰写，而第二个例子则是由ChatGPT-4o生成的。

可以看出来，GPT-4o的审稿意见“看起来不错”，但指出的问题往往是泛泛而谈，这种“challenge in real-world settings”的问题对99%的研究都是适用的，而人类审稿人的意见则更加具体和实际。

即使目前大模型的能力在不断增强，若干研究也已经探索了使用大模型辅助生成论文审稿意见的可能；但不可否认的是，大模型使得生成低质量审稿意见的成本变得无限低，这也让学术界对滥用大模型生成低质量审稿意见感到担忧。

如何激励审稿人提供高质量的审稿意见呢？

粗暴的使用大模型检测工具往往是不可取的，因为合理的运用大模型可以为非英语母语的审稿人提供必要的语言支持。而且随着大模型的进化，当他能够提供合理质量的审稿时，并不应当完全排斥大模型的使用。

问题的关键是，意见本身应当是高质量的，具有建设性和批判性的，而不是语言是如何表达的。

## 方法概述

来自北京大学和密歇根大学的研究者提出了两种机制，Generative Peer Prediction Mechanism (GPPM) 和 Generative Synopsis Peer Prediction Mechanism (GSPPM)，使得高质量的汇报能够取得（期望意义上）更高的评分。

![](img/fbb878fb6b64f4c60322a46bd4de6940.png)

研究团队提出了两种估计方法，TOKEN和JUDGMENT：

其中，

+   TOKEN利用了LLM predict next token的能力。值得注意的是，利用大模型将文本报告预处理成统一的格式非常重要，这避免了不同语言风格和用词习惯带来的噪声等影响，研究者称这一方法为Token-Preprocess。

+   JUDGMENT提供了一种在只有Chatbot API情况下的解决方案。如上图所示，带有文本预处理的TOKEN实现表现整体好于JUDGMENT实现。

## 实验结果

### 1. GPPM和GSPPM是否有能力区分出人类审稿意见和LLM生成的审稿意见？

在ICLR 2020 OpenReview数据集上，研究者随机选出300篇论文，对每篇论文随机指定一对Alice和Bob，然后他们将Alice的审稿意见替换为LLM（GPT-4或GPT-3.5）生成的审稿意见，观察Alice的分数是否显著下降。

![](img/83b57f17e1ab0b306fa1a7e39745cd5d.png)

实验发现，在两种机制下，分数下降都非常显著，而且GPT-3.5生成的审稿造成的分数下降比GPT-4更加显著。从而显示出三种质量等级，从高到低依次是人类，GPT-4，GPT-3.5，符合预期。

同时，GSPPM的分数显著性更高，这是由于GSPPM进一步降低了由论文概括等与意见无关的信息带来的噪声等影响，从而突出了审稿意见质量带来的差异。

### 2. 不同估值方法是否能检测低质量的评论意见？

与审稿类似的文本评价在互联网经济（例如餐厅点评，商品评价），在线课堂（例如同伴互评）等场景被广泛应用，它们都可能会遇到低质量报告带来的问题。因此，研究团队还分别在Yelp餐厅点评数据集和ICLR2020 OpenReview数据集，实验了人为降低文本评价质量是否会带来显著的分数下降，答案是肯定的。我们发现Token-Preprocess检测低质量评论的能力最显著。

![](img/1b0cda1e825258d840d2be076d9cc653.png)

三种人为降低报告质量带来的分数下降显著性（Yelp餐厅点评数据集）

![](img/393e57d65b1a337606f01cf69a1e5f05.png)

三种人为降低报告质量带来的分数下降显著性（ICLR2020 OpenReview数据集）

## 方法和理论结果

本文的理论依据主要来源于用信息论的方法量化随机变量之间的相关性，研究团队用大模型的预测能力作为实现量化文本相关性的手段。

![](img/c6ce8153b24755846cd3dcf6e1a93379.png)

根据以上思路，该团队证明了在LLM的估计近似准确的假设下，所有人都提供高质量的信息是一个近似贝叶斯纳什均衡（Bayesian Nash Equilibrium）：即对于每个人，提供更差的审稿意见会带来更低的期望分数。

## 未来工作

这篇论文率先在计算经济学领域中，提出激励高质量文本评价的机制。这样一个全新的交叉领域带来了若干开放问题，例如

+   能否收集人类对审稿意见的评分，验证与文中机制评分的相关系数？

+   能否进一步降低文中机制评分的噪声？

+   能否在其他领域测试文中机制的能力，如中文文本？

研究者在论文中提供了更多细节讨论，并欢迎读者进一步讨论和交流。
