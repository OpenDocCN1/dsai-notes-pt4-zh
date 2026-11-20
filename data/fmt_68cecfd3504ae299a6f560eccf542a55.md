
# 12🈷️21日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Un4pdHLnzo2OGbxiYrhcvvG7n6d](https://v11enp9ok1h.feishu.cn/docx/Un4pdHLnzo2OGbxiYrhcvvG7n6d)

> 🔔AI情报助手早报「12月21日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ 🔔 OpenAI官方论文“泄密”GPT-5：RL到底有没有教坏CoT？万字深度实测

+ 🔍 OpenAI论文《Monitoring Monitorability》用GPT-5构建CoT可监控性框架，发现当前RL未降低可监控性，思维链越长越易监控。
+ 📊 提出双向Monitorability指标，采用TPR×TNR及修正指标解决安全评估类别不平衡问题。
+ ⚖️ 对比实验显示CoT是监控关键，GPT-5/o3-mini表现佳，Claude 3.7 Sonnet垫底或因RLHF策略差异。
+ 🤔 自然RL训练中可监控性上升，强行奖励混淆会使其骤降，模型倾向输出可理解思考过程。
+ 💡 “小模型+长CoT”监控性更高（需付监控税），附录公开混淆模型训练的Reward Prompt供红队测试。

**关键词：** GPT-5，CoT可监控性，RLHF，Monitorability，监控税

**🔗 文中引用的链接**

