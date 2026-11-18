
最近，大型语言模型（LLMs）已经发展成具有规划、工具使用和跨多种任务执行能力的互动智能体。然而，没有特定智能体调整，像LLaMA这样的开源模型目前难以匹敌GPT-4的效率，尤其是缺乏用于微调的智能体调整数据集时。为此，我们推出了Mimir：一个简化的平台，提供可定制的管道，使用户能够大规模利用私有知识和公开、合法的数据集进行个性化智能体调整。此外，Mimir支持从相同输入生成通用的指令调整数据集。这种双重功能确保通过该平台开发的语言智能体既具有特定智能体能力，又具有一般能力。 Mimir将这些功能集成到一个连贯的端到端平台中，从上传个性化文件到一键智能体微调都得以便捷进行。

![](img/eb3d62b7c0e828acf16e0e6fdc87d311.png)

## 使用合成数据进行训练有多糟糕？语言模型崩溃的统计分析

贡献人：

链接：http://arxiv.org/abs/2404.05090v1

《模型崩溃现象》，由Shumailov等人在2023年引入，指的是当新模型基于先前训练模型生成的合成数据进行训练时性能下降的现象。这种递归训练循环使得原始分布的尾部消失，导致未来生成的模型忘记了最初的（真实）分布。为了深入理解语言模型中的模型崩溃现象，本文考虑了一个统计模型，允许我们描述不同递归训练情景的影响。具体而言，我们证明了仅在合成数据上训练时无法避免模型崩溃。然而，当混合使用真实和合成数据时，我们提供了一个估计，即在模型崩溃最终可以避免的情况下，合成数据的最大数量。我们的理论结论得到了经验验证的进一步支持。

![](img/36b1fb35e650a3146d0bfb807d199431.png)

## 探索与理解DPO限制的理论视角

贡献人：

链接：http://arxiv.org/abs/2404.04626v1

直接偏好优化（DPO）从成对偏好数据中直接导出奖励信号，已证实在将大型语言模型（LLM）与人类偏好对齐方面的有效性。尽管DPO在各种任务中被广泛使用，但它却因对SFT效果的敏感性和对向人类优选回复学习能力的阻碍而受到批评，从而导致性能不佳。为了克服这些限制，对DPO的理论理解至关重要但仍然缺乏。为此，我们采取了一步措施来从理论上分析和理解DPO的限制。具体而言，我们提供了一个使用场论来分析DPO优化过程的分析框架。通过分析DPO损失函数的梯度向量场，我们发现DPO损失函数在减少生成人类不偏好数据的概率速度方面比增加生成优选数据的概率更快。这为理解相关研究实验中发现的DPO限制提供了理论洞见，从而为其改进奠定了基础。

![](img/7f1d8d91b52a86e6f3ad60dd28eb0425.png)

## SqueezeAttention: 通过分层最优预算在LLM推理中管理KV缓存的二维方式

贡献人：

链接：http://arxiv.org/abs/2404.04793v1

摘要: 优化大语言模型（LLM）的键值（KV）缓存被认为是节省推理成本的关键。现有的大多数KV缓存压缩算法试图通过利用标记的不同重要性来减少标记序列的稀疏性。在这项工作中，我们发现通过识别注意力层的重要性，我们可以从两个维度共同优化KV缓存。基于我们对推理中逐层重要性的观察，我们提出了SqueezeAttention，精确优化KV缓存预算在各层之间的分配，并结合三种代表性的标记稀疏化算法，压缩每个层的KV缓存。通过从序列和层的维度优化KV缓存，SqueezeAttention在广泛的LLMs和基准测试中实现了大约30%到70%的内存减少以及高达2.2倍的吞吐量改善。代码可在https://github.com/hetailang/SqueezeAttention找到。

![](img/d9b1aafc645546251f8efbe7bc179aa1.png)

## 一个大规模的μ-转移的探索

贡献人：

链接：http://arxiv.org/abs/2404.05728v1

