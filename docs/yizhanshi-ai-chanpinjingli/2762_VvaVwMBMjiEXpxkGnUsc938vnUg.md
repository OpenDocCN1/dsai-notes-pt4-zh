# 2🈷️16日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OauNdQMhpoOJEZxo0HEco71unRd](https://v11enp9ok1h.feishu.cn/docx/OauNdQMhpoOJEZxo0HEco71unRd)

> 🔔AI情报助手早报「02月16日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🪅

> 万字长文！LangGraph Route...

>

+   介绍多智能体系统中Router模式的三层架构（路由层判断分发、专用智能体处理垂直领域任务、汇总层融合结果），Router作为分流器仅负责问题分发与结果汇总。

+   Router模式核心价值是剥离“问题分发决策”，简化Prompt与工具管理，解决多智能体系统中工具选择不可控、调试难的问题。

+   基于LangGraph实战搭建企业级多来源问答系统，涵盖GitHub/Notion/Slack三个专用智能体创建及流程串联（分类→并行查询→结果合成）。

+   Router模式适合知识分域、工具差异明显的单轮/弱多轮场景（如企业知识库），不适用于强多轮对话或智能体频繁交接任务。

+   进阶应用可将Router workflow封装为工具并添加记忆模块（如InMemorySaver），实现带记忆的多轮问答能力。

> 关键词：LangGraph，Router模式，多智能体系统，企业知识库问答，LangChain

>

> 🔗 文中引用的链接

