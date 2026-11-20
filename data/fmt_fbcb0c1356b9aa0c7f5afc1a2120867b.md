
Reinforcement learning from human feedback (RLHF) has emerged as the primary method for aligning large language models (LLMs) with human preferences. The RLHF process typically starts by training a reward model (RM) using human preference data. Conventional RMs are trained on pairwise responses to the same user request, with relative ratings indicating which response humans prefer. The trained RM serves as a proxy for human preferences. However, due to the black-box nature of RMs, their outputs lack interpretability, as humans cannot intuitively understand why an RM thinks a response is good or not. As RMs act as human preference proxies, we believe they should be human-interpretable to ensure that their internal decision processes are consistent with human preferences and to prevent reward hacking in LLM alignment. To build RMs with interpretable preferences, we propose a two-stage approach: i) train an Absolute-Rating Multi-Objective Reward Model (ArmoRM) with multi-dimensional absolute-rating data, each dimension corresponding to a human-interpretable objective (e.g., honesty, verbosity, safety); ii) employ a Mixture-of-Experts (MoE) strategy with a gating network that automatically selects the most suitable reward objectives based on the context. We efficiently trained an ArmoRM with Llama-3 8B and a gating network consisting of a shallow MLP on top of the ArmoRM. Our trained model, ArmoRM-Llama3-8B, obtains state-of-the-art performance on RewardBench, a benchmark evaluating RMs for language modeling. Notably, the performance of our model surpasses the LLM-as-a-judge method with GPT-4 judges by a margin, and approaches the performance of the much larger Nemotron-4 340B reward model.

## R-Judge: Benchmarking Safety Risk Awareness for LLM Agents

https://aclanthology.org/2024.findings-emnlp.79.pdf

![](img/7bfc5d7f2c2087cfbafcbdeab77f58b4.png)

大型语言模型（LLMs）在现实应用任务自主完成方面有很大潜力，但在交互环境中会带来意外的安全风险。本文提出R - Judge基准来评估LLMs根据代理交互记录判断和识别安全风险的能力，它包含569条多轮代理交互记录、多种应用类别中的27个关键风险场景等。对11个LLMs的评估显示其风险意识提升空间大，表现最好的GPT - 4o准确率为74.42%，其他模型表现不佳。风险意识是多维能力，对LLMs有挑战，进一步实验发现针对安全判断的微调可显著提升模型性能，而简单提示机制不行，R - Judge已公开。

Large language models (LLMs) have exhibited great potential in autonomously completing tasks across real-world applications. Despite this, these LLM agents introduce unexpected safety risks when operating in interactive environments. Instead of centering on the harmlessness of LLM-generated content in most prior studies, this work addresses the imperative need for benchmarking the behavioral safety of LLM agents within diverse environments. We introduce R-Judge, a benchmark crafted to evaluate the proficiency of LLMs in judging and identifying safety risks given agent interaction records. R-Judge comprises 569 records of multi-turn agent interaction, encompassing 27 key risk scenarios among 5 application categories and 10 risk types. It is of high-quality curation with annotated safety labels and risk descriptions. Evaluation of 11 LLMs on R-Judge shows considerable room for enhancing the risk awareness of LLMs: The best-performing model, GPT-4o, achieves 74.42% while no other models significantly exceed the random. Moreover, we reveal that risk awareness in open agent scenarios is a multi-dimensional capability involving knowledge and reasoning, thus challenging for LLMs. With further experiments, we find that fine-tuning on safety judgment significantly improve model performance while straightforward prompting mechanisms fail. R-Judge is publicly available at this https URL.

## EditEval: An Instruction-Based Benchmark for Text Improvements

https://aclanthology.org/2024.conll-1.7.pdf

![](img/469445d3bd6b7829761b8d113e1873c9.png)