大型神经网络模型已成为自然语言处理和计算机视觉的主要支柱，然而它们的初始化和学习速率在很大程度上是凭经验确定的，可能会因论文和模型大小的不同而有所变化。μ参数化（μP）提供了对这些挑战的潜在解决方案，为模型初始化和学习速率提供了缩放规则，并据称在各种情况下实现了从小到大模型的零-shot超参数转移。尽管显而易见的前景，但μP缩放规则尚未被广泛采用，可能是因为实现复杂性较高、变化较多或理论背景复杂。这项工作通过经验研究了μP，重点关注普遍存在的Transformer架构，并旨在回答一个简单问题：μ-转移在实践中是否提供了最佳学习速率？从具有2M到10B参数的模型中，我们展示了μ-转移对大多数重要情况的有效性，但也发现了一些令人惊讶的情况，可能并不适用。

![](img/bb1cff154b723441d09d852e2d98c866.png)

## nanoLM: 通过准确损失预测跨尺度预训练LM的可负担性基准

贡献人：

链接：http://arxiv.org/abs/2304.06875v4

随着语言模型的扩展，验证研究想法变得越来越昂贵，因为针对小模型的结论不能轻易转移到大模型上。一种可能的解决方案是建立一个通用系统，能准确预测大模型的某些指标，而无需对其进行训练。本文提出了一种名为μScaling的方法，通过我们的观察，最大更新参数化（μP）实现了对超参数空间中常见损失盆地附近的缩放规律的准确拟合。通过μScaling，不同模型设计可以在大规模上进行比较，只需训练它们较小的对应物。我们引入了nanoLM：一个经济实惠的LLM预训练基准，促进了这种新的研究范式。通过大约14%的一次性预训练成本，我们可以准确预测高达52B的模型损失。我们希望通过nanoLM使有限资源的研究人员能够对大型模型做出有意义的结论。我们还希望我们的基准能够成为学术界和工业界之间的桥梁。μScaling的代码可在https://github.com/cofe-ai/Mu-scaling找到。nanoLLM的代码稍后会提供

![](img/0abe08ede00f912ae4c1e563877163c8.png)

## Diffusion-RWKV：为扩散模型扩展RWKV类架构

贡献人：

链接：http://arxiv.org/abs/2404.04478v1

摘要：Transformer在计算机视觉和自然语言处理领域引发了进展。然而，对于长篇任务（如高分辨率图像生成），巨大的计算复杂性限制了它们的应用。本文介绍了一系列源自用于NLP的RWKV模型的架构，对Diffusion-RWKV进行了必要的修改，以适用于应用于图像生成任务的扩散模型。与Transformer的扩散类似，我们的模型设计为在序列中高效处理块状输入，并能有效扩展，适应大规模参数和广泛数据集。其独特优势表现在减少了空间聚合复杂性，使其在处理高分辨率图像时异常擅长，从而消除了窗口化或组缓存操作的必要性。对有条件和无条件图像生成任务的实验结果表明，Diffison-RWKV在FID和IS指标上表现与现有的CNN或Transformer基础的扩散模型相媲美甚至超越，同时显著减少了总的计算FLOP使用。

![](img/1b40494a9c454d7bee8f3333dfe168fc.png)

# 产品

## UI Bakery AI App Generator

贡献人：

https://uibakery.io/ai-app-generato

UI Bakery 是一个自助低代码平台，允许快速构建定制的 Web 应用程序（内部工具、客户应用程序和管理面板）。最近推出了 UI Bakery AI 应用生成器，这个工具允许在现有数据（目前是 SQL 数据库）的基础上生成功能性的 Web 应用程序，只需指定一个提示。这个工具可以用于创建内部工具、CRUD 应用、管理面板、对数据进行对话式搜索以及原型设计和构思。生成应用程序后，用户可以通过后续提示进行调整或澄清，或者将其转换为 UI Bakery 应用程序，以在 UI Bakery 可视化编辑器中继续开发。

![](img/f714d94ed92557c0a26ec84221f3371d.png)

## Odaptos

贡献人：

https://odaptos.com/

Odaptos 提供了一种用于用户体验（UX）研究项目的人工智能工具。他们的人工智能分析可以捕捉传统分析所忽略的情感细微差别，帮助更好地理解用户。该工具的一些关键特性包括自动生成用户会话文本、轻松招募测试参与者、清晰展示用户旅程中的关键时刻、情感视频回放、可用性评分（SUS）、以及一个名为 Oda 的聊天机器人，它可以提供快速摘要和见解。