+ [原文链接](https://mp.weixin.qq.com/s/ZHCqHoXZLyIGsU3GcHPrtg)
+ [论文地址](https://cdn.openai.com/pdf/d57827c6-10bc-47fe-91aa-0fde55bd3901/monitoring-monitorability.pdf)

## 2️⃣ 🔥 OpenAI估值冲击8300亿美元！奥特曼背水一战：我们无路可退

+ OpenAI计划筹集1000亿美元，投后估值或达8300亿美元（2026年预计营收32倍），暗示IPO前奏（奥特曼称规模需巨额资金但无兴趣当上市CEO）。
+ OpenAI承诺未来8年约1.4万亿美元基础设施投资，2025-2026年现金投入约260亿，营收远不及算力军备竞赛所需。
+ OpenAI与AWS签380亿7年算力合同，拟用Trainium芯片减少英伟达依赖，还与AMD、谷歌TPU等分散算力供应链。
+ OpenAI获2025雅虎财经年度公司，但因谷歌Gemini3竞争，奥特曼发红色警戒集中资源研发ChatGPT。
+ OpenAI去微软化，与亚马逊洽谈超100亿投资，合作公司股价多上涨（如甲骨文涨38%、AMD单日涨24%）。

**关键词：** OpenAI融资，算力军备竞赛，供应链分散，AI竞争，IPO前奏

**🔗 文中引用的链接**

+ [CNBC报道](https://www.cnbc.com/2025/12/16/openai-in-talks-with-amazon-about-investment-could-top-10-billion.html)
+ [雅虎财经年度公司报道](https://finance.yahoo.com/news/openai-is-the-2025-yahoo-finance-company-of-the-year-120054312.html)
+ The Information报道

## 3️⃣ 🔒 Anthropic公布新技术：不靠删数据，参数隔离移除AI危险

+ 🔍 大语言模型存在双重用途风险，传统后训练安全措施易被绕过，预训练数据过滤面临标注成本高、有害内容混杂等挑战。
+ 🛡 Anthropic提出SGTM技术，通过选择性梯度掩码将危险知识定位到特定参数区域，训练后置零该区域即可移除危险能力，保留通用能力。
+ 📊 在维基百科模型实验中，SGTM比数据过滤更优：捕获弱过滤漏网危险内容，避免强过滤误伤邻近学科，计算开销仅增约5%。
+ 🛡️ 对抗式微调测试显示，SGTM抗恢复能力是传统后训练遗忘方法（RMU）的7倍，与完美数据过滤模型鲁棒性相当。
+ 🧬 SGTM通过梯度路由和吸附效应，未标注危险数据也会聚集到指定参数，实现真正知识移除而非表层抑制。

**关键词：** SGTM，双重用途风险，大语言模型，知识移除，对抗式微调

**🔗 文中引用的链接**

+ [原文链接](https://mp.weixin.qq.com/s/-36m-d7VURKgoEusIwvggQ)
+ [论文地址](https://arxiv.org/pdf/2512.05648)
+ [代码地址](https://github.com/safety-research/selective-gradient-masking)
+ [参考链接](https://alignment.anthropic.com/2025/selective-gradient-masking/)

## 4️⃣ 🔍 Google突破性大模型系统Adjudicator：多智能体+知识图谱攻克数据自动标注的机器学习'噪声标签'顽疾

+ 🎯 Google研发的Adjudicator系统结合知识图谱与多智能体LLM架构，自动识别纠正训练数据噪声标签，在AlleNoise基准获0.99 F1分数，远超单一LLM（0.48）和非KG多智能体系统（0.59）。
+ 🚨 工业场景人工标注错误率高达40%，传统噪声标签技术语义理解弱，LLM直接应用易幻觉且缺乏可验证推理能力。
+ 🔍 Adjudicator创新将KG作为动态推理环境，多智能体协作/对抗验证，通过层次祖先距离捕捉深层语义错误。
+ 🛠️ 系统含动态KG构建（整合上下文）和智能体裁决（加权投票+覆盖逻辑确保100%结构性错误召回）两大阶段。
+ 🌟 Adjudicator降低标注成本、提升模型可信度，未来可扩展至医疗/法律领域，为数据中心AI提供实践案例。

**关键词：** 知识图谱，噪声标签，多智能体系统，LLM，数据质量

**🔗 文中引用的链接**

+ [原文链接](https://mp.weixin.qq.com/s/9HrfFeCA8TewkRsW3KslhQ)

## 5️⃣ 🔬 谷歌、英伟达、OpenAI在列，美国能源部宣布与24家机构达成协议，共同推进「创世纪计划」

+ 🔬 美国能源部与谷歌、英伟达、OpenAI等24家机构达成协议，共同推进“创世纪计划”，加速AI驱动的科学发现。
+ 🚀 该计划源于特朗普行政命令，旨在构建国家AI基础设施，拓展科学边界并共享发展成果。
+ 🤖 Google DeepMind将为能源部17个国家实验室提供基于Gemini的AI科研助手及AlphaEvolve等工具。
+ 🔍 OpenAI参与合作，属于其OpenAI for Science计划，聚焦基础与应用研究的推进。
+ 🧬 Anthropic将提供Claude模型访问及定制工具，助力能源、生物与生命科学领域的重大进展。

**关键词：** 创世纪计划，AI科研合作，谷歌DeepMind，OpenAI，Anthropic

**🔗 文中引用的链接：**

+ [能源部官方公告](https://www.energy.gov/articles/energy-department-announces-collaboration-agreements-24-organizations-advance-genesis)
+ Google DeepMind合作详情
+ [OpenAI合作说明](https://openai.com/index/us-department-of-energy-collaboration/)
+ [Anthropic合作新闻](https://www.anthropic.com/news/genesis-mission-partnership)

## 6️⃣ 🔬 美国「曼哈顿计划」启动！OpenAI谷歌等24巨头打响「科技珍珠港之战」

+ 美国能源部联合OpenAI、谷歌等24家科技巨头启动“创世纪计划”（AI曼哈顿计划），将AI与国家实验室科研能力结合，应用于可控核聚变、能源材料发现等重大科学研究。
+ OpenAI与谷歌首次携手合作，不再围绕大模型竞争，共同投入能源、量子计算等前沿科学问题，目标2030年美国科学生产力翻倍。
+ 计划核心是打造AI驱动科研平台，把AI变为科研默认工具，统一国家实验室、超级计算机等资源到AI平台，引入企业先进AI能力。
+ 美国能源部牵头因掌握顶级超级计算机、战略科研数据及实验室体系，适合AI介入核聚变模拟等高算力、高成本领域。
+ 24家企业涵盖全栈力量，英伟达为算力核心，OpenAI等模型公司将大模型用于硬核科学领域，成为科学发现引擎。

**关键词：** 创世纪计划，AI曼哈顿计划，OpenAI，谷歌，美国能源部

**🔗 文中引用的链接**

+ [路透社消息](https://www.reuters.com/business/retail-consumer/us-energy-department-taps-big-tech-ai-powered-research-push-2025-12-18/?utm_source=chatgpt.com)
+ [OpenAI官方博客](https://openai.com/index/us-department-of-energy-collaboration/)
+ [谷歌DeepMind官方博客](https://deepmind.google/blog/google-deepmind-supports-us-department-of-energy-on-genesis/)
