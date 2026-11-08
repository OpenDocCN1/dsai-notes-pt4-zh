# 奇绩大模型日报（5月 8日）

> 来源：[https://miracleplus.feishu.cn/docx/ONL4dNi5QoPzf5xjYBQcik93n7c](https://miracleplus.feishu.cn/docx/ONL4dNi5QoPzf5xjYBQcik93n7c)

# 资讯

### 苹果新一代iPad Pro 搭载M4芯片

https://mp.weixin.qq.com/s/Fi77Pbdl5yBL7X2L3Ul-Gg

在5月7日的春季新品发布活动上，苹果发布了新款iPad（Air和Pro）、新一代Apple Pencil和Magic Keyboard等配件。其中，人们最关注的焦点在于新款iPad Pro搭载的芯片。苹果没有辜负大家的期待，为iPad Pro配备了全新的M4芯片，这也是该芯片的首次亮相。M4芯片拥有280亿晶体管，采用第二代3nm技术，在CPU、GPU和NPU性能方面带来了显著提升。新的10核心CPU和10核心GPU分别提高了50%和4倍的性能，而16核心的神经引擎使得iPad Pro在AI任务方面表现出色。

![](img/e791b30883ac6083d8fc4a7a1ae060cd.png)

### 前特斯拉Optimus科学家跳槽HF，开源了一个机器人代码库

https://mp.weixin.qq.com/s/pb0ZAMuYENWWVyez2Amg2Q

LeRobot项目提供了一个全面的平台，包括共享、可视化数据和训练SOTA模型的多功能库，用户可以访问大量预训练模型以快速启动他们的项目。LeRobot还与物理模拟器无缝集成，允许爱好者和开发者在虚拟环境中模拟和测试AI模型。该项目旨在提供一个能够适应和控制任何形态机器人的AI系统，增强机器人在多功能性和可扩展性方面的能力。LeRobot项目的发展愿景是从多样化社区中构建软件和硬件，以在现实世界中开发下一代智能机器人。

![](img/d0d13f0df4e32f719dafca62cd482103.png)

### ACM「计算机图形学名人堂」最新名单公布，北大陈宝权入选

https://mp.weixin.qq.com/s/euucmju07Ag-GAsmdk0lgw

北京大学智能学院陈宝权教授成为首位入选ACM SIGGRAPH Academy的中国学者，这标志着中国在计算机图形学领域的重要地位和影响力。陈宝权教授的研究领域涵盖计算机图形学、三维视觉和具身智能，在大规模场景重建、点云处理和制造用形状设计方面做出了重要贡献。他是北京大学教授、智能学院副院长，也是IEEE Fellow。陈宝权教授曾领导多个重要项目，如国家自然科学基金重点项目、国家重点研发计划等。他在多个国际会议和期刊发表了200余篇论文，多次获得最佳论文奖等荣誉。除此之外，陈宝权教授还在学术组织和社区活动中发挥重要作用，如担任IEEE VIS大会主席等。

![](img/99e2697cb7e8ca6e7361a8c595023db4.png)

### Andrew Gao 爆料 Devin 2.0 的新功能！

https://mp.weixin.qq.com/s/WXIbBcGfz0OHxGxIUyJUYw

文章介绍了 Devin 2.0 的新功能，包括启动交互模式、编辑代码、使用 Cookie 登录网站等，展示了其在工程任务中的潜力。然而，质疑者对 Devin 的真实性提出了严重的疑问，指出演示视频中存在的不一致之处，并质疑其能力和成就的真实性。尽管 Cognition 公司未就这些质疑提供明确的解释，但对 Devin 技术的背后进行了神秘的描述，增加了人们对其真实性的怀疑。这一争议使得 Devin 的身份和技术价值备受关注，引发了人们对于其未来发展和公司的信任度的深思。

![](img/e4e879bb738457bc6d53c5794d11afc8.png)

### 首个ICLR时间检验奖出炉！3万被引论文奠定图像生成范式

https://mp.weixin.qq.com/s/YGvy6GrUHW3HXznWjqMnIQ

**首届ICLR时间检验奖**，颁向**变分自编码器VAE**。

VAE是一篇跨越十一年的论文，给后续生成模型带来了重要启发，如今的DALL-E3和Stable Diffusion等技术也受益于其思想。VAE的核心思想是将隐向量视为概率分布，通过编码器和解码器实现对隐变量的高斯分布采样和生成新样本。文章还介绍了VAE的主要架构师Diederik Kingma的背景及其贡献，强调了其在深度学习领域的重要性。此外，文章还提及了其他ICLR奖项和获奖论文，为ICLR大会的举办情况进行了简要介绍。

![](img/4c8c5d871fb4ff99788e7cab7d9394eb.png)

# 论文

### xLSTM: 扩展长短期记忆网络

贡献人：

链接：http://arxiv.org/abs/2405.04517v1

在上世纪90年代，长短期记忆（LSTM）的核心思想 - 恒定误差旋转木马和门控被引入。自那时起，LSTMs经受住时间的考验，贡献了许多深度学习成功案例，特别是构成了第一个大型语言模型（LLM）。然而，具有可并行化自注意力核心的Transformer技术的出现标志着一个新时代的曙光，在规模上超越了LSTMs。我们现在提出一个简单的问题：当将LSTMs扩展到数十亿个参数时，在利用现代LLMs的最新技术但缓解已知限制的同时，我们在语言建模方面能走多远？首先，我们引入了具有适当标准化和稳定技术的指数门控制度。其次，我们修改了LSTM存储结构，得到了：（i）具有标量存储、标量更新和新存储混合的sLSTM，（ii）具有完全可并行化的矩阵存储和协方差更新规则的mLSTM。将这些LSTM扩展集成到残差块主干中产生了xLSTM块，这些块然后被残留堆叠到xLSTM架构中。指数门控制度和修改后的存储结构提升了xLSTM的能力，在性能和扩展性上都表现得比最先进的Transformer和状态空间模型好。

![](img/52a731132d244248afe8373bc39ae308.png)

### Granite 代码模型：用于代码智能的开放基础模型家族

贡献人：

链接：http://arxiv.org/abs/2405.04324v1

大型语言模型（LLM）在代码上的训练正在彻底改变软件开发过程。代码LLM越来越多地被整合到软件开发环境中，以提高人类程序员的生产力，基于LLM的智能体开始展现处理复杂任务的潜力。我们引入了一系列仅解码器的Granite代码模型，用116种编程语言编写的代码进行训练。Granite代码模型系列由3到34亿参数的模型组成，适用于各种应用，从复杂应用现代化任务到内存受限的设备使用案例。在广泛的任务集上评估表明，Granite代码模型在现有开源代码LLM中始终达到最先进的性能。Granite代码模型系列针对企业软件开发工作流程进行优化，在各种编码任务（如代码生成、修复和解释）中表现良好，是一个多才多艺的代码模型。我们以Apache 2.0许可证发布我们的所有Granite代码模型，供研究和商业使用。

![](img/b23f526fc3bd020a452b4d5658a1114b.png)

### NaturalCodeBench：研究HumanEval和自然用户提示中的编码能力不匹配

贡献人：

链接：http://arxiv.org/abs/2405.04520v1

大语言模型（LLM）已经表现出很强的能力，能够生成用于生产活动的代码。然而，目前用于代码合成的基准，如HumanEval、MBPP和DS-1000，主要面向算法和数据科学入门任务，无法满足现实世界编码中普遍存在的挑战性要求。为了填补这一空白，我们提出了NaturalCodeBench（NCB），一个旨在反映真实编码任务中复杂性和多样性的具有挑战性的代码基准。NCB包含402个高质量问题，精心从在线编码服务的自然用户查询中选择，涵盖Python和Java两种语言，涵盖6个不同领域。鉴于创建真实查询的测试用例非常困难，我们还引入了一个半自动化流水线来增强测试用例构建的效率。与手动解决方案相比，它的效率提高了4倍以上。我们对39个LLM的系统实验发现，在NCB上，即使具有接近HumanEval分数的模型之间的性能差距仍可能很大，表明缺乏对实际代码合成场景的关注或对HumanEval的过度优化。另一方面，即使是表现最佳的GPT-4在NCB上仍然远未令人满意。评估工具包和开发集可在https://github.com/THUDM/NaturalCodeBench 上获得。

![](img/8a7c0c59ea68d906d570eb8bb23ae02d.png)

### QServe: W4A8KV4量化和系统共同设计，提高高效LLM服务

贡献人：

链接：http://arxiv.org/abs/2405.04532v1

量化可以加速大型语言模型（LLM）的推理。研究社区正在积极探索比INT8量化更低精度，如INT4。然而，目前最先进的INT4量化技术只能加速低批量、边缘LLM推理，在大批量、基于云的LLM服务中无法实现性能增益。我们揭示一个关键问题：现有的INT4量化方法在GPU上解量化权重或部分和时存在显著的运行时开销（20-90%）。为了解决这一挑战，我们介绍了QoQ，一个具有4位权重、8位激活和4位KV缓存的W4A8KV4量化算法。QoQ代表拉丁文中的4-8-4。QoQ是由QServe推理库实现的，可实现测得加速。QServe的关键见解是，在GPU上的LLM服务的效率受到低吞吐CUDA核心操作的重大影响。基于这一见解，在QoQ算法中，我们引入了渐进量化，可以降低W4A8 GEMM中的解量化开销。此外，我们开发了SmoothAttention来有效减少4位KV量化带来的精度降低。在QServe系统中，我们进行计算感知的权重重新排序，并利用寄存器级并行性来减少解量化延迟。我们还将融合的注意力限定在内存上，利用KV4量化带来的性能增益。结果，与TensorRT-LLM相比，QServe在A100上将Llama-3-8B的最大可实现服务吞吐量提高了1.2倍，在L40S上提高了1.4倍；在A100上，Qwen1.5-72B提高了2.4倍，在L40S上提高了3.5倍。值得注意的是，L40S GPU上的QServe甚至比A100上的TensorRT-LLM实现更高的吞吐量。因此，QServe有效地将LLM服务的成本降低了3倍。代码可在https://github.com/mit-han-lab/qserve获取。

![](img/b3c8eaee0fdd2370e9b2c9a50096468f.png)

### vAttention: 为大语言模型提供动态内存管理，无需分页注意力

贡献人：

链接：http://arxiv.org/abs/2405.04437v1

高效使用GPU内存对于高吞吐量的LLM推断至关重要。以往的系统预留内存用于KV缓存，导致由于内部碎片而造成容量浪费。受基于操作系统的虚拟内存系统启发，vLLM提出了PagedAttention，实现了对KV缓存的动态内存分配。这种方法消除了碎片，使得支持更大批次大小的高吞吐量LLM服务成为可能。然而，为了能够动态分配物理内存，PagedAttention将KV缓存的布局从连续的虚拟内存更改为非连续虚拟内存。这种改变需要重写注意力核以支持分页，并要求服务框架实现内存管理器。因此，PagedAttention模型导致软件复杂性、可移植性问题、冗余和效率低下。

在本文中，我们提出了vAttention进行动态KV缓存内存管理。与PagedAttention相比，vAttention保留了KV缓存在连续的虚拟内存中，并利用低级系统支持的需求分页来实现按需物理内存分配，这种支持已经存在。因此，vAttention减轻了注意力核开发人员明确支持分页的负担，并避免了在服务框架中重新实现内存管理。我们证明，vAttention使得各种注意力核的实现能够无缝进行动态内存管理。vAttention生成的token速度比vLLM快高达1.97倍，处理输入提示的速度比PagedAttention的FlashAttention和FlashInfer变体快3.92倍和1.45倍。

![](img/58d20b4ce623a8640d7cbc204fc45417.png)

# 产品

### **Eraser AI**

贡献人：

https://www.eraser.io/

Eraser AI 是一款针对技术设计场景的 AI 助手工具，能够通过自然语言指令快速生成各种架构图和技术文档大纲，并提供灵活的编辑功能，让用户可以对生成的输出进行微调和完善。它结合了 Figma、GPT-4 等领先技术，旨在帮助工程师、架构师等提高工作效率，解决手工绘制图表和文档的痛点，为技术设计领域带来全新的机器协作体验。

![](img/3a3abe19a562263a99d8c82c5dd5c72e.png)

### **Studio Neiro AI**

贡献人：

https://new.express.adobe.com/

Studio Neiro AI 是一款由 Avatars by Studio Neiro AI 团队开发的创新性 AI 视频创作工具。它能够生成拥有生动特性和细微表情的个性化 AI 化身，用户可以为其配音并无缝融入自己的内容，从而极大地提升内容创作的效果和效率。这款工具目前已经与 Adobe Express 集成，为所有用户免费提供。

![](img/e12235da632f41e826794482a4f8daca.png)

# HuggingFace&Github

### **StoryDiffusion**

贡献人：

https://github.com/HVision-NKU/StoryDiffusion

StoryDiffusion 是一个新的框架，通过创新的自注意力计算方式和语义时间运动预测模块，能够从文本描述生成既有高度一致的图像序列，又有平滑过渡的视频内容，为视觉故事生成领域带来了开创性的探索，值得进一步研究。

![](img/7e225ebf2d5d11a3995adc3236b485a3.png)

### JARVIS

贡献人：

https://huggingface.co/spaces/KingNish/JARVIS

模仿《钢铁侠》“贾维斯”的语音助手，可以根据用户给出的文本做出语音答复。

![](img/ac5627b5dabcd1ee78835a9a1cd7e6c2.png)

### **ScrapeGraphAI**

贡献人：

https://github.com/VinciGit00/Scrapegraph-ai

Scrapegraph-ai 是一个基于 Python 的 web 抓取库，它使用了 LLM (大语言模型) 和直接图形逻辑来创建针对网站、文档和 XML 文件的抓取管道。这个库可以帮助你轻松地提取所需的信息，用户只需告诉它你想要提取什么信息，它就会为你完成这项任务。

# 投融资

### DeepMind校友筹集2亿美元初创AI的资金

贡献人：

https://www.bloomberg.com/news/articles/2024-05-08/billionaire-sea-founder-warns-of-turmoil-from-the-shift-to-ai

巴黎的新创公司Holistic AI已完成首笔2亿美元融资，资金分为股权和可转换债券，主要用于购买计算能力。该公司由一群最近离开DeepMind的科学家组成，旨在开发“多智能体”模型，让多个AI系统相互交互。投资者包括Accel Partners LP和UiPath Inc.等公司，其中还包括前Google首席执行官Wendy和Eric Schmidt。Holistic AI的首席执行官Charles Kantor尚未回应置评请求，UiPath也未回应置评请求。Accel是早期支持Facebook、Dropbox等知名互联网公司的基金，但它远离了像OpenAI和Mistral这样构建基础AI模型的初创公司。Holistic AI在投资文件中提出了超越大语言模型如OpenAI的GPT和Google的Gemini的愿景，计划以多个垂直领域和收入流为目标。

公司官网：https://www.holisticai.com/

![](img/60d5b3c2b6efdf6844f0a3d68f85dfed.png)

### Legion获得5000万美元增长投资，Riverwood Capital领投

贡献人：

https://www.businesswire.com/news/home/20240507903815/en/Legion-Secures-50M-Growth-Investment-Led-by-Riverwood-Capital-To-Accelerate-Market-Growth-and-Drive-Global-Hourly-Workforce-Innovation

Legion科技是一家劳动力管理（WFM）创新公司，今日宣布完成由Riverwood Capital领投的5000万美元增长轮融资，现有投资者包括Norwest、Stripes、Webb Investment Network和XYZ。此次融资将加速Legion的市场推广和研发计划，为不断增长的全球客户提供优化的WFM体验。Legion已累计融资总额达1.45亿美元，致力于将每小时工作岗位转变为良好的工作岗位，同时简化劳动力运营并改善员工体验。随着组织对提高劳动力效率和员工参与度的关注增加，Legion持续推动行业现代化，为Rite Aid、Alo Yoga等知名品牌和快速增长的连锁店提供智能、自动化、以员工为中心的产品。 Legions的解决方案已成为诸如Rite Aid和Alo Yoga等知名品牌以及其他领先品牌和快速增长连锁店的首选WFM平台。

公司官网：https://legion.co/

![](img/ee3b0e21b91711173772955f9241c7f7.png)

### **投资Wiz：a16z动态**

贡献人：

https://a16z.com/announcement/investing-in-wiz/

a16z宣布投资Wiz，这是一个在云原生安全领域的全球领导者。随着工作负载和数据转移到云端，并且生成式人工智能开始主导企业，如今的网络安全比以往任何时候都更为关键。而Wiz正是在这一背景下应运而生，它成为云原生应用保护平台（CNAPP）的全球领导者之一。Wiz以其统一的云安全平台闻名，为客户提供云工作负载保护、云安全姿态管理（CSPM）、云基础设施权限管理（CIEM）、漏洞管理等服务，彻底改变了客户在云端构建和运行的一切安全方式。同时，Wiz采用现代化的云安全方法，为公司提供了对云环境和工作负载层的全栈可见性，使他们能够在没有任何笨重代理的情况下识别和优先处理风险。Wiz的创始团队同样令人印象深刻，他们拥有令人信服的合作历史和无与伦比的执行能力。安德森-霍洛维茨深信Wiz的未来将比其已经辉煌的开端更加光明。

公司官网：https://www.wiz.io/

![](img/92d43f7a141bb2017f107b9d4cae096b.png)

### 三星Medison以1,265亿韩元收购Sonio SAS

贡献人：

https://www.reuters.com/markets/deals/samsung-medison-buys-ai-software-firm-sonio-sas-92-mln-2024-05-08/

三星Medison签署协议收购Sonio SAS的全部股权，交易金额为1,265亿韩元。Sonio SAS是一家专注于胎儿超声AI解决方案的公司，成立于2020年。作为三星电子的附属公司，三星Medison将通过此次收购进一步扩展其在医疗设备领域的业务。交易完成后，Sonio SAS将继续作为独立公司运营，为法国医疗行业提供先进的技术解决方案。

公司官网：https://sonio.ai/en/

![](img/75d62aa716ce6f0c55d23b0b55fc68f9.png)

### Daloopa 完成 1800 万美元 B 轮融资

贡献人：

https://www.finsmes.com/2024/05/daloopa-raises-18m-in-series-b-funding.html

Daloopa，总部位于纽约的人工智能驱动的历史数据提供商，完成了1800万美元的B轮融资。本轮由Touring Capital领投，摩根士丹利和现有投资者Nexus Venture Partners参与。公司打算利用这笔资金进一步创新其专有算法，并构建产品解决方案，进一步提高金融服务行业数据质量，并加速其市场推广策略。此外，Daloopa打算拓展到新市场，尤其是在欧洲和亚洲。Daloopa的COO Daniel Chen，CTO Jeremy Huang和CEO Thomas Li领导了这一发展，他们开发了人工智能驱动的技术，用于全球范围内的历史数据集的获取、整理和分发。其以人工智能为先的方法正在帮助全球的买方和卖方金融机构，包括对冲基金、私募股权公司、共同基金、企业和投资银行，在盈利和模型构建工作流程中满足时间关键的数据发现要求，以加速分析师的洞察时间。

公司官网：https://daloopa.com/

![](img/57bcb3d92a9e6b09159a2882e4e1388a.png)

### VoicePanel种子轮融资

贡献人：

https://www.voicepanel.com/post/voicepanel-seed-funding

Voicepanel于2024年5月6日宣布完成了240万美元的种子轮融资。他们由多家知名投资机构支持，包括Y Combinator、Rebel Fund、Liquid 2 Ventures、Twenty Two Ventures，以及Dropbox、Instacart、Front、Gusto、Airtable和Optimizely的创始人。Voicepanel是一家致力于重新构想客户反馈的公司，他们的第一个产品是一款AI驱动的访谈工具，帮助企业与客户进行交流。通过这个工具，客户可以在几分钟内获得丰富的反馈，有助于改进产品和服务。该公司的愿景是让所有企业都能直接与客户交流，实现真正的对话，并能够从大量反馈中提取重要见解。新的融资将用于建立工程团队，支持公司的快速增长。

公司官网：https://www.voicepanel.com/

![](img/d7143e903b7dd00ffef9ba04a53cdc71.png)

# 学习

### 在英特尔至强 CPU 上使用 🤗 Optimum Intel 实现超快 SetFit 推理

贡献人：

https://mp.weixin.qq.com/s/YuEGw5Aqt1NTUXfyjcXh6g

本文介绍了如何使用 🤗 Optimum Intel 来优化 SetFit 模型，在英特尔 CPU 上实现了高达 7.8 倍的推理加速。SetFit 是一个由 Hugging Face 与 Intel 实验室以及 UKP Lab 合作开发的高效框架，用于对 Sentence Transformers 模型进行少样本微调。与基于 LLM 的方法相比，SetFit 有两个独特的优势：1. 无需提示或词-标签映射器，直接从少量标注文本样本中生成丰富的嵌入；2. 训练速度快，不依赖于 GPT-3.5 或 Llama2 等 LLM，因此训练和推理速度通常要快一个数量级。通过对模型进行量化优化，用户可以在英特尔至强 CPU 上部署生产级的 SetFit 解决方案。详情可参考原文链接。

技术细节突出：

+   量化优化技术：使用 🤗 Optimum Intel 实现了多种模型加速技术，如低比特量化、模型权重剪枝、蒸馏以及运行时加速。

+   训后静态量化 (PTQ)：通过对模型进行训后静态量化，无需任何训练即可在保持模型准确性的同时减低推理时的内存占用并降低延迟。

+   性能优化对比：与原始 transformers fp32 模型相比，优化后的模型在推理吞吐量上增加了 7.8 倍，同时准确率几乎没有下降。

![](img/e35b989703782fd4215286bb8eb6a20b.png)

### Dragonfly与LangChain聊天机器人的高效上下文管理

贡献人：

https://www.dragonflydb.io/blog/efficient-context-management-in-langchain-with-dragonfly

本文探讨了如何利用Dragonfly优化LangChain OpenAI聊天机器人的上下文管理，通过缓存技术提升性能和用户体验。在动态的软件开发世界中，针对实时AI驱动的应用程序（如聊天机器人）优化性能面临着重大挑战。大型语言模型（LLMs）是许多先进聊天机器人的核心，但它们本质上是无状态的，因此需要强大的机制来高效管理聊天上下文和会话数据。Dragonfly是一种现代、多线程、超高性能的内存数据存储，与Redis兼容，可用于缓存聊天机器人的上下文和会话数据。文章还介绍了如何使用Dragonfly提高LangChain聊天机器人的性能，并提供了相关代码示例和详细说明。

![](img/ad54564e612e71e6f67055e6d7df11e5.png)

### 使用MongoDB Atlas构建RAG应用程序

贡献人：

https://aws.amazon.com/cn/blogs/china/amazon-titan-text-v2-now-available-in-amazon-bedrock-optimized-for-improving-rag/

文章介绍了使用MongoDB Atlas构建RAG（检索增强生成）应用程序的过程。RAG技术通过结合模型训练数据和权威知识库，无需重新训练模型即可扩展模型功能到特定领域，从而提升模型输出的成本效益方法。在Amazon Bedrock知识库中，MongoDB Atlas作为向量存储的新集成，使得构建RAG解决方案更加简便。文章详细介绍了配置MongoDB Atlas和Amazon Bedrock知识库的步骤，以及如何使用MongoDB Atlas的向量搜索功能来执行查询。通过该集成，用户可以轻松地在应用程序中将RAG功能整合，从而提高应用程序的实用性和准确性。

![](img/d6c9b624327d429e5eab6ca793c24a41.png)

### 在GPU上加速RWKV6模型的Linear Attention计算

贡献人：

https://zhuanlan.zhihu.com/p/696054687?utm_psn=1770718509405687808

文章探讨了如何在GPU上通过不同方法加速RWKV6模型的Linear Attention计算。主要比较了三种实现：纯PyTorch实现、RWKV-CUDA的CUDA Kernel实现以及基于Triton的实现。RWKV-CUDA提供了针对RWKV6 Linear Attention模块的优化CUDA Kernel，显示出比纯PyTorch更高的执行效率。而Triton实现则通过在时间维度上进行Kernel融合来提高性能。文中详细讨论了各种实现的性能瓶颈，强调了Triton作为LLM时代开发的趋势，并展示了如何使用PyTorch Profiler和TensorBoard来分析程序性能。

![](img/e3051ac20677e5d6db1fd03e363aa519.png)

### 终极GPU互联技术探索：消失的内存墙

贡献人：

https://mp.weixin.qq.com/s/1tXreDUgDERChgafON6fqQ

文章探讨了在人工智能和高性能计算工作负载中，内存带宽成为性能瓶颈的问题。介绍了Eliyan公司的NuLink技术，这是一种新型的芯片组互联技术，可以显著提升AI负载的处理性能。NuLink技术通过物理层（PHY）的创新，可以在标准封装技术中实现超大型系统级封装，理论上能将性能提升10倍。这种技术通过提高内存带宽和优化数据传输，旨在突破当前硅中介层的限制，减少内存与计算之间的延迟和带宽瓶颈。此外，Eliyan的策略还包括使用特殊的自定义协议和双向数据传输，以进一步优化性能和资源利用率。

![](img/dc498df01447864b78b1673810dd8ac8.png)

### Llama-3的10个创新应用案例

贡献人：

https://analyticsindiamag.com/10-wild-use-cases-for-llama-3/

文章介绍了Meta最新发布的Llama-3模型的十个创新应用。这些应用包括：利用Llama-3 8B模型扩展其上下文长度至1048K，实现高效长文本处理；在本地运行Llama-3的RAG应用，无需互联网连接；KissanAI的Agri Vertical Dhenu 1.0模型，专为印度农业定制并优化；及其它各种通过Llama-3提升工具调用、功能调用和结构化输出的应用。这些应用案例展示了Llama-3在多个领域的潜力和灵活性，例如作为编程助手、研究助理以及访问私有数据的RAG功能。

![](img/b000708fc1f6df5e7a62220445826e85.png)

### 深入探讨Trition的Lower技术

贡献人：

https://zhuanlan.zhihu.com/p/695255185?utm_psn=1770828552008945666

Trition是一项新颖的技术，其Lower功能具有独特的技术细节。Lower技术是Trition的核心组成部分之一，它通过对数据进行低精度的量化，从而实现了显著的性能提升。该技术利用了深度学习模型中权重的分布特性，采用了精心设计的量化方案，使得模型在计算和存储方面都能够得到有效的优化。通过对模型参数进行Lower处理，Trition能够在不影响模型准确性的情况下，大幅降低模型的计算资源需求，从而在实际应用中取得了显著的性能提升。

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

##

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报****知识库****】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/1b7f8e9ef241a2e69ba8d1e85a4a057f.png)