![](img/68def055ec9cc5b38c1998ef323816db.png)

# HuggingFace&Github

## FreeAskInternet

贡献人：

https://github.com/nashsu/FreeAskInternet

FreeAskInternet 是一个完全免费、私有且在本地运行的搜索聚合器和答案生成器LLM，无需 GPU。用户可以提出一个问题，系统会进行多引擎搜索，并将搜索结果组合到 ChatGPT3.5 中LLM，并根据搜索结果生成答案。

## BIG-AGI

贡献人：

https://github.com/enricoros/big-AGI

Big-AGI 是一款面向需要功能、形式、简单性和速度的专业人士的 AI 套件。由 12 家供应商和开源服务器的最新型号提供支持， Big-AGI 提供一流的聊天、通话，包括 AI 角色、可视化、编码、绘图、并排聊天等。

![](img/44ce857ff53920a631478654eb18b7f2.png)

## Cerule-v0.1

贡献人：

https://huggingface.co/Tensoic/Cerule-v0.1

Cerule 是一个小巧而强大的视觉流水模型，基于新发布的 Google 的 Gemma-2b 和 Google 的 SigLIP。小参数但性能强大，可识别图像给出文本回复

# 投融资

## Multiverse学徒制独角兽，收购Searchlight以聚焦AI

贡献人：

https://techcrunch.com/2024/04/08/multiverse-the-apprenticeship-unicorn-acquires-searchlight-to-put-a-focus-on-ai/

Multiverse，一家英国的学徒制独角兽企业，已经收购了AI招聘初创公司Searchlight。此举旨在整合Searchlight的AI技术，以增强多元宇宙的专业培训服务。Searchlight由Kerry和Anna Wang两位双胞胎姐妹共同创立，迄今为止已经筹集了近2000万美元资金，包括2021年的1700万美元A轮融资。多元宇宙在2022年的估值为17亿美元，计划通过此次收购扩大其AI能力，进一步在专业培训领域巩固地位。收购的财务细节未予披露。

公司官网：https://www.searchlight.ai/

![](img/d908c026f6706ab0d73c9693ab5e9fbf.png)

## Lexverify完成由Midven领投的90万英镑种子轮融资

贡献人：

https://www.uktechnews.info/2024/03/26/lexverify-secures-900k-seed-investment-led-by-midven/

位于伯明翰的Lexverify，一家使用人工智能驱动的风险管理助手，已成功完成90万英镑的种子轮融资。此轮融资由Midven领投，Midven是Future Planet Capital Group的一部分。其中55万英镑来自Midlands Engine Investment Fund (MEIF 1)通过MEIF西米德兰兹股权基金，另外35万英镑来自Raspberry Ventures、The LegalTech Fund和包括Richard Yorke、Mark Watkin Jones、Jonathan Scudder、Richard Grethe和Karl Wyborn在内的天使投资者。这次投资将使公司能够进行关键的人才招聘，以实现其扩张计划，并已经成功地与金融服务和制药行业的企业进行了试点。

公司官网：https://www.lexverify.com/

![](img/bab22cec1e72b5d7c5ada55bb4279c31.png)

## Lockchain.ai完成460万美元种子轮融资

贡献人：

https://www.thesaasnews.com/news/lockchain-ai-raises-4-6-million-in-seed-round

Lockchain.ai，一家总部位于美国乔治州雅典的区块链风险管理平台，借助人工智能技术，已正式结束隐身模式，并在种子轮融资中筹集了460万美元。本轮融资由Lerer Hippeau领投，Arrington Capital、Valhalla Ventures、ARCA、RSE/Vayner、Sonic Boom Ventures、Clear Sky Capital的Alex Weiss以及Artico Search参投，White Star Capital在随后的融资周期中加入。Lockchain.ai旨在为区块链生态系统中的交易者、投资者和基金经理提供自动化风险管理解决方案。

公司官网：https://lockchain.ai/

![](img/d4236b431b44e9cb177afd4b448a3d9d.png)

## 「原粒半导体」完成新一轮融资，加速大模型算力Chiplet落地

