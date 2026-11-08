# 奇绩大模型日报（10月 24日）

> 来源：[https://miracleplus.feishu.cn/docx/Aae8dnVkeo9xqRx0EGrc0LwXnbg](https://miracleplus.feishu.cn/docx/Aae8dnVkeo9xqRx0EGrc0LwXnbg)

# 🔉潜空间活动报名

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，Le**pton AI 创始成员，曾在阿里云担任高性能 AI** 平台产品负责人，专注于 AI 在多个行业的落地及应用。Lepton AI 致力于建立高效可用的AI 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?—— 不同AI产品形态对团队的挑战》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/5dd1fce30724c321b74ad37e7336cfd9.png)

# 资讯

### Andreessen Horowitz推出Oxygen私有GPU集群

贡献人：

https://techcrunch.com/2024/10/23/andreessen-horowitz-helps-founders-meet-compute-needs-with-oxygen-private-gpu-cluster/

Andreessen Horowitz（a16z）推出了一个名为“Oxygen”的私有GPU集群，帮助其投资组合中的AI初创公司解决GPU资源紧缺的问题。Oxygen集群由Nvidia H100 GPU组成，提供给这些初创公司，用于训练或运行AI模型，无需依赖市场上高昂的云计算资源。此举旨在帮助初创公司与谷歌、Meta、微软等大型科技公司竞争，这些大公司能够通过长期合同获取大量GPU资源，而许多初创公司无法承担这样的合同。

该项目由a16z的合伙人Anjney Midha发起，他在接受采访时表示，由于Nvidia H100的供应短缺，初创公司在获取资源时经常被大型云计算客户优先，这让许多AI公司陷入困境。Oxygen集群为这些公司提供了灵活性，使它们能够在需要时获取GPU资源，而无需签订长期的云服务合同或支付巨额费用。

此外，Oxygen不仅帮助初创公司减轻了计算资源上的压力，还为a16z带来了竞争优势，使其能够吸引更多的AI初创企业。通过提供GPU资源，a16z可以在初创公司的业务中获得股份，而不是通过额外的资金投入帮助它们支付云服务费用。

据报道，Oxygen集群可能拥有超过2万个GPU，是目前规模最大的私有GPU集群之一。

a1**6z联合创始人Mar**c Andreessen接受Tom Bilyeu Youtube频道专访，公开发表于2024年10月22日。因为篇幅较长，为了便于阅读做了一定删节，原始内容参考：https://www.youtube.com/watch?v=6twxFu3bL0w

文字稿：

https://mp.weixin.qq.com/s/WjgthcZokAqlSO6hfQyCoQ

![](img/bb88ddaee2f4025716b234cb12273cea.png)

### 长期政策研究员Miles Brundage离开OpenAI，转向非营利部门

https://techcrunch.com/2024/10/23/longtime-policy-researcher-miles-brundage-leaves-openai/

长期担任OpenAI政策研究员的Miles Brundage宣布离职，选择在非营利部门继续其研究工作。他表示，希望在更自由的环境中发表研究成果，以产生更大的影响力。Brundage在2018年加入OpenAI，曾负责政策研究，并专注于ChatGPT等语言生成系统的负责任部署。

Brundage的离职伴随着OpenAI内部架构的调整，其原经济研究部门将划归新的首席经济学家Ronnie Chatterji领导，而AGI准备团队的其余项目将由其他部门接管。OpenAI发言人表示完全支持Brundage的决定，并对其贡献深表感谢。

近期，OpenAI经历了一系列高管离职，包括CTO Mira Murati和首席研究官Bob McGrew等，显示出公司内部在方向上的分歧。

![](img/0aad5837b9e0698da074f0c2dbbc0355.png)

### 谷歌发布AI生成文本水印技术SynthID Text

https://techcrunch.com/2024/10/23/google-releases-tech-to-watermark-ai-generated-text/

谷歌正式发布了SynthID Text，这是一项可以对生成式AI文本进行水印标记和检测的技术。开发者和企业可以通过Hugging Face平台和谷歌的更新版Responsible GenAI Toolkit免费获取这一工具。SynthID Text通过调整生成文本中的token分布，嵌入水印信息，从而在保持文本质量的同时帮助识别AI生成内容。

SynthID Text已集成到谷歌的Gemini模型中，且即使文本被剪裁、改写或修改后，仍能进行检测。然而，该技术在处理较短文本、翻译文本或回答事实性问题时效果较弱。尽管如此，随着全球对AI生成内容的监管力度加大，如中国和加州已经提出强制要求，水印技术可能会在未来被广泛采用。

