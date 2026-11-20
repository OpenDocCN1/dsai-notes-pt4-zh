
# 国产AI崛起

> 来源：[https://aiyanxishe.feishu.cn/docx/YPfXdJQHOo3autxuuZHcGXFSnGf](https://aiyanxishe.feishu.cn/docx/YPfXdJQHOo3autxuuZHcGXFSnGf)

> 随着生成式人工智能(AIGC)在 2022 年底席卷全球，让我们来看看这项爆炸性技术如何在中国掀起波澜。

## 专题：首批备案获批的八家大模型链接

阿里通义千问、360智脑、讯飞星火等并未在首批获批名单中

## 一口智库 百模争艳之闻名识模型

![](img/9b3694e14e0ccc0d4206a8b01eda30c0.png)

## MaaS Model as a Service 模型即服务的产业结构

![](img/fbbd7fda898d44d7f162d1f915248148.png)

[47页深度研报：揭秘ChatGPT身后的AIGC技术和它的中国同行们.pdf](https://docs.meta360.vip/server/index.php?s=/api/attachment/visitFile&sign=f7a7a6ef07ad87e2e5eb7a95d567c5f3)

## 国产AIGC大模型

### 傅盛的猎户星空发布了Orion-14B系列 LLM 模型

模型页面的测试挺全面的，主要特点有：

模型整体多语言能力强，比如日语和韩语。

支持超过 200k 的上下文。

量化版本模型大小缩小70%，推理速度提升30%，性能损失小于1%。

Orion-14B系列一共包含了 7 个不同的模型：

基座模型、对话模型、长上下文模型、RAG 模型、插件模型、两个量化模型。

模型介绍页面：[https://huggingface.co/OrionStarAI/Orion-14B-Chat-Int4/blob/main/README_zh.md](https://huggingface.co/OrionStarAI/Orion-14B-Chat-Int4/blob/main/README_zh.md)

### YAYI 2：雅意中文大模型

YAYI 2 是中科闻歌研发的新一代开源大语言模型，雅意开源了YAYI 2，30B大小的模型，评分跟李开复的YI-34B接近。官网直接标明是政企大模型，国内大模型想要挣钱，拿牌照、做政府和国企客户是比较好的路子了。

官网：yayi.wenge.com

YAYI 2 模型地址：[https://huggingface.co/wenge-research/yayi2-30b](https://huggingface.co/wenge-research/yayi2-30b)

Github：[https://github.com/wenge-research/YAYI2](https://github.com/wenge-research/YAYI2)

### ☕️ 快手自研文生图大模型“可图”（Kolors）开启内测，在AIGC领域再迎进展

+ 中国如何构建平行生成的人工智能世界

企业家、研究人员和投资者正在寻找在生成人工智能领域取得进展的方法。

科技公司正在设计基于开源模型的工具，以吸引消费者和企业客户。

个人正在从 AI 生成的内容中获利。

监管机构迅速做出反应，定义了文本、图像和视频合成的使用规则。

得益于像 Stable Diffusion 和 DALL-E 2 这样的病毒式艺术创作平台，生成式 AI 突然出现在每个人的嘴边。在地球的另一端，中国科技巨头也以其同类产品吸引了公众，为迎合中国的品味和政治气候增添了新意。

**国内LLM模型：**

### 书生(INTERN)大模型

上海人工智能实验室的这个7B 的多模态不错啊。

2021 年，商汤发布了书生(INTERN)大模型，拥有 100 亿的参数量，这是一个相当庞大的训练工作。在训练过程中，大概有 10 个以上的监督信号帮助模型，适配各种不同的视觉或者 NLP 任务，截至 2021 年中，商汤已建成世界上最大的计算器视觉模型，该模型拥有超过 300 亿个参数;

2024.1.18-书生·浦语发布 InternLM2 系列模型

### 书生·浦语发布 InternLM2 系列模型

包括 20B 和 7B 两个版本。200K 上下文，官方称 20B 模型与 Yi-34B 能力相当，7B 模型在 13B 大小内能力最佳。

Github：[https://github.com/InternLM/InternLM](https://github.com/InternLM/InternLM) 模型：[https://huggingface.co/internlm](https://huggingface.co/internlm)

视频：[https://twitter.com/i/status/1752525316940292217](https://twitter.com/i/status/1752525316940292217)

演示视频里这个根据输入图片和简短的文字生成图文混排长文章的演示看起来非常好。

[InternLM-XComposer2](https://github.com/InternLM/InternLM-XComposer/blob/main/README_CN.md)是一款前沿的视觉-语言模型，在自由组合文本和图像，以及理解这些内容方面表现卓越。这款模型的能力超越了传统的视觉-语言理解，能够巧妙地将多样化输入，如概要、详尽的文本描述和参考图片，融合成包含文本和图像的复合内容，实现高度定制化的创作。

InternLM-XComposer2引入了一种名为“部分LoRA（PLoRA）”的新方法。这种方法只对图像Token应用额外的LoRA参数，以此保持预先训练的语言知识的完整性。这样既能精确地理解视觉信息，又能保持文本内容的文学魅力，实现两者之间的完美平衡。

项目地址：https://github.com/InternLM/InternLM-XComposer/blob/main/README_CN.md

### 文心一言（ERNIE）

2023年3月，百度推出文心一言，国产版ChatGPT

2022年12月，百度推出 ERNIE 3.0 Titan 模型，参数规模达 2600 亿。

2021年7月，百度推出 ERNIE 3.0 知识增强大模型，参数规模达到百亿;

### 源

2021年10月，浪潮信息发布约 2500 亿的超大规模预训练模型“源 1.0”;

### BAGUALU

+ “脑级人工智能模型”八卦炉(BAGUALU) 2022 年，基于清华大学、阿里达摩院等研究成果以及超算基础实现的“脑级人工智能模型”八卦炉(BAGUALU)完成建立，其模型参数模型突破了 174 万亿个，完全可以与人脑中的突触数量相媲美。

### [MOSS](https://moss.fastnlp.top/)

复旦大学自然语言处理实验室

国内第一个对话式大型语言模型MOSS已由邱锡鹏教授团队发布至公开平台（https://moss.fastnlp.top/），邀公众参与内测。

### 百度 [ERNIE-ViLG](https://huggingface.co/spaces/PaddlePaddle/ERNIE-ViLG)

这是一个在 1.45 亿个中文图像文本对的数据集上训练的100 亿参数模型。

![](img/7bfbfa8dc35b0334eaeafefab574160c.png)

### 腾讯 [异次元我](https://pandaily.com/tencents-anime-ai-picture-generator-hits-global-social-platforms/)

另一个引起轰动的中国工具是腾讯的异次元我，它可以将人物照片变成动漫人物。它面向中国用户，在南美等其他喜欢动漫的地区出人意料地走红。

![](img/fee9c10ec97ade823380be5a9b5eee2a.png)

### IDEA [Taiyi](https://huggingface.co/spaces/IDEA-CCNL/Taiyi-Stable-Diffusion-Chinese)

除了 ERNIE-ViLG，另一个大型中文文本到图像模型是 Taiyi，它是 IDEA 的心血结晶，开源 AI 模型在 2000 万个经过过滤的中文图像文本对上进行训练，具有 10 亿个参数。

![](img/14aab9f98edba27a4c7d6229d1db9f3c.png)

与百度和其他以利润为导向的科技公司不同，IDEA 是近年来由地方政府支持的少数几个从事尖端技术研究的机构之一。IDEA 是一个由著名计算机科学家 Harry Shum 领导的研究实验室，他与他人共同创立了微软在美国以外最大的研究分支 Microsoft Research亚洲。

### Skywork 天工大模型

在3.2TB高质量多语言和代码数据上进行预训练。开源了模型参数，训练数据，评估数据，评估方法。

Skywork是由昆仑万维集团·天工团队开发的一系列大型模型，本次开源的模型有Skywork-13B-Base模型、Skywork-13B-Chat模型、Skywork-13B-Math模型和Skywork-13B-MM模型，以及每个模型的量化版模型，以支持用户在消费级显卡进行部署和推理。

+ Skywork-13B-Math模型经过专门的数学能力强化训练。在13B参数规模下，我们的模型在GSM8K评测上得分第一，同时在MATH数据集以及CMATH上也表现优异，处于13B模型顶尖水平。

+ Skywork/Skypile-150B数据集是根据我们经过精心过滤的数据处理流程从中文网页中筛选出的高质量数据。本次开源的数据集大小约为600GB，总的token数量约为150B，是目前开源最大中文数据集。

+ 除此之外，我们还公开了训练 [Skywork-13B模型](https://github.com/SkyworkAI/Skywork) 中使用的评估方法、数据配比研究和训练基础设施调优方案等信息。我们希望这些开源内容能够进一步启发社区对于大型模型预训练的认知，并推动人工智能通用智能（AGI）的实现。

🔗 https://github.com/SkyworkAI/Skywork
