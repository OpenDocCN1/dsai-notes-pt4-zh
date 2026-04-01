# 10🈷️23日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/MwTKdIbV2onXXUxFI7IcP26enJd](https://v11enp9ok1h.feishu.cn/docx/MwTKdIbV2onXXUxFI7IcP26enJd)

> 🔔AI情报助手晚报「10月23日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> 不用强化学习也能推理，哈佛新采样算法竟能...

>

+   哈佛大学研究探索不通过强化学习，仅用采样让基础模型具备推理能力，提出基于基础模型似然度的迭代采样算法。

+   算法核心基于分布锐化概念，采用幂分布采样，避免低温采样的缺陷，能提升推理时高似然度token权重。

+   利用Metropolis-Hastings算法从幂分布近似采样，并提出自回归MCMC幂采样，降低计算成本。

+   实验表明，新算法在多种推理基准测试中显著提升基础模型性能，媲美强化学习。

+   该算法优势在于避免多样性崩溃、推理灵活、参数稳健、计算成本可控。

> 关键词：基础模型，采样算法，推理能力，强化学习，分布锐化

>

> 🔗 文中引用的链接

+   [论文地址](https://www.arxiv.org/pdf/2510.14901)

+   [项目网站](https://aakaran.github.io/reasoning_with_sampling/)

+   [代码地址](https://github.com/aakaran/reasoning-with-sampling)

> 2️⃣

> 🎯

> X上63万人围观的Traning-Free GRPO

>

+   年初DeepSeek-R1带火GRPO算法，但训练模型参数成本高，中小团队难负担。

+   腾讯优图提出Training-Free GRPO，将GRPO训练范式迁移到上下文学习，不更新模型参数。

+   实验显示，在数学推理和网页搜索场景均有性能提升，还能减少工具调用次数。

+   该方法保留GRPO优势，成本暴降且泛化性更好。

+   此方法已开源，未来将集成到Youtu - Agent框架。

> 关键词：GRPO，Training-Free GRPO，上下文学习，强化学习，开源

>

> 🔗 文中引用的链接

+   [arXiv链接](https://arxiv.org/abs/2510.08191)

+   [GitHub地址](https://github.com/TencentCloudADP/youtu-agent/tree/training_free_GRPO)

> 3️⃣

> 🤖

> R-HORIZON：长程推理时代来临，复...

>

+   复旦大学与美团LongCat Team联合推出R-HORIZON，是首个评估与增强LRMs长链推理能力的方法与基准。

+   其提出Query Composition方法，构建评测基准与训练数据，以评估和提升模型长链推理能力。

+   评测发现顶级模型在长链推理场景性能断崖式下降，存在有效推理长度受限等瓶颈。

+   使用R-HORIZON数据训练，实现长链与单问题任务的双重性能提升，改变推理机制。

+   R-HORIZON框架已开源，推动下一代推理模型发展。

> 关键词：R-HORIZON，长链推理，Query Composition，模型瓶颈，开源

>

> 🔗 文中引用的链接

+   [论文地址](https://arxiv.org/abs/2510.08189)

+   [项目主页](https://reasoning-horizon.github.io)

+   [代码地址](https://github.com/meituan-longcat/R-HORIZON)

+   [数据集](https://huggingface.co/collections/meituan-longcat/r-horizon-68f75703a95676fbfed97879)

> 4️⃣

> 🎯

> 前Meta大神创业，用强化学习打造Pok...

>

+   前Meta大神创业成立Pokee AI，2025年10月发表论文，用强化学习打造PokeeResearch-7B模型刷新AI深度研究SOTA。

+   该模型核心创新为靠AI反馈训练及“三思而后行”推理和自我验证框架，采用自我修正等三种互补技术。

+   在10项基准测试中，基于同一开源模型，PokeeResearch-7B展现最先进性能，平均准确率超对手。

+   Pokee AI产品已公开测试并与谷歌合作，其技术路线是用强化学习帮AI智能体规划和使用工具。

+   今年7月，Pokee AI完成1200万美元种子轮融资，领投跟投方众多。

> 关键词：强化学习，PokeeResearch-7B模型，AI反馈，自我验证，融资

>

> 🔗 文中引用的链接

+   https://pokee.ai

+   https://www.zheqingbillzhu.com/

+   [https://www.linkedin.com/posts/zheqingzhubill_we-are-thrilled](https://www.linkedin.com/posts/zheqingzhubill_we-are-thrilled-to-announce-that-pokee-ai-activity-7348382608700973056-PwIC)-to-announce-that-pokee-ai-activity-7348382608700973056-PwIC

+   [https://sites.go](https://sites.google.com/view/yi-wan)ogle.com/view/yi-wan

+   https://arxiv.org/abs/2510.15862

> 5️⃣

> 🤖

> 0.027B手机AI模型，估值2亿美金，...

>

+   清华万格智能团队凭借“基于类脑架构的下一代通用模型与智能体生态”项目，获中国国际大学生创新大赛冠军，项目估值2亿美元。

+   团队研发的智人HRM模型，参数量仅0.027B，可本地运行，推理和小样本学习能力强，大幅节省算力。

+   基于HRM模型，团队研发的气候预测专家模型达业界顶尖，能有效填补气象预测空白。

+   团队勇于突破主流AI技术路线，尝试为AI更换“大脑”。

+   新生代创业者成长于数字时代，敢于挑战根本问题。

> 关键词：AI模型，类脑架构，气候预测，新生代创业者，技术突破

>

> 🔗 文中引用的链接

+   [https://www.tsinghua.edu.cn/](https://www.tsinghua.edu.cn/info/1182/121813.htm)info/1182/121813.htm

+   https://arxiv.org/abs/2506.21734

+   [https://hen.chinadaily.com.cn/a/2](https://hen.chinadaily.com.cn/a/202510/16/WS68f05b71a310c4deea5ec8c0.html)02510/16/WS68f05b71a310c4deea5ec8c0.html

+   http://www.daheapp.com/news/2219388

+   [h](https://news.cctv.com/2025/10/16/ARTIOJDHL5klfbFIfynFHAtO251016.shtml)ttps://news.cctv.com/2025/10/16/ARTIOJDHL5klfbFIfynFHAtO251016.shtml

> 6️⃣

> 🎯

> 和人类一样？LLM也会认知能力会退化，当...

>

+   研究假设让LLM在“垃圾网络文本”持续预训练，其认知能力会持久下降，并从互动度与语义质量度量“垃圾数据”。

+   用四个LLM做对照实验，发现推理与长上下文能力在两种“垃圾数据”训练下均下降，安全与人格方面，参与度造成负面更强。

+   分析错误模式发现模型易“思维跳步”，养成“少说、快答、不展开”倾向。

+   尝试两种缓解方法，效果有限，推断LLM脑腐烂是内部表征发生“偏移”。

+   研究表明数据质量至关重要，训练数据策采本质是安全问题。

> 关键词：LLM，认知退化，垃圾数据，思维跳步，数据策采

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/0h-BNSoq0hXiXYjKXsfboQ)