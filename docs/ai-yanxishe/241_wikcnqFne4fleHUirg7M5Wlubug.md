
# AI+行业解决方案

> 来源：[https://aiyanxishe.feishu.cn/docx/PaPDdgrSzoykdXxHLY6cbPfXnah](https://aiyanxishe.feishu.cn/docx/PaPDdgrSzoykdXxHLY6cbPfXnah)

<https://aiyanxishe.feishu.cn/sync/H4bBd6rOtsCpG9bjI0ZcDHxinnc>

自ChatGPT为代表的大语言模型（Large Language Model，LLM）出现以后，由于其惊人的类通用人工智能（AGI）的能力，掀起了新一轮自然语言处理领域的研究和应用的浪潮。以ChatGLM、LLaMA等平民玩家跑起较小规模的LLM开源模型后，大家一致支持了非常多基于LLM的二次配置或应用的案例。本项目旨在收集和整理中文LLM相关的开源模型、应用、数据集及教程等资料，目前收录的资源已达100+个！

[核心会员群](https://aiyanxishe.feishu.cn/wiki/WB2awt7lli6actkm7rtc3ejInmd?from=from_copylink)时也欢迎大家贡献本项目未收录的开源模型、应用、数据集等，可加入

## 常见采购模型细节概述：

### 1. 模型

#### 1.1 文本法学硕士模型

+ 聊天GLM：
  + 地址：https://github.com/THUDM/ChatGLM-6B
  + 简介：中文领域效果最好的开源模块之一，针对中文问答和对话进行了优化。经过约1T标识符的中英双语训练，辅以监督转向、反馈自助、人类反馈强化学习等技术的加持

+ 聊天GLM2-6B
  + 地址：https://github.com/THUDM/ChatGLM2-6B
  + 简介：基于开源中英双语对话模型第二代ChatGLM-6B的代版本，在保留了初代模型对话模型、部署多边等品种特性的基础之上，引入了GLM的混合目标函数，经过了1.4 T中英标识符的预训练与人类偏好训练；基础模型的上下文长度扩增了32K，并在对话阶段使用8K的上下文长度训练；基于多查询注意力技术实现更高效的推理速度推理和的显着占用；允许商业使用。

+ 聊天GLM3-6B
  + 地址：https://github.com/THUDM/ChatGLM3
  + 简介：ChatGLM3-6B 是ChatGLM3系列中的开源模型，在保留了前两代模型对话开发、部署驱动低等牲畜优秀特性的基础上，ChatGLM3-6B引入了如下特性：更强大的基础模型： ChatGLM3-6B的基础模型ChatGLM3-6B-Base采用了更多样的数据训练、更充分的训练步数和更合理的训练策略；更完整的功能支持：ChatGLM3-6B采用了全新设计的提示格式，除正常的多轮对话外。同时初期支持工具调用（函数调用）、代码执行（代码解释器）和代理任务等复杂；场景更全面的开源序列：除了对话模型ChatGLM3-6B外，还是开源的基础模型ChatGLM3-6B-Base、长文本对话模型ChatGLM3-6B-32K。以上话题重对学术研究完全开放，在填写表格进行登记后亦允许免费商业使用。

+ 中国-美洲驼-羊驼：
  + 地址：https://github.com/ymcui/Chinese-LLaMA-Alpaca
  + 简介：中文LLaMA&Alpaca大语言模型+本地CPU/GPU配置，在原版LLaMA的基础上增加了中文词表并使用了中文数据进行二次预训练

+ Chinese-LLaMA-Alpaca-2：
  + 地址：https://github.com/ymcui/Chinese-LLaMA-Alpaca-2
  + 简介：该项目将发布中文LLaMA-2 & Alpaca-2大语言模型，基于可用于的LLaMA-2进行二次开发。

+ 中文-LlaMA2：
  + 地址：https://github.com/michael-wzhu/Chinese-LlaMA2
  + 简介：该项目基于可进行的LLaMA-2进行二次开发决策，在次开展Llama 2的中文汉化工作，包括Chinese-LlaMA2:对Llama 2进行中文预训练；第一步：先在42G中文上进行训练；后续将加大训练快速规模；Chinese-LlaMA2-chat：对Chinese-LlaMA2进行指令调节和多轮对话调节，以适应各种应用场景和多轮对话交互。同时我们也考虑更严重的中文改装方案：Chinese-LlaMA2-sft-v0:采用现有的开源中文配置配置或者是对话数据，对LlaMA-2进行直接配置（将于近期开源）。

+ Llama2-中文：
  + 地址：https://github.com/FlagAlpha/Llama2-Chinese
  + 简介：该项目专注于Llama2模型在中文方面的优化和上层建设，基于大规模中文数据，从预训练开始对Llama2模型进行中文能力的持续迭代升级。

+ 奎文
  + 地址：https://github.com/QwenLM/Qwen
  + 简介：通义千问是阿里云研发的通义千问大模型系列模型，包括参数规模为18亿（1.8B）、70亿（7B）、140亿（14B）和720亿（72B）。规模的模型包括基础模型Qwen，即Qwen-1.8B、Qwen-7B、Qwen-14B、Qwen-72B，以及对话模型Qwen-Chat，即Qwen-1.8B-Chat、Qwen-7B-Chat、Qwen-14B-Chat和Qwen-72B-Chat。数据集包括文本和代码等多种数据类型，覆盖通用领域和专业领域，能支持8K的上下文长度，针对插件调用相关的横向数据做了特定优化，当前模型能有效调用插件以及升级为Agent。

+ OpenChineseLLaMA：
  + 地址：https://github.com/OpenLMLab/OpenChineseLLaMA
  + 简介：基于LLaMA-7B经过中文数据集增量预产生的中文大语言模型底座，对比原版LLaMA，该模型在中文理解能力和生成能力方面均获得了更大提升，在下游下游任务中均取得了突出的成绩。

+ 美女：
  + 地址：https://github.com/LianjiaTech/BELLE
  + 简介：开源基于BLOOMZ和LLaMA优化后的一系列模型，同时包括数据、相关模型、训练代码、应用场景等，同时引入不同的数据、训练算法等对模型表现的影响。

+ 熊猫：
  + 地址：https://github.com/dandelionsllm/pandallm
  + 简介：开源基于LLaMA-7B,-13B,-33B,-65B进行中文领域上的持续预训练的语言模型，使用了接近15M条数据进行二次预训练。

+ 罗宾（罗宾）：
  + 地址：https://github.com/OptimalScale/LMFlow
  + 简介：Robin (Robin)是香港科技大学LMFlow团队开发的中英双语大语言模型。仅使用180K条数据获取的Robin第二代模型，在Huggingface排行榜上达到了第一名的成绩。LMFlow支持快速用户训练个性化模型，单张3090和5个小时即可达到70亿参数定制化模型。

+ 风神榜-LM：
  + 地址：https://github.com/IDEA-CCNL/封神榜-LM
  + 简介：封神榜大模型是IDEA研究院认知计算与自然语言研究中心主导的大模型开源体系，该项目开源了姜子牙通用大模型V1，是基于LLaMa的130亿参数的大规模预案训练文本模型，具备翻译、编程、分类、信息采集、摘要、文案生成、常识问答和数学计算等能力。除姜子牙系列模型之外，该项目还开源了太乙、二郎神系列等模型。

+ 比拉：
  + 地址：https://github.com/Neutralzz/BiLLa
  + 简介：该项目产生了推理能力增强的中英文双语LLaMA模型。模型的主要特性有：扩大LLaMA的中文理解能力，并寻求减少对原始LLaMA中文的损伤；过程增加热点能力的任务型数据，利用ChatGPT生成解析，强化模型理解任务流程图；全量参数更新，追求更好的生成效果。

+ 苔藓：
  + 地址：https://github.com/OpenLMLab/MOSS
  + 简介：支持中英双语和多种插件的开源对话语言模型，MOSS基础语言模型在约七千亿中英文以及代码单词上预训练得到，后续对话配置自定义、插件增强学习和人类偏好训练配备多轮对话能力及使用多种插件的能力。

+ 骆驼-中文-法学硕士：
  + 地址：https://github.com/LC1332/Luotuo-Chinese-LLM
  + 简介：囊括了一系列中文大语言模型开源项目，包含了一系列已有的基于已有的开源模型（ChatGLM, MOSS, LLaMA）进行二次配置的模型，指令配置数据集等。

+ 林莉：
  + 地址：https://github.com/CVI-SZU/Linly
  + 简介：提供中文对话模型 Linly-ChatFlow 、中文基础模型 Linly-Chinese-LLaMA 及其训练数据。中文基础模型以 LLaMA 为基础，利用中文和中英平行增量预训练。项目汇总了目前公开的多语言指令数据，对中文模型进行了大规模指令接下来训练，实现了Linly-ChatFlow对话模型。

+ 萤火虫：
  + 地址：https://github.com/yangjianxin1/Firefly
  + 简介：Firefly(流萤)是一个开源的中文大语言模型项目，开源包括数据、参数代码、多个基于Bloom、baichuan等参数好的模型等；支持全量参数参数、QLoRA以及高效参数参数、LoRA指令驱动；支持主流的开源大模型，如百川百川、Ziya、Bloom、LLaMA等。持lora与基础模型进行权重合并，推理更便捷。

+ 聊天园
  + 地址：https://github.com/clue-ai/ChatYuan
  + 简介：元语智能发布的一系列支持中文的功能型对话语言大模型，在力矩数据、人类反馈强化学习、思维链等方面进行了优化。

+ 聊天RWKV：
  + 地址：https://github.com/BlinkDL/ChatRWKV
  + 简介：开源了一系列基于RWKV架构的聊天模型（包括中文和中文），发布了包括Raven，Novel-ChnEng，Novel-Ch与Novel-ChnEng-ChnPro等模型，可以直接闲聊及进行诗歌、小说等创作，包括7B和14B等规模的模型。

+ CPM-蜜蜂
  + 地址：https://github.com/OpenBMB/CPM-Bee
  + 简介：一个完全、允许太阳能训练的百亿开源参数中英文基座模型。它采用变压器自回归架构（auto-regressive），在超万亿（万亿）级语料上进行预配置，拥有强大的基础能力开发者和休止符可以在 CPM-Bee 基础模型的基础上在各类场景中进行改装来以特定创建领域的应用模型。

+ 老虎机器人
  + 地址：https://github.com/TigerResearch/TigerBot
  + 简介：一个多语言多任务的大规模训练模型(LLM)，开源语言包括模型：TigerBot-7B, TigerBot-7B-base，TigerBot-180B，基本和推理代码，100G预训练数据，内容金融、法律、百科的领域数据以及API等。

+ 书生·浦语
  + 地址：https://github.com/InternLM/InternLM-techreport
  + 简介：商汤科技、上海人工智能实验室联合香港中文大学、复旦大学和上海交通大学发布千亿级参数大语言模型“书生·浦语”（InternLM）。据悉，“书生·浦语”具有1040亿参数，基于“包含1.6万亿代币的多语言种高质量数据集”而成。

+ 书生·浦语2
  + 地址：https://github.com/InternLM/InternLM
  + 简介：商汤科技、上海人工智能实验室联合香港中文大学、复旦大学和上海交通大学发布千亿级参数大语言模型“书生·浦语”（InternLM2）。InternLM2在数理、代码、对话、创作等各方面各方面能力都取得了长足进步，综合性能达到开源模型的领先水平。InternLM2包含两个模型规格：7B和20B。7B为轻量级的研究和应用提供了一个简便但性能不俗的模型，20B模型的综合性能更加强大，可以有效支持更加复杂的实用场景。

+ 天鹰座
  + 地址：https://github.com/FlagAI-Open/FlagAI/tree/master/examples/Aquila
  + 简介：由智源研究院发布，Aquila语言大模型在技术上继承了GPT-3、LLaMA等的架构设计优点，替换了一个更高效的底层算子实现、重新设计实现了中英文的分词器，升级了BMTrain工具训练方法，是在中英文高质量语料基础上从0开始训练的，通过数据质量的控制、多种训练的优化方法，实现在更小的数据集、更短的训练时间，获得比其他开源模型更优的性能。也是第一个支持中英双语知识、支持许可协议、符合国内数据合规需要的大规模开源语言模型。

+ 天鹰座2
  + 地址：https://github.com/FlagAI-Open/Aquila2
  + 简介：由智源研究院发布，Aquila2系列，包括基础语言模型Aquila2-7B，Aquila2-34B和Aquila2-70B-Expr，对话模型AquilaChat2-7B，AquilaChat2-34B和AquilaChat2-70B-Expr，长文本对话模型AquilaChat2-7B-16k 和 AquilaChat2-34B-16。

+ 百川7B
  + 地址：https://github.com/baichuan-inc/baichuan-7B
  + 简介：Baichuan-13B 是由百川智能继Baichuan-7B后续开发的包含130亿参数的开源可容纳的大规模语言模型，在权威的中文和英文基准上均取得同尺寸最好的效果。该项目发布包含预训练 (Baichuan-13B-Base) 和彩色 (Baichuan-13B-Chat) 两个版本。

+ 百川13B
  + 地址：https://github.com/baichuan-inc/Baichuan-13B
  + 简介：由百川智能开发的一个开源可运行的大规模预训练语言模型。基于Transformer结构，在大约1.2万亿代币上训练的70亿参数模型，支持中英双语，上下文窗口长度为4096。的中文和英文权威基准（C-EVAL/MMLU）上均取得同尺寸最好的效果。

+ 百川2
  + 地址：https://github.com/baichuan-inc/Baichuan2
  + 简介：由百川智能推出的新一代开源大语言训练模型，采用2.6万亿代币的高质量语言素材，在多个权威的中文、中文和多语言的通用、领域基准上取得同尺寸的最佳效果，发布包含有7B、13B的Base和经过PPO训练的Chat版本，并提供了Chat版本的4bits量化。

+ 阿尼玛
  + 地址：https://github.com/lyogavin/Anima
  + 简介：由艾写科技开发的一个基于QLoRA的33B中文大语言模型的开源训练，该模型基于QLoRA的Guanaco 33B模型使用Chinese-Vicuna项目开放的数据集`guanaco_belle_merge_v1.0`进行finetune训练了10000个步骤，基于Elo评级锦标赛评估效果更好。

+ 知道LM
  + 地址：https://github.com/zjunlp/KnowLM
  + 简介：KnowLM项目旨在发布开源大模型框架及相应模型权重以弥补知识谬误问题，包括大模型的知识难更新及存在潜在的错误和偏见等。该项目一期发布了基于Llama的抽取大模型智析，利用中文资料对LLaMA（13B）进行进一步的全量预训练，并基于知识图谱转换指令技术对知识抽取任务进行优化。

+ 贝灵
  + 地址：https://github.com/ictnlp/BayLing



# 1. 模型

## 1.1 通用大模型

### 百临（BayLing）

简介：一个具有增强的跨语言对齐的通用大模型，由中国科学院计算技术研究所自然语言处理团队开发。百临（BayLing）以LLaMA为基础模型，探索了以交互式翻译任务为核心进行指令扭矩的方法，旨在同时完成语言间校准以及与人类意图校准，将LLaMA的生成能力和指令后续能力从英语迁移到其他语言（中文）。在多语言翻译、交互翻译、通用任务、标准化考试的评测中，百生在中文/英语中均表现出了更好的表现。百生提供了在线的内测版演示，供大家体验。

### 玉兰聊天室（YuLan-Chat）

地址：[https://github.com/RUC-GSAI/YuLan-Chat](https://github.com/RUC-GSAI/YuLan-Chat)

简介：YuLan-Chat是中国人民大学GSAI研究人员开发的基于聊天的大语言模型。它是在LLaMA的基础上配置开发的，具有高质量的中文和中文指令。YuLan-Chat与用户聊天，很可以很好地遵循英文或中文指令，并且可以在确定后部署在GPU（A800-80G或RTX3090）上。

### 聚LM（PolyLM）

地址：[https://github.com/DAMO-NLP-MT/PolyLM](https://github.com/DAMO-NLP-MT/PolyLM)

简介：一个在6400亿个词的数据上从头训练的多语言语言模型，包括两个模型大小(1.7B和13B)。PolyLM覆盖中、英、俄、西、法、葡、德、意、荷、波、阿、土、希伯来、日、韩、泰、越南、对话等语种，尤其是对亚洲语种更友好。

### 活子

地址：[https://github.com/HIT-SCIR/huozi](https://github.com/HIT-SCIR/huozi)

简介：由哈工语言处理研究所多位老师和学生参与开发的一个开源可开发的大规模预训练语言模型。该模型基于Bloom结构的70亿参数模型，支持中英双语，上下文窗口长度为2048 ，同时还开源了基于RLHF训练的模型以及全人工标注的16.9K中文偏好数据集。

### 雅仪

地址：[https://github.com/wenge-research/YaYi](https://github.com/wenge-research/YaYi)

简介：雅意大模型在百万级人工构建的高质量领域数据上进行指令获取，数据训练覆盖媒体宣传、舆情分析、公共安全、金融风控、城市治理等五个大领域，上百种自然语言指令任务。雅意大模型从预初始化训练权重到领域模型的迭代过程中，我们逐步增强了它的中文基础能力和领域分析能力，并增加了多轮对话和部分插件能力。同时，经过数百名用户内测过程中持续不断的人工反馈优化，进一步提升了模型性能和安全性。已开源基于LLaMA 2的中文优化模型版本，探索适用于中文多领域任务的最新实践。

### XVERSE-7B

地址：[https://github.com/xverse-ai/XVERSE-7B](https://github.com/xverse-ai/XVERSE-7B)

简介：由深圳元象科技自主研发的支持多语言的大语言模型，支持 8K 的上下文长度（Context Length），使用 2.6 万亿 token 的高质量、多样化的数据对模型进行充分训练，支持中、英、俄、西等40多种语言。并包含GGUF、GPTQ仿真版本的模型，支持在llama.cpp、vLLM在MacOS/Linux/Windows系统上的推理。

### XVERSE-13B

地址：[https://github.com/xverse-ai/XVERSE-13B](https://github.com/xverse-ai/XVERSE-13B)

简介：由深圳元象科技自主研发的支持多语言的大语言模型，支持8K的上下文长度（Context Length），使用3.2万亿代币的高质量、多样化的数据对模型进行充分的训练，支持中、英、俄、西等40多种语言。包含长序列对话模型 XVERSE-13B-256K ，该版本模型最大支持 256K 的上下文窗口长度，约 25w 字的输入内容，可以协助进行文献汇总、报告分析等任务。并包含GGUF、GPTQ量化版本的模型，支持在llama.cpp、vLLM在MacOS/Linux/Windows系统上的推理。

### XVERSE-65B

地址：[https://github.com/xverse-ai/XVERSE-65B](https://github.com/xverse-ai/XVERSE-65B)

简介：由深圳元象科技自主研发的支持多语言的大语言模型，支持16K的上下文长度（Context Length），使用2.6万亿代币的高质量、多样化的数据对模型进行充分的训练，支持中、英、俄、西等40多种语言。包含增量预训练到3.2万亿代币的XVERSE-65B-2模型。并包含GGUF、GPTQ量化版本的模型，支持在llama.cpp、vLLM在MacOS/Linux /Windows系统上的推理。

### XVERSE-MoE-A4.2B

地址：[https://github.com/xverse-ai/XVERSE-MoE-A4.2B](https://github.com/xverse-ai/XVERSE-MoE-A4.2B)

简介：由深圳元象科技自主研发的支持多语言的大语言模型（Large Language Model），采用混合专家模型（MoE，Mixture-of Experts）架构，模型的总参数规模为258亿，实际激活的参数量为 42 亿，支持 8K 的上下文长度（Context Length），使用 3.2 万亿 token 的高质量、多样化的数据对模型进行充分训练，支持中、英、俄、西等 40 多种语言。

### 思凯沃（Skywork）

地址：[https://github.com/SkyworkAI/Skywork](https://github.com/SkyworkAI/Skywork)

简介：该项目有了天工系列模型，开源该系列模型在3.2TB高质量多语言和代码数据上进行预训练，开源了包括模型参数，训练数据，评估，评估方法。具体包括Skywork-13B- Base模型、Skywork-13B-Chat模型、Skywork-13B-Math模型和Skywork-13B-MM模型，以及每个模型的量化版模型，以支持用户在消费级显卡进行部署和推理。

### 义（Yi）

地址：[https://github.com/01-ai/Yi](https://github.com/01-ai/Yi)

简介：该项目开源了Yi-6B和Yi-34B等模型，该系列模型最多可支持20万的超长上下文窗口版本，可以处理约40万汉超长文本输入，理解超过1000页的PDF文档。

### 元-2.0（Yuan-2.0）

地址：[https://github.com/IEIT-Yuan/Yuan-2.0](https://github.com/IEIT-Yuan/Yuan-2.0)

简介：该项目由浪潮信息发布的新一代基础语言大模型，具体开源了全部3个模型源2.0-102B，源2.0-51B和源2.0-2B。并提供了预训练，推理，推理服务的相关脚本。源2.0是在源1.0的基础上，利用更多样的高质量训练预数据和指令配置数学集，令模型在语义、推理、代码、知识等不同方面具备更强的理解能力。

### 中文-Mixtral-8x7B

地址：[https://github.com/HIT-SCIR/Chinese-Mixtral-8x7B](https://github.com/HIT-SCIR/Chinese-Mixtral-8x7B)

简介：该项目基于Mixtral-8x7B稀疏混合专家模型进行了中文扩词表增量预训练，开源了Chinese-Mixtral-8x7B扩词表模型以及代码。该模型的中文编解码效率较原模型显着同时通过在大规模语料开源上进行增量预训练，该模型具备了强大的中文生成和理解能力。

### 蓝色LM（BlueLM）

地址：[https://github.com/vivo-ai-lab/BlueLM](https://github.com/vivo-ai-lab/BlueLM)

简介：BlueLM 是由 vivo AI 全球研究院自主研发的大规模预训练语言模型，本次发布包含 7B 基础 (base) 模型和 7B 对话 (chat) 模型，同时我们开源了支持 32K 的长文本基础 (base) ) 模型和对话（聊天）模型。

### 图灵MM（TuringMM-34B-Chat）

地址：[https://github.com/lightyear-turing/TuringMM-34B-Chat](https://github.com/lightyear-turing/TuringMM-34B-Chat)

简介：TuringMM-34B-Chat是一款开源的中英文聊天模型，由北京光年无限科技有限公司基于Yi-34B开源模型、基于14w的精标教育数据进行sft参数以及15W太阳能数据进行DPO偏好学习得到一个角色模型。

### 猎户座（Orion）

地址：[https://github.com/OrionStarAI/Orion](https://github.com/OrionStarAI/Orion)

简介：Orion-14B-Base 是一个拥有 140 亿参数的多语种大模型，该模型在一个包含 2.5 万亿代币的大象数据集上进行了训练，内容包括中文、英语、日语、韩语等多种语言语言。

### OrionStar-Yi-34B-聊天

地址：[https://github.com/OrionStarAI/OrionStar-Yi-34B-Chat](https://github.com/OrionStarAI/OrionStar-Yi-34B-Chat)

简介：OrionStar-Yi-34B-Chat是猎户星空基于零一开源的Yi-34B模型，使用15W+的高质量语言料训练而来的大模型，旨在为大模型社区用户提供卓越的交互体验。

## 1.2 多模态LLM模型

### 视觉GLM-6B（VisualGLM-6B）

地址：[https://github.com/THUDM/VisualGLM-6B](https://github.com/THUDM/VisualGLM-6B)

简介：一个开源的，支持图像、中文和英文的多模态对话语言模型，基于ChatGLM-6B的语言模型，具有62亿参数；图像部分通过训练BLIP2-Qformer构建起视觉模型与语言模型的桥梁，整体模型共78亿参数。来自于CogView数据集的30M高精度中文图文对，与300M经过筛选的中文图文对进行预训练。

### CogVLM

地址：[https://github.com/THUDM/CogVLM](https://github.com/THUDM/CogVLM)

简介：一个强大的视觉语言模型（VLM）。CogVLM-17B 拥有 100 亿视觉参数和 70 亿语言参数。CogVLM-17B 在 10 个经典跨模态基准测试上取得了 SOTA 性能。CogVLM 能够准确地描述图像，几乎不会出现幻觉。

### 可见每千次展示费用（VisCPM）

地址：[https://github.com/OpenBMB/VisCPM](https://github.com/OpenBMB/VisCPM)

简介：一个开源的多模态大模型系列，支持中英双语的多模态对话能力（VisCPM-Chat模型）和文到图生成能力（VisCPM-Paint模型）。VisCPM基于百亿参数量语言大模型CPM -Bee（10B）训练，视觉融合编码器（Q-Former）和视觉解码器（Diffusion-UNet）以支持视觉信号的输入和输出。得益于CPM-Bee建立的优秀双语能力，VisCPM可以仅通过中文多模态数据预训练，泛化实现优秀的中文多模态能力。

### 视觉中国-LLaMA-羊驼

地址：[https://github.com/airaria/Visual-Chinese-LLaMA-Alpaca](https://github.com/airaria/Visual-Chinese-LLaMA-Alpaca)

简介：基于中文LLaMA&Alpaca大模型项目开发的多态中文大模型。VisualCLA在中文LLaMA/Alpaca模型上增加了图像编码等模块，使LLaMA模型可以接收信息。在此基础上，使用了中文图文对数据进行了多模态预训练，彩色图像与文本表示，赋予其基本的多模态理解能力；并利用多模态指令数据集精调，增强其对多模态指令的理解、执行和对话能力，目前开源了VisualCLA-7B-v0.1。

### LLaSM

地址：[https://github.com/LinkSoul-AI/LLaSM](https://github.com/LinkSoul-AI/LLaSM)

简介：第一个支持中英文双语语音-文本多模态对话的开源可对话模型。便捷的语音输入将大幅改善以文本为输入的大模型的使用体验，同时避免了基于ASR解决方案的繁琐流程以及可能引入的错误。目前开源了LLaSM-Chinese-Llama-2-7B、LLaSM-Baichuan-7B等模型与数据集。

### Qwen-VL

地址：[https://github.com/QwenLM/Qwen-VL](https://github.com/QwenLM/Qwen-VL)

简介：是阿里云研发的大规模视觉语言模型，可以以图像、文本、检测框作为输入，并以文本和检测框作为输出。特点包括：强大的性能：在四大类多模态任务的标准中文评测中均达到同等通用模型大小下最好效果；多语言对话模型：天然支持中文、中文等多语言对话，端到端支持图片里英双语的长文本识别；多图交错对话：支持多图输入和比较，指定图片问答，多图文学创作等；首个支持中文开放域定位的通用模型：通过中文开放域语言表达进行检测框标注；细粒度识别和理解相比：于目前其他开源LVLM使用224分辨率，Qwen-VL是第一个开源的448分辨率的LVLM模型。更高分辨率可以升级细粒度的文字识别、文档问答和检测框标签。

# 2. 应用

## 2.1 垂直领域

### 医疗

#### GLM医生（DoctorGLM）

地址：[https://github.com/xionghonglin/DoctorGLM](https://github.com/xionghonglin/DoctorGLM)

简介：基于ChatGLM-6B的中文问诊模型，通过中文医疗对话数据集进行参数，实现了包括lora、p-tuningv2等参数及配置

#### 本草（Huatuo-Llama-Med-Chinese）

地址：[https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese](https://github.com/SCIR-HI/Huatuo-Llama-Med-Chinese)

简介：开源了经过中文医学指令精调/指令调节(Instruct-tuning)的LLaMA-7B模型。通过医学知识图谱和GPT3.5 API构建了中文医学指令数据集，并在此基础上对LLaMA进行了指令扭矩，提高了LLaMA在医疗领域的快速效果。

#### 扁鹊（BianQue）

地址：[https://github.com/scutcyr/BianQue](https://github.com/scutcyr/BianQue)

简介：一个经过指令与多轮问询对话联合调整的医疗对话大模型，基于ClueAI/ChatYuan-large-v2作为基础，使用中文医疗问答指令与多轮问询对话混合数据集进行调整。

#### 华佗GPT（HuatuoGPT）

地址：[https://github.com/FreedomIntelligence/HuatuoGPT](https://github.com/FreedomIntelligence/HuatuoGPT)

简介：开源经过中文医学指令精调/指令调整(Instruct-tuning)的一个类似GPT的模型

#### Med-ChatGLM

地址：[https://github.com/SCIR-HI/Med-ChatGLM](https://github.com/SCIR-HI/Med-ChatGLM)

简介：基于中文医学知识的ChatGLM模型扭矩，扭矩数据与BenTsao相同。

#### 七珍GPT（QiZhenGPT）

地址：[https://github.com/CMKRG/QiZhenGPT](https://github.com/CMKRG/QiZhenGPT)

简介：该项目利用启真医学知识库构建的中文医学指令数据集，并在此基础上在LLaMA-7B模型上进行指令精调，大幅提高了模型在中文医疗场景下的效果，首先针对药品知识问答发布了体育数据集，后续计划优化疾病、手术、检验等方面的问答效果，并针对医患问答、病历自动生成等应用拓展拓展。

#### 聊天医学（ChatMed）

地址：[https://github.com/michael-wzhu/ChatMed](https://github.com/michael-wzhu/ChatMed)

简介：该项目推出ChatMed系列中文医疗大规模语言模型，模型主干为LlaMA-7b并采用LoRA专用，具体包括ChatMed-Consult :基于中文医疗在线问诊数据集ChatMed_Consult_Dataset的50w+在线问诊回复诊+ChatGPT作为训练集；ChatMed-TCM ：基于中医药指令数据集ChatMed_TCM_Dataset，以开源的中医药知识图谱为基础，采用以实体为中心的自指令方法（entity-centric self-instruct），调用ChatGPT得到2.6w+的围绕中医药的指令数据训练得到。

#### XrayGLM

地址：[https://github.com/WangRongsheng/XrayGLM](https://github.com/WangRongsheng/XrayGLM)

简介：该项目为促进中文领域医学多模态大模型的研究发展，发布了XrayGLM数据集及模型，其在医学影像诊断和多轮交互对话上显示出非凡的潜力。

#### MeChat

地址：[https://github.com/qiuhuachuan/smile](https://github.com/qiuhuachuan/smile)

简介：该项目的中文心理健康支持通用开源模型由 ChatGLM-6B LoRA 16-bit 指令调节得到。数据集通过调用 gpt-3.5-turbo API 扩展真实的心理互助 QA 为多轮的心理健康支持多轮对话，提高了通用语言大模型在心理健康支持领域的表现，更加符合在长程多轮对话的应用场景。

#### 医疗GPT



+ 地址：`https://github.com/shibing624/MedicalGPT`
+ 简介：训练医疗大模型，实现包括二次预训练、有监督建模、强化学习训练。发布中文医疗`LoRA`模型`shibing624/ziya-llama-13b-medical-lora`，基于`Ziya-LLaMA-13B-v1`模型，`SFT`一个版本医疗模型，医疗问答效果有提升，发布后的`LoRA`权重。
+ 孙思邈
+ 地址：`https://github.com/thomas-yanxin/Sunsimiao`
+ 简介：`Sunsimiao`是一个开源的中文医疗大模型，该模型基于`baichuan-7B`和`ChatGLM-6B`基础模型在十万级高质量的中文医疗数据中驱动而得。
+ 神农中医法学硕士
+ 地址：`https://github.com/michael-wzhu/ShenNong-TCM-LLM`
+ 简介：该项目开源了神农中医药大规模语言模型，该模型以`LlaMA`为基础，采用`LoRA (rank=16)`调节器。调节代码与`ChatMed`代码库相同。此外该项目还开源了中医药指令剂量数据集。
+ 灵魂聊天
+ 地址：`https://github.com/scutcyr/SoulChat`
+ 简介：该项目了经过百万规模心理咨询领域中文长文本指令与多轮共情对话数据联合指令调节的心理健康大模型灵心（`SoulChat`），该模型以`ChatGLM-6B`作为初始化模型，进行了了全部参数的参数配置。
+ 护理GPT
+ 地址：`https://github.com/WangRongsheng/CareGPT`
+ 简介：该项目提供了开放可用的医疗护理数据集和开放可用的医疗大语言模型，包含`LLM`的培训、评估、部署等以促进医疗`LLM`快速发展。
+ DISC医学法学硕士
+ 地址：`https://github.com/FudanDISC/DISC-MedLLM`
+ 简介：该项目是由复旦大学发布的针对医疗健康对话式场景并设计的医疗领域大模型与数据集，该模型由基于`Baichuan-13B-Base`指令得到的`DISC-Med-SFT`数据集。
+ 太乙-法学硕士
+ 地址：`https://github.com/DUTIR-BioNLP/Taiyi-LLM`
+ 简介：该项目由大连理工大学信息检索训练研究室开发的中英双语医学大模型“太一”，收集整理了丰富的中英双语生物医学自然语言处理（`BioNLP`）语料，总共包含38个中文数据集，通过丰富的中英文任务指令数据（超过100W条样本）进行大模型（`Qwen-7B-base`）指令配置，使模型具备了出色的中英文生物医学智能问答、医患对话、报告生成、信息抽取、机器翻译、标题生成、文本分类等多种`BioNLP`能力。
+ 无线NGPT
+ 地址：`https://github.com/winninghealth/WiNGPT2`
+ 简介：`WiNGPT2`是一个基于`GPT`的医疗垂直领域大模型，基于`Qwen-7b1`作为基础预模型，这里技术上进行了继续预训练，指令调节等，该项目具体开源了`WiNGPT2-7B-Base`与`WiNGPT2-7B-Chat`模型。
+ 和黄医药-GPT
+ 地址：`https://github.com/synlp/ChiMed-GPT`
+ 简介：`ChiMed-GPT`是一个开源中文医学大语言模型，通过在中文医学数据上持续训练`Ziya-v2`构建而成，其中涵盖了预训练、有监督角色（`SFT`）和来自人类反馈的强化学习（`RLHF`）等训练过程。
+ 心灵聊天
+ 地址：`https://github.com/XD-Lab/MindChat`
+ 简介：心理大模型——漫谈（`MindChat`）希望从心理咨询、心理评估、心理诊断、心理治疗四个维度帮助人们解心理压力与解决心理困惑，为用户提供隐私、温暖、安全、及时、方便`MindChat`是一个基于`Qwen`作为基础的预训练模型，并在此基础上进行指令以获得的心理垂域大模型。

## 法律

+ 獬豸(`LawGPT_zh`): 中文法律对话语言模型
+ 地址：`https://github.com/LiuHC0428/LAW-GPT`
+ 简介: 本项目开源的中文法律通用模型由`ChatGLM-6B` `LoRA` 16位指令调节获取。数据集包括现有的法律问答数据集和基于法条和真实案例指导的自我指导构建的高质量法律文本问答，提高了通用语言大模型在法律领域的表现，提高了模型回答的可靠性和专业程度。
+ LaWGPT：基于中文法律知识的大语言模型
+ 地址：`https://github.com/pengxiao-song/LaWGPT`
+ 简介：该系列模型在通用中文基础模型（如`Chinese-LLaMA`、`ChatGLM`等）的基础上增强了法律领域母词表、大规模中文法律语料预训练，增强了大模型在法律领域的基础语义理解在此基础上，构建了法律领域对话问答数据集、中国司法考试数据集进行指令精调，提升了模型对法律内容的理解和执行能力。
+ LexiLaw：中文法律大模型
+ 地址：`https://github.com/CSHaitao/LexiLaw`
+ 简介：`LexiLaw`是一个基于`ChatGLM-6B`调节的中文法律大模型，通过在法律领域的数据集上进行调节。该模型旨在为法律从业者、学生和普通用户提供准确、可靠的法律咨询服务，包括具体法律问题的咨询，还是对法律条款、案例解析、法规解读等方面的查询。
+ LLaMA律师：中文法律LLaMA
+ 地址：`https://github.com/AndrewZhe/lawyer-llama`
+ 简介：开源了一系列法律领域的指令参数数据和基于`LLaMA`训练的中文法律大模型的参数。`Lawyer LLaMA`首先在大规模法律语言料上进行了持续的预训练。在此基础上，借助`ChatGPT`收集了一些对中国国家统一法律职业资格考试侦查题（以下简称法考）的分析和对法律咨询的解答，利用收集到的数据对模型进行指令调校，让模型习得将法律知识应用到具体场景中的能力。
+ 韩非(`HanFei`)
+ 地址：`https://github.com/siat-nlp/HanFei`
+ 简介: `HanFei-1.0`(韩非)是国内首个全参数的法律大模型，参数量7b，主要功能包括：法律问答、多轮对话、撰写文章、检索等。
+ ChatLaw-法律大模型
+ 地址：`https://github.com/PKU-YuanGroup/ChatLaw`
+ 简介：由北大的一系列法律领域的大开源模型，包括`ChatLaw-13B`（基于姜子牙`Ziya-LLaMA-13B-v1`训练传来），`ChatLaw-33B`（基于`Anima-33B`训练传来，逻辑推理能力大幅提升），`ChatLaw-Text2Vec`，使用93w条评分案例的数据集基于`BERT`训练了一个相似度匹配模型，可将用户提问信息和法条相匹配。
+ lychee_law-律知
+ 地址：`https://github.com/davidpig/lychee_law`
+ 简介：该项目由德国萨尔大学团队和中国南京大学团队合作开发，开源一系列中文司法领域大模型，如`Law-GLM-10B`：基于`GLM-10B`模型，在30GB中文法律数据上进行指令得到的。
+ 智海-录问(`wisdomInterrogatory`)
+ 地址：`https://github.com/zhihaiLLM/wisdomInterrogatory`
+ 简介：该项目由浙江大学、阿里巴巴达摩院以及华院计算三家单位共同设计研发的法律大模型，基于`baichuan-7b`进行了法律领域数据的二次预训练与指令强度，并设计了知识增强的推理流程。
+ 夫子·明察司法大模型
+ 地址：`https://github.com/irlab-sdu/fuzi.mingcha`
+ 简介：该项目由山东大学、浪潮云、中国政法大学联合研发，以`ChatGLM`为大型模型基础，基于海量中文无监督司法语料（包括大众评分标准、法律法规等）与有监督司法公正数据（包括法律问答、类案检索）中文司法大模型的训练。该模型支持法条检索、案例分析、三段论推理评分以及司法对话等功能。
+ DISC法学硕士
+ 地址：`https://github.com/FudanDISC/DISC-LawLLM`
+ 简介：该项目由复旦大学数据智能与社会计算实验室（`Fudan-DISC`）开发并开源的法律领域大模型，包括数据集，基于`Baichuan-13B-Base`进行强度的模型，且增加了检索增强模块。

## 金融

+ Cornucopia（聚宝盆）：基于中文金融知识的`LLaMA`模型
+ 地址：`https://github.com/jerry1993-tech/Cornucopia-LLaMA-Fin-Chinese`
+ 简介：开源了经过中文金融知识指令精调/指令调节(`Instruct-tuning`)的`LLaMA-7B`模型。通过中文金融公开数据+爬取的金融数据构建指令数据集，并在此基础上对`LLaMA`进行了了指令扭矩，提高了`LLaMA`在金融领域的问答效果。基于相同的数据，提升还将利用`GPT3.5` `API`构建高质量的数据集，另外在中文知识图谱-金融上进一步增加高质量的指令数据集。
+ BBT-FinCUGE-应用
+ 地址：`https://github.com/ssymmetry/BBT-FinCUGE-Applications`
+ 简介：开源了中文金融领域开源料培训库`BBT-FinCorpus`，中文金融领域知识增强型预语言模型`BBT-FinT5`及中文金融领域自然语言处理金融基准`CFLEB`。
+ 轩辕（轩辕）：首个千亿级中文金融对话模型
+ 地址：`https://github.com/Duxiaoman-DI/XuanYuan`
+ 简介：轩辕是国内首个开源的千亿级中文对话大模型，同时也是第一个针对中文金融领域优化的千亿级开源对话大模型。轩辕在`BLOOM-176B`的基础上针对中文通用领域和金融领域进行了舆情的预训练，它不仅可以解决通用领域的问题，还可以解答与金融相关的人群问题，为用户提供准确、全面的金融信息和建议。
+ 芬GPT
+ 地址：`https://github.com/AI4Finance-Foundation/FinGPT`
+ 简介：该项目开源了多个金融大模型，包括`ChatGLM-6B`/`ChatGLM2-6B`+`LoRA`和`LLaMA-7B`+`LoRA`的金融大模型，收集了包括金融新闻、社交媒体、财报等中英文训练数据。
+ DISC金融法学硕士
+ 地址：`https://github.com/FudanDISC/DISC-FinLLM`
+ 简介：该项目由复旦大学数据智能与社会计算实验室（`Fudan-DISC`）开发并开源，项目中开源的资源包括：`DISC-FinLLM-SFT`训练数据样本，`DISC-FinLLM`模型参数（基于`Baichuan-13B-Chat`训练），`DISC-Fin-Eval-Benchmark`等。
+ 统一财经
+ 地址：`https://modelscope.cn/models/TongyiFinance/Tongyi-Finance-14B`
+ 简介：该模型是针对金融行业推出的大语言模型，基于义千问通基础模型进行行业语料增量学习，强化金融领域知识和场景应用能力，覆盖金融知识问答、文本分类、信息抽取、文本创作、阅读理解、逻辑推理、多模态、编码等能力象限。具有以下特点：行业语料增量学习：使用200B优质金融行业语料进行增量学习，并进行金融行业词表扩展，覆盖丰富的数据类型，支持更大上下（16k）输入和完整的语义表达。行业能力强化：自研`SFT`质量和多样性分析工具，筛选高质量`SFT`数据，解决大语言模型的对齐问题。行业后序列优化：借助多代理框架，实现知识库增强和工具`API`调用。

## 教育

+ 桃李（`Taoli`）：
+ 地址：`https://github.com/blcuicall/taoli`
+ 简介：一个在国际汉语教育领域数据上进行了额外训练的模型。项目根据目前国际汉语教育领域流传的500余册国际汉语教育教材与教辅书、汉语水平考试试题以及汉语学习者词典等，构建了国际中文教育资源库，构建了共计88000条的高质量国际中文教育问答数据集，并利用收集到的数据对模型进行指令配置，让模型练习得到将知识应用到具体场景中的能力。
+ 教育聊天室：
+ 地址：`https://github.com/icalk-nlp/EduChat`
+ 简介：该项目华东师范大学计算机训练科学与技术学院的`EduNLP`团队研发，主要研究以预大模型为融合的教育对话大模型相关技术，配制的教育垂直领域数据，辅以指令配置、价值观配置等方式，提供教育场景下自动出题、作业批改、情感支持、课程辅导、高考咨询等丰富功能，服务广大教师、学生和家长群体，推动实现因材施教、公平公正、富有温度的智能教育。
+ chatglm-数学：
+ 地址：`https://github.com/yongzhuo/chatglm-maths`
+ 简介：基于`chatglm-6b`/`LORA`/`PPO`/推理的数学题解题大模型，样本为自动生成的整数/小数加减乘除装甲，可`gpu`/`cpu`配置，开源了数据集等。
+ 数学GLM：
+ 地址：`https://github.com/THUDM/MathGLM`
+ 简介：该项目由`THUDM`研发，开源计算了多个能进行20亿参数可以进行准确的多位算计攻击的语言模型，同时开源技术提供了可用于攻击攻击的数据集。
+ 侨办：
+ 地址：`https://github.com/HIT-SCIR-SC/QiaoBan`
+ 简介：该项目旨在构建一个针对儿童情感陪伴的大模型，该仓库包含：用于指令的对话数据/数据，巧板的训练代码，配置训练文件，使用巧板进行对话的示例代码（`TODO`），`checkpoint`将发布至`huggingface`）。

## 科技

+ 天文大语言模型`StarGLM`：
+ 地址：`https://github.com/Yu-Yang-Li/StarGLM`
+ 简介：基于`ChatGLM`训练了天文大语言模型，以期缓解大语言模型在部分天文通用知识和前沿变星领域的幻觉现象，为下一步可处理天文多模态任务、部署于珊瑚礁的移民代理——司天脑（智能数据处理）打下基础。
+ `TransGPT`·致远：
+ 地址：`https://github.com/DUOMO/TransGPT`
+ 简介：开源交通大模型，主要致力于在真实交通行业中发挥实际价值。它能够实现交通情况预测、咨询智能助手、公共交通服务、交通规划设计、交通教育、辅助管理、交通报告安全事故与分析、自动驾驶辅助系统等功能。
+ 墨子：
+ 地址：`https://github.com/gmftbyGMFTBY/science-llm`
+ 简介：该项目基于`LLaMA`和百川开源的科技论文大模型，可以用于科技文献的问答和情感支持。

## 电商

+ 生态GPT
+ 地址：`https://github.com/Alibaba-NLP/EcomGPT`
+ 简介：一个由阿里发布的面向电商领域的语言模型，该模型基于`BLOOMZ`在电商指令调节数据集上的调节得到，人工评估在12个电商气压数据集上超过`ChatGPT`。

## 网络安全

+ 安全组GPT



+ 地址：[https://github.com/Clouditera/secgpt](https://github.com/Clouditera/secgpt)
+ 简介：项目开办了网络安全训练模型，开源该模型基于Baichuan-13B采用Lora做预和SFT训练，此外该项目还开源了相关预训练和指令配置数据集等资源。

### 2.1 农业

+ **后稷（AgriMa）**
  + 地址：[https://github.com/zhiweihu1103/AgriMa](https://github.com/zhiweihu1103/AgriMa)
  + 简介：第一个中文农业大模型是由山西大学、山西农业大学与The Fin AI联合开源研发，以百川为基础，基于海量有监督农业领域相关数据参数，具备广泛的农业知识和智能分析能力，该模型旨在为农业领域提供全面、高效的信息处理和决策支持。

### 2.2 浪链应用

+ **langchain-ChatGLM**
  + 地址：[https://github.com/imClumsyPanda/langchain-ChatGLM](https://github.com/imClumsyPanda/langchain-ChatGLM)
  + 简介：基于本地知识库的问答应用，目标期望建立了一套针对中文场景与开源模型支持友好、可离线运行的知识库问答解决方案。建立了全流程可使用开源模型实现的本地知识库问答应用。目前支持使用 ChatGLM-6B 等大语言模型直接接入，或通过 `fastchat api` 类型接入 Vicuna, Alpaca, LLaMA, Koala, RWKV 等模型。

+ **LangChain-ChatGLM-Webui**
  + 地址：[https://github.com/thomas-yanxin/LangChain-ChatGLM-Webui](https://github.com/thomas-yanxin/LangChain-ChatGLM-Webui)
  + 简介：利用LangChain和ChatGLM-6B系列模型制作的Webui，提供基于本地知识的大模型应用。目前支持上传txt、docx、md、pdf等文本格式文件，包括ChatGLM-6B系列、Belle系列等模型文件以及`GanymedeNil/text2vec-large-chinese`、`nghuyong/ernie-3.0-base-zh`、`nghuyong/ernie-3.0-nano-zh`等嵌入模型。

+ **Langchain-ChatGLM-and-TigerBot**
  + 地址：[https://github.com/wordweb/langchain-ChatGLM-and-TigerBot](https://github.com/wordweb/langchain-ChatGLM-and-TigerBot)
  + 简介：该项目在langchain-ChatGLM的基础上补充了加载TigerBot模型的基于本地知识库的问答应用。

+ **中文LangChain**
  + 地址：[https://github.com/yanqiangmiffy/Chinese-LangChain](https://github.com/yanqiangmiffy/Chinese-LangChain)
  + 简介：基于`ChatGLM-6b`+`langchain`实现本地化知识库检索与智能答案生成（包括互联网检索结果接入）

+ **代理**
  + 地址：https://github.com/InternLM/lagent
  + 简介：Lagent是一个轻量级、开源的基于大语言模型的智能体（agent）框架，支持用户快速将一个大语言模型转变为多种类型的智能体。具体实现了多种类型的智能体，如经典的ReAct，AutoGPT和ReWoo等智能体。框架简单易拓展。要不到20行代码你就能够创建出一个你自己的智能体（代理）。同时支持Python解释器、API调用和搜索三类常用典型工具。灵活支持多种大型语言模型。提供了多种大语言模型支持包括InternLM、Llama-2等开源模型和GPT-4/3.5等基于API的闭源模型。

+ **演示GPT**
  + 地址：[https://github.com/melih-unsal/DemoGPT](https://github.com/melih-unsal/DemoGPT)
  + 简介：⚡ DemoGPT 使您只需使用提示即可创建快速演示。 ⚡

+ **聊天开发**
  + 地址：https://github.com/OpenBMB/ChatDev
  + 简介：ChatDev是一家虚拟软件公司，通过组建不同角色的各种智能代理进行运营，包括首席、首席技术官、分析师、测试员等。这些代理形成了一个多代理组织结构，并因“通过编程彻底改变数字世界”的使命而团结在一起。ChatDev中的代理通过参加专门的功能研讨会进行协作，包括设计、编码、测试记录等任务。

### 2.3 其他应用

+ **文达**
  + 地址：https://github.com/wenda-LLM/wenda
  + 简介：一个LLM调用平台。为小模型外挂知识库查找和设计自动执行动作，实现不亚于大模型的生成能力。

+ **Jittor法学硕士**
  + 地址：[https://github.com/Jittor/JittorLLMs](https://github.com/Jittor/JittorLLMs)
  + 简介：计图大模型推理库：笔记本没有显卡也能跑大模型，具有成本低、支持广、可移植、速度快等优势。

+ **LM流量**
  + 地址：[https://github.com/OptimalScale/LMFlow](https://github.com/OptimalScale/LMFlow)
  + 简介：LMFlow是香港科技大学LMFlow团队开发的模型高效工具箱。LMFlow工具箱具有可扩展性强、方便的特性。LMFlow仅使用180K条数据强度，即可在Huggingface列表第一名中得到的Robin模型。LMFlow支持用户快速训练个性化模型，一周单张3090和5小时即可消耗70亿参数定制化模型。

+ **快速**
  + 地址：https://github.com/ztxz16/fastllm
  + 简介：纯c++的全平台llm加速库，chatglm-6B级模型单卡可达10000+token/s，支持moss、chatglm、baichuan模型，手机端流畅运行。

+ **网络每千次展示费用**
  + 地址：https://github.com/thunlp/WebCPM
  + 简介：一个支持可交互网页搜索的中文大模型。

+ **GPT学术**
  + 地址：[https://github.com/binary-husky/gpt_academic](https://github.com/binary-husky/gpt_academic)
  + 简介：为GPT/GLM提供图形交互界面，特别优化论文阅读润色体验，支持玩具问多个LLM模型，支持清华chatglm等本地模型。复兼容旦MOSS、llama、rwkv、盘古等。

+ **聊天全部**
  + 地址：https://github.com/sunner/ChatALL
  + 简介：ChatALL（中文名：齐谋）可以把一条指令同时发给多个AI，可以帮助用户找到最好的答案。

+ **创意聊天GLM**
  + 地址：[https://github.com/ypwhs/CreativeChatGLM](https://github.com/ypwhs/CreativeChatGLM)
  + 简介：可以使用修改和续写的功能来生成创意内容，可以使用“续写”按钮帮ChatGLM想一个开头，并使得继续生成更多的内容，你可以使用“修改”按钮最后一个ChatGLM的回复。

+ **docker-llama2-聊天**
  + 地址：[https://github.com/soulteary/docker-llama2-chat](https://github.com/soulteary/docker-llama2-chat)
  + 简介：开源了一个只需要三步就可以上手LLaMA2的快速部署方案。

+ **ChatGLM2-语音克隆**
  + 地址：[https://github.com/KevinWang676/ChatGLM2-Voice-Cloning](https://github.com/KevinWang676/ChatGLM2-Voice-Cloning)
  + 简介：实现了一个可以和喜欢的角色沉浸式对话的应用，主要采用ChatGLM2+语音克隆+视频对话的技术。

+ **飞扬的**
  + 地址：https://github.com/pleisto/flappy
  + 简介：一个产品级所有开拓者的LLM SDK，

### 3.数据集

#### 预训练数据集

+ **多态性非血管性静脉曲张**
  + 地址：https://github.com/esbatmop/MNBVC
  + 数据集说明：超大规模中文资料集，不仅包括主流文化，还包括各个小众文化甚至火星文的数据。MNBVC数据集包括新闻、作文、小说、书籍、杂志、论文、台词、帖子、wiki、古诗、歌词、商品介绍、笑话、糗事、聊天记录等一切形式的纯文本中文数据。数据均来源于互联网收集，且在持续更新中。

+ **武道语料库文本**
  + 地址：[https://data.baai.ac.cn/details/WuDaoCorporaText](https://data.baai.ac.cn/details/WuDaoCorporaText)
  + 数据集说明：WuDaoCorpora是北京智源人工智能研究院（智源研究院）构建的大规模、高质量数据集，用于支撑大模型训练研究。目前由文本、对话、图文对、视频文本对四部分组成，分别致力于构建微型语言世界、提炼对话核心规律、打破图文模型形态壁垒、建立视频文字关联，为大模型训练提供坚固的数据支撑。

+ **CLUE语料库2020**
  + 地址：[https://github.com/CLUEbenchmark/CLUECorpus2020](https://github.com/CLUEbenchmark/CLUECorpus2020)
  + 数据集说明：通过对Common Crawl的中文部分进行语料清理，最终获得100GB的高质量中文预训练语料，可直接用于预训练、语言模型或语言生成任务以及专用于简体中文NLP任务的小词表。

+ **万卷-1.0**
  + 地址：[https://opendatalab.org.cn/WanJuan1.0](https://opendatalab.org.cn/WanJuan1.0)
  + 数据集说明：书生·万卷1.0为书生·万卷多模态语言料库的首个开源版本，包含文本数据集、图文数据集、视频数据集三部分，数据概述超过2TB。目前，书生·万卷万卷1.0已被评估书生·多模态、书生·浦语的训练。通过对高质量语料的“消化”，书生系列模型在语义理解、知识问答、理解、视觉问答等群体生成式中任务表现出的优异性能。

#### SFT数据集

+ **RefGPT：基于RefGPT生成大量真实和定制的对话数据集**
  + 地址：[https://github.com/DA-southampton/RedGPT](https://github.com/DA-southampton/RedGPT)
  + 数据集说明：包括RefGPT-Fact和RefGPT-Code两部分，其中RefGPT-Fact给出了5万中文的关于事实性知识的多轮对话，RefGPT-Code给出了3.9万中文编程相关的多轮对话数据。

+ **科伊格**
  + 地址：[https://huggingface.co/datasets/BAAI/COIG](https://huggingface.co/datasets/BAAI/COIG)
  + 数据集说明：维护了一套无害、有用且实用的中文处方语料库，包括一个人工验证翻译的通用处方语料库、一个人工标签的考试处方语料库、一个人类价值标签语料库、一个多轮反事实修改聊天语料库和leetcode指令语料库。

+ **generated_chat_0.4M**
  + 地址：[https://huggingface.co/datasets/BelleGroup/generated_chat_0.4M](https://huggingface.co/datasets/BelleGroup/generated_chat_0.4M)
  + 数据主题集说明：包含约40万条由BELLE项目生成的个性化角色对话数据，包含角色介绍。但该数据集是由ChatGPT产生的，未经过严格验证，或解题过程可能包含错误。

+ **alpaca_chinese_dataset**
  + 地址：[https://github.com/hikariming/alpaca_chinese_dataset](https://github.com/hikariming/alpaca_chinese_dataset)
  + 数据集说明：根据斯坦福的开源alpaca数据集进行中文翻译，并再制造一些对话数据

+ **羊驼毛CoT**
  + 地址：[https://github.com/PhoebusSi/Alpaca-CoT](https://github.com/PhoebusSi/Alpaca-CoT)
  + 数据集说明：统一了丰富的IFT数据（如CoT数据，目前仍在不断补充）、多种训练效率方法（如lora，p-tuning）以及多种LLM，三个层面上的接口，打造方便研究人员上手的LLM-IFT研究平台。

+ **线索**
  + 地址：[https://github.com/CLUEbenchmark/pCLUE](https://github.com/CLUEbenchmark/pCLUE)
  + 数据集说明：基于提示的大规模预训练数据集，用于多任务学习和零样本学习。包括120万数据，73个提示，9个任务。

+ **萤火虫列车-1.1M**
  + 地址：[https://huggingface.co/datasets/YeungNLP/firefly-train-1.1M](https://huggingface.co/datasets/YeungNLP/firefly-train-1.1M)
  + 数据集说明：23个常见的中文数据集，对于每个任务，由人工编写几种指令模板，保证数据的质量和丰富度，数据量为115万

+ **百丽-data-1.5M**
  + 地址：[https://github.com/LianjiaTech/BELLE/tree/main/data/1.5M](https://github.com/LianjiaTech/BELLE/tree/main/data/1.5M)
  + 数据集说明：通过自行指令生成，使用了中文种子任务，以及openai的text-davinci-003接口，涉及175个种子任务

+ **中国科技文献数据集**
  + 地址：https://github.com/ydli-ai/csl
  + 数据集说明：中文科学文献数据集（CSL），包含396,209篇中文核心期刊论文元信息（标题、摘要、关键词、学科、门类）以及简单的提示

+ **中医对话资料**
  + 地址：[https://github.com/Toyhom/Chinese-medical-dialogue-data](https://github.com/Toyhom/Chinese-medical-dialogue-data)
  + 数据集说明：中文医疗对话数据集，包括：`<Andriatria_男科>` 94596个对`<IM_内科>`的问答 220606个对`<OAGD_妇产科>`的问答 183751个对`<Oncology_肿瘤科>`的问答 75553个问答对 `<Pediatric_儿科>` 101602个问答对 `<Surgical_外科>` 115991个问答对 总共 792099个问答对。

+ **华佗26M**
  + 地址：[https://github.com/FreedomIntelligence/Huatuo-26M](https://github.com/FreedomIntelligence/Huatuo-26M)
  + 数据集说明：Huatuo-26M是一个中文医疗问答数据集，该数据集包含超过2600个高质量的医疗问答对，涵盖了各种疾病、症状、治疗方式、药品信息等多个方面。 -26M是研究人员、开发人员和企业为了提高医疗领域的人工智能应用，如聊天机器人、智能诊断系统等需要的重要资源。

+ **羊驼毛-GPT-4**
  + 地址：[https://github.com/Instruction-Tuning-with-GPT-4/GPT-4-LLM](https://github.com/Instruction-Tuning-with-GPT-4/GPT-4-LLM)
  + 数据集说明：Alpaca-GPT-4 是一个使用自指令技术，基于 175 条中文种子任务和 GPT-4 生成接口的 50K 的指令配置数据集。

+ **指令狂野**
  + 地址：[https://github.com/XueFuzhao/InstructionWild](https://github.com/XueFuzhao/InstructionWild)
  + 数据集说明：InstructionWild 是一个从网络上收集自然指令并过滤之后使用自然指令结合 ChatGPT 接口生成指令差异数据集的项目。主要指令来源：Twitter、CookUp.AI、Github 和 Discard。

+ **分享聊天**
  + 地址：https://paratranz.cn/projects/6725
  + 数据集说明：一个建议大家一起翻译高质量ShareGPT数据的项目。
  + 项目介绍：清洗/构造/翻译中文的ChatGPT数据，推进国内人工智能的发展，人人可炼化优质中文聊天模型。本数据集为ChatGPT约九万个对话数据，由ShareGPT API获得（中文68000，中文11000）条，其他各国语言）。项目所有数据最终将以CC0协议并入多语言共享GPT语料库。



- **原驼**
  - 地址：[https://huggingface.co/datasets/JosephusCheung/GuanacoDataset](https://huggingface.co/datasets/JosephusCheung/GuanacoDataset)
  - 数据集说明：一个使用Self-Instruct的主要包含中日英德的多语言指令配置数据集。

- **chatgpt语料库**
  - 地址：[https://github.com/PlexPt/chatgpt-corpus](https://github.com/PlexPt/chatgpt-corpus)
  - 数据集说明：开源了由ChatGPT3.5生成的300万自问自答数据，包括多个领域，可用于训练大模型。

- **微笑转换**
  - 地址：[https://github.com/qiuhuachuan/smile](https://github.com/qiuhuachuan/smile)
  - 数据集说明：数据集通过ChatGPT改写真实的心理互助QA为多轮的心理健康支持多轮对话（通过ChatGPT单轮到多轮包容性语言扩展），该数据集含有56k个多轮对话，其对话主题、词汇和文章内容更加丰富多样，更加符合长程多轮对话的应用场景。

## 偏好数据集

- **C值**
  - 地址：[https://github.com/X-PLUG/CValues](https://github.com/X-PLUG/CValues)
  - 数据集说明：该项目开源规模为145k的价值横向数据集，该数据集每个提示包括拒绝&正向建议（安全与责任）>拒绝主导（安全）>风险回复（不安全）透明类型，可用于增强SFT模型的安全性或用于训练奖励模型。

- **GPT-4-法学硕士**
  - 地址：[https://github.com/Instruction-Tuning-with-GPT-4/GPT-4-LLM](https://github.com/Instruction-Tuning-with-GPT-4/GPT-4-LLM)
  - 数据集说明：该项目提供了由GPT4生成的多种数据集，包括通过GPT4生成的中英PPO数据，可以用于奖励模型的训练。

- **`zhihu_rlhf_3k`**
  - 地址：[https://huggingface.co/datasets/liyucheng/zhihu_rlhf_3k](https://huggingface.co/datasets/liyucheng/zhihu_rlhf_3k)
  - 开源项目说明：该项目提供了 3k+ 条基于知乎问答的人类偏好数据集，每个实际的知乎问题下给出了一系列数据优先（选择）和较低（拒绝）的答案，可以用于奖励模型的训练。

- **`hh_rlhf_cn`**
  - 地址：[https://huggingface.co/datasets/dikw/hh_rlhf_cn](https://huggingface.co/datasets/dikw/hh_rlhf_cn)
  - 数据集说明：基于Anthropic论文Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback开源的有用和无害的数据，使用翻译工具进行了翻译。

- **聊天机器人竞技场对话**
  - 地址：[https://huggingface.co/datasets/lmsys/chatbot_arena_conversations](https://huggingface.co/datasets/lmsys/chatbot_arena_conversations)
  - 数据集说明：该偏好数据集包含20个LLM的输出，其中包括GPT-4和Claude-v1等更强的LLM，它还包含这些最先进模型的许多失败案例。包含来自超过13K个用户的无限制对话。

- **超反馈**
  - 地址：[https://github.com/OpenBMB/UltraFeedback](https://github.com/OpenBMB/UltraFeedback)
  - 数据集说明：该数据集是一个大规模、细粒度、膳食的偏好数据集，用于训练强大的奖励模型和批评者模型。该数据集来自各种资源（包括UltraChat、ShareGPT、Evol-Instruct、 TruthfulQA、FalseQA和FLAN，数据集统计数据请参见此处）中收集了约64k条提示。然后使用这些提示来查询多个LLM（模型列表请参见此处），并为每个提示生成4个不同的的回复，从而得到总共256k个样本。

## LLM训练负载框架

- **深速聊天**
  - 地址：[https://github.com/microsoft/DeepSpeed/tree/master/blogs/deepspeed-chat](https://github.com/microsoft/DeepSpeed/tree/master/blogs/deepspeed-chat)
  - 简介：该项目提供了按键式RLHF训练框架，只需一个脚本即可实现多个步骤，包括SFT，奖励模型增强和基于人类反馈的强化学习（RLHF），此外还实现了DeepSpeed HE，统一高效混合引擎，达到训练和推理引擎之间的衔接是无缝的。

- **LLaMA高效调优**
  - 地址：[https://github.com/hiyouga/LLaMA-Efficient-Tuning](https://github.com/hiyouga/LLaMA-Efficient-Tuning)
  - 简介：该项目提供了易于使用的基于PEFT的LLaMA配置框架，实现了包括全参数，LoRA，QLoRA等的预训练，指令配置和RLHF，并支持LLaMA, BLOOM, Falcon, Baichuan, InternLM等基础模型。

- **ChatGLM高效调优**
  - 地址：[https://github.com/hiyouga/ChatGLM-Efficient-Tuning](https://github.com/hiyouga/ChatGLM-Efficient-Tuning)
  - 简介：该项目提供了基于PEFT的ChatGLM配置，支持高效LoRA，P-Tuning V2，全参数配置等模式，并配置了多个参数数据集。

- **`bert4torch`**
  - 地址：[https://github.com/Tongjilibo/bert4torch](https://github.com/Tongjilibo/bert4torch)
  - 简介：该项目提供了一个大模型的训练和部署框架，包含了目前主要的开源大模型，llama 系列，chatglm，bloom 系列等，同时还给出了预训练和可怕的示例。

## LLM推理部署框架

- **法学硕士**
  - 地址：[https://github.com/vllm-project/vllm](https://github.com/vllm-project/vllm)
  - 简介：适用于大批量提示输入，视觉推理速度要求高的场景。吞吐量比HuggingFace Transformers高14x-24倍，比HuggingFace Text Generation Inference（TGI）高2.2x-2.5倍，实现了Continuation batching和PagedAttention但该框架对闹钟（LoRA、QLoRA 等）的支持并不重要且重量权重化。

- **`DeepSpeed-MII`**
  - 地址：[https://github.com/microsoft/DeepSpeed-MII](https://github.com/microsoft/DeepSpeed-MII)
  - 简介：支持多个机器之间的负载均衡，支持不同的模型库（如Hugging Face、FairSeq等），支持模型量化推理。

- **文本生成推理**
  - 地址：[https://github.com/huggingface/text-generation-inference](https://github.com/huggingface/text-generation-inference)
  - 简介：用于文本生成推断的 Rust、Python 和 gRPC 配置框架，可以监控服务器负载，实现了闪存注意力和分页注意力，所有依赖项都安装在 Docker 中：支持 HuggingFace 模型；但该框架对硬件（LoRA） 、QLoRA等）的支持不友好。

- **C翻译2**
  - 地址：[https://github.com/OpenNMT/CTranslate2](https://github.com/OpenNMT/CTranslate2)
  - 简介：基于C++和python的推理框架，支持在CPU和GPU上的任务和异步执行，且支持提示存储及量化。但缺少对闹钟（LoRA、QLoRA等）的支持。

- **开放法学硕士**
  - 地址：[https://github.com/bentoml/OpenLLM](https://github.com/bentoml/OpenLLM)
  - 简介：支持将要部署的LLM连接多个时钟，可以实现仅使用一个支撑模型来执行多个特定的任务；支持量化推理和LangChain。但对批量处理和循环推理的支持相对不一致。

- **法学硕士**
  - 地址：[https://github.com/mlc-ai/mlc-llm](https://github.com/mlc-ai/mlc-llm)
  - 简介：支持不同平台上的不同设备部署推理，包括移动设备（iOS或Android设备等）的高效推理，压缩等。但对大规模批量调用相对不统一。

- **光法学硕士**
  - 地址：[https://github.com/ModelTC/lightllm](https://github.com/ModelTC/lightllm)
  - 简介：一个基于Python的LLM（大型语言模型）推理和服务框架，该框架采用轻量级设计、易于扩展和高速性能，LightLLM引入了一种更细粒度的kv缓存管理算法TokenAttention，并设计了一个与TokenAttention高效配合的高效路由器调度实现。在TokenAttention和高效路由器的应答下，LightLLM在大部分场景下都可以获得比vLLM和文本生成推理得到更高的吞吐，部分场景下可以得到4倍左右的性能提升。

- **航空法学硕士**
  - 地址：[https://github.com/lyogavin/Anima/tree/main/air_llm](https://github.com/lyogavin/Anima/tree/main/air_llm)
  - 简介：该项目提供了一个优化推理开源内存的推理框架，可实现4GB单卡GPU可以运行70B大语言模型推理。不需要任何损失模型性能的调整和调整，剪枝等模型压缩，该项目采用了分层推理的技术以在较低的内存下实现大模型推理。

- **LM部署**
  - 地址：[https://github.com/InternLM/lmdeploy](https://github.com/InternLM/lmdeploy)
  - 简介：该项目支持 LLM（大视觉模型）和 VL（视觉语言模型）任务在 NVIDIA 设备上进行仿真、推理和服务。LMDeploy 支持有状态的推理，可以存储对话，记住历史。它实现了持久批量（即 Continuous Batch)，Blocked K/V Cache，动态拆分和高效融合，张量，以上的计算内核等重要特性。 推理性能为 vLLM 的 1.8 倍以上。 其 4bit 量化模型推理性能达到 FP16 的 2.4 倍以上。

## 6. 法学硕士

- **FlagEval （天秤）大型足球体系及开放平台**
  - 地址：[https://github.com/FlagOpen/FlagEval](https://github.com/FlagOpen/FlagEval)
  - 简介：旨在建立科学、公正、开放的体育训练基准、方法、工具集，协助研究人员全面评估基础模型及算法的绩效，同时探索利用人工智能方法实现对体育的辅助，大幅提升体育的效率FlagEval （天秤）创新构建了“能力-任务-指标”三维三维框架，细粒度刻画基础模型的认知能力边界，可视化呈现三维维度结果。

- **`C-Eval`：构造中文大模型的知识评估基准**
  - 地址：[https://github.com/SJTU-LIT/ceval](https://github.com/SJTU-LIT/ceval)
  - 简介：构建了一个覆盖人文、社科、理工、其他专业四个大方向，52个学科（微积分，线代……），总计获得大学研究生以及职业考试，总共13948道题目的中文知识和推理类型测试集。此外还给出了当前主流中文LLM的得分结果。

- **打开指南针**
  - 地址：[https://github.com/InternLM/opencompass](https://github.com/InternLM/opencompass)
  - 简介：由上海AI实验室发布的面向大模型维度的一站式平台。主要特点包括：开源可复现；全面的能力维度：五个大维度设计，提供50+个数据集约30万题的模型维度方案；丰富的模型支持：已支持20+ HuggingFace 及 API 模型；高效篮球：一行命令实现任务分割和一个足球，数小时即可完成千亿模型全量足球；篮球篮球范式：支持零样本、小样本及思维链游戏，结合标准型或对话型提示词模板；灵活化拓展。

- **`SuperCLUElyb`: SuperCLUE琅琊榜**
  - 地址：[https://github.com/CLUEbenchmark/SuperCLUElyb](https://github.com/CLUEbenchmark/SuperCLUElyb)
  - 简介：中文通用大模型对战评价基准，这是一个中文通用大模型对战评价基准，它以大众包的方式提供匿名、随机的对战。他们发布了初步的结果并基于Elo评级系统的排行榜。

- **高考-长凳**
  - 地址：[https://github.com/OpenLMLab/GAOKAO-Bench](https://github.com/OpenLMLab/GAOKAO-Bench)
  - 题目简介：GAOKAO-bench是一个以中国高考为数据集，测评模型理解语言能力、逻辑推理能力的测评框架，收集了2010-2022年全国高考卷的题目，其中包括1781道调查题和1030道优势问题，构建起 GAOKAO-bench 的数据部分。

- **AGIE 值**
  - 地址：[https://github.com/ruixiangcui/AGIEval](https://github.com/ruixiangcui/AGIEval)
  - 简介：由微软发布的一项新型基准测试，该基准大约有20种针对普通人类考生的官方、公开、高标准往常和资格考试，包括普通大学入学考试（中国高考和美国SAT考试）、法学入学考试、数学竞赛、律师资格考试、国家公务员考试等等。

- **谢志**
  - 地址：[https://github.com/mikegu721/xiezhibenchmark](https://github.com/mikegu721/xiezhibenchmark)
  - 简介：由复旦大学发布的一个综合的、多学科的、能够自动更新的领域知识评估基准，包含了哲学、经济学、法学、教育学、文学、历史学、自然科学、工学、农学、医学、军事学、管理学、艺术学这13个门类，24万道学科题目，516个具体学科题目，249587道题目。

- **打开LLM排行榜**
  - 地址：[https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)
  - 简介：由HuggingFace组织的一个LLM排名，目前已评估了热点主流评估的开源LLM模型。主要包括AI2 Reasoning Challenge、HellaSwag、MMLU、TruthfulQA四个数据集上的表现，主要以中文为主。

- **`CMLU`**
  - 地址：[https://github.com/haonan-li/CMMLU](https://github.com/haonan-li/CMMLU)
  - 简介：CMMLU是一个综合性的汉语评估基准，专门用于评估语言模型在汉语背景下的知识和推理能力。CMMLU讲座涵盖了从基础学科到高级专业水平的67个主题。它包括：需要计算和自然科学的推理，需要人文科学和社会科学的知识，以及需要生活常识的中国驾驶规则等。此外，CMMLU中的许多任务具有中国特定的答案，可能在其他地区或语言中并不普遍适用。是一个完全中国化的中文测试基准。

- **`MMCU`**
  - 地址：[https://github.com/Felixgithub2017/MMCU](https://github.com/Felixgithub2017/MMCU)
  - 简介：该项目提供对中文大模型评分训练能力的测试，体育方式、体育数据集、体育记录全部公开，确保可以复现。该项目旨在帮助各位大家计量自己的模型表现，并验证策略是否有效。

- **chinese-llm-基准**
  - 地址：[https://github.com/jeinlee1991/chinese-llm-benchmark](https://github.com/jeinlee1991/chinese-llm-benchmark)
  - 简介：中文大模型能力排行榜：覆盖百度文心一言、chatgpt、阿里通义千问、讯飞星火、belle / chatglm6b等开源大模型，多维度能力排行榜。不仅提供能力评分排行榜，也提供所有模型的原始输出结果！

- **安全提示**
  - 地址：[https://github.com/thu-coai/Safety-Prompts](https://github.com/thu-coai/Safety-Prompts)
  - 简介：由清华大学提出了一个关于法学硕士安全足球的基准，包括安全足球平台等，用于体育和提升大模型的安全性，囊括了多种典型的安全场景和指令攻击的提示。

- **`PromptCBLUE`：中文医疗场景的LLM学业基准**
  - 地址：[https://github.com/michael-wzhu/PromptCBLUE](https://github.com/michael-wzhu/PromptCBLUE)



- 简介：为推动LLM在医疗领域的发展和落地，由华东范大学联合阿里巴巴天池平台、复旦大学附属华山医院、东北大学、哈尔滨工业大学（深圳）、鹏城实验室与同济大学推出师PromptCBLUE评价基准，将16种不同的医疗场景NLP任务全部转化为基于提示的语言生成任务，形成首个中文医疗场景的LLM评测基准。

- HalluQA：中文幻觉评估基准
    - 地址：[https://github.com/xiami2019/HalluQA](https://github.com/xiami2019/HalluQA)
    - 简介：该项目提出了一个名为HalluQA的基准测试，用于简化中文大型语言模型中的幻觉现象。HalluQA包含450个提出设计的对抗性问题，涵盖多个领域，并考虑了中国历史文化、风俗在构建 HalluQA 时，考虑了两种幻觉：模仿性设想类型和事实错误，并基于 GLM-130B 和 ChatGPT 构建对抗性样本。为了评估，设计了一种使用 GPT-4 进行自动评估的方法，判断模型输出是否是幻觉。

## 7. 法学硕士教程

### 法学硕士基础知识

- 拥抱法学硕士：
    - 地址：[https://github.com/datawhalechina/hugging-llm](https://github.com/datawhalechina/hugging-llm)
    - 简介：介绍ChatGPT原理、使用和应用，降低使用价值，让更多感兴趣的非NLP或算法专业人士能够无法使用LLM创造。

- 法学硕士实用指南：
    - 地址：[https://github.com/Mooler0410/LLMsPracticalGuide](https://github.com/Mooler0410/LLMsPracticalGuide)
    - 简介：该项目提供了关于LLM的一系列指南和资源精选列表，包括LLM发展历程、原理、成果、论文等。

### 提示工程教程

- 面向开发者的LLM入门课程：
    - 地址：[https://github.com/datawhalechina/prompt-engineering-for-developers](https://github.com/datawhalechina/prompt-engineering-for-developers)
    - 简介：一个中文版的大模型入门教程，围绕吴恩达老师的大模型系列课程展开，主要包括：吴恩达《ChatGPT Prompt Engineering for Developers》中文版课程，吴恩达《使用 ChatGPT API 构建系统》中文版课程，吴恩达《LangChain法学硕士申请开发》课程中文版等。

- 提示工程指南：
    - 地址：https://www.promptingguide.ai/zh
    - 简介：该项目基于大语言模型的兴趣，编写了一份全新的提示工程指南，介绍了大语言模型的相关论文研究、学习指南、模型、讲座、参考资料、大语言模型能力以及其他与提示工程相关的工具。

- Awesome-chatgpt-prompts-zh：
    - 地址：[https://github.com/PlexPt/awesome-chatgpt-prompts-zh](https://github.com/PlexPt/awesome-chatgpt-prompts-zh)
    - 简介：该项目是ChatGPT中文调教指南。包括各种场景使用指南，让chatgpt知道怎么听你的话，对指令构造可以提供一些参考。

### LLM应用教程

- LangChain 🦜️🔗中文网，关注LangChain一起学LLM/GPT开发：
    - 地址：https://www.langchain.asia
    - 简介：Langchain 的中文文档，由两位 LLM 创业者编写，希望对刚刚进入 AI 应用开发的朋友们有所帮助。

- OpenAI食谱：
    - 地址：[https://github.com/openai/openai-cookbook](https://github.com/openai/openai-cookbook)
    - 简介：该项目是 OpenAI 提供的使用 OpenAI API 的示例和指南，其中包括如何构建问答机器人等教程，能够为行业人员开发类似的应用时带来指导。

- 构筑大语言模型应用：应用开发与架构设计：
    - 地址：https://github.com/phodal/aigc
    - 简介：该项目了一本关于LLM在真实世界应用的开源电子书，介绍了大语言模型的基础知识和应用，以及如何构建自己的模型。其中包括提示的编写、开发和管理，探索最好的大语言模型能够带来什么，以及LLM应用开发的模式和架构设计。

### LLM实战教程

- LLMs九层妖塔：
    - 地址：[https://github.com/km1994/LLMsNineStoryDemonTower](https://github.com/km1994/LLMsNineStoryDemonTower)
    - 简介：ChatGLM、Chinese-LLaMA-Alpaca、MiniGPT-4、FastChat、LLaMA、gpt4all等实战与经验。

- LLM-动作：
    - 地址：[https://github.com/liguodongiot/llm-action](https://github.com/liguodongiot/llm-action)
    - 简介：该项目提供了一系列LLM实战的教程和代码，包括LLM的训练、推理、姿势以及LLM生态相关的一些技术文章等。

- llm大模型训练专栏：
    - 地址：[https://www.zhihu.com/column/c_1252604770952642560](https://www.zhihu.com/column/c_1252604770952642560)
    - 分析简介：该项目提供了一系列法学硕士前言理论和实战实验，包括论文解读与分析。

- 书生·浦语大模型实战营：
    - 地址：[https://github.com/InternLM/tutorial](https://github.com/InternLM/tutorial)
    - 简介：该课程由上海人工智能实验室重磅推出。课程包括大模型配置、配置与营养全仓库，目的是为广大开发者搭建大模型学习和实践开发的平台。

## 8. 相关仓库

- 找到ChatGPer：
    - 地址：[https://github.com/chenking2020/FindTheChatGPTer](https://github.com/chenking2020/FindTheChatGPTer)
    - 简介：ChatGPT爆火，开启了通往AGI的关键一步，本项目旨在汇总一些ChatGPT的开源平替代者，包括文本大模型、多模态大模型等，为大家提供便利。

- LLM_审稿人：
    - 地址：[https://github.com/SpartanBin/LLM_reviewer](https://github.com/SpartanBin/LLM_reviewer)
    - 简介：总结近期井喷式发展的大语言模型，以开源、规模缩小、可试点化部署、训练成本降低的“小羊驼类”模型为主。

- Awesome-AITools：
    - 地址：[https://github.com/ikaijua/Awesome-AITools](https://github.com/ikaijua/Awesome-AITools)
    - 简介：收藏整理了AI相关的实用工具、体育和相关文章。

- 开源ChatGPT及其他：
    - 地址：[https://github.com/SunLemuria/open_source_chatgpt_list](https://github.com/SunLemuria/open_source_chatgpt_list)
    - 简介：本仓库旨在记录开源ChatGPT，并提供如何参与的概述，包括：基础模型、技术、数据、领域模型、训练管道、加速技术、多语言、多模式等去。

- 太棒了，完全开放的Chatgpt：
    - 地址：[https://github.com/nichtdax/awesome-totally-open-chatgpt](https://github.com/nichtdax/awesome-totally-open-chatgpt)
    - 简介：这个仓库记录了 ChatGPT 完全开放的替代品列表。

- 太棒了-LLM：
    - 地址：[https://github.com/Hannibal046/Awesome-LLM](https://github.com/Hannibal046/Awesome-LLM)
    - 简介：该存储库是有关大型语言模型的精选论文列表，特别是与 ChatGPT 相关的论文。它还包含 LLM 培训框架、部署 LLM 的工具、有关 LLM 的课程和教程以及所有公开可用的 LLM 检查点和 API。

- 解密提示：
    - 地址：[https://github.com/DSXiangLi/DecryptPrompt](https://github.com/DSXiangLi/DecryptPrompt)
    - 简介：总结了Prompt&LLM论文，开源数据&模型，AIGC应用。

- 很棒的预训练中文 NLP 模型：
    - 地址：[https://github.com/lonePatient/awesome-pretrained-chinese-nlp-models](https://github.com/lonePatient/awesome-pretrained-chinese-nlp-models)
    - 简介：收集了目前网上公开的一些高质量中文预训练模型。

- 聊天貔貅：
    - 地址：https://github.com/catqaq/ChatPiXiu
    - 简介：该项目旨在打造全面实用的ChatGPT模型库和文档库。当前V1版本整理了包括：相关资料调研+通用最小化实现+领域/任务模型等。

- 法学硕士-动物园：
    - 地址：[https://github.com/DAMO-NLP-SG/LLM-Zoo](https://github.com/DAMO-NLP-SG/LLM-Zoo)
    - 简介：该项目收集了包括开源和闭源的LLM模型，具体包括发布时间、模型大小、支持的语种、领域、数据训练及相应论文/仓库等。

- 中国法学硕士：
    - 地址：[https://github.com/wgwang/LLMs-In-China](https://github.com/wgwang/LLMs-In-China)
    - 简介：该项目旨在记录中国大模型发展情况，同时持续深度分析开源开放的大模型以及数据集的情况。

- BM列表：
    - 地址：https://github.com/OpenBMB/BMList
    - 简介：该项目收集了超过10亿的参数量，并整理了各个参数量的适用模式、发布的机构、适合的语种、参数量和开源地址、API等信息。

- Awesome-free-chatgpt：
    - 地址：[https://github.com/LiLittleCat/awesome-free-chatgpt](https://github.com/LiLittleCat/awesome-free-chatgpt)
    - 简介：该项目收集了免费的 ChatGPT 镜像网站列表，ChatGPT 的替代方案，以及构建自己的 ChatGPT 的教程工具等。

- Awesome-Domain-LLM：
    - 地址：[https://github.com/luban-agi/Awesome-Domain-LLM](https://github.com/luban-agi/Awesome-Domain-LLM)
    - 简介：该项目收集和整理垂直领域的开源模型、数据集及开源模型基准。

<https://aiyanxishe.feishu.cn/sync/OtQ2dguH7slySbbLhuucEV6inQi>

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