这一技术有助于减少现有“AI检测器”的误判，尤其是在检测那些语气较为通用的文本时。

Google 宣布开源大模型数字水印技术 SynthID

https://zhuanlan.zhihu.com/p/2797328824

![](img/f609727fca5501ec7f3157b1697cc299.png)

### OpenAI发布sCM提升50倍效率

https://mp.weixin.qq.com/s/dI9mSCDbGzZIkjol_CT6Cg

今天凌晨，OpenAI发布了全新的扩散模型方法sCM (Stochastic Consistency Model)，实现了显著的技术突破。sCM只需两步即可生成高质量图片、3D模型等，时钟加速达50倍，尤其在高分辨率任务上表现出色。例如，一个15亿参数的模型在单个A100 GPU上，不经过推理优化，仅需0.11秒就能生成内容。这使得sCM比当前最快的Stability AI的SD（四步生成）版本推理效率提升了一倍。

sCM的核心基于一致性模型思路，通过将噪声直接转换为无噪声样本，避免了传统扩散模型的渐进式去噪过程。其引入了连续时间框架，消除了离散时间模型中的误差，模型参数化不再依赖离散的时间步，而是基于时间本身。sCM还通过改进的时间条件、自适应组归一化、新的激活函数以及自适应权重提高了训练稳定性和生成质量。这些技术细节包括时间条件感知、组归一化稳定性、新激活函数的非线性表达能力以及自适应权重对损失函数的动态调整，确保模型在生成过程中做出合理的决策并提高数据生成的精确度。

实验表明，sCM在CIFAR-10、ImageNet 64×64和ImageNet 512×512数据集上均表现出色，特别是在ImageNet 512×512数据集上，模型达到了1.88 FID的优异成绩，且使用算力更少。这表明sCM不仅更高效，还可以在更多场景中推广应用，未来可能在视频生成等领域大幅降低推理负担，并带来实时视频生成等新应用。该论文由两位华人提出，已公开发布。

![](img/78605a947d8d351dc29efb991c211316.png)

### Q3大模型中标项目超360个

https://mp.weixin.qq.com/s/hPpSaim5mx9yr1YOqcJp6A

2024年第三季度，大模型相关中标项目数量快速增长，达到了369个，远超上半年276个项目。大模型正在各行业加速落地，尤其是在核心业务场景、人才培训和端到端产品等领域取得进展。9月中标项目数创新高，达154个，环比增长40%。

项目采购的地区主要集中在北京、广东、上海等一线城市，行业分布上，能源、教育、政务和金融等领域尤为活跃。特别是能源行业，南方电网完成了至少20个大模型相关项目，其中三个项目中标金额超千万元。

中标项目金额呈现两极分化趋势，大额项目逐渐增多，千万元以上项目达21个，其中2个过亿元。项目应用场景更加细分，涵盖医疗、农业、司法等领域，但仍缺乏爆款应用。硬件采购仍是重点，占约20%的项目，软硬件一体化成为趋势，推动大模型的高效落地。

此外，人才缺口明显，企业加大人才培养投入，人才培训项目明显增多。第三季度的中标项目显示，企业通过合作科研机构和内部培训，努力解决大模型落地中的人才不足问题。数据问题也逐步被重视，相关项目显著增多，涵盖数据采集、治理和标注等方面。

领军企业如百度、科大讯飞、智谱AI等在不同领域表现突出，项目涵盖核心业务场景、前瞻技术研究和软硬一体化方案，显示大模型的应用前景广阔。

![](img/7a33be169cc5ca25cf30e31797a3cf3e.png)

# 推特

### ElevenLabs推出语音设计：仅通过文本提示生成独特的语音

贡献人：

https://x.com/elevenlabsio/status/1849083718838657186

介绍语音设计。

仅通过文本提示生成独特的语音。

我们的库中缺少您需要的语音吗？自己提示生成。

![](img/8047982f50418b9d1bf73e69daeb8479.png)

[ssstwitter.com_1729691076087.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/URGwbzwj9oX7fwxdk0ncawnKntd)

### 五种嵌入类型介绍：稠密嵌入，量化嵌入，二进制嵌入，可变维度嵌入，多向量嵌入

贡献人：

https://x.com/femke_plantinga/status/1849099350770585691

对不同的嵌入类型感到困惑？

你并不孤单。

嵌入类型的种类繁多，可能让人感到不知所措，因此这里是五种嵌入类型的快速概览，适用于你的AI应用：

•  **稠密嵌入**：标准形式，通过长序列的数字来表示意义。https://weaviate.io/developers/weaviate/search/similarity

