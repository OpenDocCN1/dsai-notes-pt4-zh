
大语言模型（LLM）取得了明显的成绩，但它们对资源的需求不断增加，已成为强大和可访问超人类智能发展的主要障碍。本报告介绍了JetMoE-8B，这是一个新的LLM，经过少于0.1百万美元的培训，使用了来自仔细混合的开源语料库和30,000个H100 GPU小时的1.25T token。尽管成本较低，JetMoE-8B表现出色，JetMoE-8B的性能优于Llama2-7B模型，JetMoE-8B-Chat超过了Llama2-13B-Chat模型。这些结果表明，LLM的培训成本可以比通常认为的要便宜得多。JetMoE-8B基于高效的稀疏门控专家混合（SMoE）架构，由注意力和前馈专家组成。两个层都是稀疏激活的，使得JetMoE-8B拥有8B参数，仅激活每个输入token的2B，相比于Llama2-7B，推理计算减少约70％。此外，JetMoE-8B非常开放和学术友好，仅使用公共数据集和训练代码。本报告详细介绍了所有训练参数和数据混合，以促进未来开放基础模型的发展工作。这种透明度旨在鼓励合作和促进可访问和高效LLM领域的进一步发展。模型权重可在`https://github.com/myshell-ai/JetMoE` 上公开获取。

![](img/c91e9c9d6daf0f1141c02af3c1548951.png)

### Rho-1: 并非所有 token 都是你所需的

贡献人：

链接：`http://arxiv.org/abs/2404.07965v1`

之前的语言模型预训练方法通常对所有训练token应用下一个token预测损失。挑战这一规范，我们认为“语言模型训练中并非所有token都同等重要”。我们的初步分析深入研究了语言模型的token级训练动态，揭示了不同token的独特损失模式。借助这些见解，我们引入了一种称为Rho-1的新语言模型。与传统的语言模型不同，Rho-1采用选择性语言建模（SLM），有选择地训练与期望分布对齐的有用token。这种方法包括使用参考模型对预训练token进行评分，然后通过专注于具有较高溢出损失的token的损失进行语言模型训练。在持续在15B OpenWebMath语料库上预训练时，Rho-1在9个数学任务中几乎提高了30%的few-shot准确性。微调后，Rho-1-1B和7B在MATH数据集上分别实现了40.6%和51.8%的最先进结果 - 仅占预训练token的3%的DeepSeekMath。此外，在80B通用token上进行预训练时，Rho-1在15个不同任务中实现了平均增强6.8%，提高了语言模型预训练的效率和性能。

![](img/b387b74f832958ee17a9ddaecee598af.png)

### 语言模型合成数据的最佳实践和经验教训

贡献人：

链接：`http://arxiv.org/abs/2404.07503v1`

人工智能模型的成功依赖于大量、多样化和高质量的数据集，但由于数据稀缺、隐私问题和高成本，获取这些数据可能具有挑战性。合成数据已成为一种有前途的解决方案，通过生成模拟真实世界模式的人工数据。本文概述了合成数据研究，讨论了其应用、挑战和未来方向。我们提供了先前研究的经验证据来证明其有效性，并强调确保其真实性、忠实性和无偏见的重要性。我们强调了对合成数据的负责任使用，以构建更强大、包容和可信赖的语言模型。

![](img/13e8221e8ae845f951303ec243661dd4.png)

### HGRN2：具有状态扩展的门控线性RNN

贡献人：

链接：`http://arxiv.org/abs/2404.07904v1`

摘要：HGRN (Qin等人，2023) 层级门控线性RNN 在语言建模中表现出竞争性的训练速度和性能，同时提供高效的推断。然而，HGRN的循环状态大小仍然相对较小，限制了其表达能力。为了解决这个问题，受线性注意力的启发，我们引入了一个简单的基于外积的状态扩展机制，可以显著扩大循环状态的大小，而不引入任何额外的参数。线性注意力形式还允许硬件高效的训练。我们的大量实验证实了HGRN2在语言建模、图像分类和长距离竞技场的优势。在受控实验设置中，我们最大的3B HGRN2模型略优于Mamba和LLaMa架构Transformer在语言建模方面；而在下游评估中，其与许多开源的3B模型竞争，并且使用较少的总训练tokens。

