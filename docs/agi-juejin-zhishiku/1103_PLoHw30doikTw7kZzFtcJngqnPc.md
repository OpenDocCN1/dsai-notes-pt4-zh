
# LLM大语言模型能力与开源框架选型

> 来源：[https://agijuejin.feishu.cn/docx/HezLdjx8Co58UIx79JScc0m2nFg](https://agijuejin.feishu.cn/docx/HezLdjx8Co58UIx79JScc0m2nFg)

> 作者：爱吃牛油果的璐璐

> 原文：https://zhuanlan.zhihu.com/p/652600621

## 大语言模型能力

LM的核心能力大致分为：生成（Generate）、总结（Summarize）、提取（Extract）、分类（Classify）、检索（Search）与改写（Rewrite）六部分。如果你对大语言模型已经有一定了解，可以看以下这篇文章，结合`LangChain`让你的LLM变得更强大：爱吃牛油果的璐璐：（万字长文）手把手教你认识学会LangChain

### 1、生成能力（Generate）

文本生成是给定输入与上下文生成新文本。生成能力是LLM最核心的能力。其能力的建立来源于对大量的文本进行训练，并捕捉了语言的内在联系与人类的使用模式。

【应用】：聊天助手、写作助手、知识问答助手、创意文案生成。

### 2、总结能力（Summarize）

总结是LLM的重要能力。通过Prompt Engineering，LLM可对用户输入的文本提炼总结。在工作中我们每天会处理大量会议、报告、文章、邮件等文本内容，LLM总结能力有助于快速获取关键信息，提升工作效率。

【应用】：在线视频会议、电话会议内容总结；私有化知识库文档总结；报告、文章、邮件等工作性文本总结。在推荐领域，还可以结合用户的历史数据、报表数据、case排查数据，生成总结文案和优化建议。

### 3、提取能力（Extract）

提取文本信息是NLP中常见需求。LLM有时可以提取比传统NLP方法更难提取的实体。利用LLM提取文本中的时间、地点、人物等信息，旨在将文本关键信息进行结构化表示。除此之外，还可用于提取摘录合同、法律条款中的关键信息。

【应用】：文档命名实体提取、文章关键词提取、视频标签生成。

### 4、分类能力（Classify）

大模型对文本内容分类的优势在于强语义理解能力与小样本学习能力。也就是说其不需要样本或需要少量样本学习即可具备强文本分类能力。而这与通过大量语料训练的垂域模型相比，在开发成本与性能上更具优势。比如，互联网社交媒体每天产生大量文本数据，商家通过分析文本数据评估对于公众对于产品的反馈，政府通过分析平台数据评估公众对于政策、事件的态度。与小模型相比，大模型在开发周期、模型性能更具优势。阿里巴巴达摩院&南洋理工&港中文的一篇验证性文章《Sentiment Analysis in the Era of Large Language Models: A Reality Check 》，也验证了大模型在文本情感分析中相对于小模型的优势。

总结起来大模型优势在于：仅通过few-shot学习可超越传统垂直领域模型能力。

【应用】：网络平台敏感内容审核（例如广告素材审核），社交媒体评论情感分析，电商平台用户评价分类。

### 5、检索能力（Search）

文本检索是根据给定文本在目标文档中检索出相似文本。最常用的是搜索引擎，搜索引擎根据输入返回高度相关的内容或链接。而传统方式采用关键词匹配，只有全部或部分关键词在检索文档中命中返回目标文档。这对于检索质量是不利的，原因是对于关键词未匹配但语义高度相关的内容没有召回。在检索应用中，LLM的优势在于能够实现语义级别匹配。LLM语义检索可弥补传统关键词匹配检索不足，在本地知识库与搜索引擎中的语义搜文、以文搜图中存在应用价值。

【应用】：文本语义检索、图片语义检索、视频语义检索；电商产品语义检索；招聘简历语义检索。

### 6、改写能力（Rewrite）

通过LLM对输入文本按照要求进行润色、纠错。通过LLM与提示词工程（Prompt Engineering）自动对文本内容纠错。此外，还可用其对文章进行润色处理，使文章在表述上更加清晰流畅。

【应用】：文本纠错、文本润色、文本翻译。

## 开源框架选型

既然已经有了成功ChatGPT这一成功的案例，大家都想基于LLaMA把这条路再走一遍，以期望做出自己的ChatGPT。对于开源社区来说，LLaMA2是一举成为"全村的希望"。它的出现将进一步缩小开源和闭源大型语言模型之间的差距，使每个人都有机会基于它构建自己的大型模型应用。

![](img/434eb93cac2503c6e3845421130c5672.png)

ChatGPT演化的路径可以归纳为三个阶段：Stage 1: 预训练(Pretrain)；Stage 2: 监督微调(SFT)；Stage 3: 对齐(Reward Model + RLHF)，如果说GPT系列是苹果手机，那么LLaMA就是安卓。基于LLaMA的模型虽然很多，但是基本都可以放到上面3个框架当中。

### Stage1 预训练

1、 LLaMA 复现

RedPajama 参考LLaMA论文中的训练数据，收集并且开源可商用。

https://github.com/togethercomputer/RedPajama-Data

2、Baichuan-7B(支持中文)

采用LLaMA的相同架构，在中文上做预训练。可商用。目前Baichuan可以算是第一个LLaMA中文预训练模型，所以后面的工作都可以在这上面都走一遍，估计没多久Baichuan-Alapca, Baichuan-Vicuna就都出来了。https://github.com/baichuan-inc/baichuan-7B

3、OpenLLaMA

参考LLaMA的代码，在Apache 2.0 license下的重新实现和训练。使用了RedPajama训练集合。https://github.com/openlm-research/open_llama

4、Lit-LLaMA

参考LLaMA，在Apache 2.0 license下的只有代码的重新实现。同时支持加载原始LLaMA和OpenLLaMA的权重。https://github.com/Lightning-AI

### Stage2 监督微调

因为预训练模型本质上还是个续写模型，所以并不能很好的满足人们的需求，所以监督微调的作用就是微调模型产生理想的回复。有钱可以全参数微调，没钱就只能使用一些低成本的方法，英文叫`PEFT`(Parameter-Efficient Fine-Tuning)。`PEFT`确实是像我这种平民玩家的首选，但是有钱也可以用`PEFT`，它可以让你微调更大的模型。

#### （1）`LLaMA + Instruction Finetuning`(全量参数)

小羊驼Alpacal：`lama7b` + `self-instruct`数据指令微调。算是最早迈出`LLaMA+SFT`这一步的模型。Alpaca模型是基于LLaMA模型的微调版本，其主要特点是使用了层次化的结构，以及针对特定任务进行了优化。Alpaca模型在LLaMA模型的基础上，增加了更多的层次，从而能够更好地处理长序列。同时，Alpaca模型还引入了任务特定的层，能够针对不同的任务进行优化。这些优化使得Alpaca模型在多项NLP任务中取得了优异的性能。最开始并没有提供权重，后来通过`diff`的方式给出，需要LLaMA原始模型才能恢复，github上有教程。当时他们采用1张8卡A100(80G显存)，52k的数据，训练了3个小时。训练成本大概是100刀。https://github.com/tatsu-lab/st

Vicunal：`lama13b` + `ShareGPT`对话数据，微调研发团队基于Vicuna发布了`FastChat`对话机器人。和Alpaca一样，受协议限制，vicuna模型公布的权重也是个delta，每个参数要加上llama原来的权重才是模型权重。Build software better, together

WizardLM：采用了`Evol-Instruct`来构造指令，可以产生一些很难的指令.深度演化包括五种操作：添加约束、深化、具体化、增加推理步骤并使输入复杂化。In-breadth Evolving 是突变，即根据给定的指令生成全新的指令进化是通过提示+LLM来实现的。https://github.com/nlpxucan/Wiz

GPT4ALL：LLaMA用80w的GPT3.5的数据(code, story, conversation)微调而来。https://github.com/nomic-ai/gpt

Pygmalion 7B：基于LLaMA微调，使用了不同来源的56MB 的对话数据，包含了人工和机器。https://huggingface.co/Pygmalio

#### （2）LLaMA + `PEFT`（参数高效的Fine-tuning）

`PEFT`目前最流行的是`LoRA`，低秩自适应（`LoRA`）是一种参数有效的训练方法——保留预训练的模型权重，引入可训练的秩分解矩阵。这种方法显著减少了可训练参数的数量。`LoRA`的公式如下所示，`r`表示预先确定的秩，`d`是隐含层大小，`A`和`B`是可训练的分解矩阵：

`LoRA`挺巧妙的架构，可以看看 https://arxiv.org/abs/2106.09685 。AIGC的头部网站 http://civitai.com 上很多模型也都是基于`lora`的。最近还出了`QLoRA`，在`LoRA`的基础上加入了量化，进一步降低显存的使用。https://arxiv.org/abs/2305.14314。

Baize：`LLaMA + Lora`； https://github.com/project-baize/baize-chatbot

LLaMA-Adapter：LLaMA + Adapter Layer； https://github.com/OpenGVLab/LLaMA-Adapter

CalderaAI/30B-Lazarus：似乎是多个`LoRA`的merge，但是没太公布太多细节。在huggingface的leaderboard上排名还挺靠前。https://huggingface.co/CalderaAI/30B-Lazarus

[Chinese-LLaMA-Alpaca](https://arxiv.org/pdf/2304.08177.pdf)

Chinese-Vicuna(支持中文)：基于：https://github.com/tloen/alpaca

### Stage3 对齐(`LLaMA + FT + RHLF`)

这部分可以说是把ChatGPT的路径完整走了一遍。

StableVicuna：`Vicuna = LLaMA + FT`；`StableVicuna = Vicuna + RLHF`；https://github.com/Stability-AI/StableLM

StackLLaMA：`SFT: LLaMA + Lora`；`RM: LLaMA + Lora + 分类`；https://huggingface.co/blog/zh/stackllama

`llama.cpp`

+   用C/C++实现的推理，不依赖显卡。

+   https://github.com/ggerganov/llama.cpp

`GPTQ-for-LLaMA`

+   4 bits [quantization](https://www.zhihu.com/search?q=quantization&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A3091336166%7D) of LLaMA using GPTQ.

+   https://github.com/qwopqwop200/

### 选型的指标和维度

先来介绍几个衡量大模型综合能力的指标：MMLU、C Eval、AGI Eval、GSM8K

MMLU - 一种针对大模型的语言理解能力的测评，是目前最著名的大模型语义理解测评之一，任务涵盖的知识很广泛，语言是英文，用以评测大模型基本的知识覆盖范围和理解能力。

C Eval - C-Eval 是一个全面的中文基础模型评估套件。它包含了13948个多项选择题，涵盖了52个不同的学科和四个难度级别。用以评测大模型中文理解能力。

AGI Eval - 微软发布的大模型基础能力评测基准，主要评测大模型在人类认知和解决问题的一般能力，涵盖全球20种面向普通人类考生的官方、公共和高标准录取和资格考试，包含中英文数据。

GSM8K - OpenAI发布的大模型数学推理能力评测基准，涵盖了8500个中学水平的高质量数学题数据集。数据集比之前的数学文字题数据集规模更大，语言更具多样性，题目也更具挑战性。

![](img/fae9f1ea0a4053e93e8d38e197b1cd84.png)

## 更多资料：

+   爱吃牛油果的璐璐：大模型中的思维链、思维树、思维图

+   爱吃牛油果的璐璐：大模型LLM推理框架整理

+   [大模型学习资料](https://www.zhihu.com/column/c_1710250717548453888)
