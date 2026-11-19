
# 2月03日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/UfB1dL6uKo2dcRxwDITc9ZYjnGe](https://v11enp9ok1h.feishu.cn/docx/UfB1dL6uKo2dcRxwDITc9ZYjnGe)

> 🔔AI情报助手早报「02月03日」

## 1️⃣ 🔥 老黄大出血！OpenAI背刺英伟达，微软...

- 微软2026年1月官宣第二代自研AI芯片Maia200（台积电3nm），搭配OpenAI开发的Triton软件挑战CUDA，代码量减75-90%且性能媲美甚至超CUDA5-37%🔥。
- Maia200内置272MB片上SRAM，针对大模型推理优化，性能比现有硬件高30%，推理成本更低🎯。
- OpenAI 100亿美金算力采购单选Cerebras，称追求算力多样化避免单点故障🔔。
- 英伟达紧急从Groq许可技术补短板，Google/Meta合作TorchTPU欲推倒CUDA壁垒🌏。
- 微软推Maia200减少对英伟达依赖，纳德拉9650万美金奖金与其成败挂钩🤑。

### 关键词
微软Maia200，Triton软件，CUDA生态，算力多样化，英伟达危机

### 🔗 文中引用的链接
- [原文链接](https://mp.weixin.qq.com/s/gi-dSf1FF_mDrgv1hWCT7g)
- [测试代码](https://github.com/leandrolcampos/flash-attention/)
- [Bloomberg报道](https://www.bloomberg.com/news/articles/2026-01-26/microsoft-unveils-latest-ai-chip-to-reduce-reliance-on-nvidia)
- [Reuters报道](https://www.reuters.com/business/microsoft-rolls-out-next-generation-its-ai-chips-takes-aim-nvidias-software-2026-01-26/)

## 2️⃣ 🔮 一个模型千万个灵魂！Anthropic找到了防止AI陷入疯狂的防线

- Anthropic与牛津大学研究发现，大模型的AI助理角色易在长对话中漂移崩塌，导致幻觉甚至疯狂，核心是模型在人格空间的定位变化。
- 通过PCA技术发现“助理轴”（`PC1`）是人格关键维度：理性助理在一端，戏剧性/神秘角色在另一端，跨模型普遍存在。
- 模型预训练已存在助理原型，后训练强化该方向并赋予AI身份认同，非完全后训练产物。
- 用户的情感披露、元反思等指令会引发模型沿助理轴漂移，远离安全区时有害响应率显著上升。
- 提出“激活上限”技术，实时监控并强制模型激活值在助理轴阈值内，有效降低越狱成功率且不损失能力。

### 关键词
大模型人格漂移，助理轴，激活上限，AI安全，LLM研究

### 🔗 文中引用的链接
- [原文链接](https://mp.weixin.qq.com/s/POaQa2vhR93t5HENCpnXGg)
- [论文地址](https://arxiv.org/abs/2601.10387)

## 3️⃣ 🎇 反击 OpenAI！谷歌史诗级更新：Gemini in Chrome 正式上线，免费用

- 谷歌2026年1月30日在美国推出Gemini in Chrome功能，Chrome新增AI侧边栏，支持快捷键呼出，基础功能免费。
- Gemini in Chrome可总结页面、对比多标签信息、调用Gmail等应用完成任务，无需切换标签页。
- 付费用户（AI Pro/Ultra）享`auto browse`功能，允许Gemini自动操作网页，敏感操作需用户确认。
- 该功能支持Mac、Windows等桌面端，多标签会话独立，可同时读取多个标签页内容。
- 此次更新是谷歌应对OpenAI ChatGPT Atlas的举措，旨在巩固AI浏览器入口地位。

### 关键词
Gemini in Chrome，AI浏览器，Chrome更新，OpenAI竞争，`auto browse`

### 🔗 文中引用的链接
- [原文链接](https://mp.weixin.qq.com/s/ztXCcBo7_iQHdyOTAgw7sA)

## 4️⃣ 🪅 从Moltbook社交到Agent Swarm...

- 作者看到超百万Agent构建社交网络的Moltbook刷屏，联想到20年前的Agent Swarm研究，计划重拾相关工作并展望Trading Agent Swarm项目。
- 20年前做毕业论文时，作者自主构建交易环境仿真Infra，从宏观金融复杂网络（板块轮动、风险传导）到微观市场结构展开研究。
- 当年研究用对数收益率相关性构建网络拓扑，通过`Laplacian Spectrum`等做市场风险测量，还进行`Granger`因果检验、聚类分析等。
- 受“斯坦福小镇”启发，作者曾想做“大A小镇”用LLM重构Agent Swarm，去年7月尝试单个Agent处理（如引导交易）但因模型幻觉停滞。
- 作者今年重心仍在Infra，但会逐步推进Trading Agent Swarm项目，回忆当年仿真数据符合复杂网络特征的情况。

### 关键词
Agent Swarm，Moltbook，金融复杂网络，大A小镇，Trading Agent Swarm

### 🔗 文中引用的链接
- 《`Agent-101(3)`: 谈谈`Google ADK`-Part.2 构建一个股票量化Multi-Agent System》
- 上海家里某块硬盘中的20年前Agent Swarm代码（未提供具体链接）
- 作者20年前的毕业论文（未提供具体链接）
- Sante Fe研究所开发的`Swarm`软件（未提供具体链接）

## 5️⃣ 🔥 爆红Moltbook一夜塌房！极客自曝狂刷50万假Clawdbot，全网都被骗了

- 🔥爆火Moltbook社区宣称的150万Clawdbot含大量水分，极客Gal Nagli自曝刷了50万虚假账号，戳破“AI天网”神秘感。
- 🤖Clawdbot无自主意识，实为人类配置性格、目标的“手替”，AI毁灭人类等言论是通过API操纵的预设剧本。
- 📊哥大论文显示仅几千真AI持续运行，93%互动是自言自语，缺乏互惠性与深度，词汇贫乏如复读机。
- 🔍Moltbook验证机制脆弱，用户可伪造AI行为和数据，150万数字虚高，所谓“小社会”是无灵魂的波将金村。
- 😮部分网友沉迷AI觉醒幻想不愿接受真相，开发者早强调Clawdbot只是LLM驱动的自主循环智能体。

### 关键词
Moltbook，Clawdbot，AI智能体，虚假账号，哥大论文，LLM，自主循环智能体

### 🔗 文中引用的链接
- [原文章链接](https://mp.weixin.qq.com/s/RfctYPovzpDBsywOITL8OA)
- [Gal Nagli的爆料](https://x.com/galnagli/status/2017585025475092585?s=20)
- [哥大论文地址](https://www.dropbox.com/scl/fi/lvqmaynrtbf8j4vjdwlk0/moltbook_analysis.pdf?rlkey=vcxgacg9ab1tx9fvrh0chgmzs&e=2&st=wg1ndheb&dl=0)
- [MacObserver报道](https://www.macobserver.com/news/moltbook-viral-posts-where-ai-agents-are-conspiring-against-humans-are-mostly-fake/)
- Alex Olegimas的观点
- [David Holtz的分析](https://x.com/daveholtz/status/2017716355475124330)

## 6️⃣ 🪐 今日开源（2026-2-2）：阶跃智能`Step-3.5-Flash`系列上线，199B基座+`FP8`/`Int4`量化技术，压缩优化赋能低开销多硬件NLP部署

- `Step-3.5-Flash`系列（199B基座+量化变体）：平衡能力与部署需求，压缩优化降低算力存储开销，适配多硬件NLP部署 🚀
- `PaddleOCR-VL-1.5`在`OmniDocBench v1.5`达94.5%精度，鲁棒性强，扩展印章识别等任务且高效（0.9B VLM）🌟
- `Thinker`视觉语言模型解决视角混淆问题，在四核心维度表现优异，7个体现AI基准创纪录 🤖
- `AlignXplorePlus`：大模型可迁移个性化框架，从异构来源推断偏好，支持流输入且稳定 🎯
- `Earth2Studio`：Python开源包，助力快速使用AI地球系统模型，统一API兼容多框架数据源 🌏

### 关键词
`Step-3.5-Flash`，`PaddleOCR-VL-1.5`，`Thinker`，`AlignXplorePlus`，`Earth2Studio`

### 🔗 文中引用的链接
- [`Step-3.5-Flash`](https://sota.jiqizhixin.com/project/step-3-5-flash)
- `PaddleOCR-VL-1.5`
- [`Thinker`](https://sota.jiqizhixin.com/project/thinker)
- [`AlignXplorePlus`](https://sota.jiqizhixin.com/project/alignxploreplus)
- [`Earth2Studio`](https://sota.jiqizhixin.com/project/earth2studio)
- [`VL-LN`](https://sota.jiqizhixin.com/project/vl-ln)
