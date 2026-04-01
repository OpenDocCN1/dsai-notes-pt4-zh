# 有了RAG我们还需要微调吗?

> 来源：[https://agijuejin.feishu.cn/docx/CTAHdEANGoUbC2xMCqhcMo3WnAd](https://agijuejin.feishu.cn/docx/CTAHdEANGoUbC2xMCqhcMo3WnAd)

> 作者：Zane | 智见AGI

> 原文： [https://mp.weixin.](https://mp.weixin.qq.com/s/hsmN9wQ5VgMT_hy8YE9E-w)qq.com/s/hsmN9wQ5...

![](img/287cba590fbac7780ce59d79860a2c80.png)

RAG技术已经这么强大了,我们真的还需要微调吗?

**©作者 | **Zane

**来源 | **神州问学

# **引言**

目前,大模型的热度居高不下,随着大模型的火爆越多的公司和组织开始使用大模型技术,往往大模型可以解决日常中的大部分问题,对于企业的私有知识的问答我们也可以通过RAG技术来进行实现,但是在面对特定任务、领域和场景时,基础大模型的通用知识往往不能给到我们准确的回复,而RAG技术通常依赖于通用知识库,这可能不包含特定领域的最新或者最专业的信息.最终我们仍然需要对模型本身进行微调,增强大模型本身的专业领域知识能力,来实现对专业领域等特殊场景的支持.

如何高效的微调这一问题,随着谷歌的研究人员在发布了论文《Parameter-Efficient Transfer Learning for NLP》[1] 后渐渐进入了人们的视野,PEFT技术旨在通过最小化的微调参数数量和复杂度,来提高与训练模型在新任务上的性能,从而缓解大型预训练模型的训练成本,这样即使计算资源有限,也可以利用预训练模型的知识来快速适应新的任务,实现高效的迁移学习.

本文我们将探索各个PEFT微调技术的原理和特点以及它们分别适合什么样的场景来帮助我们选择更加适合自己的PEFT技术.

# **主流PEFT技术**

**Adapter Tuning**

在论文《Parameter-Efficient Transfer Learning for NLP》[1] 中谷歌的研究人员提出针对BERT的PEFT微调方式即Adapter Tuning.

![](img/7ff3acee509044d423aadef3fa344640.png)

谷歌的研究人员设计了Adapter结构,将其嵌入到Transformer的结构里,固定原参数不变,只对新增的Adapter结构进行微调.

![](img/92c472bee0feee4c42f1800f1046c421.png)

根据论文中的实验结果来看,这个方法能够在只额外增加3.6%的参数规模情况下取得和Full-finetune接近的效果.

总的来说Adapter Tuning技术,没有改变预训练模型原本的部分而是在预训练模型的开始加入了一个Adapter层,每次训练时只针对Adapter,但是由于额外增加了Adapter层,会导致模型的层级加深,而导致模型的推理速度受到影响.

**Prefix Tuning**

Prefix Tuning 方法由斯坦福的研究人员在《Prefix-Tuning: Optimizing Continuous Prompts for Generation》[2] 首次提出,该方法是在输入Token之前构造一段任务相关的virtual token作为Prefix,然后训练的时候只更新Prefix部分的参数,而其他的部分参数固定,改方法类似于构造了一个Prompt,只是Prefix是将Prompt隐式的隐藏在模型中.

![](img/3274aef7b8d1485cd7a0b8b9feed9a47.png)

同时为了防止更新Perfix的参数导致训练不稳定,他们在Prefix层前加入MLP结构,训练完成后只保留Prefix参数.

最终根据实验结果可以看到Prefix Tuning的方式也取得了不错的效果

![](img/87ab0ec4e554a2140d7fe23abce12039.png)

总体来说Prefix Tuning相当于在预训练模型的上隐式的加入了一层Prompt,在输入token之前加入虚拟token作为前缀,从而影响结果.

**Prompt Tuning**

Prompt tuning的首次提出是谷歌的研究员在论文《The Power of Scale for Parameter-Efficient Prompt Tuning》[3],改方法可以看作简化板的Prefix tuning,只在输入层上加入Prompt token,也没有加入MLP来解决训练难的问题,随着预训练模型的参数量增加Prompt Tuning方法会逼近Full-finetune

![](img/f7c73fe55c25f30f8b9d2e3c8d7fd08a.png)

Prompt Tuning和Prefix Tuning非常相似,但是整体而言则更加简单,它仅在输入层加入了Prompt token,并且改方法非常依赖原本预训练模型的能力,只有当预训练模型参数量足够大时才能达到比较好的效果,限制较大.

**P-Tuning**

P-Tuning方法则是由清华大学的研究员在《GPT Understands, Too》[4]提出,在最早的版本中P-Tuning于Prefix-Tuning类似都是额外加入Embedding层,不过Prefix Tuning是将额外的Embedding层加在开头,而P-Tuning则是位置不固定,Prefix Tuning是在每个Attention层都加入Prefix Embedding来增加额外参数,通过MLP来初始化,而P-Tuning则是只在输入的时候加入Embedding,通过LSTM+MLP来初始化.

![](img/def23e60058bbc3e44e19495f4b13859.png)

根据实验结果来看P-Tuning的v1版本和Prompt Tuning一样,都是在预训练模型参数量足够大时才能达到和Full-finetune类似的效果,而参数规模比较小时效果很差.从实验的报告中可以看到,当规模10B时才能够基本持平.

![](img/65c3b0d033f8f24a3cbd656b2f018c82.png)

在P-Tuning的v2版本中,在多层加入了Prompt token上作为输入,带来了更多可学习的参数,同时也足够影响参数,并且加入到更深层的结构中的Prompt能够给模型预测带来更直接的影响

![](img/1f3c3fd92cde32ec86df721492078811.png)

从实验参数来看,P-tuning的v2版本在小参数规模的模型上也能达到比较好的效果

![](img/1b87c774383a447f00cc4f2c0bc6c950.png)

P-tuning的整体思路和Prefix Tuning,Prompt Tuning思路相似都是在原本的模型上构架类似Prompt层的概念从而影响模型的结果,但是在v2版本中P-tuning在更深层的结构中加入Prompt层,这样对于结果的影响则更加明显.

**LoRA**

LoRA方法是由微软和CMU的研究员在论文《LORA: LOW-RANK ADAPTATION OF LARGE LANGUAGE MODELS》中提出的一种方法.

因为大模型起到关键作用的是其中的低秩的本质维度,受此启发作者设计了如下的结构,在涉及到矩阵的相乘的模块时,引入低秩的A、B两个低秩矩阵去模拟Full-finetune的过程,相当于只对模型中起关键作用的低秩本质维度进行更新.

![](img/74cd6ace663302d525197819f9b0cf4d.png)

LoRA的出现解决了因为Adapter方法额外增加的Embeding层带来的推理延迟,并且因为没有引入Prompt所以不会有Prompt方法带来的一些问题,并且因为是模拟Full-finetune,几乎不会带来任何训练效果的损失.

根据实验结果显示LoRA也确实更加有效,训练了相当小的参数量而得到了接近甚至超过Full-finetune.

![](img/062f1e17333aa1b6064698eff10c4f5c.png)

![](img/f8433f4e1bdf25595cb01968dfd1fcf5.png)

LoRA的思路则完全不同于前几种方法,LoRA方法认为影响模型输出的最关键的是其中的部分低秩的关键矩阵,LoRA则通过额外的低维矩阵对这部分核心低秩矩阵进行微调,LoRA本身没有在模型本身额外增加层级,所以不会影响推理的速度.

**总结分类**

经过了上边的介绍,可以发现当前有很多的PEFT技术,那么当前PEFT技术可以分为几类呢,在论文《TOWARDS A UNIFIED VIEW OF PARAMETER-EFFICIENT TRANSFER LEARNING》中CMU的科学家将当前主流的几种技术按照方法进行了以下几种分类

![](img/e15c14eadc46678fbdac5acafc103529.png)

根据几种PEFT技术的特点我们进行如下总结

**Adapter Tuning**

**● 特点: **Adapter Tuning 是一种轻量级微调方法，通过在预训练模型的参数中添加小型适配器来保持大部分参数不变，仅微调适配器的参数。这样可以显著减少微调过程中需要更新的参数数量，降低计算成本和内存占用。

**●优势: **适用于资源受限的环境，能够在不引入大量额外参数的情况下进行微调，同时保持模型的泛化能力。

**●适用场景: **适用于需要在资源受限环境下进行微调的场景，如移动端部署等。

**Prefix Tuning**

**●特点: **Prefix Tuning 是一种在输入序列前添加特定标记（称为前缀）的微调方法。通过引入前缀，模型可以更好地适应特定任务的输入输出模式，提高模型性能。

**●优势: **适用于需要在特定任务上微调的场景，可以通过定制化前缀来引导模型执行特定任务。

**●适用场景: **适用于需要在特定任务上微调的自然语言处理任务，如问答、文本生成等。

**Prompt Tuning**

**●特点: **Prompt Tuning 是一种结合了前缀生成和微调的方法。它引入可学习的提示（Prompt）来指导模型执行特定任务，同时通过微调模型参数以适应特定任务的要求。

**●优势: **结合了前缀生成和微调的优点，能够灵活地指导模型执行特定任务，并在微调过程中优化模型参数。

**●适用场景: **适用于需要在特定任务上微调，并且需要精细控制模型输出的自然语言处理任务。

**P-Tuning**

**●特点: **P-Tuning 是一种基于控制变量的微调方法。它通过引入控制器来调节模型输出，在微调过程中不直接调整模型参数，从而实现对模型输出的精细控制。

**●优势: **能够对模型输出进行精细控制，适用于需要调节模型输出的任务。

**●适用场景: **适用于需要对模型输出进行调节的自然语言处理任务，如生成式任务。

**LoRA**

**●特点: **LoRA 是针对大型模型微调的综合性方法，结合了大批量优化、正则化、注意力机制等技术。它旨在解决在大批量训练和微调过程中遇到的问题，如收敛速度、泛化能力等。

**●优势: **综合考虑了大型模型微调过程中的各种问题，能够提高模型的性能和泛化能力。

**●适用场景: **适用于大型模型微调的各种任务和场景。

# **挑战与展望**

PEFT技术在提升语言模型微调效率方面取得了显著进展，但仍然面临许多挑战。首先，在性能和效率之间取得平衡仍然是一个难题，特别是在大规模数据集上。其次，由于存在多种PEFT方法，每种方法在特定任务上表现不同，因此选择最适合特定任务的方法仍然具有挑战性。此外，尽管PEFT方法可以复用预训练模型，但如何有效地复用这些模型，尤其是在跨任务设置中，仍然是一个挑战。此外，PEFT方法虽然降低了计算资源需求，但在某些情况下仍需要大量的计算资源。如何在资源受限的环境中有效部署PEFT模型是一个挑战。PEFT模型往往更复杂，其内部工作原理不如全参数微调模型直观，这增加了模型解释的难度。PEFT模型在特定任务上的表现可能优于全参数微调，但在跨任务和跨领域任务上仍存在挑战。

PEFT方法对模型的长期影响尚未完全了解，例如对模型泛化能力、稳健性等的影响。随着模型规模的不断扩大，如何有效地扩展PEFT方法以适应更大规模的模型仍然是一个挑战。PEFT方法大多集中在文本任务上，而多模态融合任务对模型提出了新的挑战。此外，PEFT模型可能更容易受到对抗性攻击，如何确保模型的安全性和隐私性是一个重要挑战。总之，尽管PEFT技术在语言模型微调方面取得了重要进展，但仍需要在多个方面进行进一步研究，以解决当前面临的挑战。

# **结论**

PEFT技术是近年来自然语言处理领域的重要突破。其核心优势在于仅需要微调模型中少量参数，即可使预训练模型适应新的下游任务，大幅降低了微调成本。此外，不同PEFT技术可以适应不同的下游任务，如Adapter Tuning、Prefix Tuning等，有效提高了模型的适应性。此外，PEFT技术可以显著降低计算资源和存储资源的需求，同时很好地保持了预训练模型的泛化能力，避免过拟合。PEFT技术对大模型应用的影响体现在加速大模型的应用落地，节省计算资源，以及提高大模型适应性。因此，我们鼓励NLP和AI领域的技术人员积极投入到PEFT技术的研究中，不断优化现有的PEFT技术，并探索新的PEFT方法。同时，建议相关企业和研究机构组织更多关于PEFT技术的交流会议，促进技术成果的分享和交流。企业和政府应加大在PEFT技术研发和应用方面的投入，以推动这一技术的快速发展和广泛应用。另外，针对PEFT模型可能存在的安全隐患，建议开展针对性的安全研究，以防止对抗攻击等安全风险。

#

# **文献参考**

[[1]《Parameter-Efficient Transfer Learning for NLP》- Neil Houlsby,Andrei Giurgiu ,Stanisław Jastrze¸bski,Bruna Morrone,Quentin de Laroussilhe,Andrea Gesmundo,Mona Attariyan,Sylvain Gelly](https://arxiv.org/pdf/1902.00751)

[[2]《Prefix-Tuning: Optimizing Continuous Prompts for Generation》- Xiang Lisa Li,Percy Liang](https://arxiv.org/pdf/2101.00190)

[[3]《The Power of Scale for Parameter-Efficient Prompt Tuning》- Brian Lester,Rami Al-Rfou,Noah Constant](https://arxiv.org/pdf/2104.08691)

[[4]《GPT Understands, Too》- Xiao Liu1, Yanan Zheng1, Zhengxiao Du, Ming Ding, Yujie Qian,Zhilin Yang, Jie Tang](https://arxiv.org/pdf/2103.10385)

[[5] P-Tuning v2: Prompt Tuning Can Be Comparable to Fine-tuning Universally Across Scales and Tasks - Xiao Liu, Kaixuan Ji, Yicheng Fu, Weng Lam Tam, Zhengxiao Du,Zhilin Yang, Jie Tang](https://arxiv.org/pdf/2110.07602)

[[6] LORA: LOW-RANK ADAPTATION OF LARGE LANGUAGE MODELS - Edward Hu,Yelong Shen, Phillip Wallis,Zeyuan Allen-Zhu,Yuanzhi Li,Shean Wang,Lu Wang,Weizhu Chen](https://arxiv.org/pdf/2106.09685)

[[7] TOWARDS A UNIFIED VIEW OF PARAMETER-EFFICIENT TRANSFER LEARNING - Junxian He,Chunting Zhou,Xuezhe Ma,Taylor Berg-Kirkpatrick,Graham Neubig](https://arxiv.org/pdf/2110.04366)

![](img/f9a4496aabde6e74b1e82e0e3bc63fed.png)

![](img/de1ef50d6278a4926eb35ea2ec88f0f2.png)

![](img/471a3545a4cf3f9716ef28b0604906ee.png)