![](img/41b087d2420124b915d05fb959ad3392.png)

### `ResearchAgent`：利用大语言模型在科学文献中进行迭代研究思想生成

贡献人：

链接：`http://arxiv.org/abs/2404.07738v1`

科学研究对于改善人类生活至关重要，但其固有的复杂性、缓慢的步伐和对专业专家的需求却对其造成了阻碍。为了增强其生产力，我们提出了一个`ResearchAgent`，这是一个由大语言模型驱动的研究思路编写智能体，可以自动生成问题、方法和实验设计，并在基于科学文献的基础上进行迭代优化。我们的`ResearchAgent`从一个核心论文作为主要焦点开始生成思路，不仅通过连接学术图中的信息得到相关出版物，还通过基于其潜在概念的实体检索库中的实体进行增强，这些实体从众多论文中挖掘和共享。此外，与人类通过同行讨论迭代改进思路的方法类似，我们利用多个提供评论和反馈的`ReviewingAgent`。此外，它们是用与人类偏好对齐的大型语言模型实例化的，其评估标准源自实际的人类判断。我们在跨多个学科的科学出版物上实验证明了我们的`ResearchAgent`，在生成基于人类和模型评估结果的新颖、清晰和有效的研究思路方面的有效性。

![](img/616d77e921506c3e5f80667ed635ee09.png)

### 为什么小型语言模型表现不佳？通过Softmax瓶颈研究语言模型饱和

贡献人：

链接：`http://arxiv.org/abs/2404.07647v1`

近期在语言建模中取得的进展在于在极大的网络采集文本语料库上对高参数化神经网络进行预训练。在实践中，使用这些模型进行训练和推理可能成本高昂，这促使人们使用规模较小的对应模型。然而，观察到规模较小的模型可能会出现饱和现象，即在训练的某个较高点出现性能下降，随后进入平台期。本文发现，这种饱和现象可以通过规模较小模型的隐藏维度与目标上下文概率分布的高秩之间存在不匹配来解释。这种不匹配通过在这些模型中使用的线性预测头的性能受到已知的 `softmax` 瓶颈现象的影响。我们衡量了 `softmax` 瓶颈在各种设置下的影响，并发现基于小于1000个隐藏维度的模型往往在预训练后期采用退化的潜在表示，导致了评估性能的降低。

![](img/f74245f5895264f5748f6c6caf5f175d.png)

### 从单词到数字：你的大语言模型在给定上下文示例时悄悄成为了一个能干的回归器

贡献人：

链接：`http://arxiv.org/abs/2404.07544v1`

我们分析了预训练的大型语言模型（如Llama2、GPT-4、Claude 3等）在给定上下文示例时，在没有额外训练或梯度更新的情况下，能够进行线性和非线性回归的能力。我们的发现显示，几个大型语言模型（如GPT-4、Claude 3）能够执行回归任务，性能可以达到甚至超过传统监督方法（如随机森林、装袋法或梯度提升）。例如，在具有挑战性的Friedman＃2回归数据集中，Claude 3胜过许多监督方法，如AdaBoost、SVM、随机森林、KNN或梯度提升。然后，我们研究了大型语言模型的性能如何随着上下文示例数量的增加而扩展。我们从在线学习中的后悔概念中借鉴，并凭经验证明，LLM能够获得次线性后悔。

![](img/e802a01610000cc18d7693f8b4d5d8ba.png)

# 产品

## Musho

贡献人：

`https://musho.ai/`

Musho 是一款新的个人AI设计助手，可以根据简单的提示即时生成专业、符合品牌形象的网站设计，是一种可以在Figma内部充当创意伙伴的工具，能够快速将想法转化为设计。

![](img/a26bc88e8494c25c107bdca07db98014.png)

## V7 Go

贡献人：

`https://www.v7labs.com/go`（网站比较玄学，🪜也有时打不开）