•  **量化嵌入**：以较低的精度（如int8）存储稠密嵌入，在不丢失过多信息的前提下节省空间。https://weaviate.io/developers/weaviate/concepts/vector-quantization

•  **二进制嵌入**：将负值存储为0，非负值存储为1，高效保留核心数据。https://weaviate.io/developers/weaviate/concepts/vector-quantization#binary-quantization

•  **可变维度嵌入**：类似“套娃嵌入”，根据需求调整向量大小。有时，较大的并不一定是更好的！https://weaviate.io/blog/openais-matryoshka-embeddings-in-weaviate

•  **多向量嵌入**：超越单一序列，编码不同部分输入。例如ColBERT，它为每个token存储嵌入。https://github.com/weaviate/recipes/blob/main/weaviate-features/named-vectors/NamedVectors-ColPali-POC.ipynb

查看嵌入类型的知识卡片： https://weaviate.io/learn/knowledgecards/variable-dimensions

![](img/7ff03c57bce736b15a1148d43e8cfa69.png)

[ssstwitter.com_1729754609387.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WFmSbxNstoWkH2xa25IctHNknod)

### agent.exe开源：让Claude 3.5 Sonnet 来控制你的电脑

贡献人：

https://x.com/corbtt/status/1849124800838713844

刚刚发布了 **agent.exe**，一款免费的开源应用，支持Mac/Windows/Linux系统，能让你使用 Claude 3.5 Sonnet 来控制你的电脑！

这是一个有趣的小项目，用来探索API并看看这个模型能做些什么。控制电脑真的很酷——我预计2025年将是智能代理的时代。

![](img/77ee7ab30584b112d10b3b81980d0650.png)

### 实用多AI代理与crewAI高级用例：在实际应用中构建和部署高级基于代理的系统

贡献人：

https://x.com/AndrewYNg/status/1849112129904738656

新短期课程：实用多AI代理与crewAI高级用例。学习在实际应用中构建和部署高级基于代理的系统，该课程由@crewAIInc创建，并由其创始人@joaomdmoura讲授！(披露：我对CrewAI进行了小额种子投资。)

在本课程中，您将学习如何创建使用外部工具的高级代理应用程序，进行性能测试，使用人类反馈进行训练，并使用不同的大型语言模型执行多项任务。

您将构建多个提供实际商业价值的代理应用程序，如自动化项目规划系统、线索评分与参与管道、客户支持数据分析，以及一个强大的内容创作系统。

具体来说，您将学习如何：

•  使用任务、代理和团队的构建模块创建这些多代理系统，并了解使它们工作的一些要素，如缓存、记忆和防护机制。

•  将您的多代理应用程序与内部和外部系统集成。

•  在复杂的设置中连接多个代理，包括并行、顺序和混合配置，并创建多个代理应用程序协同工作的流程。

•  测试您的代理工作流，并使用人类反馈进行训练，以优化其性能，实现更好和更一致的结果。

•  在多代理系统中使用多个LLM，选择合适的模型大小和提供者来满足每个代理的具体任务。

•  从零开始在您的环境中启动项目，并为部署做好准备。

您还将从João与普华永道（PwC）商业生成AI负责人Jacob Wilson的访谈中学习，他们将讨论如何在实际行业用例中部署代理工作流。

完成本课程后，您将具备为工作构建定制多代理系统的能力。

![](img/7b4475f2f18a23a9a33622f4a3b6899b.png)

[ssstwitter.com_1729754761051.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ADXJbiveEo2VI7xd9Lic2ZZqnVf)

### 为什么语言模型认为9.11 > 9.9？一个出乎意料的简单解释

贡献人：

https://x.com/mengk20/status/1849213929924513905

为什么语言模型认为9.11 > 9.9？

在@transluceAI，我们偶然发现了一个出乎意料的简单解释——以及一个不需要重新训练或提示的修复方案。

结果与月份、日期、9月11日，甚至…《圣经》有关？

![](img/3d1f32219cd3cd368be1c408efbb2844.png)

[ssstwitter.com_1729754969670.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HoDLbMebooNDjmxdOTgcAbJKnQg)

# 信号

### Beyond Browsing: API-Based Web Agents

贡献人：

原文链接：https://x.com/yueqi_song/status/1849140469621747967

推文链接：https://arxiv.org/abs/2410.16464#

信号节点：模型体系-评估体系

