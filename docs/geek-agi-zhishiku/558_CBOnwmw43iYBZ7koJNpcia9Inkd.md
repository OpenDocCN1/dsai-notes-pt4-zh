# 精读3篇 | 7月29日~8月04日

> 来源：[https://geekbang.feishu.cn/docx/NzrQd82Fmo5JH0xBY8PcczRlnqe](https://geekbang.feishu.cn/docx/NzrQd82Fmo5JH0xBY8PcczRlnqe)

# [Llama 4训练已开启！Meta科学家最新](https://mp.weixin.qq.com/s/2q3jTMM45AJAZjF5AdXzow)采访，揭秘Llama 3.1是如何炼成的

文章来源：新智元

## 一句话描述

Meta的AI科学家Thomas Scialom在播客中分享了Llama 3.1的研发过程和Llama 4的发展方向。Llama 3.1参数规模的选择考虑了多种因素，包括scaling law、训练时间、硬件限制等。模型使用了大量合成数据，未采用MoE架构。评估模型性能时，面临挑战，尝试了多种方法。Llama 4训练已开始，可能聚焦agent技术，Meta也在Toolformer等agent工具上进行了研究。

## 文章略读

本文主要是关于Meta AI科学家Thomas Scialom在播客节目Latent Space中的采访，讨论了Llama 3.1的研发思路和Llama 4的更新方向。以下是文章的主要内容概述：

1.  **Llama 3.1的研发**：Scialom讨论了参数规模的决定因素，包括scaling law、训练时间、硬件约束等，并提到了量化技术对推理成本的影响。

1.  **模型规模**：Llama 3.1的规模选择旨在与GPT-4竞争，尽管尚未完全达到目标，但差距正在缩小。Meta计划继续扩展模型规模。

1.  Scaling Law的重新审视：讨论了模型规模与训练数据量的关系，以及如何找到模型参数量和训练token数的最佳比率。

1.  **模型架构**：Llama 3的架构与Llama 2相比变化不大，但数据集规模和质量有显著提升。未来可能会探索Transformer之外的架构。

1.  **合成数据的使用**：Llama 3的后训练过程完全使用合成数据，Scialom看好合成数据的潜力。

1.  **模型评估**：评估LLM是一个开放问题，Scialom分享了评估方法和面临的挑战。

1.  **Llama 4与Agent技术**：Meta已经开始训练Llama 4，可能会重点围绕agent技术。Scialom强调了指令模型的重要性，并提到了Toolformer等agent工具。

1.  **GAIA基准**：Meta发布的GAIA基准用于评估模型解决现实世界问题的能力，显示了不同模型间的智力差距。

1.  **未来展望**：Scialom期待通过模型互联形成复杂的agent系统，实现更高级的功能。

文章提供了对Llama模型研发过程的深入了解，以及对未来AI技术发展的展望。

# [OpenAI惨遭打脸！SearchG](https://mp.weixin.qq.com/s/0HD7mFjnlwhA4npt_s_h1g)PT官方演示大翻车，源代码竟暴露搜索机制

文章来源：新智元

## 一句话描述

这篇文章讨论了OpenAI的SearchGPT在官方演示中出现的问题。尽管SearchGPT在一些测试中表现出色，能够快速提供信息和答案，但官方演示中的错误引发了关注。例如，SearchGPT在回答关于北卡罗来纳Boone音乐节的问题时，错误地提供了错误的日期信息。此外，开发者还揭示了SearchGPT的搜索机制，显示它依赖Bing索引并有自己的网络爬虫来获取实时数据。尽管存在问题，SearchGPT的快速响应和多模态模型支持仍显示出其潜力。OpenAI也承认了错误，并表示将继续改进。

## 文章略读

本文是

是一篇报道OpenAI最新推出的SearchGPT功能的新闻。以下是文章的主要内容概述：

1.  **SearchGPT的演示和反响**：OpenAI的SearchGPT在官方演示中出现了错误，被外媒《大西洋月刊》指出。尽管如此，一些用户制作的demo展示了SearchGPT快速给出搜索结果的能力，引起了广泛关注。

1.  **网友测试**：一位名为Kesku的网友自制demo，展示了SearchGPT的快速回复能力，包括提供实时信息和可视化数据。

1.  **搜索机制揭秘**：科技媒体TestingCatalog进行了内测，并透露了SearchGPT的搜索机制，指出它依赖Bing的索引，但有自己的网络爬虫，用于获取实时数据。

1.  **官方演示中的错误**：SearchGPT在官方演示中将一个音乐节的举办时间弄错，OpenAI发言人承认了错误，并表示这仅是初始原型，将不断改进。

1.  **LLM的幻觉问题**：文章讨论了大型语言模型（LLM）产生幻觉（生成错误信息）的问题，引用了Andrej Karpathy的观点，认为幻觉是LLM的特点而非bug。

1.  **搜索引擎的未来**：文章最后提出了关于搜索引擎未来的问题，探讨了LLM的创造性与准确性之间的平衡，以及它们与传统搜索引擎的关系。

这篇报道提供了对SearchGPT功能的初步评估，展示了其潜力和当前面临的挑战。

# GPT-4o 最关键功能本周上线，语音交互的时代终于要来了

文章来源：Founder Park

## 一句话描述

GPT-4o作为新一代语音代理，具有低延迟和高智能特点，适用于心理疗愈等场景。文章探讨了其在不同应用场景中的潜力、创业机会及关键问题，强调了端到端模型对传统语音交互链路的颠覆性影响，并分析了其在开发者工具、企业工作流和消费者应用中的发展前景。

## 文章略读

本文是关于GPT-4o模型及其在语音交互领域应用前景的一篇文章。以下是文章的主要内容概述：

1.  **Voice Agent的潜力**：Voice Agent作为下一代人机交互界面，提供了比文本交互更直观、低能耗的体验，特别适合简短、即时的信息交流。

1.  **GPT-4o模型**：GPT-4o是首个实现端到端voice-in, voice-out的大型模型，具有低延迟和高智能的特点，为新的交互形态带来可能性。

1.  **应用场景分析**：文章根据不同场景对实时性、准确性、创造性的要求，建立了分析voice agent能力禀赋框架，并识别了值得关注的创业和投资机会。

1.  **场景分类**：将应用场景分为四个象限，包括陪伴/心理疗愈、call center/销售/客服/教育、有声书/短视频配音/模板化外呼电话、音乐生成/NPC语音生成等。

1.  **创业机会**：分析了面向开发者（To Developer）、企业用户（To Enterprise）和消费者（To Customer）的创业机会，以及它们的价值、壁垒和关键方向。

1.  **关键问题**：讨论了voice agent在企业用例上落地的挑战、GPT-4o API的开放时间和形式，以及端到端模型如何改变voice agent工作流。

1.  **技术链条**：介绍了voice agent技术链条的各个环节，包括VAD、ASR、LLM、TTS、情感引擎和RTC技术，并列举了相关领域的代表公司。

1.  **To B和To C产品**：探讨了面向企业（To B）和消费者（To C）的voice agent产品，如AI客服、培训和招聘、AI陪伴、疗愈师、语言老师等。

1.  **消费级硬件**：讨论了基于voice agent的消费级硬件机会，如Humane、rabbit、Curio和Moxie等产品。

文章提供了对GPT-4o模型及其在语音交互领域应用的全面分析，展示了其潜力和当前面临的挑战。