+   [原文章链接](https://mp.weixin.qq.com/s/wm_OqcbvU9mEGz0_bms1WA)

+   Handoffs模式详解

+   （注：原文未提供具体URL）

> 2️⃣

> 🔔

> 创新Transformer！面壁基于稀疏...

>

+   面壁智能发布行业首个稀疏-线性混合注意力架构SALA及模型MiniCPM-SALA，突破Transformer全注意力平方复杂度瓶颈。

+   SALA混合25%InfLLM-V2稀疏注意力与75%Lightning线性注意力，无需额外技术可泛化至2048K上下文。

+   MiniCPM-SALA长文本任务优势明显，通用能力与同规模全注意力模型相当，推理性价比高。

+   256K序列下推理速度达Qwen3-8B的3.5倍，支持消费级GPU（如RTX5090）处理百万词元上下文。

+   联合发起SOAR大赛，聚焦稀疏算子优化，探索消费级GPU百万词元推理极限。

> 关键词：SALA架构，MiniCPM-SALA，稀疏-线性混合注意力，超长上下文，推理效率

>

> 🔗 文中引用的链接

+   GitHub:

+   https://github.com/openbmb/minicpm

+   HuggingFace:

+   [https://huggingface.co/](https://huggingface.co/openbmb/MiniCPM-SALA)openbmb/MiniCPM-SALA

+   ModelScope:

+   [h](https://www.modelscope.cn/models/OpenBMB/MiniCPM-SALA)ttps://www.modelscope.cn/models/OpenBMB/MiniCPM-SALA

+   GitCode:

+   [https://ai.gitcode.com/](https://ai.gitcode.com/OpenBMB/MiniCPM-SALA)OpenBMB/MiniCPM-SALA

+   技术报告:

+   [h](https://github.com/openbmb/minicpm/blob/main/docs/MiniCPM_SALA.pdf)ttps://github.com/openbmb/minicpm/blob/main/docs/MiniCPM_SALA.pdf

+   稀疏-线性混合注意力论文:

+   https://arxiv.org/pdf/2601.22156

+   InfLLM v2论文:

+   https://arxiv.org/pdf/2509.24663

+   SOAR大赛: 关注公众号次条及OpenBMB开源社区视频号

>

> （注：严格控制摘要总字数在200以内，五个要点简洁突出核心创新、架构设计、性能表现、推理效率及行业活动，符合要求。）

>

> 3️⃣

> 🔥

> 中国AI视频双雄并起：Seedance ...

>

+   中国AI视频模型Seedance2.0（以“导演思维”破圈）与Vidu Q3（登顶全球权威评测平台Artificial Analysis）集体崛起，成爆款制造机。

+   Vidu Q3“为剧而生”，支持16秒完整叙事、多角色多语种对话；Seedance2.0在镜头语言与节奏控制上表现稳定。

+   两者视听协同性高，音画同步无错位，生成内容接近“直接可投放”水准，适用于短剧、广告等场景。

+   创作路径分化：Seedance2.0侧重节奏调度，Vidu Q3侧重可控性（多参数调节），推动AI视频向创作平台进化。

+   国产模型技术跃升：Vidu生成速度超Sora2 10倍，开源框架效率提升100-200倍，整体进入全球第一梯队。

> 关键词：AI视频模型，Seedance2.0，Vidu Q3，国产AI技术，全球第一梯队

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/lo73ZrFduKYOm2vYzLa6LA)

+   全球权威AI评测平台Artificial Analysis

+   生数科技TurboDiffusion框架

> 4️⃣

> 🎮

> 春节档国产AI模型混战开打，MiniMa...

>

+   2026年春节前国产AI大模型混战升级，DeepSeek、智谱、字节等厂商密集更新，MiniMax-M2.5正式上线，重点提升Agent与编程能力。

+   MiniMax-M2.5网页制作能力突出，可视化效果佳，二次生成后可产出可游玩的"黄金矿工"网页游戏及苹果投资分析仪表盘。

+   该模型能生成6000字详实专业报告，PPT制作善用比喻润色，但定时任务中部分新闻摘要存在过时问题。

+   X用户反馈其与Opus4.6表现相当且体积小（传Mac mini可部署），视频生成还原氛围但品种识别不准确。

+   MiniMax港股当日涨幅14.62%，编程与Agent能力仍是大模型竞争焦点，多数生成结果需修改校对。

> 关键词：MiniMax-M2.5，国产AI大模型，Agent能力，编程能力，网页制作

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/hUN9BNPdXmwLI7GQh-LSTQ)

+   MiniMax AI工程负责人Skyler Miao在X平台发文

+   （注：原文未提供具体链接，此处为示意）

+   [X平台用户评价](https://x.com/user)

+   （注：原文未提供具体链接，此处为示意）

> 5️⃣

> 🤖

> AI产品用户留存仅三个月周期？对话王咏刚...

>

+   王咏刚（前谷歌工程师、Mootion创始人）指出AI编程已达及格线，程序员需转型与AI协作，他个人已从手写代码转为全依赖AI生成，工作形态巨变。

+   当前AI应用普遍面临“三个月留存魔咒”，仅基础模型（如ChatGPT）和解决实际问题的工具（如AI编程）能突破；AI视频生成需人类控制或抽卡筛选优质内容。

+   这一代Transformer-based AI上限为人类平均水平，擅长组合式模仿而非突破性创造，未来需新算法框架才能突破AGI瓶颈。

+   Mootion在中东教育市场通过符合当地教义的AI内容工具获近300万海外用户，AI视频生成需解决“塑料感”等真实感问题。

+   AI将替代初级程序员人力资源，未来程序员需培养与AI协作能力，高校传统编程教育需改革；创业建议关注垂直场景，分散投资不同类型人才。

> 关键词：AI协作，视频生成，用户留存，Transformer，中东教育

>

> 🔗 文中引用的链接

+   [原文链接](https://mp.weixin.qq.com/s/kxOjZtBXsha5S-J44qTVmw)

+   [来源：CSDN](https://www.csdn.net/)

+   （注：文中提及未附具体链接）

> 6️⃣

> 🤖

> 具身导航，感知推理到底是「上帝」，还是执...

>

+   具身导航正从几何路径规划向空间智能范式跃迁，融合三维视觉（3DGS/NeRF）、端到端智驾经验，超越简单A到B避障。

+   彭思达（三维视觉）与郝孝帅（MiMo-Embodied大模型）探讨核心争议：感知推理vs执行控制，强调空间理解与意图转化（如“渴了”→找水）。

+   技术挑战含无先验地图下的环境构建、记忆索引，端到端模型成趋势，心灵地图缓解长上下文内存压力。

+   导航数据上，真机与仿真差异可抹平，视频预训练加真机微调覆盖通用常识与物理规律（如避免视觉幻觉）。

+   未来聚焦跨场景泛化、多传感器融合鲁棒性、情感理解，工业巡检、家庭陪伴等场景5-10年有望爆发。

> 关键词：具身导航，空间智能，三维视觉，多模态大模型，心灵地图

>

> 🔗 文中引用的链接

+   [GAIR Live直播原文](https://mp.weixin.qq.com/s/lrSI8P49dwNovQ_Hc7OoSg)

+   [完整视频观看地址](https://youtu.be/vQxfw3ghuPo)