提出了两种代理：(1) API 调用代理，它尝试仅通过 API 执行在线任务，类似于传统的编码代理；(2) 混合代理，它可以通过 Web 浏览和 API 与在线数据交互。在 WebArena（一种广泛使用且现实的 Web 导航任务基准）上的实验中，我们发现基于 API 的代理优于 Web 浏览代理。混合代理在各个任务上的表现几乎一致地优于其他两种代理，与单独的 Web 浏览相比，其绝对性能提高了 20.0% 以上，成功率达到 35.8%，在与任务无关的代理中实现了 SOTA 性能。这些结果强烈表明，当 API 可用时，它们提供了一种比单纯依赖网页浏览更有吸引力的替代方案。

![](img/712bc05f753a5aee4256826b33b693b9.png)

### The Llama 3 Herd of Models

贡献人：

原文链接：https://x.com/_akhaliq/status/1849118894704800199

推文链接：https://arxiv.org/abs/2407.21783

信号节点：模型体系-大模型

本文介绍了一组新的基础模型，称为 Llama 3。它是一组语言模型，原生支持多语言、编码、推理和工具使用。我们最大的模型是一个密集的 Transformer，具有 405B 参数和最多 128K 个标记的上下文窗口。本文对 Llama 3 进行了广泛的实证评估。我们发现 Llama 3 在大量任务上提供与 GPT-4 等领先语言模型相当的质量。我们公开发布了 Llama 3，包括 405B 参数语言模型的预训练和后训练版本以及我们的 Llama Guard 3 模型，以确保输入和输出安全。本文还介绍了我们通过组合方法将图像、视频和语音功能集成到 Llama 3 中的实验结果。我们观察到这种方法在图像、视频和语音识别任务上的表现与最先进的方法不相上下。由于仍在开发中，因此最终模型尚未广泛发布。

![](img/75e576749db99eb8fce75ff7969984f1.png)

### Eliciting Language Model Behaviors with Investigator Agents

贡献人：

原文链接：https://x.com/_akhaliq/status/1849118894704800199

推文链接：https://arxiv.org/abs/2407.21783

信号节点：模型体系-评估体系

大型语言模型 (LM) 具有开放性，因此表现出各种各样的行为。因此，很难提前确定特定模型可以表现出哪些类型的行为。例如，即使模型在一系列测试提示中未能表现出某种有害行为，新的越狱仍有可能引发该行为。相反，即使模型在提示时未能表现出某种能力，更好的提示（例如“深呼吸”）也可能会成功。

为了解决语言模型的开放式输入空间问题，我们希望有工具能够搜索这个空间，自动显示感兴趣的特定行为（例如，指定故障模式的实例）。我们将此任务称为行为引出。例如，引出“有害反应”（通常称为越狱）使我们能够识别安全漏洞，而引出“幻觉”则有助于我们确定语言模型的知识差距 。

![](img/1c6009a5d6eaab5d37ed1cee6331bd00.png)

# 产品

### RapidSubs

贡献人：

https://apps.apple.com/us/app/rapidsubs-captions-subtitles/id6695720834?ref=producthunt&at=1000l6eA

RapidSubs 是一款 AI 驱动的字幕生成应用，支持99种语言，允许用户自定义字幕样式，适用于 TikTok 和 YouTube 等平台。它提供简单易用的界面，无需注册即可免费使用，提升视频的参与度和可访问性。

![](img/deba607fe8cd2dbda0d019dee81dfdeb.png)

# HuggingFace&Github

### Aimgen.ai-AI-Image-Generator-Website

贡献人：

https://github.com/nnaemek2/Aimgen.ai-AI-Image-Generator-Website

Aimgen.ai 是一个基于 AI 的图像生成网站模板，希望可以吸引并与观众产生共鸣。它拥有简洁的用户界面和引人入胜的品牌设计，将愿景转化为视觉效果。主要功能包括支持 Figma、Sketch、Adobe XD 和 Adobe Photoshop 四种文件格式的登录页面设计，易于自定义，按名称分层。

![](img/d4f49dc974f5639818686352ed8e8fef.png)

# 投融资

### Granola再获2000万美元A轮融资

贡献人：

https://techcrunch.com/2024/10/23/vcs-love-using-the-ai-meeting-notepad-granola-so-they-gave-it-20m/

Granola是一款备受风险投资人欢迎的AI会议笔记应用，能够记录会议并通过AI技术增强笔记内容。Granola在最新的A轮融资中成功获得2000万美元，此轮融资由Spark Capital领投，其他参与的投资者包括Nat Friedman、Daniel Gross、Lightspeed、Betaworks和Firstminute Capital等。Granola团队仅在一周内完成了融资，该应用的独特卖点吸引了众多投资者的兴趣。

