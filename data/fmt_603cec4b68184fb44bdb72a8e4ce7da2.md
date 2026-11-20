
# AI+法律

> 来源：[https://aiyanxishe.feishu.cn/docx/AGDQdvqh5oY50xxjOg1c7fqJn2c](https://aiyanxishe.feishu.cn/docx/AGDQdvqh5oY50xxjOg1c7fqJn2c)
> <https://aiyanxishe.feishu.cn/sync/H4bBd6rOtsCpG9bjI0ZcDHxinnc>

自从以 ChatGPT 为代表的大语言模型（Large Language Model，LLM）出现以后，由于其惊人的类通用人工智能（AGI）能力，掀起了新一轮自然语言处理领域研究和应用的浪潮。以 ChatGLM、LLaMA 等平民玩家也能跑起的较小规模 LLM 开源模型为基础，涌现了大量基于 LLM 的二次开发与应用案例。本项目旨在收集和整理中文 LLM 相关的开源模型、应用、数据集及教程等资料，目前收录的资源已达 100+ 个！

![](img/b71014045457dd99b5b8ffe7fcc27887.png)

同时也欢迎大家贡献本项目未收录的开源模型、应用、数据集等；提供新的 AI+法律信息可联系[核心会员群](https://aiyanxishe.feishu.cn/wiki/WB2awt7lli6actkm7rtc3ejInmd?from=from_copylink)。

## 獬豸（LawGPT_zh）：中文法律对话语言模型

- 地址：[https://github.com/LiuHC0428/LAW-GPT](https://github.com/LiuHC0428/LAW-GPT)
- 简介：本项目开源的中文法律通用模型由 `ChatGLM-6B` `LoRA` 16 位指令微调获取。数据集包括现有的法律问答数据集和基于法条和真实案例指导的自我指导构建的高质量法律文本问答，提高了通用语言大模型在法律领域的表现，提高了模型回答的可靠性和专业程度。

## LaWGPT：基于中文法律知识的大语言模型

- 地址：[https://github.com/pengxiao-song/LaWGPT](https://github.com/pengxiao-song/LaWGPT)
- 简介：该系列模型在通用中文基础模型（如 `Chinese-LLaMA`、`ChatGLM` 等）的基础上增强了法律领域词表、大规模中文法律语料预训练，增强了大模型在法律领域的基础语义理解。在此基础上，构建了法律领域对话问答数据集、中国司法考试数据集进行指令精调，提升了模型对法律内容的理解和执行能力。

## LexiLaw：中文法律大模型

- 地址：[https://github.com/CSHaitao/LexiLaw](https://github.com/CSHaitao/LexiLaw)
- 简介：LexiLaw 是一个基于 `ChatGLM-6B` 微调的中文法律大模型，通过在法律领域的数据集上进行微调。该模型旨在为法律从业者、学生和普通用户提供准确、可靠的法律咨询服务，包括具体法律问题的咨询，还是对法律条款、案例解析、法规解读等方面的查询。

## LLaMA 律师：中文法律 LLaMA

- 地址：[https://github.com/AndrewZhe/lawyer-llama](https://github.com/AndrewZhe/lawyer-llama)
- 简介：开源了一系列法律领域的指令参数数据和基于 `LLaMA` 训练的中文法律大模型的参数。Lawyer LLaMA 首先在大规模法律语料上进行了持续的预训练。在此基础上，借助 `ChatGPT` 收集了一些对中国国家统一法律职业资格考试真题（以下简称法考）的分析和对法律咨询的解答，利用收集到的数据对模型进行指令调校，让模型习得将法律知识应用到具体场景中的能力。

## 韩非（HanFei）

- 地址：[https://github.com/siat-nlp/HanFei](https://github.com/siat-nlp/HanFei)
- 简介：`HanFei-1.0`（韩非）是国内首个全参数的法律大模型，参数量 7B，主要功能包括：法律问答、多轮对话、撰写文章、检索等。

## ChatLaw-法律大模型

- 地址：[https://github.com/PKU-YuanGroup/ChatLaw](https://github.com/PKU-YuanGroup/ChatLaw)
- 简介：由北大开源的一系列法律领域大模型，包括 `ChatLaw-13B`（基于姜子牙 `Ziya-LLaMA-13B-v1` 训练而来）、`ChatLaw-33B`（基于 `Anima-33B` 训练而来，逻辑推理能力大幅提升）、`ChatLaw-Text2Vec`。使用 93 万条评分案例的数据集，基于 `BERT` 训练了一个相似度匹配模型，可将用户提问信息和法条相匹配。

## lychee_law-律知

- 地址：[https://github.com/davidpig/lychee_law](https://github.com/davidpig/lychee_law)
- 简介：该项目由德国萨尔大学团队和中国南京大学团队合作开发，开源了一系列中文司法领域大模型，如 `Law-GLM-10B`：基于 `GLM-10B` 模型，在 30GB 中文法律数据上进行指令微调得到。

## 智海-录问（wisdomInterrogatory）

- 地址：[https://github.com/zhihaiLLM/wisdomInterrogatory](https://github.com/zhihaiLLM/wisdomInterrogatory)
- 简介：该项目由浙江大学、阿里巴巴达摩院以及华院计算三家单位共同设计研发的法律大模型，基于 `baichuan-7b` 进行了法律领域数据的二次预训练与指令微调，并设计了知识增强的推理流程。

## 夫子·明察司法大模型

- 地址：[https://github.com/irlab-sdu/fuzi.mingcha](https://github.com/irlab-sdu/fuzi.mingcha)
- 简介：该项目由山东大学、浪潮云、中国政法大学联合研发，以 `ChatGLM` 为大模型基础，基于海量中文无监督司法语料（包括法考评分标准、法律法规等）与有监督司法裁判数据（包括法律问答、类案检索）进行中文司法大模型的训练。该模型支持法条检索、案例分析、三段论推理评分以及司法对话等功能。

## DISC-LawLLM

- 地址：[https://github.com/FudanDISC/DISC-LawLLM](https://github.com/FudanDISC/DISC-LawLLM)
- 简介：该项目由复旦大学数据智能与社会计算实验室（Fudan-DISC）开发并开源的法律领域大模型，包括数据集，基于 `Baichuan-13B-Base` 进行微调的模型，且增加了检索增强模块。

## JittorLLMs

- 地址：[https://github.com/Jittor/JittorLLMs](https://github.com/Jittor/JittorLLMs)
- 简介：计图大模型推理库：笔记本没有显卡也能跑大模型，具有成本低、支持广、可移植、速度快等优势。

![](img/3d28af95b4d50cd332a239146fe6256a.png)

## 常见开源模型细节概述

### 1. 模型

#### 1.1 文本大语言模型

##### ChatGLM

- 地址：[https://github.com/THUDM/ChatGLM-6B](https://github.com/THUDM/ChatGLM-6B)
- 简介：中文领域效果最好的开源模型之一，针对中文问答和对话进行了优化。经过约 1T 标识符的中英双语训练，辅以监督微调、反馈自助、人类反馈强化学习等技术的加持。

##### ChatGLM2-6B

- 地址：[https://github.com/THUDM/ChatGLM2-6B](https://github.com/THUDM/ChatGLM2-6B)
- 简介：`ChatGLM2-6B` 是基于开源中英双语对话模型 `ChatGLM-6B` 的第二代版本，在保留初代模型对话流畅、部署门槛低等优秀特性的基础之上，引入了 `GLM` 的混合目标函数，经过了 1.4T 中英标识符的预训练与人类偏好训练；基础模型的上下文长度扩展至 32K，并在对话阶段使用 8K 的上下文长度训练；基于多查询注意力技术实现更高效的推理速度和更低的显存占用；允许商业使用。

##### ChatGLM3-6B

- 地址：[https://github.com/THUDM/ChatGLM3](https://github.com/THUDM/ChatGLM3)
- 简介：`ChatGLM3-6B` 是 `ChatGLM3` 系列中的开源模型，在保留前两代模型对话流畅、部署门槛低等优秀特性的基础上，引入如下特性：更强大的基础模型：`ChatGLM3-6B` 的基础模型 `ChatGLM3-6B-Base` 采用了更多样的数据训练、更充分的训练步数和更合理的训练策略；更完整的功能支持：`ChatGLM3-6B` 采用全新设计的提示格式，除正常的多轮对话外，同时支持工具调用（函数调用）、代码执行（代码解释器）和 Agent 任务等复杂场景；更全面的开源序列：除对话模型 `ChatGLM3-6B` 外，还开源了基础模型 `ChatGLM3-6B-Base`、长文本对话模型 `ChatGLM3-6B-32K`。以上模型对学术研究完全开放，在填写表格登记后亦允许免费商业使用。

##### Chinese-LLaMA-Alpaca

- 地址：[https://github.com/ymcui/Chinese-LLaMA-Alpaca](https://github.com/ymcui/Chinese-LLaMA-Alpaca)
- 简介：中文 `LLaMA` & `Alpaca` 大语言模型 + 本地 CPU/GPU 部署，在原版 `LLaMA` 的基础上增加了中文词表并使用中文数据进行了二次预训练。

##### Chinese-LLaMA-Alpaca-2

- 地址：[https://github.com/ymcui/Chinese-LLaMA-Alpaca-2](https://github.com/ymcui/Chinese-LLaMA-Alpaca-2)
- 简介：该项目将发布中文 `LLaMA-2` & `Alpaca-2` 大语言模型，基于可商用的 `LLaMA-2` 进行二次开发。

##### Chinese-LlaMA2

- 地址：[https://github.com/michael-wzhu/Chinese-LlaMA2](https://github.com/michael-wzhu/Chinese-LlaMA2)
- 简介：该项目基于可商用的 `LLaMA-2` 进行二次开发部署，开展 Llama 2 的中文汉化工作，包括 `Chinese-LlaMA2`：对 Llama 2 进行中文预训练；第一步先在 42G 中文语料上进行训练，后续将加大训练规模；`Chinese-LlaMA2-chat`：对 `Chinese-LlaMA2` 进行指令微调和多轮对话微调，以适应各种应用场景和多轮对话交互。同时，也考虑更轻量的中文改造方案：`Chinese-LlaMA2-sft-v0`：采用现有的开源中文指令数据集或对话数据，对 `LlaMA-2` 进行直接微调（将于近期开源）。

##### Llama2-Chinese

- 地址：[https://github.com/FlagAlpha/Llama2-Chinese](https://github.com/FlagAlpha/Llama2-Chinese)
- 简介：该项目专注于 `Llama2` 模型在中文方面的优化和上层建设，基于大规模中文数据，从预训练开始对 `Llama2` 模型进行中文能力的持续迭代升级。

##### Qwen

- 地址：[https://github.com/QwenLM/Qwen](https://github.com/QwenLM/Qwen)
- 简介：通义千问是阿里云研发的通义千问大模型系列模型，提供参数规模为 18 亿（1.8B）、70 亿（7B）、140 亿（14B）和 720 亿（72B）的模型，包括基础模型 `Qwen`，即 `Qwen-1.8B`、`Qwen-7B`、`Qwen-14B`、`Qwen-72B`，以及对话模型 `Qwen-Chat`，即 `Qwen-1.8B-Chat`、`Qwen-7B-Chat`、`Qwen-14B-Chat` 和 `Qwen-72B-Chat`。数据集包括文本和代码等多种数据类型，覆盖通用领域和专业领域，能支持 8K 的上下文长度，针对插件调用相关的数据做了特定优化，当前模型能有效调用插件以及升级为 Agent。

##### OpenChineseLLaMA

- 地址：[https://github.com/OpenLMLab/OpenChineseLLaMA](https://github.com/OpenLMLab/OpenChineseLLaMA)
- 简介：基于 `LLaMA-7B` 经过中文数据集增量预训练产生的中文大语言模型底座。对比原版 `LLaMA`，该模型在中文理解能力和生成能力方面均获得了较大提升，在下游任务中均取得了突出成绩。

##### BELLE

- 地址：[https://github.com/LianjiaTech/BELLE](https://github.com/LianjiaTech/BELLE)
- 简介：开源基于 `BLOOMZ` 和 `LLaMA` 微调后的一系列模型，同时包括数据、相关模型、训练代码、应用场景等，同时探索不同的数据、训练算法等对模型表现的影响。

##### PandaLLM

- 地址：[https://github.com/dandelionsllm/pandallm](https://github.com/dandelionsllm/pandallm)
- 简介：开源基于 `LLaMA-7B`、`-13B`、`-33B`、`-65B` 进行中文领域持续预训练的语言模型，使用了接近 15M 条数据进行二次预训练。

##### Robin

- 地址：[https://github.com/OptimalScale/LMFlow](https://github.com/OptimalScale/LMFlow)
- 简介：`Robin` 是香港科技大学 `LMFlow` 团队开发的中英双语大语言模型。仅使用 180K 条数据训练出的 `Robin` 第二代模型，在 Hugging Face 排行榜上达到了第一名。`LMFlow` 支持快速训练个性化模型，单张 RTX 3090 和 5 个小时即可训练出 70 亿参数定制化模型。

##### 封神榜-LM

- 地址：[https://github.com/IDEA-CCNL/封神榜-LM](https://github.com/IDEA-CCNL/封神榜-LM)
- 简介：封神榜大模型是 IDEA 研究院认知计算与自然语言研究中心主导的大模型开源体系。该项目开源了姜子牙通用大模型 V1，是基于 `LLaMA` 的 130 亿参数的大规模预训练文本模型，具备翻译、编程、分类、信息抽取、摘要、文案生成、常识问答和数学计算等能力。除姜子牙系列模型之外，该项目还开源了太乙、二郎神系列等模型。

##### BiLLa

- 地址：[https://github.com/Neutralzz/BiLLa](https://github.com/Neutralzz/BiLLa)
- 简介：该项目产出了推理能力增强的中英文双语 `LLaMA` 模型。模型的主要特性有：扩大 `LLaMA` 的中文理解能力，并尽量减少对原始 `LLaMA` 中文能力的损伤；训练过程增加热点能力的任务型数据，利用 `ChatGPT` 生成解析，强化模型理解任务流程；全量参数更新，追求更好的生成效果。

##### MOSS

- 地址：[https://github.com/OpenLMLab/MOSS](https://github.com/OpenLMLab/MOSS)
- 简介：支持中英双语和多种插件的开源对话语言模型。`MOSS` 基础语言模型在约七千亿中英文以及代码单词上预训练得到，后续经过对话微调、插件增强学习和人类偏好训练，具备多轮对话能力及使用多种插件的能力。

##### Luotuo-Chinese-LLM

- 地址：[https://github.com/LC1332/Luotuo-Chinese-LLM](https://github.com/LC1332/Luotuo-Chinese-LLM)
- 简介：包含了一系列中文大语言模型开源项目，包含了一系列基于已有开源模型（`ChatGLM`、`MOSS`、`LLaMA`）进行二次微调的模型、指令微调数据集等。

##### Linly

- 地址：[https://github.com/CVI-SZU/Linly](https://github.com/CVI-SZU/Linly)
- 简介：提供中文对话模型 `Linly-ChatFlow`、中文基础模型 `Linly-Chinese-LLaMA` 及其训练数据。中文基础模型以 `LLaMA` 为基础，利用中文和中英平行语料增量预训练。项目汇总了目前公开的多语言指令数据，对中文模型进行了大规模指令微调训练，实现了 `Linly-ChatFlow` 对话模型。

##### Firefly

- 地址：[https://github.com/yangjianxin1/Firefly](https://github.com/yangjianxin1/Firefly)
- 简介：`Firefly`（流萤）是一个开源的中文大语言模型项目，开源了包括数据、训练代码、多个基于 `Bloom`、`baichuan` 等微调好的模型等；支持全量参数微调、`QLoRA` 以及高效参数微调、`LoRA` 指令微调；支持主流的开源大模型，如百川、`Ziya`、`Bloom`、`LLaMA` 等。支持 `LoRA` 与基础模型进行权重合并，推理更便捷。

##### ChatYuan

- 地址：[https://github.com/clue-ai/ChatYuan](https://github.com/clue-ai/ChatYuan)
- 简介：元语智能发布的一系列支持中文的功能型对话语言大模型，在指令数据、人类反馈强化学习、思维链等方面进行了优化。

##### ChatRWKV

- 地址：[https://github.com/BlinkDL/ChatRWKV](https://github.com/BlinkDL/ChatRWKV)
- 简介：开源了一系列基于 `RWKV` 架构的聊天模型（包括中文和英文），发布了包括 `Raven`、`Novel-ChnEng`、`Novel-Ch` 与 `Novel-ChnEng-ChnPro` 等模型，可以直接闲聊及进行诗歌、小说等创作，包括 7B 和 14B 等规模的模型。

##### CPM-Bee

- 地址：[https://github.com/OpenBMB/CPM-Bee](https://github.com/OpenBMB/CPM-Bee)
- 简介：一个完全开源、允许免费商用、百亿参数的中英文基座模型。它采用 Transformer 自回归架构（auto-regressive），在超万亿级语料上进行预训练，拥有强大的基础能力。开发者和研究者可以在 `CPM-Bee` 基础模型的基础上，在各类场景中进行微调，以创建特定领域的应用模型。

##### TigerBot

- 地址：[https://github.com/TigerResearch/TigerBot](https://github.com/TigerResearch/TigerBot)