贡献人：

https://news.pedaily.cn/202404/532122.shtml

原粒（北京）半导体技术有限公司，一家专注于多模态AI处理器设计技术和Chiplet算力融合技术的创新企业，近日宣布完成数千万元的新一轮融资。本轮融资由一维创投和华峰集团联合领投，中科创星、中关村生态雨林基金、英诺天使和清科创投等老股东集体追加投资。资金将主要用于公司的大模型AI Chiplet研发流片及相关算力产品开发和业务拓展。此次融资标志着原粒半导体在AI Chiplet领域的技术和市场布局将进一步加速。

公司官网:https://calculet.com.cn/

![](img/51f9c18b89bb83dd86f657a7cface6d4.png)

## AI Infra供应商「星凡科技」获近亿元Pre-A轮融资

贡献人：

https://news.pedaily.cn/202404/532158.shtml

星凡科技，一家提供一站式行业AGI技术服务，近日完成了近亿元人民币的Pre-A轮融资。本轮融资由盛景嘉成领投，开普云和高捷资本跟投。资金将主要用于服务器产线建设、算力中心项目建设及后期运营，以及大模型推理芯片的研发。星凡科技主要提供以大模型为核心的训练推理一体机和训推集群产品，技术聚焦于算力中心运营全业务场景的LLM-Agent、异构芯片的大模型性能加速中间件开发及模型智能化适配。

公司官网：http://xffuture.com/

![](img/e5bf424012efad7f7355a57823cb7c9a.png)

# 学习

## 如何理解 natural gradient descent?

贡献人：

https://www.zhihu.com/question/266846405/answer/3450931951

这篇知乎回答详细探讨了自然梯度下降（Natural Gradient Descent），强调了它在优化参数时考虑参数空间内在几何结构的重要性。通过使用Fisher信息矩阵的逆来调整梯度，自然梯度下降方法能更有效地进行参数更新。此外，该回答还通过将概率分布之间的KL散度用于定义参数更新的距离，进一步阐释了自然梯度下降的技术细节。

参考链接：https://people.math.sc.edu/wuchen/papers/ChenLi.pdf

![](img/d5fcb84bfc20237f9cdb2318eac36c0b.png)

## kimi chat大模型的200万长度无损上下文可能是如何做到的？

贡献人：

https://www.zhihu.com/question/649192998/answer/3445754185

陈巍在回答中指出，kimi chat大模型实现200万长度无损上下文的技术关键在于相对位置嵌入、旋转位置嵌入、位置插值和线性偏置等方法。长文本的核心挑战在于保持文本的语义和逻辑一致性，而非仅仅提升KV cache或Inference速度。例如，旋转位置嵌入(RoPE)通过旋转矩阵编码绝对位置并结合自注意力公式中的明确相对位置依赖性。另外，提及的文献如《LongRoPE: Extending LLM Context Window Beyond 2 Million Tokens》展示了通过位置插值和渐进扩展策略实现大规模上下文窗口。这些进展表明，通过技术创新可大幅提升模型处理长文本的能力，保持内容的一致性和准确性。

![](img/fb2a9ded7aac53939aa4366db863f71a.png)

## 现代GPU体系结构Cache Operators行为研究

贡献人：

https://zhuanlan.zhihu.com/p/691104610

本文对现代GPU体系结构中Cache Operators的行为进行了深入研究。文章基于Compute Capability 8.9的L20 GPU和CUDA Toolkit版本12.2进行实验，探讨了STG指令是否绕过L1缓存、L1缓存之间的一致性问题、STG指令命中L1缓存后的行为（Write Back还是Write Through）、未命中L1时的行为（是否触发Write Allocate）。实验表明，STG指令命中L1时执行Write Through，未命中时先执行Write Allocate再进行Write Through，且SM的L1缓存之间不具备一致性。文章通过详细的实验代码和分析，为理解现代GPU微架构中的缓存操作提供了宝贵的见解。

![](img/72c7a19553ec31f14f0490ff74d21923.png)

## 创业：大模型RAG系统三个月的开发心得和思考

贡献人：

https://zhuanlan.zhihu.com/p/690298565