Granola自2023年5月推出以来，用户数量增长了5倍，目前每周大约有5000人使用这款应用。虽然最初风险投资人是主要用户，但如今超过57%的用户是非投资人，Granola逐渐成为各类领导者和团队的首选工具。

Granola的工作方式是通过连接日历和视频会议工具（如Zoom、Google Meet等）来记录会议音频，并使用AI自动整理会议笔记。与其他AI会议总结工具不同，Granola允许用户在会议中主动写下要点，AI则会根据用户输入自动补充和完善笔记。

随着A轮融资的到位，Granola计划扩展其工程团队，并为企业用户开发新功能，包括客户关系管理（CRM）集成、图像支持以及会议历史记录功能。此外，Granola还计划在明年推出移动应用，以进一步扩展其市场。

![](img/774be04b7256f166d1fd10a2ed6843b3.png)

### Fixify通过自动化和人类分析师解决IT问题，再获2500万美元融资

https://techcrunch.com/2024/10/23/fixify-blends-automation-and-human-analysts-to-tackle-it-problems/

Fixify是一家专注于IT帮助平台的初创公司，结合了自动化技术和内部分析师团队来解决企业的IT问题。公司近日完成了2500万美元的A轮融资，由Costanoa Ventures、Decibel Partners和Paladin Capital Group共同领投，Scale Venture Partners也参与了此次融资。Paladin的董事总经理Mourad Yesayan将加入Fixify的董事会。此轮融资为Fixify未来的发展提供了充足的资金保障，公司预计将利用这些资金度过经济低迷期并迎接未来的市场复苏。

Fixify成立于2023年，总部位于弗吉尼亚州阿灵顿，目前已累计融资3200万美元。公司计划在短期内扩大现有的41人团队，并扩展客户群，现有客户已包括15家公司。Fixify的创新之处在于其通过自动化技术整合现有IT票务系统（如Jira和ServiceNow），自动分类和识别问题热点，并由其雇佣的IT分析师来诊断和解决问题。这一模式不仅提高了生产力，还降低了运营成本，吸引了众多投资者的关注。

Fixify的核心目标是为员工在100至2000人的技术导向型公司提供高质量的IT服务体验，而无需公司投资大量的人力和技术堆栈。

公司官网：https://www.fixify.com/

![](img/46941131ec7861065a758247c6bbcebb.png)

### Sensei再获1600万美元融资，扩展欧洲无接触商店平台

https://techcrunch.com/2024/10/22/contactles-stores-to-grow-in-europe-as-sensei-reels-in-another-16m/

葡萄牙初创公司Sensei在欧洲无接触商店领域取得了显著进展，并在最新的A轮融资中筹集了1500万欧元（约合1600万美元）。此次融资由BlueCrow Capital领投，新投资者包括Lince Capital、Explorer Investments和Kamay Ventures（背后支持者为可口可乐和Arcor Group），以及现有投资者Metro AG和Techstars Ventures。此前，Sensei于2021年完成了一轮650万美元（540万欧元）的种子轮融资，由Seaya Ventures和Iberis Capital领投。

Sensei的技术通过计算机视觉和AI传感器，自动更新顾客购物车中的商品信息，并在结账时显示所有商品列表，方便顾客支付。公司计划到2026年在欧洲中部和北部扩展其业务，并在多个国家建立1000个全自动销售点。

竞争激烈的无接触商店领域还有其他初创公司，如Standard Cognition、Trigo、Grabango、AiFi和Zippin，它们也获得了大规模融资。

公司官网：https://www.sensei.tech/

![](img/f12e2e84ab0b5fe8b5689d3b5d52e922.png)

# 学习

### 智元灵犀X1开源项目

贡献人：

https://mp.weixin.qq.com/s/7SURuaGmaOYtNERZf1VsTw

**作为业内首家全栈开源人形机器人图**纸和代码的公司，此次开源，我们将豪无保留地提供“一站式”软硬件技术资源，总资料大小超过1.2GB。

**在整机结构硬件方面，开源内容包含详尽的整机结构图纸、**硬件框图和物料清单(BOM)、装机说明，细致到每一个螺丝和齿轮，确保开发者能够精确理解和复现我们的设计理念，全部物料可通过自行加工或购买的方式获取所有必需的物料，即使是刚入门科技爱好者也能快速完成X1本体的组装并实现其行走功能。

**在整机软件算法方面，我们提供了AimRT平台组件、机器人URDF文件、仿真&Sim2Sim代码、**强化学习训练代码、模型转换代码、运控推理代码和调试工具(数据可视化、遥控器)等等，从软件、到工程、到运控算法的全部代码。