V7 Go是一个基于人工智能的数据库，解放人们后勤和行政任务的处理，使他们有更多时间专注于创造性和人际工作。通过直观配置人工智能模型并在涉及文档、视频或文本的任务上进行规模化运行，V7 Go可以提取信息、对消息进行资格认证和分类、进行文本、电子邮件或图像分类，并比较不同语言模型在相同任务中的性能。此外，它还具有许多特殊功能，包括将文件转换为小型存储库、控制模型输出选项、将项目细分为工作流程以及视觉定位，并且具有很强的规模化能力。

![](img/ebfaba1467f692e42589b74223aacf66.png)

# HuggingFace&Github

## `llm.c`

贡献人：

`https://github.com/karpathy/llm.c`

`llm.c` 其中包含了使用简单的 C/CUDA 进行 LLM 训练的代码。这种方法不需要使用大量的PyTorch（245MB）或cPython（107MB）。举例来说，训练GPT-2（CPU，fp32）只需要大约1000行清晰的代码，而且全部内容都在一个文件中。它可以立即编译和运行，并且与PyTorch的参考实现完全匹配。

## Realmdreamer

贡献人：

`https://realmdreamer.github.io/`

RealmDreamer 是一种从文本描述生成通用前向3D场景的技术。该技术通过优化 3D 高斯 Splatting 表示来匹配复杂的文本提示。通过利用最先进的文本到图像生成器来初始化这些斑点，将它们提升到3D，并计算遮挡体积。然后，将这个表示优化到多个视图上，作为一个带有图像条件扩散模型的3D 修复任务。为了学习正确的几何结构，通过结合深度扩散模型，通过在修复模型的样本上进行条件化，从而得到丰富的几何结构。最后，使用图像生成器的锐化样本对模型进行微调。值得注意的是，这个技术不需要在任何特定场景数据集上进行训练，并且可以合成多种风格的高质量3D场景，包括多个对象。其通用性还允许从单个图像进行3D合成。

![](img/2f60a9a34350a29ee70292cbe5b83422.png)

## recurrentgemma-2b

贡献人：

`https://huggingface.co/google/recurrentgemma-2b`

RecurrentGemma 是建立在 Google 开发的新递归架构上的开放语言模型。它包括预训练版本和指令调整版本，适用于各种文本生成任务，如问答、摘要和推理。与 Gemma 相比，RecurrentGemma 需要更少的内存，并且在生成长序列时推理速度更快。

# 投融资

## 内容商业AIGC视频应用平台「筷子科技」完成近5000万元B1轮融资

贡献人：

`https://36kr.com/p/2726136547976201`

筷子科技」宣布完成近5000万元B1轮融资，投资方包括金沙江创投、海珠城发集团等。筷子科技利用AI技术提高企业营销内容的生产与管理效率，主要提供企业级AIGC视频应用平台。该平台连接内容团队的多个工作流程，通过AI优化营销内容的生产和转化效率。随着AI技术的发展和市场需求增加，筷子科技加强了产品矩阵，扩展海内外市场，并推出更多AI驱动的营销应用，以支持更广泛的品牌和市场需求。

公司官网：`https://kuaizi.ai/`

![](img/f790159c36b7f9dfe8abff66d954b9c9.png)

## 新加坡AI创业公司Mindverse获得500万美元种子轮融资

贡献人：

`https://www.businesstimes.com.sg/startups-tech/singapore-based-ai-startup-mindverse-obtains-us-5-million-seed-funding`

新加坡的人工智能创业公司Mindverse在由风险投资公司Square Peg领投的种子融资轮中获得了500万美元。Mindverse成立于2022年，创始人包括前Meta高管和AI研究人员。该公司的旗舰产品包括MindOS Studio和Mebot。MindOS Studio可以帮助创建具有个性化聊天体验和界面的AI原生网站；Mebot是一款能学习并记忆用户思维和偏好的生产力工具。此轮融资将用于进一步研究和产品开发，以推进其大规模个性化模型。

公司官网：`https://www.mindos.com/`

