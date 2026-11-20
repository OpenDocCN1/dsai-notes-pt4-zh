
# 12月11日 AI情报早报

> 来源：[https://v11enp9ok1h.feishu.cn/docx/HkU8dUzlPorHFFxAgTjc4Fqsneg](https://v11enp9ok1h.feishu.cn/docx/HkU8dUzlPorHFFxAgTjc4Fqsneg)

> 🔔AI情报助手早报「12月11日」

## 1️⃣ MEET2026智能未来大会

明天！量子位的这件大事就要来了｜MEET2026

- 🔥 MEET2026智能未来大会12月10日在北京举办，涵盖大语言模型、多模态等主流AI议题。
- 🤖 设GenAI对话（文远知行韩旭对话李根）和Agent圆桌，探讨自动驾驶商业化及AI Agent落地。
- 🌟 近30位产学研顶流出席，包括清华张亚勤、孙茂松，百度、高通等企业代表。
- 📌 将发布2025年度AI十大趋势报告与人工智能年度榜单，梳理技术进展与行业领袖。
- 🔔 开放线下报名及线上直播，观众可获取前沿AI观点与实用落地指南。

关键词：MEET2026，AI大会，自动驾驶，AI Agent，AI趋势报告

🔗 文中引用的链接

- MEET2026智能未来大会原文链接

## 2️⃣ 猿辅导一个月连发两张 AI 牌：这次把 Agent 交给老师

- 猿辅导一月内发布“斑马口语”（AI外教）和“飞象老师”（教师Agent工具）两款AI产品，强调自身“扎根教育场景的AI公司”新身份。
- “飞象老师”是对话即应用工具，老师输入自然语言需求，AI生成含互动动画、板书、习题的全套课件，无需专业软件技能。
- 相比通用大模型，“飞象老师”实现0门槛操作、输出成体系教法、依托猿辅导知识库护栏，解决教学日常实用问题。
- 猿辅导将AI押注从零散功能推向垂直场景系统化重做，目标是升级课堂形态、革新学习范式。
- 教育作为超级场景，或成为最先跑出应用级Agent的领域之一，猿辅导的激进布局显示其前瞻性。

关键词：猿辅导，飞象老师，AI Agent，教育AI，对话即应用

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/APkbhRhc9oIpNYpJQtl6FQ)

## 3️⃣ 最新研究！大模型“角色扮演”无效了？

- 沃顿商学院测试6个主流AI模型（25000+次测试）发现，专家角色设定不提升对应领域回答准确性，反向低知识角色有时表现更好。
- 研究用GPQA Diamond（博士级理化生题）和MMLU-Pro（10选项题）等高难度测试集，确保结果严谨性。
- 仅Gemini 2.0 Flash例外，专家角色有显著改进；其他模型无变化或表现更差。
- Gemini存在“过度谨慎”问题：分配不匹配角色时（如物理专家答生物题）常拒绝回答，拖累整体表现。
- 角色提示仅改变回答方式/格式，不提升准确性；提供受众信息（如“我是Python程序员”）比角色扮演更有用，但角色提示在语气风格等方面仍有价值。

关键词：大模型，角色扮演提示，回答准确性，Gemini 2.0 Flash，受众信息

🔗 文中引用的链接

- 《夸夸ChatGPT可提升回答质量，原来是有依据的，来看OpenAI创始成员Andrej的解释》

## 4️⃣ SGLang Diffusion 学习笔记 (文末送书)

SGLang Diffusion是基于SGLang架构的扩散模型推理引擎，支持FLUX.1-dev等主流模型，采用模块化`PipelineStage`设计，各stage封装输入验证、文本编码等功能。

FLUX.1-dev采用双流架构：19层联合attention块+38层图像单独attention块，使用CLIP+T5双文本编码器完成prompt编码。

支持TP、Ulysses SP、USP、CFG Parallel等并行策略，可通过命令行参数灵活配置（如`--tp-size 2`、`--ulysses-degree 2`）。

提供多种Attention Backend（FlashAttention优先），根据硬件自动选择，可通过环境变量指定（如`SGLANG_DIFFUSION_ATTENTION_BACKEND=fa`）。

添加新模型流程清晰：定义配置类→实现`Transformer`→创建`Pipeline`→注册，利用`ReplicatedLinear`等组件快速支持并行。

关键词：SGLang Diffusion，扩散模型推理，并行策略，Attention Backend，FLUX.1-dev

🔗 文中引用的链接

- SGLang Diffusion官方博客
- [SGLang GitHub](https://github.com/sgl-project/sglang)
- [FastVideo](https://github.com/hao-ai-lab/FastVideo)
- [FLUX.1-dev模型](https://huggingface.co/black-forest-labs/FLUX.1-dev)
- 《Python大模型优化策略》抽奖链接

> （注：抽奖链接因原文未提供具体URL，故保留原文描述；若需实际链接请参考原文）

## 5️⃣ AutoGLM深夜开源，千千万万个手机Agent

2025年12月9日，智谱在豆包手机助手被封禁背景下开源手机Agent方案AutoGLM（含Phone Agent框架+AutoGLM-Phone-9B模型），开源链接为

- [https://github.com/zai-org/Open-AutoGLM](https://github.com/zai-org/Open-AutoGLM)

- AutoGLM提供本地/云端/混合三种部署模式，通过本地电脑或云服务当服务器，解决当前手机算力不足与隐私保护的中间态问题 🛡️
- 目前AutoGLM支持约50个应用，此前荣耀YOYO智能助手（如点咖啡功能）背后即AutoGLM技术 📱
- 作者类比Stable Diffusion、LLaMA开源的行业影响，认为AutoGLM开源或推动手机Agent生态爆发，称开源精神是"奔向共产主义的利剑" 🌟
- 文章提及技术发展的可能性与反思，强调技术不替人做价值选择，最终人生选择取决于自身 🤔

关键词：AutoGLM，手机Agent，开源，隐私保护，部署模式

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/6UNvm-🐧VjA7Ctja2VOhzA)
- [AutoGLM开源链接](https://github.com/zai-org/Open-AutoGLM)
- [作者豆包手机助手体验文章](https://mp.weixin.qq.com/s/...)
- （注：原文未提供完整链接，此处保留引用逻辑）

## 6️⃣ 法院：下班后回工作属于加班；估值 140 亿美元，软银、英伟达或投机器人公司；2025 年度词「赛博」| 极客早知道

- 🤖 软银与英伟达拟以140亿美元估值投资机器人基础模型开发商Skild AI，其专注机器人通用软件研发，此前B轮估值47亿。
- 🔥 特斯拉Grok升级车载功能，海外车主可用其新增导航目的地，支持“脱缰”等人格模式。
- 🎯 智元第5000台人形机器人灵犀X2量产下线，标志具备批量出货能力，黄晓明现场提货。
- 🌏 Google发布XR设备矩阵，依托Gemini大模型，覆盖AI眼镜到旗舰头显，实现多模态交互。
- 🚀 李笛新公司Nextie押注认知大模型，奇绩参与投资，预计2026年1月7日上线。

关键词：AI机器人，Grok，XR设备，认知大模型，赛博年度词

🔗 文中引用的链接

- [原文链接](https://mp.weixin.qq.com/s/XDL5T6vT2zTF6ScbBMfYKQ)
