# 盘点 ACL 2024 安全方向都发了啥

> 来源：[https://v11enp9ok1h.feishu.cn/docx/UTImdINYTouDwNx25Ktcy2INnlh](https://v11enp9ok1h.feishu.cn/docx/UTImdINYTouDwNx25Ktcy2INnlh)

# **序**

随着 ACL 中稿情况的公布，各路巨佬的新思路逐步走入了大家的视野。我们使用 InternLM2.5 大模型粗略筛选、概述了其中安全方向的中稿文章 28 篇，这些文章主要研究方向包括：安全性、隐私保护和道德。

![](img/9a6c5fefb9c32fc0733d3c48975b0652.png)

# 应用 InternLM2.5 进行文章概览

> **💡 Prompt：**

> “你是一个论文解读专家，下面有 {number} 篇论文需要你进行概述解读，请你首先输出一个综合总结，面向所有的文章，并简要描述方向及特点；第二步，你需要分别总结每个文章的具体方向及任务，下面是你的回答：”

这些论文主要关注大型语言模型（LLMs）在安全性、隐私保护和道德方面的研究。

+   其中一些研究提出了新的攻击方法，用于检测和利用 LLMs 的安全漏洞和隐私风险。例如，使用梯度分析检测监狱破解提示，利用转移嵌入反向攻击揭露文本嵌入中的隐私风险，以及采用 ASCII 艺术攻击对抗对齐的 LLMs 等。

+   也有研究探索了新的防御和保护方法，以提高 LLMs 的安全性和隐私保护水平。例如，通过安全解码防御监狱破解攻击，利用可学习的隐私神经元定位实现语言模型中的隐私保护，以及构建安全通用框架支持可解释、可广泛化和可控制的假新闻检测等。

+   此外，还有一些研究关注了 LLMs 在道德和社会方面的问题，如性别偏见、仇恨表情检测和模拟道德判断等，并提出了相应的解决方案，如减少在线自我披露的隐私风险、探索短视频假新闻检测中的观点演变等。

总体而言，这些研究为提高 LLMs 的安全性、隐私保护和道德水平提供了重要的思路和方法，为人工智能的可靠性和健康发展做出了贡献。

1.  GradSafe: Detecting Jailbreak Prompts for LLMs via Safety-Critical Gradient Analysis

1.  该文章提出了一种基于关键安全梯度分析的检测监狱破解提示的方法，以保护大型语言模型（LLMs）的安全性。

1.  How Johnny Can Persuade LLMs to Jailbreak Them: Rethinking Persuasion to Challenge AI Safety by Humanizing LLMs

1.  该文章重新考虑了说服LLMs进行监狱破解的问题，并提出了一种通过人类化LLMs来挑战AI安全性的方法。

1.  ToolSword: Unveiling Safety Issues of Large Language Models in Tool Learning Across Three Stages

1.  该文章揭示了大型语言模型在工具学习过程中存在的安全问题，并分析了三个阶段的安全隐患。

1.  Transferable Embedding Inversion Attack: Uncovering Privacy Risks in Text Embeddings without Model Queries

1.  该文章提出了一种可转移的嵌入反向攻击方法，用于揭露文本嵌入中的隐私风险，而无需模型查询。

1.  Uncertainty-Guided Modal Rebalance for Hateful Memes Detection

1.  该文章提出了一种基于不确定性的模态再平衡方法，用于检测仇恨表情符号。

1.  Learnable Privacy Neurons Localization in Language Models

1.  该文章提出了一种可学习的隐私神经元定位方法，用于保护语言模型中的隐私信息。

1.  Jailbreak Open-Sourced Large Language Models via Enforced Decoding

1.  该文章提出了一种强制解码的方法，用于监狱破解开源的大型语言模型。

1.  SafeDecoding: Defending against Jailbreak Attacks via Safety-Aware Decoding

1.  该文章提出了一种安全感知解码的方法，用于防御监狱破解攻击。

1.  ArtPrompt: ASCII Art-based Jailbreak Attacks against Aligned LLMs

1.  该文章提出了一种基于ASCII艺术的监狱破解攻击方法，用于对抗对齐的大型语言模型。

1.  A Glitch in the Matrix? Locating and Detecting Language Model Grounding with Fakepedia

1.  该文章提出了一种使用 Fakepedia 定位和检测语言模型基础的方法，以解决“虚拟世界”问题。

1.  Towards Privacy-Aware Sign Language Translation at Scale

1.  该文章研究了如何在大规模翻译手语时保护隐私，并提出了一种基于差分隐私的方法。

1.  Reducing Privacy Risks in Online Self-Disclosures with Language Models

1.  该文章研究了如何使用语言模型减少在线自我披露的隐私风险，并提出了一种基于对抗训练的方法。

1.  Pro-Woman, Anti-Man? Identifying Gender Bias in Stance Detection

1.  该文章研究了立场检测中的性别偏见问题，并提出了一种识别性别偏见的方法。

1.  A Survey on Modelling Morality for Text Analysis

1.  该文章对文本分析中的道德建模进行了综述，并总结了现有方法的优缺点。

1.  Mitigating Privacy Seesaw in Large Language Models: Augmented Privacy Neuron Editing via Activation Patching

1.  该文章提出了一种基于激活修补的增强隐私神经元编辑方法，用于减轻大型语言模型中的隐私风险。

1.  Unveiling Opinion Evolution via Prompting and Diffusion for Short Video Fake News Detection

1.  该文章提出了一种基于提示和扩散的方法，用于检测短视频中的假新闻和观点演变。

1.  Truth-Aware Context Selection: Mitigating Hallucinations of Large Language Models Being Misled by Untruthful Contexts

1.  该文章提出了一种基于真相感知的上下文选择方法，用于减轻大型语言模型被误导的幻觉问题。

1.  CodeAttack: Revealing Safety Generalization Challenges of Large Language Models via Code Completion

1.  该文章通过代码补全任务揭示了大型语言模型的安全泛化问题，并提出了一种攻击方法。

1.  SecFormer: Fast and Accurate Privacy-Preserving Inference for Transformer Models via SMPC

1.  该文章提出了一种基于安全多方计算的快速和准确的隐私保护推理方法，用于保护Transformer模型中的隐私信息。

1.  TELLER: A Trustworthy Framework for Explainable, Generalizable and Controllable Fake News Detection

1.  该文章提出了一种可解释、可广泛化和可控制的假新闻检测框架，以提高检测的可靠性。

1.  Protecting Privacy Through Approximating Optimal Parameters for Sequence Unlearning in Language Models

1.  该文章研究了如何通过近似最优参数实现语言模型中的序列卸载，以保护隐私信息。

1.  A Critical Study of What Code-LLMs (Do Not) Learn

1.  该文章对代码语言模型的学习能力进行了批判性研究，旨在识别其存在的问题和挑战。

1.  Defending LLMs against Jailbreaking Attacks via Backtranslation

1.  该文章提出了一种基于反向翻译的方法，用于防御对大型语言模型的监狱破解攻击。

1.  RePair: Automated Program Repair with Process-based Feedback

1.  该文章提出了一种基于过程反馈的自动化程序修复方法，用于提高程序的可靠性。

1.  VeraCT Scan: Retrieval-Augmented Fake News Detection with Justifiable Reasoning

1.  该文章提出了一种基于检索增强的假新闻检测方法，并提供了可证明的推理过程。

1.  Event-Radar: Event-driven Multi-View Learning for Multimodal Fake News Detection

1.  该文章提出了一种基于事件驱动的多视图学习方法，用于多模态假新闻检测。

1.  DrugWatch: A Comprehensive Multi-Source Data Visualisation Platform for Drug Safety Information

1.  该文章提出了一种综合性的多源数据可视化平台，用于监测药物安全信息。

1.  The Good and The Bad: Exploring Privacy Issues in Retrieval-Augmented Generation (RAG)

1.  该文章探讨了检索增强生成中的隐私问题，并提出了一种基于RAG的方法来保护隐私信息。

# **附前五篇文章摘要**

1.  GradSafe: Detecting Jailbreak Prompts for LLMs via Safety-Critical Gradient Analysis

> Abstract - Large Language Models (LLMs) face threats from jailbreak prompts. Existing methods for detecting jailbreak prompts are primarily online moderation APIs or finetuned LLMs. These strategies, however, often require extensive and resource-intensive data collection and training processes. In this study, we propose GradSafe, which effectively detects jailbreak prompts by scrutinizing the gradients of safety-critical parameters in LLMs. Our method is grounded in a pivotal observation: the gradients of an LLM’s loss for jailbreak prompts paired with compliance response exhibit similar patterns on certain safety-critical parameters. In contrast, safe prompts lead to different gradient patterns. Building on this observation, GradSafe analyzes the gradients from prompts (paired with compliance responses) to accurately detect jailbreak prompts. We show that GradSafe, applied to Llama-2 without further training, outperforms Llama Guard despite its extensive finetuning with a large dataset—in detecting jailbreak prompts. This superior performance is consistent across both zero-shot and adaptation scenarios, as evidenced by our evaluations on ToxicChat and XSTest. The source code is available at https://github.com/xyq7/GradSafe.

> 摘要 - 大型语言模型（LLMs）面临来自监狱破解提示的威胁。目前检测监狱破解提示的方法主要是在线审核 API 或微调的 LLMs。然而，这些策略通常需要大量的、资源密集型的数据收集和训练过程。在本研究中，我们提出了GradSafe，它通过检查 LLMs 中关键安全参数的梯度有效地检测监狱破解提示。我们的方法基于一个关键观察：监狱破解提示与合规响应配对的 LLM 的损失梯度在某些关键安全参数上呈现出类似的模式。相比之下，安全提示会导致不同的梯度模式。基于这一观察，GradSafe 分析来自提示（与合规响应配对）的梯度，以准确检测监狱破解提示。我们显示，GradSafe 在不进行进一步训练的情况下应用于 Llama-2 时，在检测监狱破解提示方面优于经过大规模数据集微调的 Llama Guard - 这种优越的性能在零样本和适配场景下都是一致的，如我们在 ToxicChat 和 XSTest 上的评估所证实。

1.  How Johnny Can Persuade LLMs to Jailbreak Them: Rethinking Persuasion to Challenge AI Safety by Humanizing LLMs

> Abstract - Most traditional AI safety research has approached AI models as machines and centered on algorithm-focused attacks developed by security experts. As large language models (LLMs) become increasingly common and competent, non-expert users can also impose risks during daily interactions. This paper introduces a new perspective on jailbreaking LLMs as human-like communicators to explore this overlooked intersection between everyday language interaction and AI safety. Specifically, we study how to persuade LLMs to jailbreak them. First, we propose a persuasion taxonomy derived from decades of social science research. Then we apply the taxonomy to automatically generate interpretable persuasive adversarial prompts (PAP) to jailbreak LLMs. Results show that persuasion significantly increases the jailbreak performance across all risk categories: PAP consistently achieves an attack success rate of over 92% on Llama 2-7b Chat, GPT-3.5, and GPT-4 in 10 trials, surpassing recent algorithm-focused attacks. On the defense side, we explore various mechanisms against PAP, find a significant gap in existing defenses, and advocate for more fundamental mitigation for highly interactive LLMs.

> 摘要 - 传统的 AI 安全研究大多将 AI 模型视为机器，并由安全专家开发算法 focused 攻击。随着大型语言模型（LLMs）越来越普遍和熟练，非专家用户在日常交互中也可能造成风险。本文提出了一种将 LLMs 视为人类似的沟通者的新角度，以探索这种日常语言交互和 AI 安全之间被忽视的交叉点。具体来说，我们研究如何说服 LLMs 进行监狱破解。首先，我们提出了一个从数十年社会科学研究中派生出来的说服分类法。然后，我们应用该分类法自动生成可解释的说服对抗性提示（PAP）来进行LLMs的监狱破解。结果显示，说服在所有风险类别中显著提高了监狱破解性能：在 Llama 2-7b Chat、GPT-3.5 和 GPT-4 上，PAP 在10次试验中始终实现了超过92％的攻击成功率，超过了最近的算法 focused 攻击。在防御方面，我们探索了各种机制来对抗 PAP，发现了现有防御中的显著缺陷，并主张对高度交互式的 LLMs 采取更基本的缓解措施。

1.  ToolSword: Unveiling Safety Issues of Large Language Models in Tool Learning Across Three Stages

![](img/d4e07ab04e6c0638d813bdd99f962ef4.png)

> Abstract -  Tool learning is widely acknowledged as a foundational approach or deploying large language models (LLMs) in real-world scenarios. While current research primarily emphasizes leveraging tools to augment LLMs, it frequently neglects emerging safety considerations tied to their application. To fill this gap, we present ToolSword, a comprehensive framework dedicated to meticulously investigating safety issues linked to LLMs in tool learning. Specifically, ToolSword delineates six safety scenarios for LLMs in tool learning, encompassing malicious queries and jailbreak attacks in the input stage, noisy misdirection and risky cues in the execution stage, and harmful feedback and error conflicts in the output stage. Experiments conducted on 11 open-source and closed-source LLMs reveal enduring safety challenges in tool learning, such as handling harmful queries, employing risky tools, and delivering detrimental feedback, which even GPT-4 is susceptible to. Moreover, we conduct further studies with the aim of fostering research on tool learning safety.

> 摘要 - 在当今的人工智能领域，大语言模型（LLMs）的实际部署已成为一个热门话题。随着技术的不断进步，LLMs 在处理复杂的自然语言处理任务方面展现出了巨大的潜力。然而，随着这些模型在现实世界中的应用越来越广泛，如何确保它们的安全性和可靠性也成为了一个不容忽视的问题。工具学习作为一种基础方法，对于在真实场景中部署 LLMs 至关重要。通过工具学习，LLMs 能够理解用户的意图，选择合适的工具与外部环境互动，并根据环境反馈调整其输出。这一过程涉及到输入、执行和输出三个阶段，每个阶段都有可能引入新的安全风险。例如，LLMs 可能会响应恶意查询，使用风险工具，或提供有害的反馈，这些都可能对用户造成伤害。因此，对 LLMs 在工具学习中的安全问题进行全面分析，对于推动这一领域的研究和实际应用具有重要意义。本文将介绍一个名为  ToolSword 的综合框架，该框架旨在揭示 LLMs 在工具学习过程中的安全问题，并通过对多个开源和闭源 LLMs 的实验，展示了当前模型在各个阶段面临的安全挑战。

1.  Transferable Embedding Inversion Attack: Uncovering Privacy Risks in Text Embeddings without Model Queries

> Abstract - This study investigates the privacy risks associated with text embeddings, focusing on the scenario where attackers cannot access the original embedding model. Contrary to previous research requiring direct model access, we explore a more realistic threat model by developing a transfer attack method. This approach uses a surrogate model to mimic the victim model’s behavior, allowing the attacker to infer sensitive information from text embeddings without direct access. Our experiments across various embedding models and a clinical dataset demonstrate that our transfer attack significantly outperforms traditional methods, revealing the potential privacy vulnerabilities in embedding technologies and emphasizing the need for enhanced security measures.

> 摘要 -  此研究调查了与文本嵌入相关的隐私风险，重点关注攻击者无法访问原始嵌入模型的情景。与之前需要直接模型访问的研究不同，我们通过开发一个转移攻击方法探索了一个更现实的威胁模型。这种方法使用代理模型模拟受害者模型的行为，使攻击者可以在没有直接访问的情况下从文本嵌入中推断敏感信息。我们在各种嵌入模型和临床数据集上的实验表明，我们的转移攻击方法显著优于传统方法，揭示了嵌入技术中的潜在隐私漏洞，并强调了增强安全性措施的必要性。

1.  Uncertainty-Guided Modal Rebalance for Hateful Memes Detection

![](img/bac4bc60fe7b18e942e780f45d257ef0.png)

> Abstract - Memes are widely used to spread ideas and messages on social networks. While many memes are intended for humor, some memes can be hateful and harmful, especially when images and text are combined in a specific way. Detecting hateful memes automatically can help reduce their negative impact on society. However, unlike conventional multimodal tasks, hateful memes detection is challenging due to the unique characteristics of memes' multimodal information. The image and text in memes are often weakly aligned or even irrelevant, requiring the model to understand the content and perform reasoning across multiple modalities. In this paper, we propose a novel method for multimodal hateful memes detection that incorporates the image captioning process into the memes detection process. Our approach is evaluated on multiple meme datasets, and the results demonstrate its effectiveness in detecting hateful memes. Our model achieves promising results on the Hateful Memes Detection Challenge.

> 摘要 -  表情包在社交网络上被广泛用于传播思想和信息。尽管大多数表情包是出于幽默的目的，但有些表情包在图片和文字的特定组合下会变得仇恨和有害。自动检测这些仇恨表情包可以帮助减少它们对社会的负面影响。然而，与传统的多模态任务不同，仇恨表情包检测由于表情包多模态信息的特殊性而具有挑战性。表情包中的图像和文本通常弱相关甚至无关，需要模型理解内容并在多个模态上进行推理。在本文中，我们提出了一种新的多模态仇恨表情包检测方法，它将图像描述过程融入到表情包检测过程中。我们在多个表情包数据集上进行了广泛的实验，并证明了我们的方法在检测仇恨表情包方面的有效性。我们的模型在仇恨表情包检测挑战赛中取得了令人期待的结果。