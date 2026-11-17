
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



# 1.1 单模态LLM模型

### TigerBot

+   简介：一个多语言多任务的大规模训练模型(LLM)，开源语言包括模型：`TigerBot-7B`, `TigerBot-7B-base`，`TigerBot-180B`，基本和推理代码，100G预训练数据，内容金融、法律、百科的领域数据以及API等。

### 书生·浦语

+   地址：[https://github.com/InternLM/InternLM-techreport](https://github.com/InternLM/InternLM-techreport)
+   简介：商汤科技、上海人工智能实验室联合香港中文大学、复旦大学和上海交通大学发布千亿级参数大语言模型“书生·浦语”（`InternLM`）。据悉，“书生·浦语”具有1040亿参数，基于“包含1.6万亿代币的多语言种高质量数据集”而成。

### 书生·浦语2

+   地址：[https://github.com/InternLM/InternLM](https://github.com/InternLM/InternLM)
+   简介：商汤科技、上海人工智能实验室联合香港中文大学、复旦大学和上海交通大学发布千亿级参数大语言模型“书生·浦语”（`InternLM2`）。`InternLM2`在数理、代码、对话、创作等各方面各方面能力都取得了长足进步，综合性能达到开源模型的领先水平。`InternLM2`包含两个模型规格：`7B`和`20B`。`7B`为轻量级的研究和应用提供了一个简便但性能不俗的模型，`20B`模型的综合性能更加强大，可以有效支持更加复杂的实用场景。

### 天鹰座

+   地址：[https://github.com/FlagAI-Open/FlagAI/tree/master/examples/Aquila](https://github.com/FlagAI-Open/FlagAI/tree/master/examples/Aquila)
+   简介：由智源研究院发布，`Aquila`语言大模型在技术上继承了`GPT-3`、`LLaMA`等的架构设计优点，替换了一个更高效的底层算子实现、重新设计实现了中英文的分词器，升级了`BMTrain`工具训练方法，是在中英文高质量语料基础上从0开始训练的，通过数据质量的控制、多种训练的优化方法，实现在更小的数据集、更短的训练时间，获得比其他开源模型更优的性能。也是第一个支持中英双语知识、支持许可协议、符合国内数据合规需要的大规模开源语言模型。

### 天鹰座2

+   地址：[https://github.com/FlagAI-Open/Aquila2](https://github.com/FlagAI-Open/Aquila2)
+   简介：由智源研究院发布，`Aquila2`系列，包括基础语言模型`Aquila2-7B`，`Aquila2-34B`和`Aquila2-70B-Expr`，对话模型`AquilaChat2-7B`，`AquilaChat2-34B`和`AquilaChat2-70B-Expr`，长文本对话模型`AquilaChat2-7B-16k` 和 `AquilaChat2-34B-16`。

### 百川7B

+   地址：[https://github.com/baichuan-inc/baichuan-7B](https://github.com/baichuan-inc/baichuan-7B)
+   简介：`Baichuan-13B` 是由百川智能继`Baichuan-7B`后续开发的包含130亿参数的开源可容纳的大规模语言模型，在权威的中文和英文基准上均取得同尺寸最好的效果。该项目发布包含预训练 (`Baichuan-13B-Base`) 和彩色 (`Baichuan-13B-Chat`) 两个版本。

### 百川13B

+   地址：[https://github.com/baichuan-inc/Baichuan-13B](https://github.com/baichuan-inc/Baichuan-13B)
+   简介：由百川智能开发的一个开源可运行的大规模预训练语言模型。基于`Transformer`结构，在大约1.2万亿代币上训练的70亿参数模型，支持中英双语，上下文窗口长度为4096。的中文和英文权威基准（`C-EVAL`/`MMLU`）上均取得同尺寸最好的效果。

### 百川2

+   地址：[https://github.com/baichuan-inc/Baichuan2](https://github.com/baichuan-inc/Baichuan2)
+   简介：由百川智能推出的新一代开源大语言训练模型，采用2.6万亿代币的高质量语言素材，在多个权威的中文、中文和多语言的通用、领域基准上取得同尺寸的最佳效果，发布包含有`7B`、`13B`的`Base`和经过`PPO`训练的`Chat`版本，并提供了`Chat`版本的4bits量化。

### 阿尼玛

+   地址：https://github.com/lyogavin/Anima
+   简介：由艾写科技开发的一个基于`QLoRA`的33B中文大语言模型的开源训练，该模型基于`QLoRA`的`Guanaco 33B`模型使用`Chinese-Vicuna`项目开放的数据集`guanaco_belle_merge_v1.0`进行finetune训练了10000个步骤，基于`Elo`评级锦标赛评估效果更好。

### 知道LM

+   地址：https://github.com/zjunlp/KnowLM
+   简介：`KnowLM`项目旨在发布开源大模型框架及相应模型权重以弥补知识谬误问题，包括大模型的知识难更新及存在潜在的错误和偏见等。该项目一期发布了基于`Llama`的抽取大模型智析，利用中文资料对`LLaMA`（`13B`）进行进一步的全量预训练，并基于知识图谱转换指令技术对知识抽取任务进行优化。

### 贝灵

+   地址：https://github.com/ictnlp/BayLing
+   简介：一个具有增强的跨语言对齐的通用大模型，由中国科学院计算技术研究所自然语言处理团队开发。百临（`BayLing`）以`LLaMA`为基础模型，探索了以交互式翻译任务为核心进行指令扭矩的方法，旨在同时完成语言间校准以及与人类意图校准，将`LLaMA`的生成能力和指令后续能力从英语迁移到其他语言（中文）。在多语言翻译、交互翻译、通用任务、标准化考试的评测中，百生在中文/英语中均表现出了更好的表现。百生提供了在线的内测版演示，供大家体验。

### 玉兰聊天室

+   地址：[https://github.com/RUC-GSAI/YuLan-Chat](https://github.com/RUC-GSAI/YuLan-Chat)
+   简介：`YuLan-Chat`是中国人民大学GSAI研究人员开发的基于聊天的大语言模型。它是在`LLaMA`的基础上配置开发的，具有高质量的中文和中文指令。`YuLan-Chat`与用户聊天，很可以很好地遵循英文或中文指令，并且可以在确定后部署在GPU（`A800-80G`或`RTX3090`）上。

### 聚LM

+   地址：[https://github.com/DAMO-NLP-MT/PolyLM](https://github.com/DAMO-NLP-MT/PolyLM)
+   简介：一个在6400亿个词的数据上从头训练的多语言语言模型，包括两个模型大小(`1.7B`和`13B`)。`PolyLM`覆盖中、英、俄、西、法、葡、德、意、荷、波、阿、土、希伯来、日、韩、泰、越南、对话等语种，尤其是对亚洲语种更友好。

### 活子

+   地址：https://github.com/HIT-SCIR/huozi
+   简介：由哈工语言处理研究所多位老师和学生参与开发的一个开源可开发的大规模预训练语言模型。该模型基于`Bloom`结构的70亿参数模型，支持中英双语，上下文窗口长度为2048 ，同时还开源了基于`RLHF`训练的模型以及全人工标注的16.9K中文偏好数据集。

### 雅仪

+   地址：[https://github.com/wenge-research/YaYi](https://github.com/wenge-research/YaYi)
+   简介：雅意大模型在百万级人工构建的高质量领域数据上进行指令获取，数据训练覆盖媒体宣传、舆情分析、公共安全、金融风控、城市治理等五个大领域，上百种自然语言指令任务。雅意大模型从预初始化训练权重到领域模型的迭代过程中，我们逐步增强了它的中文基础能力和领域分析能力，并增加了多轮对话和部分插件能力。同时，经过数百名用户内测过程中持续不断的人工反馈优化，进一步提升了模型性能和安全性。已开源基于`LLaMA 2`的中文优化模型版本，探索适用于中文多领域任务的最新实践。

### XVERSE-7B

+   地址：[https://github.com/xverse-ai/XVERSE-7B](https://github.com/xverse-ai/XVERSE-7B)
+   简介：由深圳元象科技自主研发的支持多语言的大语言模型，支持 8K 的上下文长度（Context Length），使用 2.6 万亿 token 的高质量、多样化的数据对模型进行充分训练，支持中、英、俄、西等40多种语言。并包含`GGUF`、`GPTQ`仿真版本的模型，支持在`llama.cpp`、`vLLM`在MacOS/Linux/Windows系统上的推理。

### XVERSE-13B

+   地址：[https://github.com/xverse-ai/XVERSE-13B](https://github.com/xverse-ai/XVERSE-13B)
+   简介：由深圳元象科技自主研发的支持多语言的大语言模型，支持8K的上下文长度（Context Length），使用3.2万亿代币的高质量、多样化的数据对模型进行充分的训练，支持中、英、俄、西等40多种语言。包含长序列对话模型 `XVERSE-13B-256K` ，该版本模型最大支持 256K 的上下文窗口长度，约 25w 字的输入内容，可以协助进行文献汇总、报告分析等任务。并包含`GGUF`、`GPTQ`量化版本的模型，支持在`llama.cpp`、`vLLM`在MacOS/Linux/Windows系统上的推理。

### XVERSE-65B

+   地址：[https://github.com/xverse-ai/XVERSE-65B](https://github.com/xverse-ai/XVERSE-65B)
+   简介：由深圳元象科技自主研发的支持多语言的大语言模型，支持16K的上下文长度（Context Length），使用2.6万亿代币的高质量、多样化的数据对模型进行充分的训练，支持中、英、俄、西等40多种语言。包含增量预训练到3.2万亿代币的`XVERSE-65B-2`模型。并包含`GGUF`、`GPTQ`量化版本的模型，支持在`llama.cpp`、`vLLM`在MacOS/Linux /Windows系统上的推理。

### XVERSE-MoE-A4.2B

+   地址：[https://github.com/xverse-ai/XVERSE-MoE-A4.2B](https://github.com/xverse-ai/XVERSE-MoE-A4.2B)
+   简介：由深圳元象科技自主研发的支持多语言的大语言模型（Large Language Model），采用混合专家模型（`MoE`，Mixture-of Experts）架构，模型的总参数规模为258亿，实际激活的参数量为 42 亿，支持 8K 的上下文长度（Context Length），使用 3.2 万亿 token 的高质量、多样化的数据对模型进行充分训练，支持中、英、俄、西等 40 多种语言。

### 思凯沃

+   地址：[https://github.com/SkyworkAI/Skywork](https://github.com/SkyworkAI/Skywork)
+   简介：该项目有了天工系列模型，开源该系列模型在3.2TB高质量多语言和代码数据上进行预训练，开源了包括模型参数，训练数据，评估，评估方法。具体包括`Skywork-13B-Base`模型、`Skywork-13B-Chat`模型、`Skywork-13B-Math`模型和`Skywork-13B-MM`模型，以及每个模型的量化版模型，以支持用户在消费级显卡进行部署和推理。

### 义

+   地址：https://github.com/01-ai/Yi
+   简介：该项目开源了`Yi-6B`和`Yi-34B`等模型，该系列模型最多可支持20万的超长上下文窗口版本，可以处理约40万汉超长文本输入，理解超过1000页的PDF文档。

### 元-2.0

+   地址：[https://github.com/IEIT-Yuan/Yuan-2.0](https://github.com/IEIT-Yuan/Yuan-2.0)
+   简介：该项目由浪潮信息发布的新一代基础语言大模型，具体开源了全部3个模型`源2.0-102B`，`源2.0-51B`和`源2.0-2B`。并提供了预训练，推理，推理服务的相关脚本。`源2.0`是在`源1.0`的基础上，利用更多样的高质量训练预数据和指令配置数学集，令模型在语义、推理、代码、知识等不同方面具备更强的理解能力。

### 中文-Mixtral-8x7B

+   地址：[https://github.com/HIT-SCIR/Chinese-Mixtral-8x7B](https://github.com/HIT-SCIR/Chinese-Mixtral-8x7B)
+   简介：该项目基于`Mixtral-8x7B`稀疏混合专家模型进行了中文扩词表增量预训练，开源了`Chinese-Mixtral-8x7B`扩词表模型以及代码。该模型的中文编解码效率较原模型显着同时通过在大规模语料开源上进行增量预训练，该模型具备了强大的中文生成和理解能力。

### 蓝色LM

+   地址：[https://github.com/vivo-ai-lab/BlueLM](https://github.com/vivo-ai-lab/BlueLM)
+   简介：`BlueLM` 是由 vivo AI 全球研究院自主研发的大规模预训练语言模型，本次发布包含 7B 基础 (`base`) 模型和 7B 对话 (`chat`) 模型，同时我们开源了支持 32K 的长文本基础 (`base`) ) 模型和对话（聊天）模型。

### 图灵MM

+   地址：[https://github.com/lightyear-turing/TuringMM-34B-Chat](https://github.com/lightyear-turing/TuringMM-34B-Chat)
+   简介：`TuringMM-34B-Chat`是一款开源的中英文聊天模型，由北京光年无限科技有限公司基于`Yi-34B`开源模型、基于14w的精标教育数据进行`sft`参数以及15W太阳能数据进行`DPO`偏好学习得到一个角色模型。

### 猎户座

+   地址：[https://github.com/OrionStarAI/Orion](https://github.com/OrionStarAI/Orion)
+   简介：`Orion-14B-Base` 是一个拥有 140 亿参数的多语种大模型，该模型在一个包含 2.5 万亿代币的大象数据集上进行了训练，内容包括中文、英语、日语、韩语等多种语言语言。

### OrionStar-Yi-34B-聊天

+   地址：[https://github.com/OrionStarAI/OrionStar-Yi-34B-Chat](https://github.com/OrionStarAI/OrionStar-Yi-34B-Chat)
+   简介：`OrionStar-Yi-34B-Chat`是猎户星空基于零一开源的`Yi-34B`模型，使用15W+的高质量语言料训练而来的大模型，旨在为大模型社区用户提供卓越的交互体验。

# 1.2 多模态LLM模型

### 视觉GLM-6B

+   地址：[https://github.com/THUDM/VisualGLM-6B](https://github.com/THUDM/VisualGLM-6B)
+   简介：一个开源的，支持图像、中文和英文的多模态对话语言模型，基于`ChatGLM-6B`的语言模型，具有62亿参数；图像部分通过训练`BLIP2-Qformer`构建起视觉模型与语言模型的桥梁，整体模型共78亿参数。来自于`CogView`数据集的30M高精度中文图文对，与300M经过筛选的中文图文对进行预训练。

### CogVLM

+   地址：https://github.com/THUDM/CogVLM
+   简介：一个强大的视觉语言模型（`VLM`）。`CogVLM-17B` 拥有 100 亿视觉参数和 70 亿语言参数。`CogVLM-17B` 在 10 个经典跨模态基准测试上取得了 SOTA 性能。`CogVLM` 能够准确地描述图像，几乎不会出现幻觉。

### 可见每千次展示费用

+   地址：https://github.com/OpenBMB/VisCPM
+   简介：一个开源的多模态大模型系列，支持中英双语的多模态对话能力（`VisCPM-Chat`模型）和文到图生成能力（`VisCPM-Paint`模型）。`VisCPM`基于百亿参数量语言大模型`CPM-Bee`（10B）训练，视觉融合编码器（`Q-Former`）和视觉解码器（`Diffusion-UNet`）以支持视觉信号的输入和输出。得益于`CPM-Bee`建立的优秀双语能力，`VisCPM`可以仅通过中文多模态数据预训练，泛化实现优秀的中文多模态能力。

### 视觉中国-LLaMA-羊驼

+   地址：[https://github.com/airaria/Visual-Chinese-LLaMA-Alpaca](https://github.com/airaria/Visual-Chinese-LLaMA-Alpaca)
+   简介：基于中文`LLaMA` & `Alpaca`大模型项目开发的多态中文大模型。`VisualCLA`在中文`LLaMA`/`Alpaca`模型上增加了图像编码等模块，使`LLaMA`模型可以接收信息。在此基础上，使用了中文图文对数据进行了多模态预训练，彩色图像与文本表示，赋予其基本的多模态理解能力；并利用多模态指令数据集精调，增强其对多模态指令的理解、执行和对话能力，目前开源了`VisualCLA-7B-v0.1`。

### LLaSM

+   地址：[https://github.com/LinkSoul-AI/LLaSM](https://github.com/LinkSoul-AI/LLaSM)



+ 简介：第一个支持中英文双语语音-文本多模态对话的开源可对话模型。便捷的语音输入将大幅改善以文本为输入的大模型的使用体验，同时避免了基于ASR解决方案的繁琐流程以及可能引入的错误。目前开源了LLaSM-Chinese-Llama-2-7B、LLaSM-Baichuan-7B等模型与数据集。

### Qwen-VL

+ 地址：`https://github.com/QwenLM/Qwen-VL`
+ 简介：是阿里云研发的大规模视觉语言模型，可以以图像、文本、检测框作为输入，并以文本和检测框作为输出。特点包括：强大的性能：在四大类多模态任务的标准中文评测中均达到同等通用模型大小下最好效果；多语言对话模型：天然支持中文、中文等多语言对话，端到端支持图片里英双语的长文本识别；多图交错对话：支持多图输入和比较，指定图片问答，多图文学创作等；首个支持中文开放域定位的通用模型：通过中文开放域语言表达进行检测框标注；细粒度识别和理解相比：于目前其他开源LVLM使用224分辨率，Qwen-VL是第一个开源的448分辨率的LVLM模型。更高分辨率可以升级细粒度的文字识别、文档问答和检测框标签。

---

> 本站由 [AI研习社](https://zl49so8lbq.feishu.cn/wiki/wikcnu3kSXr75E50w8PFefhJMYf?from=from_copylink) 志愿者共同维护，👉 加入 [ 核心会员群](https://zl49so8lbq.feishu.cn/wiki/WB2awt7lli6actkm7rtc3ejInmd?from=from_copylink)做一个用AI持续赚钱的人。

> 推荐阅读：

> [聚焦｜两会 AI 热点](https://aiyanxishe.feishu.cn/wiki/QXPrwY5MwiiwUukBE1dcDEuOnM6?from=from_copylink) [9000+AI副业搞钱的野路子](https://aiyanxishe.feishu.cn/wiki/Go55wlU1DiuEYJkCgStcbqepnu2?from=from_copylink)🔥S[ora学习知识库 ](https://zl49so8lbq.feishu.cn/wiki/PiW6wlVlqi3sdnkIRHXcUK8CnK7?from=from_copylink) [【0月租】新西兰 Skinny 电话卡](https://zl49so8lbq.feishu.cn/wiki/QYxqw2UDtiYSnVkyqQxcwLgHnBd?from=from_copylink) [🔥AI研习社「会员内容」](https://aiyanxishe.feishu.cn/wiki/OpSCwqcT9iHVHZkpOnBcjK87nYM?from=from_copylink)

![](img/740a991e9e8ebad2e1b059fc345e82a6.png)

👆 扫码加入AI研习社 👆

踩在人工智能浪潮最前沿

🔥 AI研习社正在努力搭建硬核实用的AI知识库&学习社区，由30余位AI爱好者共同组建。

目前已有 **6000+**AI人工智能相关爱好者加入，目前已经建立20个🛰️群

请注意，这是一个**免费的公益社群**

社群里，我们将狂甩最新观点，同步实时资讯，交换资料干货，分享工具和经验，还可以组队搞事情!

欢迎积极入群，踩在浪潮最前沿
