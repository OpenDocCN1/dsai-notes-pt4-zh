
## 总结

本文探讨了向 LLMs 注入知识的方法。背景指出 LLMs 知识存在局限，引出知识注入问题。实验比较了微调（FT）和检索增强生成（RAG）两种方法，通过 MMLU 和时事任务等实验发现，RAG 在多数情况下表现更优，FT 虽有提升但不稳定且在学习新知识方面存在问题，数据增强实验表明重复知识对模型学习新知识有重要意义，未来还需进一步研究知识注入相关问题。

Large language models (LLMs) encapsulate a vast amount of factual information within their pre-trained weights, as evidenced by their ability to answer diverse questions across different domains. However, this knowledge is inherently limited, relying heavily on the characteristics of the training data. Consequently, using external datasets to incorporate new information or refine the capabilities of LLMs on previously seen information poses a significant challenge. In this study, we compare two common approaches: unsupervised fine-tuning and retrieval-augmented generation (RAG). We evaluate both approaches on a variety of knowledge-intensive tasks across different topics. Our findings reveal that while unsupervised fine-tuning offers some improvement, RAG consistently outperforms it, both for existing knowledge encountered during training and entirely new knowledge. Moreover, we find that LLMs struggle to learn new factual information through unsupervised fine-tuning, and that exposing them to numerous variations of the same fact during training could alleviate this problem.

## GottBERT: a pure German Language Model

`https://aclanthology.org/2024.emnlp-main.1183.pdf`

![](img/5cf759c281b996eac5e6e433d93e1c01.png)

《GottBERT：一个纯德语语言模型》：预训练语言模型推动了自然语言处理（NLP）领域发展，`BERT`及其优化版`RoBERTa`影响重大。该领域研究始于英语数据，后有使用多语文本语料库训练的模型，但当前研究表明多语模型不如单语模型，且目前还没有德语单语的`RoBERTa`模型。

Pre-trained language models have significantly advanced natural language processing (NLP), especially with the introduction of `BERT` and its optimized version, `RoBERTa`. While initial research focused on English, single-language models can be advantageous compared to multilingual ones in terms of pre-training effort, overall resource efficiency or downstream task performance. Despite the growing popularity of prompt-based LLMs, more compute-efficient `BERT`-like models remain highly relevant. In this work, we present the first German single-language `RoBERTa` model, `GottBERT`, pretrained exclusively on the German portion of the `OSCAR` dataset. Additionally, we investigated the impact of filtering the `OSCAR` corpus. `GottBERT` was pre-trained using `fairseq` and standard hyperparameters. We evaluated its performance on two Named Entity Recognition (`NER`) tasks (`Conll 2003` and `GermEval 2014`) and three text classification tasks (`GermEval 2018` fine and coarse, and `10kGNAD`) against existing German `BERT` models and two multilingual models. Performance was measured using the F1 score and accuracy. The `GottBERT` base and large models showed competitive performance, with `GottBERT` leading among the base models in 4 of 6 tasks. Contrary to our expectation, the applied filtering did not significantly affect the results. To support the German NLP research community, we are releasing the `GottBERT` models under the MIT license.

## ORPO: Monolithic Preference Optimization without Reference Model

`https://aclanthology.org/2024.emnlp-main.626.pdf`

![](img/5ee4a1825b212e0f3c1286a20db89c8b.png)

《ORPO：无参考模型的整体偏好优化》：近期语言模型的偏好对齐算法虽成果可期，但监督微调（`SFT`）对成功收敛仍不可或缺。本文在偏好对齐的背景下重新审视`SFT`，强调对不被偏好的风格给予小惩罚足以实现偏好对齐。在此基础上，引入一种简单的无参考模型的整体优势比偏好优化算法`ORPO`。

While recent preference alignment algorithms for language models have demonstrated promising results, supervised fine-tuning (`SFT`) remains imperative for achieving successful convergence. In this paper, we revisit `SFT` in the context of preference alignment, emphasizing that a minor penalty for the disfavored style is sufficient for preference alignment. Building on this foundation, we introduce a straightforward reference model-free monolithic odds ratio preference optimization algorithm, `ORPO`, eliminating the need for an additional preference alignment phase. We demonstrate, both empirically and theoretically, that the odds ratio is a sensible choice for contrasting favored and disfavored styles during `SFT` across diverse sizes from 125M to 7B. Specifically, finetuning `Phi-2` (2.7B), `Llama-2` (7B), and `Mistral` (7B) with `ORPO` on the `UltraFeedback` alone surpasses the performance of state-of-the-art language models including `Llama-2 Chat` and `Zephyr` with more than 7B and 13B parameters: achieving up to 12.20% on `AlpacaEval2.0` (Figure 1), and 7.32 in `MT-Bench` (Table 2). We release code1 and model checkpoints2 for `Mistral-ORPO-α` and `Mistral-ORPO-β`.

