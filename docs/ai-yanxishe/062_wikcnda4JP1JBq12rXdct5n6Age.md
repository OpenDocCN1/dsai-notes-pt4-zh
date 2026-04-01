#  GPT 高级玩法

> 来源：[https://aiyanxishe.feishu.cn/docx/Vp3IdTbLYovYBKxEWhWcKmbUnRb](https://aiyanxishe.feishu.cn/docx/Vp3IdTbLYovYBKxEWhWcKmbUnRb)

OpenAI 的 GPT 大型语言模型（LLM）并未开源，这部分收录一些深度学习开源的 LLM 供感兴趣的同学学习参考。

### [开源LLMs 收集：🤖 LLMs: aweso](https://github.com/nichtdax/awesome-totally-open-chatgpt)me-totally-open-GPT

### [能在本地运行的资源 LLMs 收集: aweso](https://github.com/imaurer/awesome-decentralized-llm)me-decentralized-llm

### [minGPT](https://github.com/karpathy/minGPT)

karpathy大神发布的一个 OpenAI GPT(生成预训练转换器)训练的最小 PyTorch 实现，代码十分简洁明了，适合用于动手学习 GPT 模型。

### [OpenChatKit](https://github.com/togethercomputer/OpenChatKit#pre-trained-weights)

GitHub:

> [h](https://github.com/togethercomputer/OpenChatKit#pre-trained-weights)ttps://github.com/togethercomputer/OpenChatKit#pre-trained-weights

开源了数据、模型和权重，以及提供训练，微调教程，下面是项目介绍

### 自称更亲民开放版的GPT 模型: dolly

Dolly 使用 Alpaca 数据，对两年前的开源EleutherAI 60亿参数模型进行微调，从而产生了在原模型中没有的理解和文本生成能力。

### 国产的支持中英双语的功能型对话语言大模型：ChatYuan

目前已经更新到 v2, ChatYuan-large-v2是一个支持中英双语的功能型对话语言大模型。ChatYuan-large-v2使用了和 v1版本相同的技术方案，在微调数据、人类反馈强化学习、思维链等方面进行了优化。

ChatYuan-large-v2是ChatYuan系列中以轻量化实现高质量效果的模型之一，用户可以在消费级显卡、 PC甚至手机上进行推理（INT4 最低只需 400M ）。

### [gpt4all](https://github.com/nomic-ai/gpt4all)

基于 LLaMa 的 LLM 助手，提供训练代码、数据和演示，训练一个自己的 AI 助手。

### [Stanford Alpaca](https://github.com/tatsu-lab/stanford_alpaca)

来自斯坦福，建立并共享一个遵循指令的LLaMA模型。

### [Alpaca-CoT](https://github.com/PhoebusSi/Alpaca-CoT/blob/main/CN_README.md)

Alpaca-CoT项目旨在探究如何更好地通过instruction-tuning的方式来诱导LLM具备类似GPT的交互和instruction-following能力。为此，我们广泛收集了不同类型的instruction（尤其是Chain-of-Thought数据集），并基于LLaMA给出了深入细致的实证研究，以供未来工作参考。作者声称这是首个将CoT拓展进Alpaca的工作，因此简称为"Alpaca-CoT"。

### 大型多模态模型训练和评估开源框架：OpenFlamingo

OpenFlamingo 是一个用于评估和训练大型多模态模型的开源框架，是 DeepMind Flamingo 模型的开源版本，也是 AI 世界关于大模型进展的一大步。

### [中文LLaMA&Alpaca大语言模型+本地部署: ](https://github.com/ymcui/Chinese-LLaMA-Alpaca)Chinese-LLaMA-Alpaca

项目开源了中文LLaMA模型和经过指令精调的Alpaca大模型。这些模型在原版LLaMA的基础上扩充了中文词表并使用了中文数据进行二次预训练，进一步提升了中文基础语义理解能力。同时，在中文LLaMA的基础上，本项目使用了中文指令数据进行指令精调，显著提升了模型对指令的理解和执行能力。

### [Visual OpenLLM](https://github.com/visual-openllm/visual-openllm)

一种基于开源模型, 已交互方式连接不同视觉模型的开源工具。

+   基于 ChatGLM + Visual GPT + Stable Diffusion

+   开源版的"文心一言"

### 高效微调一个聊天机器人：LLaMA-Adapter🚀

### [⚡ Lit-LLaMA](https://github.com/Lightning-AI/lit-llama)

Lightning-AI 基于nanoGPT的LLaMA语言模型的实现。支持量化，LoRA微调，预训练。

### [FastChat](https://github.com/lm-sys/FastChat)

继草泥马（Alpaca）后，斯坦福联手CMU、UC伯克利等机构的学者再次发布了130亿参数模型骆马（Vicuna），仅需300美元就能实现GPT 90%的性能。FastChat 是Vicuna 的GitHub 开源仓库。

### [LMFlow](https://github.com/OptimalScale/LMFlow)

共建大模型社区，让每个人都训得起大模型。

### GPT 控制所有AI模型: HuggingGPT

[GitHub](https://github.com/microsoft/JARVIS)

Arxiv 论文

大语言模型LLM在语言理解、生成、交互和推理方面的表现，让人想到：

> 可以将它们作为中间控制器，来管理现有的所有AI模型，通过“调动和组合每个人的力量”，来解决复杂的AI任务。

在这个系统中，语言是通用的接口。

于是，HuggingGPT就诞生了。

它的工程流程分为四步：

+   首先，任务规划。GPT将用户的需求解析为任务列表，并确定任务之间的执行顺序和资源依赖关系。

+   其次，模型选择。GPT根据HuggingFace上托管的各专家模型的描述，为任务分配合适的模型。

+   接着，任务执行。混合端点（包括本地推理和HuggingFace推理）上被选定的专家模型根据任务顺序和依赖关系执行分配的任务，并将执行信息和结果给到GPT。

+   最后，输出结果。由GPT总结各模型的执行过程日志和推理结果，给出最终的输出。

##

本文由[**AI研习社**](https://zl49so8lbq.feishu.cn/wiki/wikcnu3kSXr75E50w8PFefhJMYf)整理发布，AI研习社是一个AI行业研究专题知识兴趣小组，目前分享包含： AI工具使用、AI商业模式研究、AI行业研究报告、AI经验分享、大厂动态等专题内容，包含GPT、MidJourney等主流工具使用说明，通过知识手册的方式帮助大家构建系统化的AI知识库，让更多人可以轻松学习使用AI技术。

![](img/5aa260150e257f266d6a656ac807d5da.png)

扫描二维码，加入[**AI研习社**](https://zl49so8lbq.feishu.cn/wiki/wikcnu3kSXr75E50w8PFefhJMYf)** **