# ZhipuAI Papers List

> 来源：[https://v11enp9ok1h.feishu.cn/docx/E0qXd7AaCo85a3xrHqwc625znli](https://v11enp9ok1h.feishu.cn/docx/E0qXd7AaCo85a3xrHqwc625znli)

持续更新中 ……

## 2024

1.  CogCoM: Train Large Vision-Language Models Diving into Details through Chain of Manipulations

https://arxiv.org/abs/2402.04236

> 《CogCoM: Train Large Vision-Language Models Diving into Details through Chain of Manipulations》这篇论文介绍了一个名为CogCoM的通用视觉语言模型（VLM），它具备“操作链”（Chain of Manipulations, CoM）机制。CogCoM能够通过主动操纵输入图像进行多轮证据性的视觉推理。该模型家族包括CogCoM-base-17b、CogCoM-grounding-17b和CogCoM-chat-17b，这些模型拥有100亿视觉参数和70亿语言参数，分别针对不同的通用语料库进行训练，融合了四种数据能力的类型（指令遵循、OCR、详细字幕和CoM）。

> CogCoM通过一系列操作解决视觉问题，这些操作可能来源于先前的预训练（如grounding），也可能来源于模仿人类行为（如放大图像）。这种机制鼓励VLM通过证据性视觉推理生成忠实响应，并允许用户在可解释的路径中追踪错误原因。CogCoM模型及其代码已在GitHub上开源，并且提供了网页版的Demo，供人们体验和反馈。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqsx

1.  NaturalCodeBench: Examining Coding Performance Mismatch on HumanEval and Natural User Prompts

https://arxiv.org/abs/2405.04520

> 《NaturalCodeBench: Examining Coding Performance Mismatch on HumanEval and Natural User Prompts》这篇论文主要探讨了大型语言模型（LLMs）在代码生成方面的性能，特别是在现实世界编码任务中的应用。论文指出，尽管LLMs在生成代码方面表现出强大的能力，但现有的代码合成基准测试，如HumanEval、MBPP和DS-1000，主要针对算法和数据科学的入门级任务，无法充分满足现实世界编码中普遍存在的更具挑战性的要求。

> 为了填补这一空白，研究人员提出了NaturalCodeBench（NCB），这是一个具有挑战性的代码基准测试，旨在反映实际编码任务中的复杂性和多样性场景。NCB包含402个高质量的问题，这些问题是用Python和Java编写的，精心挑选自在线编码服务的自然用户查询，覆盖了6个不同的领域。为了提高测试用例构建的效率，研究人员还引入了半自动化管道，与人工解决方案相比，效率提升了4倍以上。

> 通过对39个LLMs进行的系统实验，研究发现，即使在HumanEval分数接近的情况下，模型在NCB上的性能差距仍然很大，这表明现有模型可能缺乏对实际代码合成场景的关注，或者过度优化于HumanEval的特定要求。此外，即使是性能最好的GPT-4，在NCB上的表现也远未达到理想水平。这项研究的结果和评估工具包可以在https://github.com/THUDM/NaturalCodeBench上找到。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqs9

1.  Inf-DiT: Upsampling Any-Resolution Image with Memory-Efficient Diffusion Transformer

https://arxiv.org/abs/2405.04312

> 《Inf-DiT: Upsampling Any-Resolution Image with Memory-Efficient Diffusion Transformer》这篇论文提出了一种名为Inf-DiT的新型图像上采样模型。该模型利用了一种单向块注意力机制，这种机制能够在生成超高清分辨率图像（例如4096×4096）时，将内存使用从二次方减少到线性，从而显著降低了内存需求。具体来说，与常用的UNet架构相比，Inf-DiT在生成4096×4096图像时，能够节省超过5倍的内存。

> Inf-DiT模型基于DiT结构进行上采样，并能够处理各种形状和分辨率的图像。该模型在生成超高清分辨率图像方面达到了最先进的性能，无论是在机器评估还是人类评估中。此外，Inf-DiT还引入了全局图像嵌入和交叉注意力等技术，以增强图像的一致性，并通过CLIP图像-文本潜在空间实现了灵活的文本引导。

> 总的来说，Inf-DiT为图像上采样提供了一种高效且灵活的解决方案，特别适用于需要处理高分辨率图像的场景。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqsd

1.  BOND: Bootstrapping From-Scratch Name Disambiguation with Multi-task Promoting

https://arxiv.org/abs/2404.08322

> 《BOND: Bootstrapping From-Scratch Name Disambiguation with Multi-task Promoting》这篇论文讨论了从零开始的姓名消歧问题，这对于建立学术平台的可靠基础至关重要。姓名消歧涉及将同名个人撰写的文档划分为代表不同现实专家的组。传统上，这一过程分为两个解耦的任务：局部估计文档之间的成对相似性，然后全局地将这些文档分组到适当的簇中。然而，这种解耦的方法往往阻碍了这两个交织任务之间的最佳信息交换。

> 因此，论文提出了BOND方法，它通过端到端的方式相互促进局部和全局信息信号。具体来说，BOND利用局部成对相似性来驱动全局聚类，随后生成伪聚类标签。这些全局信号进一步细化了局部成对特征。实验结果表明，BOND优于其他先进基线，性能提升显著。此外，一个改进版本BOND+，结合了集成和后匹配技术，与WhoIsWho竞赛中的顶级方法相媲美。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqsT

1.  Transferable and Efficient Non-Factual Content Detection via Probe Training with Offline Consistency Checking

https://arxiv.org/abs/2404.06742

> 《Transferable and Efficient Non-Factual Content Detection via Probe Training with Offline Consistency Checking》是一项关于提高大型语言模型（LLMs）生成内容可信度的研究。这项研究提出了一种名为PiNose的方法，它通过离线自我一致性检查的结果来训练探测模型，从而避免了使用人工标注数据的需要，并在不同的数据分布中实现了迁移性。由于一致性检查过程是离线的，PiNose减少了在线一致性验证过程中生成多个响应的计算负担。此外，PiNose在响应解码之前分析LLMs的各种内部状态方面做出了贡献，从而更有效地检测事实不准确之处。在事实检测和问题回答基准测试上的实验结果表明，PiNose在多个QA数据集上超过了基于监督式探测的基线，尽管它是没有标注标签的情况下训练的。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqsG

1.  AutoWebGLM: Bootstrap And Reinforce A Large Language Model-based Web Navigating Agent

https://arxiv.org/abs/2404.03648

> 《AutoWebGLM: Bootstrap And Reinforce A Large Language Model-based Web Navigating Agent》这篇论文介绍了一个名为AutoWebGLM的大型语言模型（LLM）驱动的自动化网页导航代理。AutoWebGLM基于ChatGLM3-6B模型构建，旨在更有效地导航网页并更好地解决现实世界的浏览挑战。

> AutoWebGLM的主要特点包括：

+   **HTML简化算法**：受人类浏览模式启发，AutoWebGLM设计了一种简化HTML的算法，使网页对LLM代理更具可消化性，同时保留关键信息。

+   **混合人机训练**：结合人类和AI知识构建网页浏览数据，用于课程训练，增强模型的实际导航技能。

+   **强化学习和拒绝采样**：通过强化学习和拒绝采样引导模型，进一步促进网页理解、浏览器操作和高效的任务分解。

+   **双语网页导航基准**：引入AutoWebBench，这是一个中英双语的网页浏览任务基准，为AI网页导航代理的测试和调整提供了强大的工具。

> AutoWebGLM及其相关代码已在GitHub上开源，并提供公开的评估代码、数据和环境，供研究者进行实验。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6t

1.  ChatGLM-Math: Improving Math Problem-Solving in Large Language Models with a Self-Critique Pipeline

https://arxiv.org/abs/2404.02893

> 《ChatGLM-Math: Improving Math Problem-Solving in Large Language Models with a Self-Critique Pipeline》这篇论文介绍了一种名为ChatGLM-Math的新方法，用于提升大型语言模型（LLMs）解决数学问题的能力。这项工作主要针对LLMs在数学问题解决方面的挑战，尤其是在实际应用中。论文的核心内容如下：

+   **问题背景**：尽管LLMs在语言能力方面表现出色，但在需要解决数学问题的实际应用中仍然面临挑战。目前，虽然已经开发了许多策略和数据集来增强LLMs的数学能力，但在已部署的LLM系统中同时保持和提高语言和数学能力仍然是一个挑战。

+   Self-Critique Pipeline：为了解决这一挑战，研究人员开发了一种名为Self-Critique的流程。这个流程在LLM的对齐阶段工作，首先从LLM本身训练一个通用的Math-Critique模型以提供反馈信号。然后，依次对LLM自己的生成结果进行拒绝采样微调和直接偏好优化。

+   **实验与结果**：基于ChatGLM3-32B，研究人员在学术数据集和他们新创建的挑战性数据集MathUserEval上进行了一系列实验。结果显示，这种方法显著增强了LLM的数学问题解决能力，同时还提高了其语言能力，性能超过了可能是其两倍大的LLM。

+   **MathUserEval测试集**：MathUserEval是一个为真实使用场景设计的测试集，针对用户关心的问题和更具挑战性的数学问题。这个测试集包含545道高质量数学问题，以及22道补充交叉学科数学问题。

+   **评价方法**：为了有效评估响应的质量，MathUserEval目前采用GPT-4-1106-Preview来分析并随后对响应进行评分。

> 这项研究对于提升LLMs在数学问题解决方面的能力具有重要意义，尤其是在实际应用场景中的表现。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6O

1.  ChatGLM-RLHF: Practices of Aligning Large Language Models with Human Feedback

https://arxiv.org/abs/2404.00934

> 《ChatGLM-RLHF: Practices of Aligning Large Language Models with Human Feedback》这篇论文介绍了一种名为ChatGLM-RLHF的方法，用于改进大型语言模型（LLMs）与人类偏好的一致性。ChatGLM-RLHF是一个基于人类反馈的强化学习（RLHF）系统，旨在增强ChatGLM模型与人类偏好的对齐。

> ChatGLM-RLHF包含三个主要组成部分：人类偏好数据的收集、奖励模型的训练和策略的优化。在将ChatGLM-RLHF集成到生产环境中的整个过程中，作者遇到了几个前所未有的挑战，并提出了相应的解决策略。例如，为了稳定大规模训练，他们减少了奖励的方差，实现了融合梯度下降的模型并行性，并设计了正则化约束，以避免LLMs中的灾难性遗忘。

> 实验结果表明，与ChatGLM的监督微调（SFT）版本相比，ChatGLM-RLHF在对齐任务方面带来了显著的改进。例如，在中文对齐任务中，ChatGLM-RLHF平均比ChatGLM-SFT多赢了15%。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6z

1.  Extensive Self-Contrast Enables Feedback-Free Language Model Alignment

https://arxiv.org/abs/2404.00604

> 《Extensive Self-Contrast Enables Feedback-Free Language Model Alignment》这篇论文介绍了一种名为Self-Contrast的方法，用于大型语言模型（LLM）的对齐，无需反馈。这种方法通过利用LLM自身生成的大量多样化候选来工作，并使用预训练的嵌入模型根据文本相似性过滤多个负样本。理论上，研究显示，在这种设置下，仅仅扩大负响应的数量仍然可以有效近似具有更平衡的正负偏好注释的情况。在三个数据集上的直接偏好优化（DPO）实验表明，Self-Contrast在性能上始终优于监督式微调（SFT）和标准DPO训练，并且随着自生成负样本数量的增加，其性能持续提升。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6p

1.  TableLLM: Enabling Tabular Data Manipulation by LLMs in Real Office Usage Scenarios

https://arxiv.org/abs/2403.19318

> 《TableLLM: Enabling Tabular Data Manipulation by LLMs in Real Office Usage Scenarios》是一项研究，旨在通过大型语言模型（LLMs）高效处理实际办公场景中的表格数据操作任务。TableLLM是一个强大的大型语言模型，专为处理表格数据操作任务而设计，无论是嵌入在文档还是电子表格中，都能满足实际办公需求。

> TableLLM系列包括两种不同规模：TableLLM-7B和TableLLM-13B，它们都是基于CodeLlama-7B和13B微调得到的。TableLLM能够根据不同场景生成代码解决方案或直接文本回答。例如，代码生成用于处理嵌入在电子表格中的表格数据，通常涉及表格的插入、删除、更新、查询、合并和图表操作。而文本生成则用于处理嵌入在文档中的表格数据，通常涉及短表格的查询操作。

> TableLLM的评估结果显示，它在处理表格数据方面表现出显著优势。例如，TableLLM-13B在WikiTQ、TAT-QA、FeTaQA和OTTQA等四个文本回答生成能力测试基准上的表现超过了其他通用及表格数据专用的模型。此外，TableLLM-13B在WikiSQL、Spider和自创的表格操作基准上的代码解决方案生成能力也得到了验证。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6J

1.  Understanding Emergent Abilities of Language Models from the Loss Perspective

https://arxiv.org/abs/2403.15796

> 《Understanding Emergent Abilities of Language Models from the Loss Perspective》这篇论文探讨了语言模型中新兴能力的本质，特别是在预训练损失（pre-training loss）方面的表现。这项研究挑战了传统的观点，即大型模型才具有新兴能力。研究发现，小型模型在某些新兴能力上也能表现出高水平。此外，用于衡量这些能力的指标存在非连续性的问题。

> 论文的主要贡献包括：

+   **新兴能力的重新定义**：论文提出了从预训练损失的角度来研究新兴能力，而非仅仅基于模型大小或训练计算量。

+   **性能一致性**：研究发现，即使模型和数据大小不同，具有相同预训练损失的模型在各种下游任务上的表现是相同的。

+   **预训练损失阈值**：当模型的预训练损失低于特定阈值时，无论度量的连续性如何，该模型在某些任务上会展现出新兴能力。

+   **随机猜测与性能差异**：在达到这个阈值之前，模型的性能仍然处于随机猜测的水平。

> 这项研究对于理解语言模型中的新兴能力，特别是在预训练损失对模型性能的影响方面，具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6w

1.  Extensive Self-Contrast Enables Feedback-Free Language Model Alignment

https://arxiv.org/abs/2404.00604

《Extensive Self-Contrast Enables Feedback-Free Language Model Alignment》这篇论文介绍了一种名为Self-Contrast的新方法，用于无需人类反馈的大型语言模型（LLM）对齐。这种方法通过利用LLM大量生成的负样本来实现。在只有监督微调（SFT）目标的情况下，Self-Contrast利用LLM本身生成大量多样化的候选项，并利用预训练的嵌入模型根据文本相似性过滤多个负样本。实验结果表明，Self-Contrast在三个数据集上的直接偏好优化（DPO）中表现出色，明显优于SFT和标准的DPO训练。随着自生成负样本数量的增加，Self-Contrast的性能也持续提升。

这项研究对于LLM的进一步发展和应用具有重要意义，特别是在不需要昂贵的人类反馈的情况下，提高了LLM对齐的效率和效果。

点击查看智谱清言的回答：https://chatglm.cn/share/FWq6I

1.  CogView3: Finer and Faster Text-to-Image Generation via Relay Diffusion

https://arxiv.org/abs/2403.05121

> 《CogView3: Finer and Faster Text-to-Image Generation via Relay Diffusion》这篇论文介绍了一个名为CogView3的文本到图像生成模型，这是第一个在文本到图像生成领域实现中继扩散（Relay Diffusion）的模型。CogView3通过首先创建低分辨率图像，然后应用基于中继的超分辨率来执行任务。这种方法不仅产生了竞争力强的文本到图像输出，而且大大减少了训练和推理成本。

> CogView3的关键创新点包括：

+   **中继扩散框架**：CogView3采用了一种新的级联扩散框架，将生成高分辨率图像的过程分解为多个阶段。首先使用标准扩散生成低分辨率图像，然后执行中继超分辨率生成。这种方法避免了在低分辨率条件下的复杂增强技术，并有可能纠正前一扩散阶段产生的不满意的伪影。

+   **显著的性能提升**：CogView3在人类评价中比当前最先进的开源文本到图像扩散模型SDXL表现提高了77.0%，同时推理时间仅需SDXL的约1/2。

+   **蒸馏变体的性能**：CogView3的蒸馏变体在仅利用SDXL推理时间的1/10的情况下实现了可比较的性能。

+   **高效训练和推理**：CogView3通过在基础和超分辨率阶段之间合理分配采样步骤，显著降低了推理成本，同时保持了高分辨率的生成能力。

> 这项工作展示了CogView3在文本到图像生成领域的突破，特别是在提高生成效率和图像质量方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq66

1.  Does Negative Sampling Matter? A Review with Insights into its Theory and Applications

https://arxiv.org/abs/2402.17238

> 《Does Negative Sampling Matter? A Review with Insights into its Theory and Applications》这篇论文对负采样的重要性进行了深入探讨。负采样是一种在机器学习、计算机视觉、自然语言处理、数据挖掘和推荐系统等领域广泛应用的技术。这篇综述文章提出了一些关键问题，例如负采样是否真的重要，是否存在一个可以包含所有现有负采样方法的通用框架，以及它在哪些领域中被应用。

> 文章首先回顾了负采样的历史，并追踪了其通过五种进化路径的发展。接着，它剖析和分类了用于选择负样本候选者的策略，包括全局、局部、小批量、跳跃和基于内存的方法。这些策略被进一步分类为五种类型的负采样方法：静态、硬、基于GAN、基于辅助和批量方法，为理解负采样提供了清晰的结构。

> 此外，文章还强调了负采样在各个领域的应用，并深入了解了其实际好处。最后，文章简要讨论了负采样的未解决问题和未来的发展方向。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6e

1.  PST-Bench: Tracing and Benchmarking the Source of Publications

https://arxiv.org/abs/2402.16009

> 《PST-Bench: Tracing and Benchmarking the Source of Publications》是一项研究，旨在追踪和基准测试研究论文的来源。这项研究指出，追踪研究论文的来源对于研究人员来说是一项基本但具有挑战性的任务。论文之间数以亿计的引用关系可能阻碍研究人员有效地理解科学的演变。迄今为止，仍然缺乏一个由专业研究人员构建的准确数据集，用以识别他们研究论文的直接来源，基于此可以开发自动算法来扩展科学的演化知识。

> 在这项研究中，作者们正式定义了论文来源追踪（PST）问题，并构建了一个高质量且不断增长的基准数据集PST-Bench，涵盖了计算机科学领域。基于PST-Bench，他们还揭示了一些有趣的发现，例如不同领域（如AI和HPC）论文的生命力差异。此外，他们探索了各种方法，验证了PST-Bench的难度，并指出了这个主题的潜在研究方向。该数据集和代码已公开发布。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq6Z

1.  OAG-Bench: A Human-Curated Benchmark for Academic Graph Mining

https://arxiv.org/abs/2402.15810

> 《OAG-Bench: A Human-Curated Benchmark for Academic Graph Mining》这篇论文介绍了一个名为OAG-Bench的学术图挖掘的全面、多方面和细粒度的人工策划基准。这个基准是基于开放学术图谱（OAG）构建的，旨在为学术图挖掘领域提供一个共同的基础，用于评估和比较算法，从而加速算法开发和进步。

> OAG-Bench的主要特点包括：

+   **全面的覆盖**：OAG-Bench涵盖了10个任务、20个数据集、70多个基线和120多个实验结果。

+   **新的数据标注策略**：为某些任务提出了新的数据标注策略，并提供了一系列数据预处理代码、算法实现和标准化评估协议，以促进学术图挖掘。

+   **挑战与实验结果**：大量实验表明，即使是高级算法如大型语言模型（LLMs）在解决某些任务的关键挑战时也会遇到困难，例如论文来源追踪和学者分析。

+   **OAG挑战赛**：引入了开放学术图谱挑战赛（OAG-Challenge），以鼓励社区参与和分享。

> OAG-Bench的目的是作为一个共同的基础，用于评估和比较学术图挖掘算法，从而加速该领域的算法开发和进步。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqkB

1.  TriSampler: A Better Negative Sampling Principle for Dense Retrieval

https://arxiv.org/abs/2402.11855

> 《TriSampler: A Better Negative Sampling Principle for Dense Retrieval》这篇论文介绍了一种名为TriSampler的新型负采样方法，旨在改进密集检索中的负采样技术。在密集检索中，负采样是一种关键技术，用于有效训练检索模型，对检索性能产生显著影响。尽管现有的负采样方法已经通过利用难负样本取得了有希望的结果，但仍然缺乏一个通用的原则来指导负采样，包括负候选构建和负采样分布设计。

> 为了解决这个问题，论文进行了密集检索中负采样的理论分析，并提出了准三角形原则，以阐述查询、正文档和负文档之间的三角形类似关系。基于这一原则，论文开发了TriSampler，这是一个简单但非常有效的负采样方法，旨在在受约束的区域中选择性地采样更有信息量的负样本。实验结果显示，TriSampler在不同代表性的检索模型上均能实现卓越的检索性能。

> 这项研究对于改进密集检索中的负采样技术具有重要意义，特别是在提高检索模型的训练效率和性能方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqk2

1.  CogCoM: Train Large Vision-Language Models Diving into Details through Chain of Manipulations

https://arxiv.org/abs/2402.04236

> 《CogCoM: Train Large Vision-Language Models Diving into Details through Chain of Manipulations》这篇论文介绍了一种名为CogCoM的视觉语言模型（VLM）训练方法。CogCoM利用了一种名为“Chain of Manipulations”的机制，通过一系列对视觉输入的操作来解决问题。这些操作可以来自先前的训练获得的内在能力，如grounding（定位），或者是模仿人类行为，如放大图像。

> CogCoM的核心思想是通过对视觉输入进行一系列的操纵来解决视觉问题。这种方法鼓励VLM生成具有证据支持的视觉推理的忠实响应，并允许用户在可解释的路径中跟踪错误的原因。CogCoM-17B模型采用了这种方法，并在训练过程中分为两个阶段：第一阶段是预训练，用于建立基础的视觉理解和图像-问题-答案三元组的生成；第二阶段是对齐训练，进一步训练模型，使其与人类在解决实际视觉问题上的偏好相一致。

> 实验证明，CogCoM在来自三个类别的八个基准上实现了最先进的性能，并且在有限数量的训练步骤中，数据迅速获得了竞争性能。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqkE

1.  Towards Efficient and Exact Optimization of Language Model Alignment

https://arxiv.org/abs/2402.00856

> 《Towards Efficient and Exact Optimization of Language Model Alignment》这篇论文提出了一种名为EXO（Efficient Exact Optimization）的新方法，用于高效和精确地对齐语言模型与人类偏好。这项研究的核心在于解决强化学习（RL）在优化语言模型策略时遇到的方差高的问题，尤其是在处理人类偏好数据时。

> 论文的主要贡献包括：

+   提出了一种新的优化目标函数，该函数通过重要性采样和贝叶斯推断的原理，旨在最小化策略与奖励之间的反向KL散度。

+   EXO方法被证明在任意策略参数化下，其优化方向与RL算法在渐近线上相同，同时通过绕过与RL算法相关的复杂性，实现了更有效的优化。

+   论文通过理论分析和实证研究，将EXO方法与直接偏好优化（DPO）进行了比较，并展示了EXO方法在现实人类偏好数据上的优势。

> 此外，EXO算法的官方Python实现可在GitHub上找到，该实现基于PyTorch，支持直接在偏好数据上进行训练，以及利用学习到的奖励模型进行监督的训练。EXO算法在训练语言模型以适应人类偏好方面提供了新的视角和方法。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqkN

1.  LongAlign: A Recipe for Long Context Alignment of Large Language Models

https://arxiv.org/abs/2401.18058

> 《LongAlign: A Recipe for Long Context Alignment of Large Language Models》是一项研究，旨在改进大型语言模型（LLMs）在处理长上下文任务时的性能。这项研究提出了一种名为LongAlign的方法，用于在输入序列长度相似的情况下对LLMs进行微调，以有效处理长上下文。LongAlign包括三个主要部分：构建一个长指令遵循数据集、采用打包和排序批处理策略以及开发损失加权方法。

> 首先，LongAlign利用Self-Instruct构建了一个长指令遵循数据集，确保数据多样性，覆盖了来自各种长上下文来源的广泛任务。其次，为了解决数据长度分布不均导致的训练效率问题，研究团队采用了打包和排序批处理策略，以加快监督式微调过程。此外，他们还开发了一种损失加权方法，以平衡打包训练期间不同序列对损失的贡献。最后，研究团队引入了LongBench-Chat基准，用于评估LLMs在10k-100k长度查询上的指令遵循能力。实验结果显示，LongAlign在长上下文任务上比现有方法提高了高达30%的性能，同时保持了处理短通用任务的能力。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqkt

1.  RecDCL: Dual Contrastive Learning for Recommendation

https://arxiv.org/abs/2401.15635

> 《RecDCL: Dual Contrastive Learning for Recommendation》这篇论文介绍了一种名为RecDCL的双对比学习推荐框架。这个框架结合了批处理对比学习（BCL）和特征对比学习（FCL），以提高推荐系统的性能。

> RecDCL的主要特点包括：

+   **解决数据稀疏性问题**：自监督学习（SSL）通过对比原始和增强数据的嵌入来解决Web平台上的数据稀疏性问题。

+   批处理对比学习和特征对比学习的结合：现有的基于对比学习的SSL方法主要关注批处理方式的对比，而忽略了特征维度上的潜在规律。这种方法可能导致用户和物品表示学习中的冗余解决方案。

+   **消除冗余解决方案**：RecDCL通过结合BCL和FCL来消除冗余解决方案，同时不会错过最优解。

+   **FCL和BCL的目标**：在RecDCL中，FCL目标旨在消除用户-物品正对中的冗余解决方案，并使用多项式核优化用户和物品内的均匀分布，使表示正交；BCL目标则用于生成输出向量上的对比嵌入，以增强表示的鲁棒性。

+   **实验结果**：在四个广泛使用的基准测试和一个行业数据集上的实验表明，RecDCL可以持续优于最先进的基于图神经网络（GNNs）和SSL的模型（在Recall@20方面有高达5.65%的改进）。

> 这项研究对于推荐系统领域，特别是在提高推荐系统性能和解决数据稀疏性问题上具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqkg

1.  SciGLM: Training Scientific Language Models with Self-Reflective Instruction Annotation and Tuning

https://arxiv.org/abs/2401.07950

> 《SciGLM: Training Scientific Language Models with Self-Reflective Instruction Annotation and Tuning》这篇论文介绍了一种名为SciGLM的科学语言模型训练方法。这种方法的核心是利用自我反思式的指令注释框架，以解决科学领域数据稀缺的挑战。具体来说，SciGLM利用现有的大型语言模型（LLMs）为未标记的科学问题生成逐步推理，然后通过自我反思的批评和修正过程进行优化。

> 论文的主要贡献包括：

+   **自我反思式指令注释框架**：这个框架是SciGLM的核心，它利用LLMs生成对未标记科学问题的逐步推理，并通过自我反思的批评和修正过程来优化这些推理。

+   **SciInstruct数据集**：SciGLM创建了一个包含物理、化学、数学和形式证明的多样化和高质量的数据集，用于微调ChatGLM家族的语言模型，从而提高它们在科学和数学推理方面的能力。

+   **性能提升**：SciGLM在基础模型（ChatGLM3-6B-Base）上提高了4.87%，在大规模模型（32B）上提高了2.67%，同时保持了基础模型的语言理解能力。

+   **开源发布**：为了促进科学研究社区的利益，SciGLM和SciInstruct已经开源发布，包括自我反思框架和微调代码。

> 这篇论文对于科学语言模型的训练和应用是一个重要的贡献，特别是在解决科学领域数据稀缺和提高模型在科学和数学推理方面的能力方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqkJ

1.  APAR: LLMs Can Do Auto-Parallel Auto-Regressive Decoding

https://arxiv.org/abs/2401.06761

> APAR（Auto-Parallel Auto-Regressive）论文的创新点在于提出了一种新型的解码策略，旨在提高大型语言模型（LLMs）的推理效率。这项策略利用了LLMs在文本生成中的固有并行结构，通过在具有层次结构的数据上进行微调，使模型能够独立规划其生成过程，并执行自动并行自回归（APAR）生成。这种方法显著减少了生成步骤的数量，APAR本身可以实现高达2倍的速度提升，结合推测性解码时，速度提升可达4倍。

> APAR通过训练语言模型识别可并行化的结构，并在遇到可并行化的响应结构时，自动启动并行解码线程。这种方法将传统的线性生成转变为可并行化的段落树结构，不仅提高了解码并行性，还通过基于树的注意力机制减少了注意力跨度，并使得消耗的键值（KV）缓存内存能够更早释放。

> 在实验中，研究人员使用了Vicuna模型家族进行测试。在内存受限的场景中，APAR有助于减少模型延迟，并在Vicuna Bench上实现了平均2倍的生成速度提升。此外，在高吞吐量场景中，APAR与vLLM的兼容性使得可以提前释放内存，减少了高达50%的KV缓存需求，同时保持了相同水平的吞吐量。APAR还减少了参与注意力计算的令牌数量。使用相同数量的KV缓存内存时，与原始自回归过程相比，吞吐量提高了20-70%，延迟降低了20-35%，同时保持了相同的服务并发性。重要的是，使用APAR的生成质量并未受到影响，在多个类别的MT Bench和Vicuna Bench上的评估表明，响应质量与其自回归对应物大致一致，变化范围在±2%之内。这表明基于APAR的模型在提升解码速度和效率的同时，保留了上下文生成能力。

> 点击查看智谱清言智能体「论文助手」的回答https://chatglm.cn/share/FWqAu

1.  xTrimoPGLM: Unified 100B-Scale Pre-trained Transformer for Deciphering the Language of Protein

https://arxiv.org/abs/2401.06199

> xTrimoPGLM 论文的创新点在于它提出了一种统一的蛋白质语言模型，通过创新的预训练框架同时解决蛋白质理解和生成任务。这项工作探索了两类目标的兼容性和联合优化潜力，从而制定了一个前所未有的1000亿参数规模和消耗1万亿训练代币的策略来训练xTrimoPGLM。实验结果表明，xTrimoPGLM在18个蛋白质理解基准测试中显著优于其他先进基线，并且能够提供原子分辨率的蛋白质结构视图，从而超越现有的基于语言模型的工具，为3D结构预测模型带来了先进的改进。

> 点击查看智谱清言智能体「论文助手」的回答：https://chatglm.cn/share/FWqAK

## 2023

1.  Deep learning-based system for automatic prediction of triple-negative breast cancer from ultrasound images

https://link.springer.com/article/10.1007/s11517-022-02728-4

> 《Deep learning-based system for automatic prediction of triple-negative breast cancer from ultrasound images》这篇论文介绍了一个深度学习系统，用于仅从超声图像自动识别三阴性乳腺癌（TNBC）。该研究在2018年4月至2019年3月期间，共招募了145名患者和831张超声图像。研究使用了一种基于VGG架构的卷积神经网络（CNN）来预测TNBC。模型的性能通过随机k折分层交叉验证进行评估。结果显示，该深度学习系统表现出良好的效能，具有0.86的AUC值，准确率为85%，敏感性和特异性均为86%，F1分数为0.74。此外，模型学习到的内部表示特征在不同分子亚型组之间有清晰的区分。这种深度学习系统可以预先准确地预测三阴性乳腺癌，有助于实现更精确和全面的管理。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqh1

1.  Towards Lightweight and Automated Representation Learning System for Networks

https://arxiv.org/abs/2302.07084

> 《Towards Lightweight and Automated Representation Learning System for Networks》这篇论文介绍了一个名为LightNE 2.0的网络嵌入系统，旨在提供成本效益高、可扩展、自动化且高质量的嵌入解决方案。该系统能够处理拥有数百亿条边的图，并可在单个机器上运行。与主流观点相反，该研究证明了在共享内存、仅使用CPU的架构下，可以实现更高的质量、更好的可扩展性、更低的成本和更快的运行时间，而无需分布式架构和GPU。

> LightNE 2.0结合了两种理论基础牢固的嵌入方法NetSMF和ProNE。论文首次介绍了以下技术：1）一种新提出的降采样方法，以减少NetSMF的样本复杂性，同时保留其理论优势；2）一种高性能的并行图处理堆栈GBBS，以实现高内存效率和可扩展性；3）一种稀疏并行哈希表，用于聚合和维护矩阵稀疏化器在内存中；4）一种通过功率迭代和快速正交化增强的快速随机奇异值分解（SVD），以提高常规随机SVD在效率和有效性方面的表现；5）Intel MKL用于提出的快速随机SVD和谱传播；6）一个快速轻量级的AutoML库FLAML，用于自动超参数调优。实验结果表明，LightNE 2.0的性能优于GraphVite、PBG和NetSMF，同时能够更快地嵌入非常大规模的图，例如在CPU服务器上以半小时嵌入17亿个节点和1240亿条边的图，而其他基线无法处理这种规模的非常大数据图。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqhp

1.  Web-Scale Academic Name Disambiguation: the WhoIsWho Benchmark, Leaderboard, and Toolkit

https://arxiv.org/abs/2302.11848

> 《Web-Scale Academic Name Disambiguation: the WhoIsWho Benchmark, Leaderboard, and Toolkit》这篇论文介绍了一个名为WhoIsWho的框架，用于解决在线学术系统中一个基本问题：姓名消歧。随着研究论文数量的增加，姓名消歧问题变得更加复杂。例如，在AMiner这样的在线学术搜索平台上，约有10%的姓名对应超过100名作者。现有的研究由于使用了小规模或低质量的数据集，未能有效解决这些实际挑战案例。

> WhoIsWho框架的特点包括：

+   **大规模基准测试**：该框架包含超过100万篇论文的大型基准测试，这些论文是通过交互式注释过程构建的。

+   **综合任务排行榜**：提供了一个定期的排行榜，其中包括全面任务。

+   **易于使用的工具包**：该工具包封装了整个流程以及用于解决任务的最强大功能和基准模型。

+   **在线部署**：开发出的强基准模型已在AMiner系统中在线部署，用于每日arXiv论文分配。

> 这项研究对于提高在线学术系统中姓名消歧的效率和准确性具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq8Z

1.  GLM-Dialog: Noise-tolerant Pre-training for Knowledge-grounded Dialogue Generation

https://arxiv.org/abs/2302.14401

> 《GLM-Dialog: Noise-tolerant Pre-training for Knowledge-grounded Dialogue Generation》这篇论文介绍了一个名为GLM-Dialog的预训练语言模型，旨在解决知识驱动对话生成中的噪声数据挑战。GLM-Dialog是一个大规模语言模型（LLM），拥有100亿参数，能够使用搜索引擎访问互联网知识进行中文知识驱动对话。该模型提供了一系列适用于利用各种外部知识（包括有用的和噪声知识）的技术，从而能够创建稳健的知识驱动对话语言模型，即使数据集有限也能有效工作。

> GLM-Dialog的训练过程分为两个阶段：

+   **无知识预训练阶段**：在这一阶段，模型从基准来源、社交媒体平台和在线服务收集了大量对话数据进行训练，重点是让模型熟悉对话的结构和模式。

+   **外部知识和噪声识别训练阶段**：在这一阶段，模型被注入外部知识以增强对话生成能力。同时，为了减轻将不可靠或不正确的信息纳入生成回应的风险，训练过程中引入了噪声识别任务，帮助模型学习在对话生成过程中识别并忽略噪声或不可靠信息，确保回应的质量和准确性。

> 通过这两个阶段的结合，GLM-Dialog旨在创建一个容忍噪声的知识驱动对话生成的预训练框架，使模型能够在利用外部知识的同时对不准确性或噪声保持稳健性。此外，论文还提出了一种新的评估方法，允许人类与多个部署的机器人同时对话，并通过隐式比较它们的性能，而不是使用多维指标进行显式评分。这些评估结果表明，GLM-Dialog能够产生比其他对比模型更吸引人的回应。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqIv

1.  SketchNE: Embedding Billion-Scale Networks Accurately in One Hour

https://ieeexplore.ieee.org/document/10057080

> 《SketchNE: Embedding Billion-Scale Networks Accurately in One Hour》这篇论文介绍了一个名为SketchNE的网络嵌入方法，旨在在大约一个小时内准确地嵌入拥有数十亿个节点的网络。SketchNE的目标是为拥有超过10亿个节点和1000亿条边的网络生成高质量的嵌入。这项研究特别关注于大规模网络嵌入，以解决网络规模扩大时面临的挑战。

> SketchNE的主要特点包括：

+   **大规模网络嵌入**：SketchNE专注于生成高质量的网络嵌入，适用于拥有超过10亿个节点和1000亿条边的网络。

+   **高效计算**：SketchNE能够在单台CPU上快速运行，大大降低了计算复杂性。

+   **技术挑战**：在大规模网络嵌入中，技术挑战包括计算效率和内存管理。SketchNE通过其创新方法解决了这些挑战。

> 这项研究对于网络嵌入领域，特别是在处理大规模网络时，具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqYD

1.  Parameter-efficient fine-tuning of large-scale pre-trained language models

https://arxiv.org/abs/2312.12148

> 《Parameter-efficient fine-tuning of large-scale pre-trained language models》这篇论文介绍了一种名为Delta Tuning的参数高效微调方法，用于大规模预训练语言模型。该方法旨在解决全参数微调带来的高计算和存储成本问题。Delta Tuning的核心思想是冻结预训练模型99%以上的参数，仅利用少量下游任务数据微调少于1%的模型参数。

> Delta Tuning的主要特点包括：

+   **增量微调**：与全参数微调相比，Delta Tuning只调整增量参数，从而显著降低计算和存储开销。

+   **性能比较和分析**：研究团队对超过100个自然语言处理任务进行了全面细致的性能比较和分析，表明Delta Tuning方法在性能上可以媲美全参数微调。

+   **方法分类**：Delta Tuning方法分为三类：添加式、指定式和重参数化方法。

+   **理论框架**：团队提出了增量微调的理论框架，从参数优化和最优控制两个角度进行分析。

+   **开源工具包OpenDelta**：基于该论文成果研制发布了开源工具包OpenDelta，支持研究者和开发者灵活高效地在各类预训练模型上实现和应用增量微调方法。

> 这项研究对于大规模预训练语言模型的应用和优化具有重要意义，特别是在降低计算和存储成本方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqYe

1.  Glm-130b: An open bilingual pre-trained model

https://arxiv.org/abs/2210.02414

> 《GLM-130B: An Open Bilingual Pre-trained Model》这篇论文介绍了一个名为GLM-130B的双语（中文和英文）预训练语言模型，该模型拥有1300亿参数。GLM-130B旨在支持在一台A100（40G * 8）或V100（32G * 8）服务器上对千亿规模参数的模型进行推理。

> GLM-130B的主要特点包括：

+   **双语支持**：同时支持中文和英文。

+   **高精度**：在英文的LAMBADA测试上优于GPT-3 175B davinci、OPT-175B和BLOOM-176B，在MMLU测试上也略优于GPT-3 175B。在中文方面，在7个零样本CLUE数据集和5个零样本FewCLUE数据集上明显优于ERNIE TITAN 3.0 260B。

+   **快速推理**：支持在一台A100服务器上使用SAT和FasterTransformer进行快速推理，速度最高可达2.5倍。

+   **可复现性**：所有结果（超过30个任务）均可通过开源代码和模型参数轻松复现。

+   **跨平台**：支持在NVIDIA、Hygon DCU、Ascend 910和Sunway处理器上进行训练与推理。

> 此外，GLM-130B还具有独特的优势，如其独特的缩放属性使其能够实现INT4量化而不需要后训练，几乎没有性能损失。这使得它是第一个能够有效推理在4xRTX 3090（24G * 8）或8xRTX 2080 Ti（11G）GPU上的100B级模型。GLM-130B模型的权重是公开可用的，其代码、训练日志、相关工具包和经验教训也已在GitHub上开源。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqxW

1.  Cogvideo: Large-scale pretraining for text-to-video generation via transformers

https://arxiv.org/abs/2205.15868

> 《Cogvideo: 大规模预训练用于通过变换器生成文本到视频》这篇论文介绍了一个名为CogVideo的9亿参数变换器模型，用于文本到视频的生成。这个模型是通过继承一个预训练的文本到图像模型CogView2来训练的，这显著降低了训练成本，并缓解了数据稀缺和相关性弱的问题。研究团队还提出了一个多帧率分层训练策略，以更好地对齐文本和视频片段。CogVideo在机器评估中实现了最先进的性能，在人类评估中大幅超过了公开可用的模型。此外，CogVideo的代码和模型也是公开可用的。

> CogVideo的主要贡献包括：

+   提出了CogVideo，这是迄今为止最大且是第一个在通用领域开源的预训练文本到视频生成变换器模型。

+   CogVideo有效地将预训练的文本到图像生成模型微调为文本到视频生成，避免了从头开始预训练的高昂成本。

+   提出了多帧率分层训练策略，以更好地对齐文本-片段对，显著提高了生成精度，特别是对于复杂语义动作的生成。

> 这篇论文对于文本到视频生成领域是一个重要的里程碑，展示了预训练变换器模型在这一挑战性任务中的应用潜力。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqx3

1.  MoocRadar: A Fine-grained and Multi-aspect Knowledge Repository for Improving Cognitive Student

Modeling in MOOCs

https://arxiv.org/abs/2304.02205

> 《MoocRadar: A Fine-grained and Multi-aspect Knowledge Repository for Improving Cognitive Student Modeling in MOOCs》这篇论文介绍了一个名为MoocRadar的知识库，旨在改进大规模开放在线课程（MOOCs）中的认知学生建模。这个知识库包含2513个练习题、5600个知识概念，以及超过1270万条行为记录。该论文提出了一种框架，以确保对细粒度概念和认知标签进行高质量和全面的注释。

> MoocRadar的目的是通过学生的课程互动来推断学生的学习特征，这是智能教育中的一个基本问题。尽管知识追踪和认知诊断的最新尝试为改进当前模型的可用性和有效性提出了几个有希望的方向，但现有的公共数据集仍然不足以满足这些潜在解决方案的需求，因为它们忽略了完整的练习背景、细粒度概念和认知标签。

> 该论文的统计和实验结果表明，MoocRadar提供的数据集为现有方法的未来改进提供了基础。此外，为了方便研究人员使用，作者发布了一套工具，用于数据查询、模型适配，甚至是对知识库的扩展，这些工具现在可以在GitHub上找到。

> 这篇论文被SIGIR 2023接受，并发表在ACM数字图书馆和arXiv上。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqxc

1.  Imagereward: Learning and evaluating human preferences for text-to-image generation

https://arxiv.org/abs/2304.05977

> 《ImageReward: Learning and Evaluating Human Preferences for Text-to-Image Generation》这篇论文介绍了一个名为ImageReward的模型，这是第一个通用的文本到图像的人类偏好奖励模型。ImageReward的训练基于系统化的标注流程，包括评分和排名，至今已收集了137,000对专家比较。在人类评估中，ImageReward在理解文本到图像合成中的人类偏好方面超过了现有的评分模型和指标，成为评估文本到图像合成的有希望的自动指标。此外，论文还提出了Reward Feedback Learning (ReFL)，这是一种直接调优算法，用于优化扩散模型相对于评分器。自动和人工评估都支持ReFL相对于比较方法的优点。所有代码和数据集都可在GitHub上找到。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqxb

1.  Cogdl: A comprehensive library for graph deep learning

https://arxiv.org/abs/2103.00959

> 《CogDL: A Comprehensive Library for Graph Deep Learning》这篇论文介绍了一个名为CogDL的图深度学习综合库。CogDL旨在解决图数据稀疏性、图神经网络（GNNs）训练复杂性和图任务非标准化评估等挑战。该库通过提供一个统一的GNN模型训练和评估设计，使其在现有的图学习库中独树一帜。CogDL利用这个统一训练器，可以优化GNN训练循环，使用多种训练技术，如混合精度训练。此外，CogDL还开发了高效的稀疏运算符，使其成为效率最高的图库之一。CogDL还专注于易用性，旨在促进图学习的开放和可重复研究。研究团队利用CogDL报告并维护了基本图任务的基准结果，这些结果可以被社区复现和直接使用。

> CogDL的主要特点包括：

+   易用性：CogDL提供了简单的API，用户只需编写一行代码即可训练和评估任何图表示学习方法。

+   端到端性能：效率是CogDL最显著的特点之一。它不仅提供了高效的训练技术，还通过内置的GNN特定包装器优化了内存和计算效率。

+   统一训练器：CogDL的设计核心是统一的GNN训练器，它标准化了GNN模型的训练和评估过程，使得实验易于进行且结果具有可比性。

> 这篇论文对于图深度学习领域是一个重要的贡献，它提供了一个高效、易用且全面的库，旨在加速图深度学习的研究和应用。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqxA

1.  GraphMAE2: A Decoding-Enhanced Masked Self-Supervised Graph Learner

https://arxiv.org/abs/2304.04779

> 《GraphMAE2: A Decoding-Enhanced Masked Self-Supervised Graph Learner》这篇论文介绍了一种名为GraphMAE2的新型图自监督学习框架。这个框架旨在解决现实世界图数据中标签稀缺的基本挑战。GraphMAE2通过在特征重构中引入随机性和潜在表示预测的策略来规范特征重构，以此来提高图自监督学习中的特征重构性能。

> 具体来说，GraphMAE2的设计包括多视图随机重掩码解码和潜在表示预测。多视图随机重掩码解码是为了在特征空间的重构中引入随机性，而潜在表示预测则是在嵌入空间中强制重构。这种方法自然作为生成图SSL中的目标构建的正则化。

> GraphMAE2在包括ogbn-Papers100M在内的各种公共数据集上取得了顶尖结果，至少比最先进的基线提高了2.45%。此外，该论文还展示了GraphMAE2作为一种自监督方法，在所有数据集上均优于经典的监督式图神经网络。

> 总的来说，GraphMAE2是一个简单而更有效的生成自监督图框架，可以直接与现有的图神经网络架构结合使用。这项工作对于大规模图的自监督学习提供了新的视角和工具。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqxw

1.  ApeGNN: Node-Wise Adaptive Aggregation in GNNs for Recommendation

https://dl.acm.org/doi/10.1145/3543507.3583530

> ApeGNN: Node-Wise Adaptive Aggregation in GNNs for Recommendation 是一篇关于图神经网络（GNNs）在推荐系统中的应用的论文。这篇论文提出了一个名为ApeGNN的框架，旨在解决现有GNN在推荐场景中的局限性。传统GNN在处理用户和项目时采取平等对待的方式，无法区分每个节点的不同局部模式，因此在推荐系统中并非最优选择。ApeGNN通过开发一种节点级自适应扩散机制来改善这一点，使每个节点能够根据其局部结构（例如度）自适应地决定其扩散权重。该方法在六个广泛使用的推荐数据集上进行了实验，结果显示ApeGNN比最先进的GNN-based推荐方法更优，最高提升了48.94%，证明了节点级自适应聚合的有效性。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqxI

1.  BatchSampler: Sampling Mini-Batches for Contrastive Learning in Vision, Language, and Graphs

https://arxiv.org/abs/2306.03355

> 《BatchSampler: Sampling Mini-Batches for Contrastive Learning in Vision, Language, and Graphs》这篇论文介绍了一个名为BatchSampler的采样方法，用于在视觉、语言和图结构中进行对比学习。BatchSampler是一种简单而通用的生成方法，用于采样难以区分的实例（即对彼此来说是硬负和真负的实例）。这项技术可以轻松地集成到现有的视觉、语言和图结构中的对比学习模型中。

> BatchSampler的主要特点包括：

+   **改进对比学习**：通过从输入数据中采样小批量，BatchSampler旨在改进对比学习。

+   **减少假阴性**：为了使每个小批量中包含更少的假阴性，论文设计了随机选择实例的邻近图。

+   **随机游走与重启**：在邻近图上利用随机游走与重启来帮助采样难以区分的实例。

+   **实验结果**：在三个模态的数据集上的广泛实验表明，BatchSampler可以持续提高强大对比模型的性能，例如在ImageNet-100上的SimCLR、在STS（语言）上的SimCSE，以及在图数据集上的GraphCL和MVGRL。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq4V

1.  WebGLM: Towards An Efficient Web-Enhanced Question Answering System with Human Preferences

https://arxiv.org/abs/2306.07906

> 《WebGLM: Towards An Efficient Web-Enhanced Question Answering System with Human Preferences》这篇论文介绍了一个名为WebGLM的系统，旨在通过增强预训练大语言模型（LLM）的网络搜索和检索功能，同时提高实际部署的效率。WebGLM的核心模块包括基于LLM的增强检索器（LLM-augmented retriever）、引导生成器（bootstrapped generator）和人类偏好感知评分器（human preference-aware scorer）。

+   LLM-augmented Retriever: 这个两阶段检索器结合了粗粒度Web搜索和细粒度LLM蒸馏检索。首先，使用Google API进行初步搜索，然后使用轻量级检索器Contriever与LLM结合，通过微调提高检索准确率。

+   Bootstrapped Generator: 利用LLM的in-context learning特性，生成高质量的QA数据集。设计校正和选择策略来过滤高质量样本，产出WebGLM-QA数据集，用于训练GLM生成器。

+   Human Preference-aware Scorer: 利用在线问答论坛的用户点赞信号训练，以了解大多数人对不同答案的偏好，从而提高答案质量。

> WebGLM在多维人工评估中表现出优越性，其10亿参数版本在性能上优于13亿参数的WebGPT，甚至可与175亿参数的WebGPT相媲美。WebGLM的设计和实现体现了对现有技术的改进和优化，对于问答系统和搜索引擎的发展具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqxC

1.  KoLA: Carefully Benchmarking World Knowledge of Large Language Models

https://arxiv.org/abs/2306.09296

> 《KoLA: Carefully Benchmarking World Knowledge of Large Language Models》这篇论文提出了一种新的评估基准，旨在精确评估大型语言模型（LLMs）的世界知识能力。这个基准被称为KoLA，它精心设计了三个关键因素：能力建模、数据和评估标准。

+   **能力建模**：KoLA模仿人类认知，形成了一个涵盖19个任务的四级知识相关能力的分类系统。这个系统包括知识记忆、知识理解、知识应用和知识创造四个级别，旨在提供更具体和有用的评估结果，详细说明评估模型在知识的哪个方面可能存在不足。

+   **数据**：为了确保公平比较，KoLA使用了维基百科和持续收集的新兴语料库。维基百科作为已知数据源，而新兴语料库则用于评估处理未见数据和不断变化知识的能力。这种数据设计使得KoLA能够更公平地评估模型，并更好地跟踪模型的发展。

+   **评估标准**：KoLA采用了一个对比的评估系统，包括总体标准得分和自我对比的知识创造度量。总体标准得分系统使观众能够轻松理解和比较不同任务和模型之间的表现，而自我对比的度量则用于自动评估知识幻觉。

> 此外，KoLA评估了21个广泛使用的LLM，包括商业和开源模型，如GPT-4、Cohere-command、GLM130B和LLaMa。实验结果显示，较大的基础模型倾向于记忆更多的知识，对齐在更高级别的能力上释放了较大模型的潜力，但可能会对记忆产生不利影响，并且与商业模型相比，开源模型在整体上表现较差。

> KoLA数据集和开放参与的排行榜已公开发布，并将持续更新，为开发LLM和知识相关系统提供参考。这项工作对于理解和改进大型语言模型具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq4R

1.  Improved the heterodimer protein complex prediction with protein language models

https://academic.oup.com/bib/article/24/4/bbad221/7199490

> 文章《Improved the heterodimer protein complex prediction with protein language models》提出了一种新的方法，名为ESMPair，用于改进蛋白质复合物的预测。这项研究主要针对的是AlphaFold-Multimer在蛋白质复合物结构预测方面的局限性。AlphaFold-Multimer虽然提高了蛋白质复合物结构的预测能力，但其准确性在很大程度上取决于多序列比对（MSA）的质量，特别是由相互作用同源物（即互作物）形成的MSA。

> ESMPair方法利用蛋白质语言模型来识别复合物的互作物，从而生成比AlphaFold-Multimer默认的MSA生成方法更好的互作物。研究显示，ESMPair在复杂结构预测方面比AlphaFold-Multimer有显著提升，特别是在预测结构置信度较低的情况下。此外，通过结合几种MSA生成方法，可以进一步提高AlphaFold-Multimer的复合结构预测准确性。作者还发现，互作物的MSA多样性对预测准确性有显著影响。ESMPair在真核生物的复合物上表现尤为出色。

> 此外，另一篇相关的论文《Improve the Protein Complex Prediction with Protein Language Models》提出了一种名为ColAttn的方法。ColAttn利用蛋白质语言模型来识别复合物间的相互作用，并结合多序列比对方法来提高结构预测的准确性。ColAttn算法将蛋白质语言模型的输出组合成联合MSAs形式，并利用MSA Transformer中的注意力得分从单链中识别配对同源物。这种方法在异二聚体上展现了最佳的结构预测准确率。作者还将ColAttn与其他MSA配对算法结合，进一步提高了准确率。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq4Q

1.  MTDiag: An Effective Multi-Task Framework for Automatic Diagnosis

https://ojs.aaai.org/index.php/AAAI/article/view/26666

> MTDiag: An Effective Multi-Task Framework for Automatic Diagnosis 是一项关于自动诊断系统的研究。这个系统旨在通过多轮对话与患者交流，以探测症状并进行疾病诊断。以往的研究通常将此问题视为一个顺序决策过程，并使用强化学习（RL）来决定是询问症状还是进行诊断。然而，这些基于RL的方法往往依赖于精心设计的奖励函数，并且通常遭受训练过程不稳定和数据效率低的问题。

> 在这项工作中，研究者提出了一种有效的多任务框架，称为MTDiag，用于自动诊断。他们首先将症状检查重新定义为一种多标签分类任务，通过直接监督来实现。每个医疗对话都被等价地转换成多个用于分类的样本，这也有助于缓解数据稀缺的问题。此外，他们设计了一种多任务学习策略，利用疾病信息来指导症状检查过程，并进一步利用对比学习来更好地区分不同疾病之间的症状。广泛的实验结果表明，他们的方法在四个公共数据集上实现了最先进的性能，疾病诊断的准确率提高了1.7%到3.1%，证明了所提方法的有效性。此外，他们的模型现在已被部署在一个在线医疗咨询系统中，作为真实医生的一个辅助工具。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq4c

1.  Dropconn: Dropout connection based random gnns for molecular property prediction

https://ieeexplore.ieee.org/document/10164235

> 《DropConn: Dropout Connection Based Random GNNs for Molecular Property Prediction》这篇论文介绍了一种新的数据增强和训练框架，用于分子属性预测。这个框架的核心是一种名为DropConn的数据增强策略，它通过软化化学键的硬连接（作为边缘）来生成伪分子图。DropConn中的软权重是根据边缘特征计算的，以便可以合并不同原子之间的自适应相互作用。

> 此外，为了增强模型的泛化能力，论文还提出了一致性正则化训练策略，以充分利用海量无标签数据。DropConn可以作为插件无缝添加到许多现有模型中。在非预训练设置和微调设置下的大量实验表明，DropConn在分子性质预测任务上比最先进的方法可以获得更优越的性能（高达8.22%）。

> 这项研究对于分子数据挖掘领域，特别是在处理标记分子图稀缺的挑战时，具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq4p

1.  xTrimoPGLM: unified 100B-scale pre-trained transformer for deciphering the language of protein

https://arxiv.org/abs/2401.06199

> 《xTrimoPGLM: Unified 100B-Scale Pre-trained Transformer for Deciphering the Language of Protein》这篇论文介绍了一个名为xTrimoPGLM的统一蛋白质语言模型。这个模型通过创新的预训练框架，同时解决蛋白质理解和生成任务，从而发展出一个超越现有基于语言模型的工具的高级3D结构预测模型。xTrimoPGLM结合了1D序列和3D结构，在单一模型中同时建模两种模态，并在后续预测任务以及“逆折叠”（即采用给定结构支架生成新的蛋白质序列）方面显示出改进的性能。

> xTrimoPGLM的主要贡献包括：

+   提出了一个统一的蛋白质语言模型，能够同时处理蛋白质理解和生成任务。

+   通过创新的预训练框架，结合了1D序列和3D结构，提高了后续预测任务和逆折叠的性能。

+   发展了一个超越现有基于语言模型的工具的高级3D结构预测模型。

> 这篇论文对于蛋白质语言模型的研究是一个重要的贡献，它提供了一个新的框架，通过结合不同的数据模态来提高蛋白质结构预测的准确性和效率。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq4A

1.  CodeGeeX: A Pre-Trained Model for Code Generation with Multilingual Benchmarking on HumanEval-X

https://arxiv.org/abs/2303.17568

> 《CodeGeeX: A Pre-Trained Model for Code Generation with Multilingual Benchmarking on HumanEval-X》这篇论文介绍了一个名为CodeGeeX的大型多语言代码生成模型。CodeGeeX是一个具有130亿参数的模型，预训练在超过20种编程语言的大型代码语料库上，截至2022年6月，该模型已经训练了超过8500亿个标记。

> CodeGeeX的主要特点包括：

+   **多语言代码生成**：CodeGeeX在生成多种主流编程语言的可执行程序方面表现良好，包括Python、C++、Java、JavaScript、Go等。

+   **跨语言代码翻译**：CodeGeeX支持在不同语言之间翻译代码片段，能够通过一键操作将程序转换为任何期望的语言，并具有高准确性。

+   **可定制的编程助手**：CodeGeeX在Visual Studio Code扩展市场免费提供，支持代码补全、解释、总结等功能，提升用户编码体验。

+   **开源和跨平台**：所有代码和模型权重均公开用于研究目的。CodeGeeX支持Ascend和NVIDIA平台，并支持在单个Ascend 910、NVIDIA V100或A100上进行推理。

> 此外，为了标准化多语言代码生成和翻译的评估，作者开发并发布了HumanEval-X基准。HumanEval-X是一个包含820个人工制作的数据样本（每个样本都有测试用例）的新多语言基准，涵盖Python、C++、Java、JavaScript和Go等5种编程语言。

> CodeGeeX在HumanEval-X上的实验结果显示，它在代码生成和翻译任务上优于其他类似规模的多语言代码模型。CodeGeeX的性能优势体现在其支持在多个流行的集成开发环境（IDEs）中使用，包括Visual Studio Code、JetBrains和Cloud Studio。

> 该论文最后指出，CodeGeeX自2022年9月起公开可用，其代码、模型权重（850B标记版本）、API、扩展和HumanEval-X均已开源。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq4P

1.  WinGNN: Dynamic Graph Neural Networks with Random Gradient Aggregation Window

https://dl.acm.org/doi/abs/10.1145/3580305.3599551

> WinGNN: Dynamic Graph Neural Networks with Random Gradient Aggregation Window 是一篇关于动态图神经网络的研究论文。该论文提出了一种名为WinGNN的框架，旨在解决传统动态图神经网络在处理动态图时的一些问题，如需要额外的时间编码器、参数数量过多和效率低下等。WinGNN通过结合元学习策略和一种新颖的随机梯度聚合机制来实现。它计算当前快照的帧损失，并将损失梯度传递到下一个快照，以模拟图动态，无需时间编码器。此外，WinGNN引入了随机滑动窗口来获取连续快照的窗口感知梯度，将计算出的两种梯度聚合以更新GNN，从而减少参数大小并提高鲁棒性。在六个公共数据集上的实验表明，WinGNN在二十四项性能指标中的二十二项上优于现有基线。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHB

1.  Agentbench: Evaluating llms as agents

https://arxiv.org/abs/2308.03688

> AgentBench: Evaluating LLMs as Agents 是一项旨在评估大型语言模型（LLMs）作为自主代理能力的研究。这项研究提出了一个多维度的基准测试框架，称为AgentBench，它包含8个不同的环境，用于评估LLMs在复杂环境中的推理和决策能力。这些环境包括5个新创建的领域（操作系统、数据库、知识图谱、数字卡游戏、横向思维谜题）和3个从已发布数据集中重新编译的领域（家庭管理、网络购物、网络浏览）。

> 研究者在27个基于API和开源的LLMs上进行了广泛的测试。结果显示，顶级商业LLMs在复杂环境中表现出色，与开源模型存在显著差距。研究还发现，LLMs在长期推理、决策和指令遵循方面的能力不足是开发可用LLM代理的主要障碍。此外，研究指出，训练代码对不同的代理任务有不同影响，这与现有的假设不同。

> 总的来说，这项研究提供了一个全面的基准测试，用于评估LLMs作为自主代理的能力，并揭示了它们在复杂环境中的一些优势和局限性。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHV

1.  GPT understands, too

https://arxiv.org/abs/2103.10385

> 《GPT Understands, Too》这篇论文提出了一种新的微调方法，称为P-tuning，用于改进自然语言理解（NLU）中预训练语言模型的性能。P-tuning的核心是使用可训练的连续空间内的prompt embeddings，通过这种方式，模型能够更好地适应特定任务。

> P-tuning的主要特点包括：

+   **模板构建**：在离散的template上对部分token替换为可在连续空间微调的pseudo token。

+   **离散和连续模板的混合**：在离散和连续的template token混合时，显式地插入一些anchor（领域有关的离散token）可以有助于template的优化。

+   **知识探索**：通过知识探索任务（如LAMA数据集），P-tuning可以显著提高模型在自然语言理解任务上的性能。

+   **实验结果**：在多个数据集上的实验表明，P-tuning能够与BERT相媲美，甚至在小样本和监督学习上超过BERT。

> 这项研究对于自然语言处理领域，特别是在提高预训练语言模型在NLU任务上的性能方面具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHL

1.  Longbench: A bilingual, multitask benchmark for long context understanding

https://arxiv.org/abs/2308.14508

> 《LongBench: A Bilingual, Multitask Benchmark for Long Context Understanding》这篇论文介绍了一个名为LongBench的双语多任务长文本理解基准测试。LongBench旨在评估大型语言模型（LLMs）在处理长文本输入时的能力，如书籍、报告和代码库。该基准测试包含21个数据集，涵盖6个任务类别，分别是英文和中文，平均长度分别为6,711个单词和13,386个字符。这些任务包括单文档问答、多文档问答、总结、少样本学习、合成任务和代码补全。所有LongBench的数据集都标准化为统一格式，便于LLMs的自动评估。

> LongBench的主要贡献包括：

+   提出了LongBench，这是第一个用于长文本理解的双语多任务基准测试，能够更严格地评估长文本理解能力。

+   LongBench包含21个数据集，涵盖6个任务类别，覆盖关键的长文本应用领域。

+   通过全面评估8个LLMs在LongBench上的表现，发现了一些有意义的趋势和结论，例如商业模型（GPT-3.5-Turbo-16k）在长文本处理上优于开源模型，但仍然存在挑战；缩放位置嵌入和更长的序列微调对长文本理解有显著提升；检索等上下文压缩技术对长文本理解能力较弱的模型有帮助，但性能仍不及具有强大长文本理解能力的模型。

> 这篇论文对于长文本理解领域是一个重要的贡献，它提供了一个全面的基准测试，用于评估和比较不同LLMs在处理长文本输入时的性能和局限性。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqH2

1.  Relay Diffusion: Unifying diffusion process across resolutions for image synthesis

https://arxiv.org/abs/2309.03350

> 《Relay Diffusion: Unifying diffusion process across resolutions for image synthesis》这篇论文介绍了一种新型的图像合成方法，名为Relay Diffusion Model (RDM)。RDM通过模糊扩散过程和块状噪音，将低分辨率的图像或噪声转换为等效的高分辨率图像，用于扩散模型。这种方法使得扩散过程可以在任何新的分辨率或模型中无缝继续，而无需从纯噪声或低分辨率条件重新开始。

> RDM的设计解决了高分辨率生成中的主要挑战。通过离散余弦变换的分析，研究发现相同噪声水平在更高分辨率下会导致频率域中的信噪比（SNR）更高。这意味着自然图像的低频信息没有被很好地破坏。因此，RDM提出了一种像素点间具有相关性的块状噪音，它在高分辨率下对应的SNR在低频部分和高斯噪音在低分辨率下的SNR相当。

> 在实验结果上，RDM在CelebA-HQ上的FID和ImageNet 256×256上的sFID均达到了最先进的水平，超过了之前的工作，如ADM、LDM和DiT。此外，所有的代码和检查点都已开源。

> 总的来说，RDM为图像合成提供了一种新颖且有效的方法，特别是在处理高分辨率图像生成方面展现了其优势。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqH0

1.  Safetybench: Evaluating the safety of large language models with multiple choice questions

https://arxiv.org/abs/2309.07045

> 《SafetyBench: Evaluating the Safety of Large Language Models with Multiple Choice Questions》这篇论文介绍了一个名为SafetyBench的综合基准测试，用于评估大型语言模型（LLMs）的安全性。这个基准测试包含了11,435个多样化的多项选择题，涵盖了7个不同的安全问题类别。值得注意的是，SafetyBench不仅包含了中文和英文数据，还便于在这两种语言下进行评估。

> 该研究在零样本和少样本设置下对25个流行的中文和英文LLMs进行了广泛的测试。结果显示，GPT-4在性能上明显优于其他模型，并且当前LLMs的安全性仍有很大的提升空间。作者们相信，SafetyBench将能够快速和全面地评估LLMs的安全性，并促进更安全的LLMs的发展。数据和评估指南可以在github.com/thu-coai/SafetyBench获取，提交入口和排行榜可以在llmbench.ai/safety查看。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHz

1.  Agenttuning: Enabling generalized agent abilities for llms

https://arxiv.org/abs/2310.12823

> AgentTuning: Enabling Generalized Agent Abilities for LLMs 是一项研究，旨在通过指令调整（instruction tuning）来提升大型语言模型（LLMs）作为自主代理的能力。这种方法使用了一个名为 AgentInstruct 的数据集，该数据集包含了1866个高质量的交互轨迹，覆盖了6个多样化的现实世界场景任务。这些任务包括日常家务到操作数据库等，平均回合数为5到35不等。通过这种调整，LLMs在未见过的代理任务上展现出了强大的泛化能力，同时其通用语言能力也基本保持不变。

> 此外，这项研究还开发了 AgentLM 模型，它是基于 Llama2-chat 开源模型系列，在 AgentInstruct 和 ShareGPT 混合数据集上微调得到的。这些模型遵循 Llama-2-chat 的对话格式，系统提示词固定为 “You are a helpful, respectful and honest assistant.”。AgentLM 模型包括7B、13B和70B三个版本，均已开源。

> 总的来说，这项研究通过指令调整和专门设计的数据集，成功提升了LLMs作为自主代理的通用能力，同时保持了其原有的语言处理能力。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHa

1.  Detecting Social Bot on the Fly using Contrastive Learning

https://dl.acm.org/doi/abs/10.1145/3583780.3615468

> 《Detecting Social Bot on the Fly using Contrastive Learning》这篇论文介绍了一种新的社交机器人检测方法，特别适用于快速检测和应对社交媒体上的恶意机器人。这项技术的核心是一个名为CBD（对比学习驱动的社交机器人检测框架）的两阶段模型学习策略。

> CBD的主要特点包括：

+   **对比预训练阶段**：这个阶段从大量未标记的社交图中挖掘泛化模式，旨在提取有价值的知识。

+   **半监督微调阶段**：在这个阶段，模型对潜藏在社交图中的特定任务知识进行建模，仅需少量标注。

+   **智能反馈机制**：为了进一步提高检测性能，论文还提出了一种智能反馈机制。

+   **实验结果**：在真实机器人检测数据集上的综合实验表明，CBD在少量（5个镜头）标记数据的少镜头机器人检测方面，始终大幅优于10个最先进的基线。

> 这项研究对于社交媒体安全和机器人检测领域具有重要意义，特别是在处理标记数据极度匮乏的情况下，能够有效检测社交机器人。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHx

1.  GOAL: A challenging knowledge-grounded video captioning benchmark for real-time soccer commentary generation

https://arxiv.org/abs/2303.14655

> 《GOAL: A Challenging Knowledge-grounded Video Captioning Benchmark for Real-time Soccer Commentary Generation》这篇论文介绍了一个名为GOAL的基准测试，旨在为实时足球评论生成提出一个具有挑战性的新任务设置，即知识引导的视频字幕（KGVC）。GOAL基于足球比赛视频和同步评论数据，包含超过8.9千个足球视频片段、22千个句子和42千个知识三元组。该论文通过实验测试了现有最先进（SOTA）方法在这个资源上的表现，以展示这个挑战性任务的未来改进方向。研究团队希望他们的数据资源（现可在https://github.com/THU-KEG/goal获得）能服务于对知识引导的跨模态应用感兴趣的研究人员和开发者。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqH9

1.  Cogvlm: Visual expert for pretrained language models

https://arxiv.org/abs/2311.03079

> 《CogVLM: Visual Expert for Pretrained Language Models》这篇论文介绍了一种名为CogVLM的视觉语言基础模型。CogVLM是一个开源的多模态模型，旨在实现视觉和语言特征的深度融合，同时不牺牲任何自然语言处理（NLP）任务的性能。

> CogVLM的核心思想是“视觉优先”，通过使用一个大的视觉编码器和视觉专家模块，实现了深度视觉-语言特征对齐。这个模型包含四个基本组件：ViT编码器、MLP适配器、预训练大语言模型（GPT-style）和视觉专家模块。ViT编码器在CogVLM-17B中采用预训练的EVA2-CLIP-E，而MLP适配器用于将ViT的输出映射到与词嵌入的文本特征相同的空间。CogVLM的模型设计与任何现有的GPT-style的预训练大语言模型兼容，例如Vicuna-7B-v1.5、GLM系列模型和Llama系列模型。

> CogVLM-17B在14个数据集上取得了state-of-the-art或者第二名的成绩，是多模态权威学术榜单上综合成绩第一的模型。它还在10项基准中取得SOTA性能，而在另外四项（包括VQAv2, OKVQA, TextVQA, COCO captioning等）取得第二的成绩，整体性能超越或匹配谷歌的PaLI-X 55B。CogVLM在图像理解、模型幻觉以及文本识别方面都表现出色。

> 此外，CogVLM的代码和检查点已开源，支持在单台3090服务器上运行的微调代码，供研究和使用。这表明CogVLM不仅是一个强大的模型，也是一个促进多模态基础模型领域研究和工业应用的平台。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHd

1.  Characterglm: Customizing chinese conversational ai characters with large language models

https://arxiv.org/abs/2311.16832

> 《CharacterGLM: Customizing Chinese Conversational AI Characters with Large Language Models》这篇论文介绍了一种名为CharacterGLM的模型，它是基于ChatGLM构建的一系列模型，参数量从6B到66B不等。CharacterGLM旨在生成基于角色的对话（CharacterDial），目的是为对话式AI系统配备角色定制功能，以满足人们固有的社交欲望和情感需求。通过CharacterGLM，可以定制各种AI角色或社交代理，配置它们的属性（如身份、兴趣、观点、经历、成就、社交关系等）和行为（如语言特征、情感表达、交互模式等）。该模型在一致性、拟人性和参与度方面优于大多数主流闭源大型语言模型，包括GPT系列。作者们计划发布6B版本的CharacterGLM和部分训练数据，以促进基于角色的对话生成方向的进一步研究开发。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHC

1.  CritiqueLLM: Scaling LLM-as-Critic for Effective and Explainable Evaluation of Large Language Model Generation

https://arxiv.org/abs/2311.18702

> CritiqueLLM: Scaling LLM-as-Critic for Effective and Explainable Evaluation of Large Language Model Generation 是一项关于评估大型语言模型生成文本质量的研究。这项研究提出了一种名为CritiqueLLM的新批评生成模型，该模型包括一种基于对话的高质量参考/无参考评估数据的提示方法。实验结果表明，CritiqueLLM模型在系统级相关性方面可以达到与GPT-4相当的评价性能，甚至在具有挑战性的无参考文本设定下的8个任务中有3个任务超过了GPT-4，显示出在生成批评的质量方面的良好缩放特性。此外，研究还表明，CritiqueLLM生成的批评可以作为可扩展的反馈，直接提高LLMs的生成质量。

> 传统的模型评测方法，如BLEU和ROUGE，基于参考文本和生成文本的n-gram重合度计算评价分数，缺乏对生成文本整体语义的把握。而基于模型的评价方法则严重依赖基座模型的选取，只有像GPT-4这样的顶级大模型才能取得令人满意的评价效果。然而，GPT-4仅能通过API访问，给研究者带来了花费高昂、访问困难、数据泄露等一系列挑战。CritiqueLLM模型旨在解决这些问题，提供一种高质量、低成本的文本质量评价方法。

> CritiqueLLM支持含参考文本和无参考文本两种评价设定，其核心目标是训练出能高效生成评价分数和评价理由的文本质量评价模型。这一过程包括用户询问增广、含参考文本评价数据收集、无参考文本评价数据改写和训练CritiqueLLM等四个主要步骤。最终，得到了能够在含参考文本和无参考文本设定下使用的两种CritiqueLLM模型，可分别适用于两种设定下的文本质量评价任务。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqHP

1.  AlignBench: Benchmarking Chinese Alignment of Large Language Models

https://arxiv.org/abs/2311.18743

> 《AlignBench: Benchmarking Chinese Alignment of Large Language Models》这篇论文介绍了一个名为AlignBench的多维度评测基准，用于评估中文大语言模型（LLM）与人类意图的对齐程度。AlignBench的主要特点包括：

+   **多维度评估**：AlignBench在多个维度上细致评估模型和人类意图的对齐水平，包括基本能力、中文理解、综合问答、写作能力、逻辑推理、数学能力、角色扮演和专业知识。

+   **真实场景构建**：数据主要来自ChatGLM在线服务中真实用户的问题，确保了评测数据的实际性和挑战性。

+   **评分模型**：使用GPT-4或CritiqueLLM作为评分模型，对每个模型的回答进行1-10的综合评分，代表其回答质量。CritiqueLLM是一个能够达到GPT-4 95%评估能力的专用评测模型。

+   **规则校准**：引入了细致的打分规则，提高了模型评分和人类评分的一致性。

+   **自动评估和可复现性**：AlignBench采用评分模型对每个模型的回答进行自动化评分，确保了评测结果的可复现性。

+   **研究意义**：AlignBench填补了中文大模型对齐水平评估领域的空白，为中文大语言模型的实际应用提供了有效的评估工具。

> 这项研究对于中文大语言模型的评估和优化具有重要意义，特别是在提高模型与人类意图对齐程度方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqsj

1.  On Computing Paradigms-Where Will Large Language Models Be Going

https://www.computer.org/csdl/proceedings-article/icdm/2023/078800b577/1Ui3qbryySA

> 《On Computing Paradigms - Where Will Large Language Models Be Going》这篇论文探讨了大型语言模型（LLMs）在计算范式中的未来发展方向。这篇论文是在2023年IEEE国际数据挖掘会议（ICDM 2023）上发表的，由Xindong Wu、Xingquan Zhu、Ruqian Lu等人撰写。论文详细讨论了LLMs的优势和局限性，以及它们在适应性、可解释性、普遍性和特异性等方面的应用。这篇论文对于理解LLMs在当前和未来计算环境中的作用和潜在发展路径具有重要意义。

> 论文的主要内容可能包括：

+   **LLMs的优势和局限性**：分析LLMs在处理自然语言处理任务中的能力，以及它们在适应新环境和处理复杂问题时的限制。

+   **计算范式的发展**：探讨LLMs如何影响和被当前的计算范式所影响，包括云计算、边缘计算和物联网等领域。

+   **LLMs的未来发展**：讨论LLMs在未来的计算环境中的可能发展方向，如增强的交互性、更高级的推理能力和与新兴技术的集成。

+   **LLMs的应用**：探索LLMs在不同领域的应用，如人工智能、大数据分析、自然语言理解和生成等。

> 这篇论文对于LLMs在当前和未来的计算环境中的角色和潜力提供了深入的见解，对于相关领域的学者和研究人员来说是一个重要的参考资源。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqsN

1.  Cogagent: A visual language model for gui agents

https://arxiv.org/abs/2312.08914

> 《CogAgent: A Visual Language Model for GUI Agents》这篇论文介绍了一个名为CogAgent的视觉语言模型，专门用于图形用户界面（GUI）的理解和导航。CogAgent是一个拥有180亿参数的视觉语言模型（VLM），它能够接受高达1120×1120分辨率的图像输入，这使得它能够识别屏幕上的微小元素和文本。作为一个通用的视觉语言模型，CogAgent在五个文本丰富的视觉问答（VQA）基准和四个通用VQA基准上达到了最先进水平，包括VQAv2、OK-VQA、Text-VQA、ST-VQA、ChartQA、infoVQA、DocVQA、MM-Vet和POPE。

> CogAgent使用屏幕截图作为输入，在PC和Android GUI导航任务（如Mind2Web和AITW）上表现出色，超过了仅使用提取的HTML文本的基于语言模型（LLM）的方法。CogAgent模型及其代码已在GitHub上开源，并且提供了网页版的Demo，供人们体验和反馈。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqsi

## 2022

1.  COAD: Contrastive Pre-training with Adversarial Fine-tuning for Zero-shot Expert Linking

https://arxiv.org/abs/2012.11336

> 《COAD: Contrastive Pre-training with Adversarial Fine-tuning for Zero-shot Expert Linking》这篇论文提出了一种名为CODE的方法，用于解决零样本专家链接问题。这个问题的主要挑战在于如何将外部来源的专家信息与AMiner数据库中的专家信息相链接，尤其是在缺乏足够的标签数据的情况下。

> CODE方法首先在AMiner数据库上使用对比学习进行预训练，以捕获专家的表现和匹配模式，而不需要监督信号。然后，该方法在AMiner和外部来源之间进行对抗性微调，以提高模型的迁移性。为了评估，研究团队设计了两个内在任务：作者识别和论文聚类，以验证对比学习赋予的表现和匹配能力。此外，研究团队还展示了CODE的在线部署，并通过主动学习不断改进其在线性能。

> 点击查看智谱清言智能体「论文助手」的回答https://chatglm.cn/share/FWq5o

1.  Region or Global A Principle for Negative Sampling in Graph-based Recommendation

https://ieeexplore.ieee.org/abstract/document/9723516

> 《Region or Global? A Principle for Negative Sampling in Graph-Based Recommendation》这篇论文提出了一种名为RecNS的通用负采样方法，旨在解决图推荐系统中的负采样问题。RecNS方法包含两种采样策略：正向辅助采样和曝光增强采样。这些策略不是从图数据中采样现有的负项目，而是在嵌入空间中合并这两种策略以生成负项目嵌入。这一方法基于一个原则，即在中间区域进行更多的负采样，而在相邻和远距离区域进行较少的负采样。这种方法旨在提高推荐系统的性能和效率。

> 点击查看智谱清言智能体「论文助手」的回答https://chatglm.cn/share/FWq5b

1.  Reinforced moocs concept recommendation in heterogeneous information networks

https://arxiv.org/abs/2203.11011

> 《Reinforced MOOCs Concept Recommendation in Heterogeneous Information Networks》这篇论文提出了一种名为HinCRec-RL的方法，用于在异构信息网络中推荐MOOCs（大规模开放在线课程）的概念。这种方法基于强化学习和异构信息网络，旨在捕捉用户与知识概念之间的动态互动，并更好地学习用户的语义表示。HinCRec-RL方法通过将用户、课程、视频和概念之间的互动构建成一个异构信息网络，并使用基于元路径的注意力图神经网络在HIN中表示用户。该方法在来自中国MOOC平台XuetangX的真实世界数据集上进行了广泛实验，以验证其有效性。实验结果和分析表明，与几种最先进的模型相比，HinCRec-RL表现良好。

> 点击查看智谱清言智能体「论文助手」的回答https://chatglm.cn/share/FWq54

1.  Delta tuning: A comprehensive study of parameter efficient methods for pre-trained language models

https://arxiv.org/abs/2203.06904

> 这篇论文的标题是《Delta Tuning: A Comprehensive Study of Parameter Efficient Methods for Pre-trained Language Models》。其主要研究内容是针对预训练语言模型（PLMs）的参数高效调整方法，即Delta Tuning。这种方法与传统的全参数微调不同，它只调整模型的一小部分参数，而其余参数保持不变，从而大大减少了计算和存储成本。研究表明，一系列具有不同调整参数选择的Delta Tuning方法可以与全参数微调的性能相媲美，这为大规模PLMs的调整提供了一种新的有效途径。论文还提出了一个统一的分类标准，将现有的Delta Tuning方法分为三类：基于添加的方法、基于指定参数的方法和基于重新参数化的方法。此外，论文还讨论了Delta Tuning有效性的理论基础，并提出了从优化和最优控制的角度解释Delta Tuning的框架。最后，通过对100多个NLP任务的实证研究，对不同方法的性能进行了全面比较，并分析了Delta Tuning的组合、扩展和可转移特性。

> 点击查看智谱清言智能体「论文助手」的回答：https://chatglm.cn/share/FWqax

1.  BaGuaLu: targeting brain scale pretrained models with over 37 million cores

https://dl.acm.org/doi/abs/10.1145/3503221.3508417

> 论文《BaGuaLu: Targeting Brain Scale Pretrained Models with over 37 Million Cores》主要研究了如何在超级计算机上训练与大脑规模相当的预训练人工智能模型。随着预训练AI模型规模的急剧增长，为了实现更高的准确度，这些模型的训练需要巨大的计算和内存能力，这加速了人工智能和高性能计算（HPC）的融合。然而，在HPC系统上部署AI应用仍然存在差距，需要基于特定硬件特征的应用和系统共同设计。

> 为了解决这个问题，论文提出了BaGuaLu，这是第一个旨在整个exascale超级计算机上训练大脑规模模型的工作，该超级计算机名为“新一代神威超级计算机”。BaGuaLu通过结合硬件特定的节点内优化和混合并行策略，使得前所未有的超大模型能够实现良好的性能和可扩展性。研究评估表明，BaGuaLu可以在混合精度的条件下，以超过1 EFLOPS的性能训练14.5万亿参数的模型，并且有能力训练174万亿参数的模型，这与人脑中的突触数量相媲美。

> 点击查看智谱清言智能体「论文助手」的回答：https://chatglm.cn/share/FWqJ0

1.  GRAND+: Scalable Graph Random Neural Networks

https://arxiv.org/abs/2203.06389

> 《GRAND+: Scalable Graph Random Neural Networks》这篇论文主要研究了图神经网络（GNNs）在图上的半监督学习问题。该论文提出了一种新型的图随机神经网络模型GRAND+，旨在解决传统GRAND模型在处理大规模图数据时的难题。GRAND模型虽然能提供最先进的性能，但其有效性依赖于计算成本高昂的图数据增强过程，这使得其在处理大规模图时遇到困难。

> 为了应对这一挑战，GRAND+通过开发一种广义前推（GFPush）算法，预先计算一个通用的传播矩阵，并以小批量方式执行图数据增强。GFPush算法的低时间和空间复杂度使得GRAND+能够高效地扩展到大规模图。此外，GRAND+在模型优化中引入了一种置信度感知的一致性损失，促进了模型的泛化优势。

> 研究者在七个不同规模的公共数据集上进行了广泛的实验。结果表明，GRAND+不仅能够扩展到大规模图，并且运行时间比现有的可扩展GNNs要少，同时在所有数据集上都能提供一致的准确度提升，超过了全批量和其他可扩展GNNs。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqJd

1.  SelfKG: Self-Supervised Entity Alignment in Knowledge Graphs

https://arxiv.org/abs/2203.01044

> 《SelfKG: Self-Supervised Entity Alignment in Knowledge Graphs》这篇论文探讨了在知识图谱（KGs）中无需标签监督的实体对齐问题。实体对齐的目标是识别不同KGs中的等价实体，这对于构建网络规模KGs是一个基本问题。传统上，这一过程依赖于标签监督，即使用标签信息（正实体对）来监督将每个正实体对中的对齐实体拉近的过程。然而，论文的理论分析表明，实体对齐的学习实际上可以从推动未标记的负对远离彼此中受益更多，而不是仅仅拉近标记的正对。

> 受此发现启发，研究者开发了用于实体对齐的自我监督学习目标，并提出了有效的策略来优化这一目标，从而在没有标签监督的情况下调整实体。他们在基准数据集上进行了大量实验，结果表明，SelfKG无需监督即可与最先进的监督基线相匹配或实现可比的结果。这表明自我监督学习在KG中的实体对齐方面具有巨大潜力。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqyw

1.  STAM: A Spatiotemporal Aggregation Method for Graph Neural Network-based Recommendation

https://dl.acm.org/doi/10.1145/3485447.3512041

> 《STAM: A Spatiotemporal Aggregation Method for Graph Neural Network-based Recommendation》这篇论文专注于图神经网络（GNN）在推荐系统中的应用，特别是在处理时空信息方面。传统的方法主要集中于如何从空间结构信息的角度聚合信息，但对于邻居的时序信息探索不足。为了解决这一问题，研究者提出了STAM，一种时空聚合方法，有效地将时序信息纳入邻居嵌入学习中。

> STAM从空间结构信息和时序信息的角度生成时空邻居嵌入，从而促进了从空间到时空的聚合方法的发展。它使用缩放点积注意力来捕捉一跳邻居的时序顺序，并采用多头注意力在不同潜在子空间上执行联合注意力。通过在基于GNN的推荐系统中使用STAM来学习用户和物品嵌入，实验结果表明，与基于空间的聚合方法相比，STAM在GNN-based推荐方面带来了显著的改进，例如在MovieLens、Amazon和Taobao上的MRR@20分别提高了24%、8%和13%。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqyd

1.  FewNLU: Benchmarking State-of-the-Art Methods for Few-Shot Natural Language Understanding

https://arxiv.org/abs/2109.12742

> 《FewNLU: Benchmarking State-of-the-Art Methods for Few-Shot Natural Language Understanding》这篇论文集中于对少样本自然语言理解（Few-Shot NLU）任务的评估框架的研究。少样本学习是一种通过极少量（通常少于一百个样本）的示例来学习新任务的方法，受益于预训练语言模型（PLMs）中存储的先前知识。

> 论文提出了一种新的评估框架，该框架在三个方面改进了之前的评估程序：测试性能、开发-测试相关性和稳定性。通过这个新的评估框架，研究者重新评估了多种最新的少样本NLU方法。研究发现，先前文献中对于方法的绝对性能和相对差距的估计并不准确，没有一种方法能在大多数任务上表现出一致的性能，一些方法的改进随着更大预训练模型的使用而减弱，不同方法之间的增益通常是互补的，最佳的组合模型性能接近于强大的全监督基线。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqyG

1.  GLM: General Language Model Pretraining with Autoregressive Blank Infilling

https://arxiv.org/abs/2103.10360

> 《GLM: General Language Model Pretraining with Autoregressive Blank Infilling》这篇论文介绍了一种新的预训练框架，称为GLM（通用语言模型），旨在解决自然语言理解（NLU）、无条件生成和条件生成等不同NLP任务的需求。现有的预训练架构，如自回归模型（如GPT）、自编码模型（如BERT）和编码器-解码器模型（如T5），各有所长，但没有一种能在所有任务上表现出色。

> GLM通过自回归空白填充来克服这一挑战。这种方法随机地从输入文本中空白掉连续的标记跨度，并训练模型依次重构这些跨度。GLM通过添加2D位置编码和允许任意顺序预测跨度来改进空白填充预训练，从而在NLU任务上超越了BERT和T5。此外，通过改变空白的数量和长度，GLM可以针对不同类型的任务进行预训练。

> 在广泛的NLU、有条件和无条件生成任务中，GLM在相同模型大小和数据的情况下，超过了BERT、T5和GPT，并且仅用1.25倍于BERT Large的参数就实现了最佳性能，展示了其适应不同下游任务的泛化能力。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqAQ

1.  P-Tuning: Prompt Tuning Can Be Comparable to Fine-tuning Across Scales and Tasks

https://arxiv.org/abs/2110.07602

> 《P-Tuning: Prompt Tuning Can Be Comparable to Fine-tuning Across Scales and Tasks》这篇论文探讨了提示调整（prompt tuning）方法，该方法仅调整冻结语言模型的连续提示，从而大幅减少了训练时每个任务所需的存储和内存。然而，在自然语言理解（NLU）的背景下，先前的工作显示，提示调整对于常规大小的预训练模型并不表现良好。此外，现有的提示调整方法无法处理困难的序列标记任务，这表明其缺乏普遍性。

> 该论文提出了一个新的实证发现，即经过适当优化的提示调整可以普遍有效地适用于广泛的模型规模和NLU任务。它能够与微调（finetuning）的性能相匹配，同时仅需调整0.1%-3%的参数。作者提出的方法P-Tuning v2是深度提示调整（Deep Prompt Tuning）的一种实现，针对NLU进行了优化和适配。鉴于P-Tuning v2的普遍性和简单性，作者认为它可以作为微调的替代方案，并为未来的研究提供强大的基线。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqAa

1.  Subgraph Retrieval Enhanced Model for Multi-hop Knowledge Base Question Answering

https://arxiv.org/abs/2202.13296

> 《Subgraph Retrieval Enhanced Model for Multi-hop Knowledge Base Question Answering》这篇论文探讨了知识库问答（KBQA）中子图检索的重要性。在KBQA中，检索与问题相关的子图对于推理过程至关重要。论文指出，小的子图可能排除答案，而大的子图则可能引入噪音。现有的子图检索方法要么是启发式的，要么与推理过程交织在一起，这会导致在部分子图上进行推理，增加推理偏差。

> 为了解决这个问题，论文提出了一种可训练的子图检索器（SR），它与后续的推理过程解耦，实现了一个即插即用的框架，以增强任何面向子图的KBQA模型。SR被设计成一个高效的双编码器，可以扩展路径来诱导子图，并可以自动停止扩展。任何面向子图的推理器，如GRAFT-Net或NSM，都可以用来从子图中推导出答案。这种可分离的检索和推理确保了只对最终的整体而不是中间的部分子图进行推理。

> 实验结果显示，SR与现有的面向子图的推理器相结合，比用其他检索方法进行的同一推理器取得了若干收益。在答案覆盖率相同的情况下，SR可以得到更小的子图，推导出更准确的答案。此外，无监督预训练可以提高约20%的Hits@1，而端到端微调可以提高检索器和推理器的性能。这项研究为KBQA领域提供了新的思路和方法，特别是在子图检索和多跳推理方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqAD

1.  Flipda: Effective and robust data augmentation for few-shot learning

https://arxiv.org/abs/2108.06332

> 《FlipDA: Effective and Robust Data Augmentation for Few-Shot Learning》这篇论文提出了一种新的数据增强方法，旨在改善少样本学习（few-shot learning）的性能。传统的方法通常仅适用于简单任务和弱基线，而FlipDA则探索了在困难任务（如少样本自然语言理解）和强基线（如具有超过一亿参数的预训练模型）上的数据增强。

> 在研究中，作者发现大多数先前的数据增强方法最多只能带来微小的收益，有时甚至大大降低性能。为了解决这一挑战，FlipDA方法结合了生成模型和分类器来生成标签翻转的数据。该方法的核心发现是生成标签翻转数据对性能的提升比生成保留原始标签的数据更为关键。实验结果表明，FlipDA在有效性和稳健性之间取得了良好的平衡，显著提高了许多任务的性能，同时不会对其他任务产生负面影响。

> 具体来说，FlipDA首先使用基于预训练T5的单词替换生成数据，然后使用分类器选择标签翻转的数据。这种方法在许多困难任务上显著提高了性能，相比之前的增强基线在平均性能上取得了很大的进步。此外，FlipDA在不同预训练模型和不同任务上表现出鲁棒性，避免了性能的不稳定和失败模式。

> 这项研究对于改进少样本学习任务中的数据增强技术具有重要意义，特别是在使用大规模预训练语言模型时。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqAX

1.  HOSMEL: A Hot-Swappable Modularized Entity Linking Toolkit for Chinese

https://aclanthology.org/2022.acl-demo.21/

> 《HOSMEL: A Hot-Swappable Modularized Entity Linking Toolkit for Chinese》这篇论文介绍了一种新的中文实体链接（Entity Linking, EL）工具包，旨在解决下游任务中的实体链接问题。HOSMEL是首个模块化的实体链接工具包，其设计特点包括低耦合的模块化、增量开发和灵活的使用模式。这种模块化设计允许每个模块单独训练和自由组合，使得在不重新训练整个模型的情况下，可以灵活地添加新特性。

> 具体来说，HOSMEL包括提及过滤、提及检测和实体消歧等模块，每个模块都可以单独进行训练和调整。这种解耦设计不仅提高了模型的可解释性，还使得可以更轻松地优化最佳结果。此外，HOSMEL提供了三种使用模式：直接调用API或访问web应用程序的现成版本、部分版本用于模型召回的前步骤，以及易于更改的版本，支持添加额外的特性或使用自定义数据进行训练。

> 实验结果表明，HOSMEL在轻量级训练方面的时间减少了4-5倍，存储占用率比EntQA降低了78%。在较少训练数据的情况下，HOSMEL的性能比最佳基线EntQA提高了8.49-17.06%的准确率。此外，HOSMEL的热插拔能力也得到了评估，当添加新的特征关系时，它可以快速更新并进一步提高3.71-5.02%的准确率。

> 这项研究对于改进中文实体链接工具，特别是在处理下游任务时，具有重要意义。HOSMEL的发布有望吸引更多关注，以研究非英语语言中的实体链接问题。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqAU

1.  DeepStruct: Pretraining of language models for structure prediction

https://arxiv.org/abs/2205.10475

> 《DeepStruct: Pretraining of Language Models for Structure Prediction》这篇论文介绍了一种新的方法，用于提高语言模型的结构理解能力。与先前通过特定任务增强进行微调的方法不同，这篇论文提出的方法是预先训练语言模型，使其能够从文本中生成结构，而这些文本来源于一系列与特定任务无关的语料库。这种结构预训练使得模型能够实现零样本转移，即无需特定任务训练即可应用模型所学到的关于结构任务的知识。

> 研究者在28个数据集上评估了这种方法，这些数据集涵盖了10个结构预测任务，包括开放信息提取、联合实体与关系提取、命名实体识别、关系分类、语义角色标注、事件提取、共指消解、事实探测、意图检测和对话状态跟踪。结果显示，一个100亿参数的语言模型在大多数任务上都有显著的转移效果，并在21个评估数据集上达到了最先进的性能。此外，研究者还通过特定任务的训练集进一步增强了预训练。

> 这项研究对于改善语言模型在结构预测任务上的性能具有重要意义，特别是在处理需要深入理解文本结构的任务时。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq7t

1.  GACT: Activation Compressed Training for Generic Network Architectures

https://arxiv.org/abs/2206.11357

> 《GACT: Activation Compressed Training for Generic Network Architectures》这篇论文介绍了一种新的框架，用于在训练大型神经网络模型时减少内存资源的需求。这项技术名为激活压缩训练（ACT），它通过压缩训练期间保存的激活来减少内存消耗。论文中提出的GACT框架支持广泛的机器学习任务，适用于各种神经网络架构，无需深入了解特定领域知识。

> GACT的核心贡献包括：

提出了一般收敛理论，适用于各种网络架构，无需对操作类型或模型架构有任何先验知识。

开发了一种算法，能够自动估计每个压缩张量的敏感性，并选择最佳压缩策略。

在PyTorch上实现了GACT的高效实现，具有易于使用的API，并能与其他节省内存的技术无缝结合。

> 实验结果显示，GACT可以将卷积神经网络、变换器和图神经网络的激活内存减少高达8.1倍，使得在相同GPU上训练时可以实现24.7倍更大的批量大小，且精度损失微乎其微。这项研究对于在有限内存资源下训练大型神经网络具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq7y

1.  Rethinking the Setting of Semi-supervised Learning on Graphs

https://arxiv.org/abs/2205.14403

> 《Rethinking the Setting of Semi-supervised Learning on Graphs》这篇论文探讨了图上的半监督学习（SSL）的设置，指出当前的设置可能导致不公平的比较，因为它存在过度调整超参数的风险。论文强调，调整超参数对性能有显著影响，这些调整利用验证集中的标签信息来提高性能。为探索过度调整超参数的极限，研究者提出了ValidUtil方法，该方法通过一组额外的超参数充分利用验证集中的标签信息。例如，使用ValidUtil，即使GCN也能轻松在Cora数据集上达到85.8%的高精度。

> 为了避免过度调整，研究者将训练集和验证集合并，构建了一个独立的图基准（IGB），该基准由4个数据集组成，每个数据集包含100个从大型图中独立采样的i.i.d.图，以减少评估方差。实验表明，IGB比以前的图半监督学习数据集更稳定。论文中还提出，目前SSL的设置可能存在问题，因为验证集的大小通常远大于训练集（例如，Cora数据集中140个训练样本对500个验证样本），这可能导致模型过度适应验证集。

> 这项研究对于改进图神经网络（GNN）在半监督学习中的应用具有重要意义，特别是在处理图数据和优化模型性能方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWq7Z

1.  Parameter-efficient prompt tuning makes generalized and calibrated neural text retrievers

https://arxiv.org/abs/2207.07087

> 《Parameter-Efficient Prompt Tuning Makes Generalized and Calibrated Neural Text Retrievers》这篇论文探讨了在神经文本检索中使用参数高效提示调整（prompt tuning）的方法。这项技术旨在通过更新预训练模型中的少量特定任务参数来提高模型的性能，特别是在文本检索方面。

> 论文的主要贡献包括：

提出了参数高效的提示调整策略，这种方法在领域内、跨领域和跨主题设置中都能有效提高文本检索的性能。

通过广泛的分析，论文展示了这种策略可以缓解基于微调的检索方法所面临的两个问题：参数效率低下和弱泛化能力。

特别值得注意的是，这种提示调整策略可以显著提高检索模型在域外零样本泛化方面的性能。

仅通过更新模型参数的0.1%，提示调整策略就能帮助检索模型实现比传统方法更好的泛化性能。

为了促进对检索器跨主题泛化性的研究，研究者构建并发布了一个包含18K查询结果对和87个主题的学术检索数据集，这是迄今为止最大的主题特定数据集。

> 这项研究对于改进神经文本检索，特别是在处理跨领域和跨主题任务时的泛化能力方面具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqD1

1.  GraphMAE: Self-Supervised Masked Graph Autoencoders

https://arxiv.org/abs/2205.10803

> 《GraphMAE: Self-Supervised Masked Graph Autoencoders》这篇论文介绍了一种新的自监督图自动编码器（GAE）方法，称为GraphMAE。该方法旨在解决现有GAE在图的自监督学习领域中存在的问题，特别是在节点分类和图分类任务上。

> 论文的主要贡献包括：

提出了一种新的GAE模型，GraphMAE，专注于重建被遮蔽的节点特征，而不是重构整个图结构。

GraphMAE引入了带有二次遮蔽策略的GNN作为解码器，以提高模型的表达能力和稳定性。

为了训练的稳定，GraphMAE采用可缩放的余弦误差作为特征重建的标准。

论文通过在21个公共数据集上的实验表明，GraphMAE可以持续优于现有的对比性和生成性基线。

> 这项研究对于改进图的自监督学习，特别是在处理图数据和优化模型性能方面具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqD6

1.  Mask and Reason: Pre-Training Knowledge Graph Transformers for Complex Logical Queries

https://arxiv.org/abs/2208.07638

> 《Mask and Reason: Pre-Training Knowledge Graph Transformers for Complex Logical Queries》这篇论文介绍了一种新的方法，用于处理复杂逻辑查询的知识图谱（KG）推理。该方法的核心是Knowledge Graph Transformer（kgTransformer），它结合了屏蔽预训练和微调策略。

> kgTransformer的设计包括：

> **KG三元组转换方法**：将关系转换为关系节点，将KG转换为没有边属性的有向图。

> Mixture-of-Experts (MoE)策略：利用Transformer前馈层的稀疏激活特性。

> **掩码预测**：将复杂的逻辑查询制定为掩码预测，以提高模型的可迁移性和泛化性。

> **两阶段掩码预训练策略**：第一阶段旨在使用KGs的一般知识初始化kgTransformer，第二阶段进一步完善其在推理过程中进行小而稀疏查询的能力。

> 实验结果表明，kgTransformer在两个基准测试上均优于基于KG嵌入的基线和先进的编码器。此外，kgTransformer能够提供完整的推理路径来解释给定的答案，从而进行可解释性推理。

> 这项研究对于改进知识图谱推理，特别是在处理复杂逻辑查询和提高模型泛化能力方面具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqKL

1.  OAG-BERT: Towards a Unified Backbone Language Model for Academic Knowledge Services

https://arxiv.org/abs/2103.02410

> 《OAG-BERT: Towards a Unified Backbone Language Model for Academic Knowledge Services》这篇论文介绍了一种新的语言模型OAG-BERT，旨在为学术知识服务提供一个统一的骨干模型。OAG-BERT结合了Open Academic Graph（OAG）中的异构实体知识和科学文献语料，通过预训练策略和零样本推理技术进行优化。

> 论文的主要贡献包括：

+   **OAG-BERT模型的提出**：OAG-BERT是一种基于OAG的预训练学术语言模型，整合了大量的异构实体知识，包括论文、作者、研究领域、会议和机构等。

+   **预训练策略和零样本推理技术**：为了处理异构知识，OAG-BERT设计了针对不同类型实体的实体类型嵌入。此外，还引入了一种基于span的实体遮蔽策略，可以按照实体长度选择遮蔽的连续token。为了更好地指示模型实体span和序列顺序，论文还提出了实体感知的2D位置编码。

+   **应用领域**：OAG-BERT被应用于九个学术知识任务，包括作者去歧义、文献检索、实体图补全、论文推荐、用户活动预测、研究领域标签、会议预测、机构预测和自动标题生成。此外，还展示了OAG-BERT的零样本应用，如预测论文的会议、机构和研究领域。

+   **实际应用案例**：OAG-BERT已被部署到多个实际应用中，如中国国家自然科学基金委员会（NSFC）的审稿人推荐功能和AMiner系统的论文标签。

> 这项研究对于推动学术知识服务的应用和发展具有重要意义，特别是在处理复杂的学术数据和优化模型性能方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqKl

1.  XDAI: A Tuning-free Framework for Exploiting Pre-trained Language Models in Knowledge Grounded Dialogue Generation

https://dl.acm.org/doi/abs/10.1145/3534678.3539135

> 《XDAI: A Tuning-free Framework for Exploiting Pre-trained Language Models in Knowledge Grounded Dialogue Generation》这篇论文介绍了一种名为XDAI的新框架，旨在利用预训练语言模型（PLM）生成基于知识的对话系统，而无需任何调优成本。这项技术特别适用于那些希望快速创建开放域对话系统以及轻松定制自己特定领域系统的开发者。

> XDAI框架的主要特点包括：

+   **快速启动**：XDAI提供了一个开放域知识基础对话服务，配备了足够的现成开放域知识资源，使开发者可以轻松部署一个对话系统。

+   **高效推理**：与其它对话系统相比，XDAI利用了一种新颖的提示模式，其中包含知识注入，优化了PLM生成的对话，无需进一步训练或调优。

+   **定制部署**：对于特定领域的开发者，XDAI提供了易于更改的插件，以实现自动搜索和更新仅从几个特定领域种子开始的额外知识。

+   **增量修改**：XDAI还提供了一系列工具包，鼓励开发者细化和定制个性化组件。

> 此外，XDAI框架还支持增量开发，通过一系列工具包，开发者可以逐步改进和定制他们的个性化组件。XDAI的GitHub页面提供了关于如何使用XDAI的详细信息和示例，以及如何开始构建自己的对话系统的指导。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqKI

1.  Toward Graph Minimally-Supervised Learning

https://dl.acm.org/doi/abs/10.1145/3534678.3542602

> 《Toward Graph Minimally-Supervised Learning》这篇论文探讨了图的最小监督学习，特别是在深度图学习和图神经网络领域。论文指出，现有的图学习方法通常依赖于大量的标注数据进行模型训练。然而，由于图的数据标注和标记通常耗时且资源消耗大，因此实际中图数据往往标记不足。在这种情况下，研究最小人工监督的图学习变得尤为重要。

> 论文的主要内容包括：

+   弱监督学习、少样本学习和自监督学习方法：论文专注于图最小监督学习的前沿技术，特别是针对图结构数据的弱监督学习、少样本学习和自监督学习方法。

+   **实际应用**：讨论了这些方法在实际应用中的表现，包括其在处理低资源环境下的应用，其中数据量有限甚至没有标注数据。

+   **研究目标**：论文旨在正式分类图最小监督学习中的问题，讨论不同学习场景下的挑战，全面回顾现有和最近的研究进展，并阐明开放问题和未来研究方向。

> 这项研究对于图学习领域，特别是在处理有限或无标注数据的情况下，具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqKG

1.  GCCAD: Graph Contrastive Learning for Anomaly Detection

https://arxiv.org/abs/2108.07516

> 《GCCAD: Graph Contrastive Learning for Anomaly Detection》这篇论文介绍了一种新的图对比学习方法，用于异常检测。该方法名为GCCAD，它利用图对比编码来区分正常节点和异常节点，并基于这些节点与全局上下文（例如所有节点的平均值）的距离。

> GCCAD的主要特点包括：

+   **监督学习框架**：GCCAD采用监督学习框架，用于对比正常节点和异常节点。

+   **图神经网络编码器**：为了实现对比学习目标，GCCAD设计了一种图神经网络编码器，能够在信息传递过程中推断并移除可疑链接，同时学习输入图的全局上下文。

+   **自监督学习**：针对标签稀缺的场景，GCCAD通过设计一种图破坏策略来生成合成节点标签，从而实现自监督学习。

+   **实验结果**：在四个公共数据集上的广泛实验表明，GCCAD显著且一致地优于各种先进基线，并且其自监督版本（无需微调）可以与完全监督版本达到可比性能。

> 这项研究对于图结构数据异常检测领域具有重要意义，特别是在处理有限或无标注数据的情况下。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqwS

1.  Parameter-Efficient Tuning Makes a Good Classification Head

https://arxiv.org/abs/2210.16771

> 《Parameter-Efficient Tuning Makes a Good Classification Head》这篇论文研究了如何通过参数高效调优来改善自然语言理解（NLU）中的分类头性能。传统的NLU模型，如BERT，通常会在预训练的backbone后接一个随机初始化的分类头，并通过微调整个模型来优化其性能。然而，这种方法在微调过程中可能会导致分类头输入的变化，从而影响其性能。

> 论文的主要发现是，通过参数高效调优可以得到一个良好的分类头，这种分类头可以简单地替换掉随机初始化的头，从而带来稳定的性能提升。实验结果表明，这种联合预训练的分类头在GLUE和SuperGLUE的9个任务上持续提高性能。这项研究对于优化NLU模型，特别是在处理自然语言理解和分类任务方面具有重要意义。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqwa

1.  OAG: Linking Entities across Large-scale Heterogeneous Knowledge Graphs

https://ieeexplore.ieee.org/document/9950622

> 《OAG: Linking Entities Across Large-Scale Heterogeneous Knowledge Graphs》这篇论文介绍了一个名为LinKG的统一框架，用于有效地链接不同知识图谱中的实体。这个框架特别适用于处理大规模和异构的实体图，这对于构建开放和全面的知识图谱至关重要。

> LinKG框架的主要特点包括：

+   **多模块设计**：LinKG包含三个链接模块，每个模块针对不同类型的实体。例如，对于基于词序列的实体（如会议），LinKG提出了一种基于LSTM的方法来捕获词之间的依赖关系。

+   **大规模实体链接**：对于大规模实体（如论文），LinKG利用局部敏感哈希和卷积神经网络进行可扩展和精确的链接。

+   **处理模糊实体**：对于模糊实体（如作者），LinKG提出了异构图注意力网络来建模不同类型的实体。

+   **实验结果**：LinKG在实验中展现了显著的性能，其整体F1分数达到95.15%，并且已被部署和发布为Open Academic Graph（OAG），这是迄今为止公开可用的最大异构学术图谱。

> 这项研究对于知识图谱构建和实体链接领域具有重要意义，特别是在处理大规模和异构数据集时。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqwe

1.  CogView2: Faster and Better Text-to-Image Generation via Hierarchical Transformers

https://arxiv.org/abs/2204.14217

> 《CogView2: Faster and Better Text-to-Image Generation via Hierarchical Transformers》这篇论文介绍了一种新的基于层次化变换器（hierarchical transformers）的文本到图像生成方法，旨在提高生成速度和图像质量。论文的主要贡献包括：

+   **层次化变换器**：CogView2使用了一个包含6B-9B-9B参数的层次化变换器，这使得模型在处理高分辨率图像时能够更快速和高效。

+   **自监督预训练**：CogView2使用了一种简单灵活的自监督任务，称为跨模态通用语言模型（CogLM），用于预训练其变换器。

+   **快速超分辨率**：CogView2经过微调，能够快速实现超分辨率，从而生成高质量的图像。

+   **竞争性生成性能**：CogView2在生成性能上与当前最先进的DALL-E-2相竞争，并自然支持交互式文本引导的图像编辑。

> 这项研究对于文本到图像生成领域具有重要意义，特别是在提高生成速度和图像质量方面。

> 点击查看智谱清言的回答：https://chatglm.cn/share/FWqhF