## Chain-of-Note: Enhancing Robustness in Retrieval-Augmented Language Models

`https://aclanthology.org/2024.emnlp-main.813.pdf`

![](img/c0ef45f492bc89c8d2f13aee6d2da3bc.png)

本文提出了 `Chain-of-Note`（`CON`）框架，旨在增强检索增强语言模型（`RALM`）的鲁棒性。针对当前 `RALM` 框架中信息检索不可靠、易忽略内在知识及大语言模型易产生幻觉等问题，`CON` 通过为检索文档生成顺序阅读笔记来评估相关性，进而合成最终答案。实验表明，`GPT-4` 结合 `CON` 优于 `Chain-of-Thought` 方法，基于 `CON` 训练的 `RALM` 在四个开放域 QA 基准测试中表现出色，显著优于标准微调 `RALM`。

Retrieval-augmented language model (`RALM`) represents a significant advancement in mitigating factual hallucination by leveraging external knowledge sources. However, the reliability of the retrieved information is not always guaranteed, and the retrieval of irrelevant data can mislead the response generation. Moreover, standard `RALM`s frequently neglect their intrinsic knowledge due to the interference from retrieved information. In instances where the retrieved information is irrelevant, `RALM`s should ideally utilize their intrinsic knowledge or, in the absence of both intrinsic and retrieved knowledge, opt to respond with "unknown" to avoid hallucination. In this paper, we introduces `CHAIN-OF-NOTE` (`CON`), a novel approach to improve robustness of `RALM`s in facing noisy, irrelevant documents and in handling unknown scenarios. The core idea of `CON` is to generate sequential reading notes for each retrieved document, enabling a thorough evaluation of their relevance to the given question and integrating this information to formulate the final answer. Our experimental results show that `GPT-4`, when equipped with `CON`, outperforms the `CHAIN-OF-THOUGHT` approach. Besides, we utilized `GPT-4` to create 10K `CON` data, subsequently trained on `LLaMa-2` 7B model. Our experiments across four open-domain QA benchmarks show that fine-tuned `RALM`s equipped with `CON` significantly outperform standard fine-tuned `RALM`s.

## AnyMAL: An Efficient and Scalable Any-Modality Augmented Language Model

`https://aclanthology.org/2024.emnlp-industry.98.pdf`

![](img/ef4566a3b32caf76806cceb98297d7c4.png)

论文提出 `Any-Modality Augmented Language Model`（`AnyMAL`），这是一个能处理多种输入模态信号（如文本、图像、视频、音频、惯性测量单元运动传感器）并生成文本响应的统一模型。`AnyMAL` 继承了包括 `Llama-3`（70B）在内的最先进大型语言模型强大的基于文本的推理能力，通过预训练的对齐器模块将特定模态信号转换到联合文本空间，还提供了为有效扩展而实施的优化细节。

We present `Any-Modality Augmented Language Model` (`AnyMAL`), a unified model that reasons over diverse input modality signals (i.e. text, image, video, audio, `IMU` motion sensor), and generates textual responses. `AnyMAL` inherits the powerful text-based reasoning abilities of the state-of-the-art `LLM`s including `Llama-3` (70B), and converts modality-specific signals to the joint textual space through a pretrained aligner module. In this paper, we provide details on the optimizations implemented to efficiently scale the training pipeline, and present a comprehensive recipe for model and training configurations. We conduct comprehensive empirical analysis comprising both human and automatic evaluations, and demonstrate state-of-the-art performance on various multimodal tasks compared to industry-leading models – albeit with a relatively small number of trainable parameters

## Prometheus 2: An Open Source Language Model Specialized in Evaluating Other Language Models

`https://aclanthology.org/2024.emnlp-main.248.pdf`

![](img/425d8dde94b42c06ba944bd34e18c4bd.png)

专有语言模型（如 `GPT-4`）常被用于评估其他语言模型的响应质量，但透明度、可控性和成本方面的担忧促使开发专门用于评估的开源语言模型。不过，现有的开源评估语言模型存在严重缺陷：一是给出的分数与人类评定的分数差异很大，二是缺乏进行直接评估和成对排序（两种最常见形式）的灵活性。