《EditEval: An Instruction - Based Benchmark for Text Improvements》指出，目前文本生成的评估主要关注顺序创建的内容而非文本改进，但写作是一个迭代和渐进的过程，对模型执行编辑技能的能力全面评估还很少。本文提出EditEval这一基于指令的基准和评估套件，可自动评估编辑能力。评估多个预训练模型后发现InstructGPT和PEER表现最佳，多数基准低于有监督的SOTA，常用编辑任务指标并非都高度相关，针对最高性能提示的优化不一定意味着对不同模型有最强的鲁棒性。希望通过发布此基准和公开排行榜挑战推动相关研究发展。

Evaluation of text generation to date has primarily focused on content created sequentially, rather than improvements on a piece of text. Writing, however, is naturally an iterative and incremental process that requires expertise in different modular skills such as fixing outdated information or making the style more consistent. Even so, comprehensive evaluation of a model's capacity to perform these skills and the ability to edit remains sparse. This work presents EditEval: An instruction-based, benchmark and evaluation suite that leverages high-quality existing and new datasets for automatic evaluation of editing capabilities such as making text more cohesive and paraphrasing. We evaluate several pre-trained models, which shows that InstructGPT and PEER perform the best, but that most baselines fall below the supervised SOTA, particularly when neutralizing and updating information. Our analysis also shows that commonly used metrics for editing tasks do not always correlate well, and that optimization for prompts with the highest performance does not necessarily entail the strongest robustness to different models. Through the release of this benchmark and a publicly available leaderboard challenge, we hope to unlock future research in developing models capable of iterative and more controllable editing.

## MAgIC: Investigation of Large Language Model Powered Multi-Agent in Cognition, Adaptability, Rationality and Collaboration

https://aclanthology.org/2024.emnlp-main.416.pdf

![](img/f7f6a7040d8448c5b95f286355131511.png)

《MAgIC：大型语言模型驱动的多智能体在认知、适应性、理性和协作方面的研究》介绍了一种新的基于竞赛的基准框架，以评估多智能体环境中的大型语言模型（LLMs），用两个社会推理游戏和三个博弈论场景创建不同环境，借助概率图建模（PGM）方法增强LLMs在复杂社会和认知维度的能力。对七个LLMs进行评估，结果表明GPT o1和Llama - 2 - 70B之间能力差距超三倍，PGM增强使所选模型能力平均提升37%，相关数据和代码可在https://github.com/cathyxl/MAgIC找到。

Large Language Models (LLMs) have significantly advanced natural language processing, demonstrating exceptional reasoning, tool usage, and memory capabilities. As their applications expand into multi-agent environments, there arises a need for a comprehensive evaluation framework that captures LLMs’ reasoning, planning, collaboration, and other social abilities. This work introduces a novel competition-based benchmark framework specifically designed to assess LLMs within multi-agent settings, providing quantitative metrics to evaluate their judgment, reasoning, deception, self-awareness, cooperation, coordination, and rationality.We utilize two social deduction games alongside three game-theory scenarios to create diverse environments.Our frame is fortified with the probabilistic graphic modeling (PGM) method, enhancing the LLMs’ capabilities in navigating complex social and cognitive dimensions. We evaluate seven LLMs, quantitatively highlighting a significant capability gap of over threefold between the strongest, GPT o1, and the weakest, Llama-2-70B. It also confirms that our PGM enhancement boosts the abilities of all selected models by an average of 37%. Our data and code can be found here https://github.com/cathyxl/MAgIC.

## Leave No Document Behind: Benchmarking Long-Context LLMs with Extended Multi-Doc QA

https://aclanthology.org/2024.emnlp-main.322.pdf

![](img/c538b9051219a58ffd56fd1b7f92b5c7.png)

长语境建模能力受到广泛关注，超长语境窗口的大型语言模型（LLMs）出现，长语境LLMs的基准测试也在逐步发展。但现有基准测试用人为添加不相关噪声文本来延长测试用例长度，偏离现实场景。为此提出新的长语境基准测试Loong，通过扩展多文档问答（QA）与现实场景结合，其测试用例中每个文档都与最终答案相关。Loong还引入四种不同语境长度的任务以更全面评估长语境理解能力。大量实验表明现有长语境语言模型仍有很大提升空间，检索增强生成（RAG）表现不佳，证明Loong能够可靠评估模型的长语境建模能力。