智元**机**器人灵犀X1全身由两款自研的关节电机组装而成——PowerFlow R86、R52，具备中空走线、输出端绝对值编码、支持 PF-Link 智能接口等众多功能。我们自研的关节模组现已实现量产化迭代升级，提供了出色的扭矩性能，同时保持了较轻的重量，这为机器人提升高度的灵活性和精确的运控能力。

灵犀X1配备的OmniPicker自适应夹爪，可实现对各类物体的高效精准夹持和定位，协助机器人灵活作业。

BOM清单、整机STEP、整机图纸（SolidWorks）、装机SOP请通过百度云盘或谷歌云盘下载。

百度云盘：[h](https://pan.baidu.com/s/1UEdeDBTJiXRmIqMKwmO5RA?pwd=1234)ttps://pan.baidu.com/s/1UEdeDBTJiXRmIqMKwmO5RA?pwd=1234（提取码：1234）

谷歌云盘：[https://drive.google.com/drive/u/1/f](https://drive.google.com/drive/u/1/folders/1MECbyKRJbnc_XKWsdUbn-70xmYFmw9FW)olders/1MECbyKRJbnc_XKWsdUbn-70xmYFmw9FW

推理代码：[https://github.com/Agibot](https://github.com/AgibotTech/agibot_x1_infer)Tech/agibot_x1_infer

训练代码：[https://github.com/Agibot](https://github.com/AgibotTech/agibot_x1_train)Tech/agibot_x1_train

![](img/2c345e3512e822068146bf5d3f41188e.png)

### 活动回顾｜Triton&vLLM WorkShop

https://mp.weixin.qq.com/s/xHGAuztOHpfQWTFLYrn9IA

本文介绍了智源研究院在GOSIM开源盛会上成功举办的Triton&vLLM联合Workshop，聚焦于Triton编译器及大模型推理的技术进展。Triton因其强大的模型优化和硬件加速能力受到全球开发者的关注，但在中国的开发者社区仍处于发展阶段，智源研究院希望通过此次活动加强技术交流与合作，推动Triton技术在中国的普及和发展。

活动亮点包括Triton与vLLM技术的强强联合、专家分享大模型推理和性能优化的最新研究，以及行业实践中的案例展示。特别是在Triton kernel优化、vLLM推理框架、多模态模型的压缩与推理等领域，技术专家们深入探讨了各自的研究成果和应用经验。

技术议题涵盖了Triton kernel的优化策略、大模型推理框架vLLM的创新、多模态模型的优化实践，以及Triton社区的贡献计划等。这些分享为开发者们提供了丰富的学习机会和宝贵的实战经验，展现了Triton和vLLM在AI推理中的强大潜力。摩尔线程和阿里巴巴等企业也展示了其在Triton编译器实现和大规模语言模型优化方面的创新贡献。

**视频回放：**https://live.csdn.net/room/csdnnews/DoHfn6Ok

**PPT下载：**链接: https://pan.baidu.com/s/1Zs3IBP7rfjdUdYv2GciepA?pwd=0mv8 提取码: 0mv8

![](img/f124ffaca47a631ceb24e3dc28df34f1.png)

### OpenAI Triton分享：Triton编译流程

https://zhuanlan.zhihu.com/p/2768070031

Triton编译器架构：

1.  前端（Frontend）：将用户用Python编写的kernel或PyTorch 2.0中的Triton kernel转换为Triton IR。

1.  优化器（Optimizer）：对Triton IR进行多次pass优化，最终生成Triton GPU IR。

1.  后端（Backend）：将Triton GPU IR转换为LLVM IR，最后生成可执行的GPU代码，如NVIDIA硬件上的cubin文件。

编译流程： 以Triton官网的向量加示例为例，编译流程包括：

1.  将代码转换为Triton IR，并进行优化；

1.  转换为Triton GPU IR，进一步优化内存访问和线程布局等硬件相关细节；

1.  最后转化为LLVM IR，通过LLVM强大的优化和代码生成能力生成适合目标硬件的机器代码。

各层IR的细节：

+   Triton IR：硬件无关的高层抽象，描述深度学习计算图，支持诸如矩阵乘法、卷积等操作，并应用常规优化（如常量折叠）。

+   Triton GPU IR：专门针对GPU优化的低层表示，支持并行性表示（如线程块）和硬件相关优化（如内存访问模式）。

+   LLVM IR：更底层的硬件无关表示，提供平台无关的代码生成能力，通过丰富的优化通道生成特定硬件平台的机器码。

OpenAI Triton分享：Triton调试方法及工具

https://zhuanlan.zhihu.com/p/2440320343

完整系列可查看作者知乎

![](img/22db198d9ca49bc7d563e5de082dcd91.png)

### Ray 分布式计算框架介绍

https://zhuanlan.zhihu.com/p/111340572

Ray 是 UC Berkeley RISELab 开发的一个分布式计算框架，旨在应对 AI 和大数据应用带来的挑战，尤其是对高性能计算的需求。它专门优化了 Python 环境下的计算，支持像 Pandas 和 Numpy 这样的库进行矩阵运算。

Ray 的架构具备异步的分布式函数调用和内存调度功能，底层采用 Apache Arrow 作为内存数据结构，这为与其他系统的融合提供了良好的支持和出色的性能。Ray 开发了一个名为 Plasma 的内存服务，基于 Arrow 实现了高效的内存共享，允许不同进程通过共享内存进行数据交互。

相比 Dask，Ray 更加注重加速机器学习的调优和训练过程，除了基础的计算平台外，还提供了像 Ray Tune（用于超参数调优）和 RLlib（用于强化学习）这样的功能。Ray 与 Pandas 和 Numpy 的兼容性非常好，而 Dask 则侧重于弥补 Python 在大规模数据科学中的不足，更多用于数据科学中的通用分布式工作负载。

Ray 的调度器 Raylet 通过一个称为任务谱系（lineage）的机制管理任务的分布。它跟踪任务依赖关系，并通过有向无环图（DAG）结构有效地提交和执行任务。Raylet 通过与全局控制存储（GCS）通信来管理任务的状态和结果，从而实现跨集群的任务调度与执行。

![](img/878e1eb723488a13e5696b291abe9a18.png)

### LLM数据管理

https://zhuanlan.zhihu.com/p/792623609

"SemDeDup: Data-efficient learning at web-scale through semantic deduplication" 研究了在大规模网络数据中，如何通过语义去重来提高训练效率。该方法在处理数据冗余时，通过识别和删除重复的数据对，保持性能的同时减少了数据量，进而加速了训练过程。在LAION子集上的实验表明，SemDeDup能够以最小的性能损失删除50%的数据，进而将训练时间减半。

冗余数据分为以下几类：

1.  **感知重复**：指像素或标记完全相同的样本。

1.  **语义重复**：指信息内容相同但感知上有所差异的样本，例如两张角度不同的相同物体图像。

1.  **语义冗余数据**：尽管来自不同对象，但包含大量重复信息，例如两张不同公园中相似场景的图片。

1.  **误导性数据**：提供负面或有害信号的数据，移除这些数据反而能提高模型性能。

该方法采用了预训练模型的嵌入空间进行语义相似度计算，并使用聚类算法（如k-means）对数据进行划分，在每个聚类内计算数据对的余弦相似度，删除与中心点相似度较高的冗余数据，从而减少训练集的大小并保持模型性能。

这一技术尤其适用于处理大规模数据集，在提升数据效率的同时，减少了计算资源的浪费，提高了模型训练的速度和效能。

![](img/ae541dfb331f4fb667e21a48647592d7.png)

### CUDA-Free Inference for LLMs

https://zhuanlan.zhihu.com/p/2130907920

在无CUDA的环境下进行大语言模型（LLM）推理，本文探讨了如何使用OpenAI的Triton语言实现Meta的Llama3-8B和IBM的Granite-8B Code等模型的FP16推理。通过完全使用Triton编写kernel，在Nvidia H100和A100 GPU上分别达到了CUDA kernel工作流性能的78%和82%。Triton为跨GPU平台（包括Nvidia、AMD等）提供了通用的编程方法，使开发者能够以更高效的方式编写高性能kernel。

在推理过程中，Transformer模型的核心操作分为矩阵乘法、注意力机制等。这些操作大部分通过手写的Triton kernel实现，特别是矩阵乘法和flash attention两个主要瓶颈操作。通过手写的SplitK GEMM kernel，矩阵乘法得到了优化，该方法通过工作分解增加了并行性和负载均衡，最终提高了推理效率。

Triton的kernel性能与CUDA相比，矩阵乘法慢1.2-1.4倍，flash attention kernel慢1.6倍。然而，随着更高效的kernel如FlexAttention和FlashAttention-3的发展，Triton的性能差距有望缩小。未来工作将进一步优化核心原语操作，利用新硬件特性（如TMA和FP8支持）提升性能。

![](img/ebd54beb2b146b9a2688dcd1c6f8a75d.png)

### 浅谈自回归语言模型的“推理”能力

https://zhuanlan.zhihu.com/p/2794534026

自回归语言模型（例如GPT系列）的核心训练方式是通过next-token prediction进行训练。这种训练方法使用因果掩码（causal mask）将自回归模型的attention矩阵变为三角矩阵，模型依赖前面的token预测下一个token。这种方法的优势是数据量巨大，适用于整个互联网的数据，因此训练模型可以有效利用大规模无监督数据。

GPT模型最早并不以推理能力著称，GPT-2则开始展现出一些实际应用，如文本续写等。而到GPT-3，模型的规模和训练数据量大幅提升，使得它可以执行zero-shot任务，如文本生成和简单的推理。然而，这种所谓的“推理”能力，更多是因为模型在大量序列化数据（如数学、代码）上的训练，形成了流式结构中的模式匹配，模型通过在vocabulary中内插接近的分布来预测下一个token。

因此，GPT的推理能力并非真正的推理，而是模型通过语言模式的重复匹配来输出合理的短逻辑片段。这些片段在局部是连贯的，但当逻辑链条拉长时往往会出现问题。原因在于自回归模型缺乏规划（planning）能力，也没有多步逻辑推理机制，这导致其推理表现有限。

目前的研究表明，自回归语言模型的神经元主要在执行简单的语义叠加（superposition），并没有复杂的推理机制。虽然通过不同的后训练（如强化学习）可以提升模型的表现，但依赖next-token prediction的模型本质上仍缺乏真正的推理能力。因此，未来的研究可能需要在模型架构上进行更深层次的创新，以实现真正的推理和规划能力。

### 大模型时代还需要传统NLP吗？

https://zhuanlan.zhihu.com/p/2553177881

**大模型的优势**主要体现在以下几点：

1.  **强大的泛化能力**：大规模预训练允许大模型在零样本或小样本任务上有良好表现。

1.  **上下文理解能力强**：大模型能够处理复杂、长文本，并捕捉长期依赖关系。

1.  **特征表示学习能力**：自动学习多层次的文本表示，避免了手工特征工程。

1.  **零样本或小样本学习能力**：即使没有针对特定任务的训练数据，大模型也能通过预训练的知识进行推理和任务执行。

**大模型的劣势**包括：

1.  **计算资源需求巨大**：训练和部署大模型需要大量算力和硬件投入。

1.  **可解释性差**：其决策过程不透明，特别是在医疗、金融等领域。

1.  **更新和优化困难**：对大模型进行微调或更新常导致灾难性遗忘。

1.  **数据偏见与幻觉问题**：训练数据的偏差可能会影响模型生成结果的公平性和准确性。

相比之下，**传统NLP的优势**包括：

1.  **计算资源需求低**：适合资源受限的环境，如嵌入式设备。

1.  **可解释性好**：基于明确的数学模型，决策过程易于理解。

1.  **针对性强**：传统NLP在特定领域内表现较好，且对数据需求较少。

**传统NLP的劣势**则在于：

1.  **泛化能力有限**：在不同领域和任务间的迁移能力较差。

1.  **上下文理解能力弱**：无法处理复杂的长文本。

1.  **手工特征工程繁琐**：需要大量的领域知识和人力。

**两者的互补方式**包括：

1.  **结合应用**：在资源有限的特定领域，传统NLP可以进行初步处理，而大模型进一步优化结果。

1.  **提升大模型性能**：传统NLP的方法可以提高大模型的可解释性和性能。

1.  **任务分解与协作**：通过任务分解，结合两者优势处理不同子任务。

1.  **知识图谱与大模型结合**：大模型可以辅助知识图谱构建，知识图谱增强大模型的推理能力。

![](img/6023258f093242d18d2706a8d292bd0b.png)

# 招聘公告

---

![](img/bf5c1b34a243a035269da07fd56c472a.png)

**奇绩创坛****由陆奇博士**于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀F**24实习岗位**

##### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

##### 行研-GenAI研究岗位-战略分析（偏产品）

##### Web / AI 全栈开发工程师实习生

## **火热开放中🚀**

##### 行研-GenAI研究岗位-前沿学术探索

##### 行研-GenAI研究岗位-战略分析（偏咨询）

##### 创新活动产品运营 （大模型系列活动产品运营）

> **点击**相关岗位可查看对应**岗位需求【JD】；**相关岗位投递请**扫描二维码**

![](img/609fa1ec91aae10b4c7cd0b31426452d.png)

> 欢迎**推荐/自荐**

> 期待在**奇绩创坛**遇见更好的你

---

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

![](img/ca95770ec7d21b5d0bb50f97d90327c5.png)