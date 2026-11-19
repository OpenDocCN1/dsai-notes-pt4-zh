
# 多模态 Top15 被引盘点

> 来源：`https://v11enp9ok1h.feishu.cn/docx/DGmydzfbSoxnFpxvPhzcBrOpnag`

本文精选了 [EMNLP 2024 论文集](https://aclanthology.org/events/emnlp-2024/) 中与多模态相关的、被引量最高的15篇论文。后续我们还会继续陆续发布不同领域的 EMNLP 2024 高引盘点，在机智流👸号后台对话框回复“盘点”，加入顶会论文盘点交流群。

注：引用数据来自谷歌学术，数据统计截止 2024 年 11 月 13 日

![](img/6b5e3ebbb37206f0b53cdeca613236c7.png)

1. `Video-LLaVA`: Learning United Visual Representation by Alignment Before Projection（258次被引）
1. `AnyMAL`: An Efficient and Scalable Any-Modality Augmented Language Model（62次被引）
1. `mPLUG-DocOwl 1.5`: Unified Structure Learning for OCR-free Document Understanding（48次被引）
1. Multimodal Procedural Planning via Dual Text-Image Prompting（32次被引）
1. `MLLM-Protector`: Ensuring `MLLM`’s Safety without Hurting Performance（31次被引）
1. `LOOK-M`: Look-Once Optimization in KV Cache for Efficient Multimodal Long-Context Inference（10次被引）
1. `RAGAR`, Your Falsehood Radar: RAG-Augmented Reasoning for Political Fact-Checking using Multimodal Large Language Models（10次被引）
1. `ImageInWords`: Unlocking Hyper-Detailed Image Descriptions（7次被引）
1. `MIND`: Multimodal Shopping Intention Distillation from Large Vision-language Models for E-commerce Purchase Understanding（7次被引）
1. Benchmarking Vision Language Models for Cultural Understanding（6次被引）
1. `AutoHallusion`: Automatic Generation of Hallucination Benchmarks for Vision-Language Models（6次被引）
1. `TV-TREES`: Multimodal Entailment Trees for Neuro-Symbolic Video Reasoning（5次被引）
1. `MMNeuron`: Discovering Neuron-Level Domain-Specific Interpretation in Multimodal Large Language Model（4次被引）
1. `TraveLER`: A Modular Multi-LMM Agent Framework for Video Question-Answering（4次被引）
1. `TroL`: Traversal of Layers for Large Language and Vision Models（4次被引）

## `Video-LLaVA`: Learning United Visual Representation by Alignment Before Projection

`https://aclanthology.org/2024.emnlp-main.342.pdf`

![](img/4e5d21f42d5bb118ce69eae91b7162a3.png)

### 总结

《`Video-LLaVA`: Learning United Visual Representation by Alignment Before Projection》指出大型视觉 - 语言模型（`LVLM`）提升了视觉 - 语言理解中各项下游任务的性能。现有多数方法将图像和视频编码到不同特征空间再输入大型语言模型，但由于图像和视频缺乏统一标记化（投影前错位），大型语言模型（`LLM`）难以从几个不佳的投影层中学习多模态交互。

The Large Vision-Language Model (`LVLM`) has enhanced the performance of various downstream tasks in visual-language understanding. Most existing approaches encode images and videos into separate feature spaces, which are then fed as inputs to large language models. However, due to the lack of unified tokenization for images and videos, namely misalignment before projection, it becomes challenging for a Large Language Model (`LLM`) to learn multi-modal interactions from several poor projection layers. In this work, we unify visual representation into the language feature space to advance the foundational `LLM` towards a unified `LVLM`. As a result, we establish a simple but robust `LVLM` baseline, `Video-LLaVA`, which learns from a mixed dataset of images and videos, mutually enhancing each other. `Video-LLaVA` achieves superior performances on a broad range of 9 image benchmarks across 5 image question-answering datasets and 4 image benchmark toolkits. Additionally, our `Video-LLaVA` also outperforms `Video-ChatGPT` by 5.8%, 9.9%, 18.6%, and 10.1% on MSRVTT, MSVD, TGIF, and ActivityNet, respectively. Notably, extensive experiments demonstrate that `Video-LLaVA` mutually benefits images and videos within a unified visual representation, outperforming models designed specifically for images or videos. We aim for this work to provide modest insights into the multi-modal inputs for the `LLM`.

## `AnyMAL`: An Efficient and Scalable Any-Modality Augmented Language Model

`https://aclanthology.org/2024.emnlp-industry.98.pdf`

![](img/8810353bf5fddfa3d144fd767bf15c75.png)

### 总结

本文介绍了`Any-Modality Augmented Language Model`（`AnyMAL`），这是一个能处理多种输入模态信号（如文本、图像、视频、音频、IMU运动传感器）并生成文本响应的统一模型。`AnyMAL`继承了包括`Llama-3`（70B）在内的最先进的大型语言模型（`LLMs`）强大的基于文本的推理能力，并通过预训练的对齐器模块将特定模态的信号转换到联合文本空间。文中详细介绍了为有效扩展训练流程而实施的优化措施，还给出了模型和训练配置的全面方案。作者进行了包括人工和自动评估在内的综合实证分析，证明与行业领先的模型相比，`AnyMAL`在各种多模态任务上达到了最先进的性能，尽管其可训练参数数量相对较少。

We present Any-Modality Augmented Language Model (`AnyMAL`), a unified model that reasons over diverse input modality signals (i.e. text, image, video, audio, IMU motion sensor), and generates textual responses. `AnyMAL` inherits the powerful text-based reasoning abilities of the state-of-the-art `LLMs` including `Llama-3` (70B), and converts modality-specific signals to the joint textual space through a pre-trained aligner module. In this paper, we provide details on the optimizations implemented to efficiently scale the training pipeline, and present a comprehensive recipe for model and training configurations. We conduct comprehensive empirical analysis comprising both human and automatic evaluations, and demonstrate state-of-the-art performance on various multimodal tasks compared to industry-leading models – albeit with a relatively small number of trainable parameters.

## `mPLUG-DocOwl 1.5`: Unified Structure Learning for OCR-free Document Understanding

`https://aclanthology.org/2024.findings-emnlp.175.pdf`

![](img/d2bca217c25a6b481c367eca3b3e3152.png)

### 总结

结构信息对理解文档、表格和图表等富含文本的图像语义至关重要，现有视觉文档理解的多模态大语言模型（`MLLMs`）具备文本识别能力，但缺乏对富含文本文档图像的通用结构理解能力。本文强调结构信息在视觉文档理解中的重要性，提出统一结构学习（`Unified Structure Learning`）以提升`MLLMs`的性能。统一结构学习涵盖5个领域（文档、网页、表格、图表和自然图像）的结构感知解析任务和多粒度文本定位任务。为更好编码结构信息，设计了简单有效的视觉到文本模块`H-Reducer`，它能通过卷积合并水平相邻块来保持布局信息并减少视觉特征长度，让`LLM`更高效理解高分辨率图像。还构建了结构感知文本序列和多粒度文本与边界框对的综合训练集`DocStruct4M`以支持结构学习，构建了小型高质量推理调优数据集`DocReason25K`以激发文档领域的详细解释能力。模型`DocOwl 1.5`在10个视觉文档理解基准测试中取得了最先进的性能，在5/10基准测试中将7B `LLM`的`MLLMs`的SOTA性能提高了10多个点，代码、模型和数据集可通过特定网址获取。

Structure information is critical for understanding the semantics of text-rich images, such as documents, tables, and charts. Existing Multimodal Large Language Models (`MLLMs`) for Visual Document Understanding are equipped with text recognition ability but lack general structure understanding abilities for text-rich document images. In this work, we emphasize the importance of structure information in Visual Document Understanding and propose the Unified Structure Learning to boost the performance of `MLLMs`. Our Unified Structure Learning comprises structure-aware parsing tasks and multi-grained text localization tasks across 5 domains: document, webpage, table, chart, and natural image. To better encode structure information, we design a simple and effective vision-to-text module `H-Reducer`, which can not only maintain the layout information but also reduce the length of visual features by merging horizontal adjacent patches through convolution, enabling the `LLM` to understand high-resolution images more efficiently. Furthermore, by constructing structure-aware text sequences and multi-grained pairs of texts and bounding boxes for publicly available text-rich images, we build a comprehensive training set `DocStruct4M` to support structure learning. Finally, we construct a small but high-quality reasoning tuning dataset `DocReason25K` to trigger the detailed explanation ability in the document domain. Our model `DocOwl 1.5` achieves state-of-the-art performance on 10 visual document understanding benchmarks, improving the SOTA performance of `MLLMs` with a 7B `LLM` by more than 10 points in 5/10 benchmarks. Our codes, models, and datasets are publicly available at this https URL.

## `Multimodal Procedural Planning via Dual Text-Image Prompting`

`https://aclanthology.org/2024.findings-emnlp.641.pdf`

### 总结

具身智能体在遵循人类指令完成任务方面表现突出，但利用图文信息辅助人类完成任务的潜力尚未充分发掘。为此提出多模态程序规划（`MPP`）任务，模型根据给定的高级目标生成图文步骤计划，这比单模态计划更具互补性和信息性。`MPP`面临确保跨模态计划的信息性、时间连贯性和准确性等挑战。解决方法是提出文本 - 图像提示（`TIP`）这一双模态提示法，利用大语言模型（`LLM`）的零样本推理能力和扩散模型的文本 - 图像生成能力，通过文到图和图到文的桥梁改善双模态交互。为解决相关数据集缺乏的问题，收集了`WIKIPLAN`和`RECIPEPLAN`作为`MPP`测试集，实验结果在信息性、时间连贯性和计划准确性方面，对比单模态和多模态基准有较好的人类偏好和自动评分，同时给出代码和数据的网址。
