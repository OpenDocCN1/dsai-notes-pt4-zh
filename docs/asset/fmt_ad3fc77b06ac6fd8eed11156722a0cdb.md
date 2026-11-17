
# 1-2B参数规模大模型使用心得及模型汇总

> 来源：[https://geekbang.feishu.cn/docx/TMaIdOx7goTodDx9b1Xc7GX1nlg](https://geekbang.feishu.cn/docx/TMaIdOx7goTodDx9b1Xc7GX1nlg)

> 👨🏻 作者：刘聪NLP

> 📃 作者介绍：AIGC前沿知识分享&落地经验总结

> 🛖 知乎主页：https://www.zhihu.com/people/LiuCongNLP

> ⏰ 收录时间：2024-05-23 11:28:42

> 🔗 原文链接：https://mp.weixin.qq.com/s/Jnc3fhd-jf0XK4b9PrbZzg

![](img/829b189c665ac2559e7e1682570214b5.png)

关注作者👸号，获取更多AI知识

## 写在前面

大家好，我是刘聪NLP。

大模型时代，根据大模型缩放定律，大家通常都在追求模型的参数规模更大、训练的数据更多，从而使得大模型涌现出更多的智能。但是，模型参数越大部署压力就越大。即使有`gptq`、`fastllm`、`vllm`等推理加速方法，但如果GPU资源不够也很难保证高并发。

那么如何在模型变小的同时，模型效果不明显下降，在指定任务上也可以媲美大模型的效果呢？

Google前几天发布的Gemini，在移动端采用1.8B参数模型面向低端手机，3.25B参数模型面向高端手机。

![](img/ba2475b96838d53331f0b96567db3bc6.png)

An overview of the Gemini 1.0 model family

而微软最近也是推出了2.7B的Phi-2模型，评测效果绝群。

![](img/c092b64a16e11b76ce93614324724706.png)

Averaged performance on grouped benchmarks compared to popular open-source SLMs

![](img/5ffc6ee698a7b96e947a33ceb95140ca.png)

Comparison between Phi-2 and Gemini Nano 2 Model on Gemini’s reported benchmarks

恰好笔者前段时间也在研究1-2B参数量左右的模型，因此写写心得体会；并汇总了一下现在市面上开源的1-2B参数量的大模型。

> 这波反向操作，佛曰：不可说。

## 如何使大模型变小

模型压缩的方法包括：用更多的数据硬训练小模型、通过大模型对小模型进行蒸馏、通过大模型对小模型进行剪枝、对大模型进行量化、对大模型进行低秩分解。

![](img/0315a62778ebef58e6296558c8d46112.png)

Taxonomy of Model Compression methods for Large Language Models

但是模型参数量变小还是硬训练或蒸馏一个参数量小的模型，剪枝和量化只是对模型进行推理加速，本质上参数量没有变少。

```
Paper: https://arxiv.org/pdf/2308.07633.pdf
```

对于预训练阶段来说，往往需要更多的数据硬训练。参数规模不够，只能数据质量和数据数量来凑。

![](img/8cedc109039ff8c442383e768659ac24.png)

Textbooks Are All You Need

而在指令微调阶段，往往是蒸馏更优秀的大模型，来让小模型效果更好。利用GPT3.5、GPT4的数据直接指令微调是对闭源大模型蒸馏的方法之一，也是目前大家主流的做法。但也可以在蒸馏过程中，利用闭源大模型充当一个裁判来判别教师模型回答和学生模型回答的差异，让学生模型向老师模型（闭源）进行反馈，重点是找到困难数据让学生模型进行更好的学习。

![](img/655f19b5746c034c0145adec4f6d2cff.png)

Lion: Adversarial Distillation of Proprietary Large Language Models

当然如果你本身拥有更大更好的大模型，那么就可以用标准的知识蒸馏来进行模型蒸馏，利用KL散度，对教师模型和学生模型输出概率分布之间的差异进行训练学习。

将更大模型的效果蒸馏到小模型上，会比硬训练的效果要理想，但首先要有一个可获取网络各层`logits`的大&好&强的模型。

## 训练1-2B参数规模使我痛并快乐

+ 训练1-2B模型让我又找到了全量调参的快乐，之前受显卡限制，都是`Lora`、`QLora`等方法训练。

+ 模型部署阶段，再也不用为显存发愁，老卡也轻轻松松进行模型部署。对于2B模型，`Float32`进行参数部署也就8G、`Float16`就需要4G，如果再做量化的话更小，甚至CPU部署速度也可以接受。

+ 同等数据情况下，效果确实不如更大的模型。以Qwen1.8B和7B为例，在自己任务上指标差了7-10个点。

+ 在个人任务上，通过增加数据，将训练数据扩大2-4倍并提高数据质量之后，效果基本上可以媲美。

+ 小模型在没有定制优化的任务上，就一言难尽了，泛化能力等都远不如更大的模型。

+ 用了小模型之后，再也没被吐槽过速度了。

## 主流1-2B参数规模的大模型汇总

共整理了14个1-2B参数规模的大模型，按照参数量从大到小排序，如下所示。

![](img/925cc83d5c6326df7b66c511194c9b93.png)

[PS: HF访问不了的小](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247487260&idx=1&sn=a6db35312dd894c6df84bf9ba5515845&scene=21#wechat_redirect)伙伴，可以看一下 《大模型下载使我痛苦》 。

### Yuan2.0-2B

Yuan2.0-2B是浪潮发布的Yuan2.0大模型的2B版本，采用中英文的高质量资料，包括书籍、百科、论文等。Yuan2.0-2B模型层数24层，隐藏层维度2048，支持最大长度8192。

```
Github: https://github.com/IEIT-Yuan/Yuan-2.0
HF: https://huggingface.co/IEITYuan/Yuan2.0-2B
Paper: https://arxiv.org/ftp/arxiv/papers/2311/2311.15786.pdf
```

### Qwen-1.8B

Qwen-1.8B是阿里发布的Qwen大模型的1.8B版本，采用2.2TB Token的数据进行预训练，包含高质量中、英、多语言、代码、数学等数据，涵盖通用及专业领域的训练语料。Yuan2.0-2B模型层数24层，隐藏层维度2048，支持最大长度8192，并且开源了对应的Chat模型。

```
Github: https://github.com/QwenLM/Qwen
HF: https://huggingface.co/Qwen/Qwen-1_8B
Paper: https://arxiv.org/pdf/2309.16609.pdf
```

### Bloom-1.7B&1.1B

Bloom-1.7B&1.1B是Hugging Face牵头组织的BigScience项目开源的Bloom大模型的1.7B和1.1B版本。训练数据涉及46种自然语言和13种编程语言，共计1.6TB的文本数据。Bloom-1.7B模型层数24层，隐藏层维度2048，支持最大长度2048。Bloom-1.1B模型层数24层，隐藏层维度1536，支持最大长度2048。

```
HF: https://huggingface.co/bigscience/bloom
Paper: https://arxiv.org/pdf/2211.05100.pdf
```

### Pythia-1.4B&1B

Pythia-1.4B&1B是EleutherAI开源的Pythia的1.4B和1B版本。主要使用300B Token的The Pile数据集进行训练。Pythia-1.4B模型层数24层，隐藏层维度2048。Pythia-1B模型层数16层，隐藏层维度2048。

```
Github: https://github.com/EleutherAI/pythia
HF: https://huggingface.co/EleutherAI/pythia-1b
&&
HF: https://huggingface.co/EleutherAI/pythia-1.4b
Paper: https://arxiv.org/pdf/2304.01373.pdf
```

### Phi-1&Phi-1.5

Phi-1&Phi-1.5是微软开源的Phi大模型的两个不同版本，均有1.3B参数，模型层数24层，隐藏层维度2048。Phi-1模型训练54B Token的数据，而Phi-1.5模型训练150B Token的数据。

```
HF: https://huggingface.co/microsoft/phi-1
Paper: https://arxiv.org/pdf/2306.11644.pdf
&
HF: https://huggingface.co/microsoft/phi-1_5
Paper: https://arxiv.org/pdf/2309.05463.pdf
```

### Deepseek-Coder-1.3B

Deepseek-Coder-1.3B是深度求索发布开源的Deepseek-Coder的1.3B版本，采用1TB Token的数据进行预训练，数据由87%的代码和13%的中英文自然语言组成，模型层数24层，隐藏层维度2048。

```
Github: https://github.com/deepseek-ai/deepseek-coder
HF: https://huggingface.co/deepseek-ai/deepseek-coder-1.3b-base
```

### Galactica-1.3B

Galactica-1.3b是MetaAI开源的Galactica大模型的1.3B版本，采用106B Token数据进行训练，数据主要来自论文、参考资料、百科全书和其他科学来源等。模型层数24层，隐藏层维度2048。

```
Github: https://github.com/paperswithcode/galai/blob/main/docs/model_card.md
HF: https://huggingface.co/facebook/galactica-1.3b
Paper: https://galactica.org/static/paper.pdf
```

### GPT-Sw3-1.3B

GPT-Sw3-1.3B是AI Sweden开源的GPT-SW3大模型的1.3B版本，采用320B Token数据进行训练，数据主要由瑞典语、挪威语、丹麦语、冰岛语、英语和编程代码数据集组成。模型层数24层，隐藏层维度2048。

```
HF: https://huggingface.co/AI-Sweden-Models/gpt-sw3-1.3b
```

### GPT-Neo-1.3B

GPT-Neo-1.3B是EleutherAI开源的GPT-Neo大模型的1.3B版本，GPT-Neo模型主要为了复现的GPT-3模型，采用380B Token数据进行训练，模型层数24层，隐藏层维度2048。

```
HF: https://huggingface.co/EleutherAI/gpt-neo-1.3B
```

### OPT-1.3B

OPT-1.3B模型是由MetaAI开源的OPT大模型的1.3B版本，采用180B Token数据进行训练，模型层数24层，隐藏层维度2048。

```
HF: https://huggingface.co/facebook/opt-1.3b
Paper: https://arxiv.org/pdf/2205.01068.pdf
```

### TinyLlama-1.1B

TinyLlama模型是一个1.1B参数的Llama模型，旨在3TB Token的数据上进行训练，目前训练到2.5TB Token数据，模型层数22层，隐藏层维度2048。

```
Github: https://huggingface.co/TinyLlama/TinyLlama-1.1B-intermediate-step-1195k-token-2.5T
HF: https://huggingface.co/facebook/opt-1.3b
```

## 写到最后

如果领导非要部署大模型，但对效果要求没有那么高，又没有资源支持，那么选择一个1-2B的模型也是不错的呦。

往期推荐：

+ 大模型微调技巧 | 高质量指令数据筛选方法-MoDS

+ 辟谣！微软撤回声称ChatGPT为20B参数的论文，并给出解释。

+ [如何看待微软论文声称 ChatGPT 是 20B (200亿) 参数量的模型？](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247487327&idx=1&sn=593923b9ca8fd01a58e769b52d002884&scene=21#wechat_redirect)

+ 大模型微调技巧-在`Embeeding`上加入噪音提高指令微调效果

+ 如何从数据集中自动识别高质量的指令数据

+ BaiChuan2技术报告细节分享&个人想法

+ 大模型LLM微调经验总结&项目更新

+ [打造LLM界的Web UI](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486578&idx=1&sn=8fc505e81e2a479bfaf9b5b15d120808&scene=21#wechat_redirect)

+ 是我们在训练大模型，还是大模型在训练我们？

+ [Llama2技术细节&开源影响](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486512&idx=1&sn=73d2b6dd73051e85bf6b9f630919f3d9&scene=21#wechat_redirect)

+ [大模型时代-行业落地再思考](https://mp.weixin.qq.com/s?__biz=Mzg5MTU1NTE1OQ==&mid=2247486493&idx=1&sn=d58830d103a21e141a20d308f2ad3a83&scene=21#wechat_redirect)

+ 垂直领域大模型的一些思考及开源模型汇总

+ 如何评估大模型-LLMs的好坏？

+ 总结|Prompt在NER场景的应用