![](img/7a7ea7acffbc709fcb6a83cd472a73ad.png)

## AI音乐创业公司Udio及出版平台Spines AI分别完成融资

贡献人：

`https://www.forbes.com/sites/charliefink/2024/04/11/udio-ai-music-raises-10-million-65-million-for-spines-ai-more-sora-cinematic-ai/?sh=7ed4d4d712ed`

在最新的融资轮中，基于人工智能的音乐生成器Udio AI Music获得了1000万美元的种子资金。这轮融资由包括a16z、Instagram联合创始人Mike Kreiger以及音乐人will.i.am和Common在内的知名投资者和名人参与。此外，AI出版平台Spines AI也在其种子轮融资中获得了650万美元，该轮融资由Aleph领投，前投资者M-Fund、畅销书《创业国家》的合著者Dan Senor和LionTree LLC参与。Spines AI平台能将出版时间从六个月缩短至两周，同时减少30%的成本，为出版过程的每个阶段提供服务。

公司官网：`https://www.udio.com/`

![](img/1b221f2f85e335d3434f8f49b74ae6e3.png)

## AI加密应用Sharpe AI完成种子轮融资

贡献人：

`https://www.binance.com/en/square/post/2024-04-11-ai-powered-crypto-app-sharpe-ai-secures-seed-funding-6618188480697`

根据Foresight News的报道，由AI驱动的加密货币应用Sharpe AI成功完成了其种子融资轮。此次融资吸引了包括Animoca Brands、GBV Capital、Morningstar Ventures、Maven Capital、Metabros、Contango Digital Assets、Ape Terminal和AIT Protocol等多家投资者的参与。具体的融资金额尚未披露。关于Sharpe AI的投资细节和未来计划预计将在未来几天内公布。

公司官网：`https://www.sharpe.ai/`

![](img/250f0e0153266a4d74e9f8064c24fbfd.png)

## Simbian获得1000万美元种子轮融资以全自动化安全运营

贡献人：

`https://siliconangle.com/2024/04/11/simbian-raises-10m-fully-automate-security-operations-generative-ai/`

Simbian公司，一家开发了首个针对网络安全团队的生成性人工智能副驾驶的初创公司，宣布已完成超额认购的1000万美元种子轮融资，以推出其新平台。本轮融资由专注于安全和人工智能的投资者领投，包括Cota Capital、Icon Ventures、Firebolt和Rain Capital。同时，包括Datadog联合创始人兼CEO Olivier Pomel、Nile Global联合创始人兼CEO Pankaj Patel以及前Meta Platforms、Uber Technologies和Cloudflare的首席安全官Joe Sullivan等知名天使投资者也参与了投资。此资金将用于推动Simbian的安全协同工具，该工具旨在提供完全自主的安全解决方案。

公司官网：`https://simbian.ai/`

![](img/cb1180aea8b54265f1bf2352639268b9.png)

## AI专利工作流平台Patlytics完成450万美元种子轮融资

贡献人：

`https://www.businesswire.com/news/home/20240411104124/en/Patlytics-Secures-4.5-Million-Led-by-Gradient-Google%E2%80%99s-AI-Fund-to-Transform-Patent-Intelligence-with-AI`

Patlytics，一家以AI驱动的专利工作流平台，已成功完成由Google的AI专注风险基金Gradient领投的450万美元种子轮融资。此轮融资还包括了8VC、Joe Montana的Liquid 2 Ventures、Tribe Capital、Vermilion Ventures、Gaingels、Alumni Ventures和Position Ventures等参投。Patlytics利用AI的变革性创新，旨在重新定义知识产权保护行业，通过自动化专利流程的关键环节，例如初稿撰写、办公室行动响应、侵权评估、高级搜索和竞争对手情报，从而增强专利专业人士的能力。

公司官网：`https://www.patlytics.ai/`

![](img/fff27adc10daffcd383683f30395c51f.png)

## Tabs获得700万美元种子资金以自动化应收账款管理

贡献人：

`https://www.axios.com/pro/fintech-deals/2024/04/11/accounts-receivable-tabs-7-million`
